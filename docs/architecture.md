# Architecture

## What the System Is

Tinker is a Raspberry Pi-based mobile robot that can listen, think, speak, see, and move. All subsystems run concurrently on a single `asyncio` event loop.

## Modules

| Module | Domain | Responsibility |
|---|---|---|
| `brain` | LLM | Conversation and reasoning via Claude API |
| `ears` | STT | Microphone capture + speech-to-text via faster-whisper |
| `voice` | TTS | Text-to-speech output via piper-tts |
| `eyes` | Vision | Camera capture + face detection/recognition |
| `body` | Hardware | Servo and motor control via gpiozero |
| `config` | Config | Settings from environment variables via pydantic-settings |
| `main` | Orchestration | Async entry point, wires all modules together |

## How Modules Connect

```
          ┌──────────┐
          │   main   │  ← async entry point, wires everything
          └────┬─────┘
               │
    ┌──────────┼──────────┬──────────┐
    │          │          │          │
┌───▼──┐  ┌───▼──┐  ┌────▼───┐  ┌──▼───┐
│ ears │  │ eyes │  │  body  │  │voice │
│ (mic)│  │(cam) │  │(motors)│  │(spkr)│
└───┬──┘  └──────┘  └────────┘  └──▲───┘
    │                               │
    │  audio text                   │  response text
    │         ┌────────┐            │
    └────────►│ brain  │────────────┘
              │ (LLM)  │
              └────────┘
```

## Data Flow

1. `ears` captures audio from microphone → transcribes to text via faster-whisper
2. Text is sent to `brain` → Claude API processes and generates a response
3. Response text is sent to `voice` → piper-tts synthesizes speech → plays through speaker
4. `eyes` runs concurrently — captures camera frames, detects faces, provides visual context to `brain`
5. `body` receives movement commands from `brain` — controls servos and motors
6. `config` provides settings to all modules from environment variables

## Key Constraints

- All I/O is async — no blocking calls on the main event loop
- Each module must work independently in mock mode (`--mock`)
- If a subsystem fails (no camera, no mic), the robot continues without it
