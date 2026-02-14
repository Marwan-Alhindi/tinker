# main — Orchestration

## Purpose
Async entry point that wires all modules together and runs the main event loop.

## Public API
- `async def run(mock: bool = False) -> None` — start all subsystems and run until shutdown
- `def main() -> None` — CLI entry point, parses `--mock` flag, calls `asyncio.run(run())`

## Data Structures
- None — main owns no state, it only wires modules together

## Constraints / Edge Cases
- Must handle graceful shutdown on SIGINT/SIGTERM
- If any subsystem fails to initialize, log warning and continue without it
- `--mock` flag must propagate to all modules
