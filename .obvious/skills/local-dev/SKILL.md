---
name: local-dev
---

# local-dev (gcalcli)

Durable record of LOCAL-DEV onboarding (2026-10-30), from a fresh checkout of martinm3s/gcalcli.

## What this repo needs to run locally

1. `git submodule update --init` — bats + helpers live in git submodules under `tests/cli/`; functional tests silently misbehave without them.
2. `pip install -e '.[dev,vobject]' pytest pytest-cov ruff mypy` — editable install with dev/type-stub extras (the `[dev]` extra alone lacks pytest/ruff/mypy).
3. No services to start. No `.env` required. The only external system is the Google Calendar API, which tests never touch: they run in `GCALCLI_USERLESS_MODE=1` mock mode.

## Gotchas learned during onboarding

- **Python 3.13 vs CI**: sandbox has 3.13.14 but CI/upstream targets 3.10–3.12. On 3.13: `mypy gcalcli` shows 4 pre-existing errors in `gcalcli/config.py`, and the bats test `prints correct help` fails its snapshot (help-text rendering differs). Both are pre-existing environment artifacts — verify against Python 3.10–3.12 before treating as regressions.
- **`./tests/cli/run_tests.sh` requires an explicit test path** when run directly (`tests/cli/test.bats`); bare invocation prints bats usage and exits 1. tox supplies the default posargs.
- Version is dynamic (setuptools-scm); `gcalcli/_version.py` is generated at install time and gitignored.
- Real API commands (`gcalcli list`, etc.) need OAuth and fail locally by design; smoke-test with `gcalcli --help` and `GCALCLI_USERLESS_MODE=1 gcalcli init --client-id=SOME_CLIENT` (stdin: secret + newline), which exits 0.

## Verification commands

```shell
ruff check
python3 -m pytest tests -q
./tests/cli/run_tests.sh tests/cli/test.bats
mypy gcalcli   # note the 3.13 caveat above
```
