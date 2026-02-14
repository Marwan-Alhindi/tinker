# Coding Standards

- PEP 8 enforced via `ruff`
- Type hints required on all function signatures
- No global mutable state; pass dependencies explicitly
- `asyncio` for concurrency — no threads, no multiprocessing unless unavoidable
- Graceful degradation — if a subsystem (camera, mic) is unavailable, log a warning and continue without it
- Use `logging` module, never `print()` for runtime output
- Modules use a body-part metaphor: brain, ears, voice, eyes, body
