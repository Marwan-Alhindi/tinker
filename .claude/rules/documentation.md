# Documentation Standards

- Every `.py` source file has a companion `.md` spec right beside it
- Companion `.md` defines: purpose, public API, input/output contracts, dependencies, error handling, test plan
- `docs/` holds architecture-level docs (project overview, module relationships, design decisions)
- `src/tinker/*.md` holds file-level detail (interfaces, helpers, data structures)
- Google-style docstrings on public functions and classes
- Always update the companion `.md` when adding or changing public APIs
- Write docs BEFORE writing any code — no exceptions
