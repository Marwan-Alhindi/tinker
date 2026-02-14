# Code Style

## Naming Conventions
- snake_case for functions, variables, modules
- PascalCase for classes
- UPPER_SNAKE_CASE for constants
- Body-part metaphor for module names: brain, ears, voice, eyes, body

## Patterns to Use
- `asyncio` for all concurrency
- Dependency injection — pass dependencies explicitly, no global mutable state
- Graceful degradation — log warning and continue if a subsystem is unavailable
- `logging` module for all runtime output

## Patterns to Avoid
- No `print()` for runtime output
- No threads or multiprocessing unless unavoidable
- No bare `except:` — always catch specific exceptions
- No mutable default arguments

## Language-Specific
- Python 3.11+
- PEP 8 enforced via `ruff`
- Type hints required on all function signatures
- Google-style docstrings on public functions and classes
