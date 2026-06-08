# Python Fixtures

This directory contains Python dependency fixtures across manifest, lockfile, and installed metadata formats.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `dist-info/` | `requirements.txt`, `site-packages/*.dist-info/METADATA` | checked-in installed package metadata parsing from Python `.dist-info` directories |
| `pip/` | `requirements.txt` | plain requirements parsing with exact pins, range constraints, comments, and inline comments |
| `pipenv/` | `Pipfile`, `Pipfile.lock` | Pipenv manifest and lockfile parsing across `default` and `develop` dependency sections |
| `pyproject/` | `pyproject.toml` | PEP 621 dependency extraction from the `[project].dependencies` array |
| `poetry/` | `poetry.lock` | Poetry lockfile parsing |
| `pdm/` | `pdm.lock` | PDM lockfile parsing |
| `uv/` | `uv.lock` | uv lockfile parsing |
| `conda_lock/` | `conda-lock.yml` | Conda lockfile parsing for package entries recorded in YAML |

## Expected Detection Behavior

- `dist-info/` mixes types and expects:
  - `manifest`: `requests` `2.31.0`
  - `installed`: `requests` `2.31.0`
  - `installed`: `urllib3` `1.26.16`
- `pip/` uses `type: "manifest"` and expects:
  - `requests` `2.31.0`
  - `flask` `3.0.0`
  - `django` `5.0.0`
- `pipenv/` mixes types and expects:
  - `manifest`: `urllib3` `2.0.5`
  - `manifest`: `pytest` `7.4.0`
  - `locked`: `urllib3` `2.0.5`
  - `locked`: `iniconfig` `2.0.0`
  - `locked`: `packaging` `23.2`
  - `locked`: `pluggy` `1.3.0`
  - `locked`: `pytest` `7.4.0`
- `pyproject/` uses `type: "manifest"` and expects `urllib3` `1.26.16` and `jinja2` `2.11.2`
- `poetry/` uses `type: "locked"` and expects `requests` `2.31.0` and `urllib3` `2.1.0`
- `pdm/` uses `type: "locked"` and expects `pdm-test` `2.0.1` and `requests` `2.31.0`
- `uv/` uses `type: "locked"` and expects `anyio` `4.3.0` and `idna` `3.6`
- `conda_lock/` uses `type: "locked"` and expects `pandas` `2.1.1`, `numpy` `1.26.0`, `requests` `2.31.0`, and `django` `5.0.0`

The expected ecosystem label is `PyPI` across all current fixtures.

## Notes

- `dist-info/` uses the standardized `.dist-info` directory structure under a checked-in `site-packages/` tree so scanners can validate installed-package discovery from Python installation metadata.
- `dist-info/` intentionally keeps `requests` `2.31.0` as both a declared and installed package while adding installed-only `urllib3` `1.26.16`, which is a historically vulnerable transitive package version.
- `pip/` is intentionally useful for scanners that need to ignore comments while still extracting normalized package names and versions from mixed requirement syntax.
- `pipenv/` uses an exact-pinned `Pipfile` and a checked-in `Pipfile.lock`, which exercises both top-level declarations and fully resolved locked versions for the same project.
- `pipenv/` intentionally places `urllib3` `2.0.5` in the runtime dependency set and `pytest` `7.4.0` in the development dependency set so scanners must inspect both `default` and `develop` sections of `Pipfile.lock`.
- `pyproject/` covers the standardized PEP 621 `[project]` table without any tool-specific lockfile, which makes it useful for scanners that claim generic `pyproject.toml` support outside Poetry, PDM, or uv.
- `pyproject/` intentionally includes the historically vulnerable `urllib3` `1.26.16` and `jinja2` `2.11.2` as exact direct dependencies so scanners can normalize package names from TOML string requirement entries.
- `poetry/`, `pdm/`, and `uv/` provide three distinct lockfile syntaxes that should all resolve to PyPI package/version pairs.
- `conda_lock/` currently expects packages to be reported with the `PyPI` ecosystem label even though the source file is a Conda lockfile; this is part of the current fixture contract and should be preserved unless the corpus is intentionally redefined.
