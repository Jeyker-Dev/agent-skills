---
name: python-conventions
description: >
  Jeyker's Python project conventions: uv for Python versions, environments and
  dependencies; src layout organized by domain; strict mypy; Pydantic at the
  boundary and frozen dataclasses inside; ruff and pytest. Use when creating,
  structuring, or changing a Python project, adding dependencies, or writing
  Python modules and tests.
metadata:
  author: Jeyker-Dev
  version: "1.0"
---

# Python Conventions

Applies `clean-code`, `testing-strategy`, and `error-handling` to Python.
Tool configuration is in [references/tooling.md](references/tooling.md).

## Project and dependencies

- uv manages everything: Python version (`uv python pin`), virtual
  environment, dependencies, and `uv.lock`. Never call `pip`, `poetry`, or
  `python -m venv` directly.
- `pyproject.toml` uses the standard `[project]` table.
- Runtime dependencies with `uv add`; test and tooling dependencies with
  `uv add --dev` (pytest, mypy, ruff, ipython never go in runtime deps).
- Run everything through `uv run` (`uv run pytest`, `uv run python -m ...`).
- Commit `uv.lock`.

## Layout

```
pyproject.toml
src/<package>/
├── <domain>/          One folder per domain area (risk/, execution/)
├── config.py          Settings loaded once at startup
└── __init__.py
scripts/               Thin entry points that wire objects and call the domain
config/                Non-secret config per environment (default.yml, production.yml)
tests/unit/            Mirrors src/<package>/
tests/integration/     Tests that touch real I/O
.env.example           Every secret the app reads, with empty values
```

- Folders are named after the domain, never after technical roles
  (`utils/`, `helpers/`, `models/`).
- Scripts only parse arguments, build objects, and call the domain. No
  business logic in `scripts/`.

## Types

- Every function and method is fully annotated; `mypy --strict` must pass.
- `Any` is not allowed in domain code. Use `TypedDict` or a model at the
  boundary instead.
- No bare `dict` or `list` crossing a function boundary when it has a known
  shape; make it a model.

## Boundary and domain data

- At the boundary (environment, config files, exchange or HTTP responses,
  CLI arguments), parse into Pydantic models. Secrets and environment come
  from `pydantic-settings`.
- Inside the domain, use `@dataclass(frozen=True, slots=True)` for value
  objects and results. Domain code never receives a raw `dict`.
- States and fixed choices are `StrEnum` or `Enum`; fixed numbers are
  module-level `Final` constants.

## Code

- Inject collaborators through `__init__`; never build a client or service
  inside a function that uses it.
- Prefer comprehensions and built-ins (`sum`, `any`, `max`) over loops that
  accumulate. A loop is fine when it exists for side effects.
- Business rule errors are subclasses of one project base exception
  (`class BusinessRuleError(Exception)`), one per rule.
- Use `logging` with a module logger (`logger = logging.getLogger(__name__)`);
  never `print` outside `scripts/`.

## Tests

- pytest, plain functions and fixtures; no `unittest.TestCase` classes.
- Test files are named `<module>_test.py` and mirror the source path.
- Fake external services (exchange, HTTP, clock) with fixtures or small fake
  classes; do not mock your own domain classes.
