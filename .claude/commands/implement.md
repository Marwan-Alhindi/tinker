# Implement Command

Step-by-step workflow for implementing a feature:

1. Read `docs/architecture.md` to understand the big picture
2. Read the module's `.md` spec (`src/tinker/<module>/<module>.md`)
3. Update or write the `.md` spec if the feature changes the API
4. Write failing tests in `src/tinker/<module>/test_<module>.py`
5. Write code to pass tests in `src/tinker/<module>/<module>.py`
6. Run `pytest src/tinker/<module>/test_<module>.py -v`
7. Run `ruff check src/ && ruff format src/`
8. Update the `.md` spec if new APIs were added
