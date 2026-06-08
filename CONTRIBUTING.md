# Contributing

Thank you for contributing to this repository.

The project exists to provide a clean, reviewable set of dependency-discovery fixtures for tools that inspect repositories and surface package versions. Contributions should improve fixture coverage, clarity, or correctness without turning the repository into a collection of full applications.

## What To Contribute

Good contributions include:

- new fixture directories for unsupported ecosystems or package managers
- additional edge-case fixtures for existing ecosystems
- corrections to `expected.json` data
- documentation improvements that clarify fixture intent or scanner expectations

Changes that are out of scope:

- large sample applications that add noise beyond the dependency files being tested
- unrelated refactors that do not improve fixture behavior or documentation
- generated artifacts that are not necessary for dependency detection
- exploit code, proof-of-concept attack code, or runtime payloads

## Fixture Guidelines

When adding or editing fixtures:

- keep the fixture as small and deterministic as possible
- include only the files needed to exercise the dependency-discovery behavior under test
- prefer synthetic samples over real application code
- preserve intentionally unusual cases if they are part of the test contract
- add or update `expected.json` in the relevant `fixtures/<language>/<fixture>/` directory

Each `expected.json` entry should continue to use:

- `name`: package identifier to surface
- `version`: expected version string
- `eco`: ecosystem label used by this repository
- `type`: one of `manifest`, `locked`, or `installed`

## Documentation Expectations

Update documentation when behavior changes:

- update `fixtures/<language>/README.md` when you add or modify fixtures for that language
- update the root [README.md](README.md) if you add a new language under `fixtures/` or change the repository-wide fixture contract
- explain non-obvious fixture choices in the README instead of leaving them implicit

## Pull Request Expectations

Before opening a pull request:

- verify that the fixture files and `expected.json` agree
- make sure file names and directory names are consistent with the `fixtures/<language>/<fixture>/` layout
- keep vendored metadata limited to the minimum needed for the test case
- describe the dependency-discovery behavior the change is intended to exercise

A useful pull request description usually includes:

- the ecosystem and package manager involved
- the files that scanners are expected to inspect
- the expected dependency entries and discovery types
- any intentional edge cases or limitations

## Issues

Use GitHub issues for:

- incorrect expected dependency data
- missing fixture coverage for a supported ecosystem
- unclear repository documentation
- scanner-behavior mismatches that reveal a gap in fixture design

If the issue is actually a security concern in this repository, follow [SECURITY.md](SECURITY.md) instead of opening a public issue first.
