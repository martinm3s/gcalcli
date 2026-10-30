# gcalcli Codebase Map

Depth-2 overview. `gcalcli/` and `tests/` are the two code packages; everything else is metadata/docs.

| Path | Purpose |
|---|---|
| `gcalcli/` | Main Python package (the `gcalcli` console script: `cli:main`) |
| `gcalcli/cli.py` | Entry point and command dispatch |
| `gcalcli/gcal.py` | Google Calendar API access and core operations |
| `gcalcli/auth.py` | OAuth2/auth flows |
| `gcalcli/argparsers.py` | Argument parsing and command definition |
| `gcalcli/actions.py` | Command action implementations |
| `gcalcli/config.py` | Config loading (config.toml, schema-validated) |
| `gcalcli/validators.py`, `details.py`, `utils.py`, `printer.py`, `env.py`, `exceptions.py`, `deprecations.py`, `conflicts.py`, `ics.py`, `_types.py` | Supporting modules (parsing, output, validation) |
| `tests/` | pytest unit tests (`test_*.py`), shared fixtures (`conftest.py`, `_utils.py`), test data |
| `tests/cli/` | Bats functional tests; `run_tests.sh` + `test.bats`; `bats*/` and `test_helper/` are git submodules |
| `stubs/` | Local type stubs for third-party libs (mypy path) |
| `docs/` | Manual page (`man1/`), auth docs, screenshots |
| `data/` | `config-schema.json` (config.toml JSON Schema) |
| `.github/workflows/` | CI (tests, lint) |
| `pyproject.toml` | Package metadata, deps, ruff and mypy config |
| `tox.ini` | Canonical test runner config (lint, type, py310-312, cli envs) |
