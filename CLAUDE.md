# Tinker — AI-Powered Mobile Robot

## Project Overview

Tinker is a small Raspberry Pi-based mobile robot with AI capabilities:
- **LLM conversation** via Claude API
- **Speech-to-text** via faster-whisper (local, on-device)
- **Text-to-speech** via piper-tts (neural TTS, on-device)
- **Computer vision** via OpenCV + face_recognition
- **Physical movement** via servos and DC motors (gpiozero)

The robot listens, thinks, speaks, sees, and moves — all coordinated through an async Python runtime.

## Tech Stack

| Capability | Library | Notes |
|---|---|---|
| LLM | `anthropic` SDK | Claude API for conversation and reasoning |
| STT | `faster-whisper` | CTranslate2-based, fast and low-memory on Pi |
| TTS | `piper-tts` | Neural TTS optimized for ARM; `pyttsx3` as fallback |
| Vision | `opencv-python` + `face_recognition` | Camera capture, face detection/recognition |
| Hardware | `gpiozero` with `pigpio` pin factory | Servo/motor control with hardware PWM |
| Audio I/O | `pyaudio` or `sounddevice` | Mic input and speaker output |
| Concurrency | `asyncio` | Single event loop, no threads |
| Config | `pydantic-settings` + `.env` | Typed settings from environment variables |

## Project Structure

Modules use a **body-part metaphor** for clarity:

Each `.py` source file has a **companion `.md` spec** right beside it (DDD pattern).

```
tinker/
├── CLAUDE.md
├── README.md
├── pyproject.toml
├── .env                    # local env vars (git-ignored)
├── .claude/
│   └── rules/              # SDD: coding, testing, and documentation standards
├── docs/
│   └── architecture.md     # big picture: module relationships, design decisions
├── src/
│   └── tinker/
│       ├── __init__.py
│       ├── main.py         # async entry point — wires everything together
│       ├── main.md         # spec: orchestration, startup/shutdown
│       ├── brain.py        # LLM conversation (Claude API)
│       ├── brain.md        # spec: conversation API, prompt contracts
│       ├── ears.py         # STT / microphone input
│       ├── ears.md         # spec: audio capture, whisper integration
│       ├── voice.py        # TTS / speaker output
│       ├── voice.md        # spec: synthesis pipeline, piper integration
│       ├── eyes.py         # camera / computer vision
│       ├── eyes.md         # spec: camera capture, face detection
│       ├── body.py         # motor and servo control
│       ├── body.md         # spec: movement commands, GPIO mapping
│       ├── config.py       # pydantic-settings config from env
│       └── config.md       # spec: env vars, settings schema
├── tests/
│   ├── test_brain.py
│   ├── test_ears.py
│   ├── test_voice.py
│   ├── test_eyes.py
│   └── test_body.py
├── models/                 # downloaded STT/TTS model files (git-ignored)
├── scripts/                # helper scripts (deploy, setup, etc.)
└── systemd/                # systemd unit files for auto-start on Pi
```

**Structure rule:** `docs/` = the big picture. `src/tinker/*.md` = the details, right next to the code.

## Hardware Components

- **Computer:** Raspberry Pi 4 or 5
- **Camera:** Pi Camera Module (CSI) or USB webcam
- **Microphone:** USB mic or USB audio adapter + mic
- **Speaker:** 3.5mm or USB speaker
- **Servos:** SG90 (light duty) / MG996R (heavy duty) — for head/arm movement
- **DC Motors + Driver:** L298N or DRV8833 H-bridge — for wheel drive
- **Chassis:** custom or kit-based wheeled platform
- **Power:** 5V for Pi, separate battery pack for motors

## Coding Conventions

- **Style:** PEP 8 enforced via `ruff`
- **Type hints:** required on all function signatures
- **Docstrings:** Google style, on public functions and classes
- **Logging:** use `logging` module, never `print()` for runtime output
- **Concurrency:** `asyncio` — no threads, no multiprocessing unless unavoidable
- **State:** no global mutable state; pass dependencies explicitly
- **Degradation:** graceful — if a subsystem (camera, mic) is unavailable, log a warning and continue without it
- **Testing:** `pytest` with `pytest-asyncio` for async tests

## SDD/DDD/TDD Workflow

> "If you don't know what you want, neither does AI."
> "Without docs, the AI has to read thousands of lines of code to guess what's going on — and it will guess wrong."
> "Code first, doc later" is **WRONG**. Document first, always.

### The Three Pillars

**SDD — Spec/Standard-Driven Development**
- `CLAUDE.md` and `.claude/rules/` define coding standards, documentation standards, and testing standards
- AI reads the spec → reads the feature request → makes a plan → executes following the spec

**DDD — Document-Driven Development**
- `docs/` = architecture-level big picture (project overview, module relationships, design decisions)
- `src/tinker/*.md` = file-level detail (interfaces, helpers, data structures) — companion `.md` next to each `.py`
- Each companion `.md` defines: purpose, public API, input/output contracts, dependencies, error handling, test plan
- Human writes docs initially; AI updates them as it adds new functions/APIs

**TDD — Test-Driven Development**
- Write failing tests based on the documented interface BEFORE implementation
- Tests cover: happy path, edge cases, error conditions, mock mode behavior
- Implement minimum code to make tests pass

### Workflow Order for Every Feature

```
1. Document first  → write/update the companion .md spec
2. Test second     → write tests based on the spec (they fail)
3. Code last       → implement until tests pass
4. Update docs     → AI updates the .md if new APIs were added
```

**Division of labor:**
- Human writes specs, standards, and docs (the *what* and *how*)
- AI writes tests and code (the *implementation*)
- Module docs are hybrid — human writes initially, AI updates as it adds functions

### Plan Mode

For every non-trivial feature:

1. **Plan** — AI reads spec + feature request → produces a plan answering:
   - What files to read?
   - What files to change?
   - How to change from current state to desired state?
2. **Review** — Human checks:
   - Does it follow DDD → TDD → Code order?
   - Are all files covered?
   - Take your time. Ask questions. Request changes.
3. **Execute (Ralph Loop)** — Once approved:
   - Feed the plan as prompt
   - AI executes one iteration
   - Check success criteria
   - If not done: feed plan + previous output back
   - Repeat until done
   - The plan stays constant, context accumulates
4. **If it fails** — Don't re-run blindly. Look at *why* it failed and update the plan/docs/tests before trying again.

## Development Workflow

### Local development (laptop)
Run with `--mock` flag to simulate hardware:
```bash
python -m tinker.main --mock
```
Mock mode stubs out GPIO, camera, and audio so you can develop and test LLM/TTS/STT logic on a laptop.

### Deploy to Pi
```bash
rsync -avz --exclude '.venv' --exclude 'models/' . pi@tinker.local:~/tinker/
```

### Run on Pi
```bash
ssh pi@tinker.local
cd ~/tinker
python -m tinker.main
```

### Auto-start via systemd
```bash
sudo cp systemd/tinker.service /etc/systemd/system/
sudo systemctl enable --now tinker
```

## Important Commands

```bash
# Lint
ruff check src/ tests/
ruff format src/ tests/

# Test
pytest tests/ -v

# Run a single module for testing
python -m tinker.brain     # test LLM conversation
python -m tinker.ears      # test mic + STT
python -m tinker.voice     # test TTS
python -m tinker.eyes      # test camera + vision
python -m tinker.body      # test motor/servo movement

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

## Key Design Decisions

| Decision | Chosen | Over | Why |
|---|---|---|---|
| STT engine | `faster-whisper` | `openai-whisper` | 4x faster, 4x less memory via CTranslate2 — critical for Pi |
| TTS engine | `piper-tts` | `pyttsx3` | Natural-sounding neural voices, optimized for ARM, still fast on Pi |
| GPIO library | `gpiozero` | `RPi.GPIO` | Higher-level API, built-in device classes, supports mock pins for testing |
| Module naming | Body-part metaphor | Generic names | Intuitive mapping (brain=LLM, ears=STT, voice=TTS, eyes=vision, body=motors) |
| Concurrency | `asyncio` | `threading` | Single event loop avoids race conditions; all I/O is async-friendly |
| Dev workflow | `--mock` flag | Pi-only dev | Enables fast iteration on a laptop without hardware |
