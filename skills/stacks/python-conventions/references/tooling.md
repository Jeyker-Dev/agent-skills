# Python Tooling

Tools enforce what can be measured. Run them and obey their output.

| Tool | Enforces |
|---|---|
| uv | Python version, environment, locked dependencies |
| ruff | Style, imports, and several `clean-code` rules (see below) |
| mypy (strict) | Types are real, so "trust the types" is safe |
| pytest | Tests |

## Commands

```bash
uv init --package <name>        # new project with src layout
uv python pin 3.12
uv add <package>
uv add --dev pytest mypy ruff pydantic
uv sync                         # install from uv.lock
uv run ruff format --check .
uv run ruff check .
uv run mypy
uv run pytest
```

## pyproject.toml

```toml
[tool.ruff]
line-length = 100

[tool.ruff.lint]
select = [
  "E", "F", "I", "N", "UP", "B", "SIM",
  "C90",      # cyclomatic complexity
  "FBT",      # boolean trap: no flag parameters
  "PLR2004",  # magic value comparison
  "RET505",   # no else after return
  "PLR0913",  # too many arguments
  "PLR0915",  # too many statements
]

[tool.ruff.lint.mccabe]
max-complexity = 10

[tool.ruff.lint.per-file-ignores]
"tests/**" = ["PLR2004", "FBT"]

[tool.mypy]
strict = true
plugins = ["pydantic.mypy"]
files = ["src", "tests"]

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["*_test.py"]
```

## Migrating from Poetry

1. Move `[tool.poetry.dependencies]` into `[project] dependencies` with
   standard version specifiers (`^4.5` becomes `>=4.5,<5`).
2. Move dev-only packages to `uv add --dev`.
3. Replace the build backend if the project is a package (`uv init --package`
   generates one), delete `poetry.lock` and `poetry.toml`, then run `uv lock`.
4. Run the full test suite before deleting anything else.
