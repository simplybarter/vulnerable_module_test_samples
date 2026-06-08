# Python Fixtures

This directory contains Python dependency fixtures across several package managers and lockfile formats.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `pip/` | `requirements.txt` | plain requirements parsing with exact pins, range constraints, comments, and inline comments |
| `poetry/` | `poetry.lock` | Poetry lockfile parsing |
| `pdm/` | `pdm.lock` | PDM lockfile parsing |
| `uv/` | `uv.lock` | uv lockfile parsing |
| `conda_lock/` | `conda-lock.yml` | Conda lockfile parsing for package entries recorded in YAML |

## Expected Detection Behavior

- `pip/` uses `type: "manifest"` and expects:
  - `requests` `2.31.0`
  - `flask` `3.0.0`
  - `django` `5.0.0`
- `poetry/` uses `type: "locked"` and expects `requests` `2.31.0` and `urllib3` `2.1.0`
- `pdm/` uses `type: "locked"` and expects `pdm-test` `2.0.1` and `requests` `2.31.0`
- `uv/` uses `type: "locked"` and expects `anyio` `4.3.0` and `idna` `3.6`
- `conda_lock/` uses `type: "locked"` and expects `pandas` `2.1.1`, `numpy` `1.26.0`, `requests` `2.31.0`, and `django` `5.0.0`

The expected ecosystem label is `PyPI` across all current fixtures.

## Notes

- `pip/` is intentionally useful for scanners that need to ignore comments while still extracting normalized package names and versions from mixed requirement syntax.
- `poetry/`, `pdm/`, and `uv/` provide three distinct lockfile syntaxes that should all resolve to PyPI package/version pairs.
- `conda_lock/` currently expects packages to be reported with the `PyPI` ecosystem label even though the source file is a Conda lockfile; this is part of the current fixture contract and should be preserved unless the corpus is intentionally redefined.
