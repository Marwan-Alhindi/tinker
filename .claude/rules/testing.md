# Testing Standards

## Test Framework
- `pytest` with `pytest-asyncio` for async tests

## What to Test
- Happy path for every public function
- Edge cases (empty input, None, boundary values)
- Error conditions (missing hardware, bad config, API failures)
- Mock mode behavior (all modules must work with `--mock`)

## Test File Naming
- `test_<module>.py` inside each module directory
- e.g., `src/tinker/brain/test_brain.py`

## TDD Workflow
- Write tests BEFORE code
- Tests should fail first (red)
- Write minimum implementation to pass (green)
- All tests must pass before merging
