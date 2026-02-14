hello world!

## Project Structure

```
tinker/
├── .claude/
│   ├── CLAUDE.md                    # project context, enforces DDD & TDD
│   ├── rules/
│   │   ├── code-style.md            # coding standards (SDD)
│   │   ├── testing.md               # testing standards (SDD)
│   │   └── documentation.md         # documentation standards (SDD)
│   ├── commands/
│   │   └── implement.md             # step-by-step implement workflow
│   └── skills/
│       └── deploy/
│           └── SKILL.md             # multi-step deploy to Pi
├── README.md
├── pyproject.toml
├── .env                             # local env vars (git-ignored)
├── docs/
│   └── architecture.md              # system design, module map, data flow
├── src/
│   └── tinker/
│       ├── __init__.py
│       ├── main.py                  # async entry point
│       ├── main.md                  # spec: orchestration
│       ├── brain/
│       │   ├── brain.md             # spec: conversation API, prompt contracts
│       │   ├── test_brain.py        # ← TDD: written FIRST
│       │   └── brain.py             # ← Code: written SECOND
│       ├── ears/
│       │   ├── ears.md              # spec: audio capture, whisper integration
│       │   ├── test_ears.py
│       │   └── ears.py
│       ├── voice/
│       │   ├── voice.md             # spec: synthesis pipeline, piper integration
│       │   ├── test_voice.py
│       │   └── voice.py
│       ├── eyes/
│       │   ├── eyes.md              # spec: camera capture, face detection
│       │   ├── test_eyes.py
│       │   └── eyes.py
│       ├── body/
│       │   ├── body.md              # spec: movement commands, GPIO mapping
│       │   ├── test_body.py
│       │   └── body.py
│       └── config/
│           ├── config.md            # spec: env vars, settings schema
│           ├── test_config.py
│           └── config.py
├── models/                          # downloaded STT/TTS model files (git-ignored)
├── scripts/                         # helper scripts (deploy, setup, etc.)
└── systemd/                         # systemd unit files for auto-start on Pi
```

## Development Workflow

This project follows an SDD/DDD/TDD workflow. See the full methodology notes here:

- [AI Agent Development — RemNote](https://remnote.com/a/AI-Agent-Development/6990f4b9047f55d3208ad9a9)
