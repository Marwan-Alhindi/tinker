# Tinker — AI-Powered Mobile Robot

## Project Overview

Tinker is a small Raspberry Pi-based mobile robot with AI capabilities:
- **LLM conversation** via Claude API (`anthropic` SDK)
- **Speech-to-text** via `faster-whisper` (local, on-device)
- **Text-to-speech** via `piper-tts` (neural TTS, on-device)
- **Computer vision** via `opencv-python` + `face_recognition`
- **Physical movement** via servos and DC motors (`gpiozero` + `pigpio`)

Coordinated through an async Python runtime (`asyncio`). Config via `pydantic-settings` + `.env`.

## Project Structure

```
tinker/
├── .claude/
│   ├── CLAUDE.md              # project context, enforces DDD & TDD
│   ├── rules/
│   │   ├── coding.md          # coding standards (SDD)
│   │   ├── testing.md         # testing standards (SDD)
│   │   └── documentation.md   # documentation standards (SDD)
│   ├── commands/              # reusable workflows (/command-name)
│   └── skills/                # complex workflows (/skill-name)
├── README.md
├── pyproject.toml
├── .env                       # local env vars (git-ignored)
├── docs/
│   └── architecture.md        # system design, module map (DDD)
├── src/
│   └── tinker/
│       ├── __init__.py
│       ├── main.py            # async entry point — wires everything together
│       ├── main.md            # spec: orchestration, startup/shutdown
│       ├── brain.py           # LLM conversation (Claude API)
│       ├── brain.md           # spec: conversation API, prompt contracts
│       ├── ears.py            # STT / microphone input
│       ├── ears.md            # spec: audio capture, whisper integration
│       ├── voice.py           # TTS / speaker output
│       ├── voice.md           # spec: synthesis pipeline, piper integration
│       ├── eyes.py            # camera / computer vision
│       ├── eyes.md            # spec: camera capture, face detection
│       ├── body.py            # motor and servo control
│       ├── body.md            # spec: movement commands, GPIO mapping
│       ├── config.py          # pydantic-settings config from env
│       └── config.md          # spec: env vars, settings schema
├── tests/
│   ├── test_brain.py
│   ├── test_ears.py
│   ├── test_voice.py
│   ├── test_eyes.py
│   └── test_body.py
├── models/                    # downloaded STT/TTS model files (git-ignored)
├── scripts/                   # helper scripts (deploy, setup, etc.)
└── systemd/                   # systemd unit files for auto-start on Pi
```

## How This Repo Works

| Location | Purpose | Written by | Read by | When |
|---|---|---|---|---|
| `.claude/CLAUDE.md` | Project context, enforces DDD & TDD | You | AI | Always, every conversation |
| `.claude/rules/*.md` | Coding & testing standards (SDD) | You | AI | Auto, when touching matching files |
| `.claude/commands/*.md` | Reusable workflows | You | AI | When you type /command-name |
| `.claude/skills/` | Complex workflows + supporting files | You | AI | When you type /skill-name |
| `docs/architecture.md` | System design, module map (DDD) | You + humans | AI + humans | When understanding big picture |
| `src/tinker/<module>.md` | Module API & details (DDD) | You (AI updates) | AI | When working on that module |
| `tests/test_<module>.py` | Tests (TDD) | AI | AI | Written FIRST |
| `src/tinker/<module>.py` | Implementation | AI | AI + humans | Written LAST, after tests exist |

## Development Flow

For every feature or change:

1. **Read** `docs/architecture.md` to understand the big picture
2. **Read** `src/tinker/<module>.md` for the module you're working on
3. **Write/update** the companion `.md` spec BEFORE any code
4. **Write tests** based on the spec (they should fail)
5. **Write implementation** to make tests pass
6. **Update** the companion `.md` if new APIs were added

Never write code without a spec. Never write implementation before tests.

## Setup

```bash
git clone https://github.com/Marwan-Alhindi/tinker.git
cd tinker
python -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
cp .env.example .env   # fill in ANTHROPIC_API_KEY
```

## How to Test

```bash
pytest tests/ -v
```

## How to Run

```bash
# Local (laptop) — mock hardware
python -m tinker.main --mock

# On the Pi
python -m tinker.main
```

## Important Commands

```bash
# Lint
ruff check src/ tests/
ruff format src/ tests/

# Run a single module
python -m tinker.brain
python -m tinker.ears
python -m tinker.voice
python -m tinker.eyes
python -m tinker.body

# Deploy to Pi
./scripts/deploy.sh

# Check service on Pi
ssh pi@tinker.local 'systemctl status tinker'
ssh pi@tinker.local 'journalctl -u tinker -f'
```

## Environment Variables

| Variable | Description | Default |
|---|---|---|
| `ANTHROPIC_API_KEY` | Claude API key | (required) |
| `WHISPER_MODEL` | faster-whisper model size | `tiny` |
| `PIPER_VOICE` | piper-tts voice model path | `en_US-lessac-medium` |
| `CAMERA_INDEX` | camera device index | `0` |
| `LOG_LEVEL` | logging level | `INFO` |
| `MOCK_MODE` | stub hardware for laptop dev | `false` |

## Hardware

- **Computer:** Raspberry Pi 4 or 5
- **Camera:** Pi Camera Module (CSI) or USB webcam
- **Microphone:** USB mic or USB audio adapter
- **Speaker:** 3.5mm or USB speaker
- **Servos:** SG90 / MG996R — head/arm movement
- **DC Motors + Driver:** L298N or DRV8833 — wheel drive
- **Chassis:** custom or kit-based wheeled platform

## Key Design Decisions

| Decision | Chosen | Over | Why |
|---|---|---|---|
| STT engine | `faster-whisper` | `openai-whisper` | 4x faster, 4x less memory via CTranslate2 — critical for Pi |
| TTS engine | `piper-tts` | `pyttsx3` | Natural-sounding neural voices, optimized for ARM |
| GPIO library | `gpiozero` | `RPi.GPIO` | Higher-level API, mock pins for testing |
| Module naming | Body-part metaphor | Generic names | Intuitive (brain=LLM, ears=STT, voice=TTS, eyes=vision, body=motors) |
| Concurrency | `asyncio` | `threading` | Single event loop, no race conditions |
| Dev workflow | `--mock` flag | Pi-only dev | Fast iteration on laptop without hardware |
