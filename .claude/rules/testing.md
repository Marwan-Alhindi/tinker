# Testing Standards

- `pytest` with `pytest-asyncio` for async tests
- Tests live in `tests/test_<module>.py`, one per source module
- Write tests BEFORE implementation — tests should fail first
- Cover: happy path, edge cases, error conditions, mock mode behavior
- Write tests based on the companion `.md` spec
- All tests must pass before merging
