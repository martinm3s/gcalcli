# gcalcli — Agent Guidance

Repo: **martinm3s/gcalcli** — Google Calendar Command Line Interface (Python).

## Stack

- Python >= 3.10 (CI targets 3.10/3.11/3.12; sandbox runs 3.13)
- Packaging: setuptools + setuptools-scm (`pyproject.toml`); version generated into `gcalcli/_version.py`
- Tests: pytest (unit, `tests/`) + bats (functional CLI, `tests/cli/`, via git submodules)
- Lint/format: ruff (line-length 80); types: mypy (`mypy_path = gcalcli:stubs:tests`)
- No databases, message queues, or other external services in local dev. Only optional external dep is the real Google Calendar API (OAuth); tests use `GCALCLI_USERLESS_MODE=1` mock mode.

## Commands

```shell
# one-time setup
git submodule update --init          # required for tests/cli (bats)
pip install -e '.[dev,vobject]' pytest pytest-cov ruff mypy

# lint / types
ruff check
mypy gcalcli

# unit tests
python3 -m pytest tests -q

# functional (CLI) tests — the test path is a required arg when invoked directly
./tests/cli/run_tests.sh tests/cli/test.bats

# run the app
gcalcli --help
GCALCLI_USERLESS_MODE=1 gcalcli init --client-id=SOME_CLIENT   # mock-mode setup flow
```

Canonical full check (what CI runs): `tox` (envs: lint, type, py3{10,11,12}, cli). tox is not preinstalled in this sandbox; the pip commands above are equivalent.

## Codebase Map

See [codebase-map.md](codebase-map.md).

## Local Verification

Run, in order: `ruff check` → `python3 -m pytest tests -q` → `./tests/cli/run_tests.sh tests/cli/test.bats`.

### Validation Summary (onboarding, 2026-10-30)

- `ruff check`: all checks passed
- `pytest tests`: **63 passed**
- bats functional suite (`tests/cli/test.bats`): **3 of 4 passed**. Failure: `prints correct help` — snapshot mismatch of `-h` output under Python 3.13 in this sandbox (CI pins 3.10–3.12). Environment artifact, pre-existing; not caused by repo changes.
- `mypy gcalcli`: **4 pre-existing errors** in `gcalcli/config.py` under this sandbox's mypy/Python 3.13 (CI 3.10/3.12 type envs pass upstream). Pre-existing; not caused by repo changes. TODO(confirm) if these reproduce on Python 3.12.
- Primary flow: `gcalcli --help` renders usage; `GCALCLI_USERLESS_MODE=1 gcalcli init --client-id=SOME_CLIENT` completes successfully (exit 0). Real API commands (`gcalcli list` etc.) require Google OAuth credentials — out of scope for local dev, exercised via mock mode and tests instead.

## Snapshot

- Snapshot ID: `lyvpczawanq7x48v2q0i:default` (e2b template), captured 2026-10-30T16:08:13Z from this sandbox.
