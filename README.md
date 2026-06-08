# Vulnerable Module Test Samples

This repository is a fixture set for testing services and tools that discover dependencies in source repositories and surface packages that may map to vulnerable versions.

The emphasis is on dependency detection, not on building or running complete applications. Most samples are intentionally minimal and are designed to exercise parsing of manifests, lockfiles, and vendored dependency metadata across multiple ecosystems.

As an open-source repository, the goal is to provide a transparent, reusable corpus that others can use to benchmark dependency extraction and vulnerable-package surfacing behavior across ecosystems.

## Repository Purpose

Use this repository to validate whether a scanner can:

- detect dependencies across several programming languages and package managers
- distinguish between direct manifest entries, resolved lockfile entries, and vendored or installed packages
- normalize package names and ecosystems correctly
- return the versions intentionally embedded in each fixture

Each fixture directory under `fixtures/` includes an `expected.json` file that acts as the oracle for what a scanner should surface from that sample.

Where src code is included the intention is to surface referenced modules and related versions NOT whether the code is actually functional.

## Open Source Use

This repository is intended to be:

- publicly readable and easy to inspect on GitHub
- reusable by scanner vendors, researchers, and maintainers who want a shared dependency-discovery test corpus
- open to contributions that add new ecosystems, package-manager formats, or narrowly scoped fixture cases

Public issue reports and pull requests should stay focused on fixture quality, dependency-discovery expectations, and documentation clarity.

## The Repository Is Not Intended For

This repository is not intended to validate or demonstrate:

- whether a dependency is currently the latest available release
- whether a dependency is outdated relative to the current package ecosystem
- whether a package is actually vulnerable according to a live advisory database at the time of scanning
- whether the sample projects build, install, resolve, or run successfully as real applications
- whether a scanner supports every edge case for a given ecosystem beyond the specific fixture files checked into this repository

The fixtures are intentionally static. They should be treated as dependency-discovery test cases, not as a source of truth for current package health, upgrade guidance, or runtime behavior.

## Layout

The repository is organized under `fixtures/`, first by language, then by package manager or dependency format:

```text
.
`-- fixtures/
    |-- csharp/
    |   |-- legacy/
    |   |-- lockfile/
    |   `-- msbuild/
    |-- go/
    |   `-- mod/
    |-- java/
    |   |-- gradle/
    |   `-- maven/
    |-- node/
    |   |-- bun/
    |   |-- deno/
    |   |-- npm/
    |   `-- pnpm/
    |-- php/
    |   |-- composer/
    |   `-- composer-vendor/
    |-- python/
    |   |-- conda_lock/
    |   |-- pdm/
    |   |-- pip/
    |   |-- poetry/
    |   `-- uv/
    |-- ruby/
    |   |-- bundle-vendor/
    |   `-- bundler/
    |-- rust/
    |   |-- cargo/
    |   `-- cargo-vendor/
    `-- zig/
        `-- zig/
```

## Fixture Contract

Every sample directory is expected to contain:

- one or more dependency-related input files such as a manifest, lockfile, or vendored package metadata
- an `expected.json` file describing the packages that should be detected from that fixture

The `expected.json` entries use this shape:

```json
[
  {
    "name": "package-name",
    "version": "1.2.3",
    "eco": "ecosystem-name",
    "type": "manifest|locked|installed"
  }
]
```

Field meaning:

- `name`: package identifier as it should be reported by the scanner
- `version`: version string expected from the fixture
- `eco`: ecosystem label, for example `PyPI`, `Maven`, `NuGet`, or `crates.io`
- `type`: where the package is expected to be discovered

Discovery types currently used in this repository:

- `manifest`: declared directly in a manifest or project file
- `locked`: resolved in a lockfile or equivalent pinned dependency file
- `installed`: found in vendored or installed dependency metadata checked into the repository

Some fixtures intentionally include more than one discovery type so scanners can be tested against both declared and installed dependency sources.

## Coverage

The current fixture coverage under `fixtures/` is:

| Language | Fixture Directories | Package Manager / Format Coverage |
| --- | --- | --- |
| C# | `legacy`, `lockfile`, `msbuild` | `packages.config`, `packages.lock.json`, SDK-style `.csproj` |
| Go | `mod` | `go.mod`, `go.sum` |
| Java | `gradle`, `maven` | `build.gradle`, `pom.xml` |
| Node.js | `bun`, `deno`, `npm`, `pnpm` | `bun.lock`, `deno.lock`, `package-lock.json`, `pnpm-lock.yaml` |
| PHP | `composer`, `composer-vendor` | `composer.json`, `composer.lock`, vendored Composer metadata |
| Python | `conda_lock`, `pdm`, `pip`, `poetry`, `uv` | `conda-lock.yml`, `pdm.lock`, `requirements.txt`, `poetry.lock`, `uv.lock` |
| Ruby | `bundle-vendor`, `bundler` | `Gemfile`, `Gemfile.lock`, vendored gem specifications |
| Rust | `cargo`, `cargo-vendor` | `Cargo.toml`, vendored Cargo package metadata |
| Zig | `zig` | `build.zig.zon` |

## How To Use This Repository

Typical uses:

1. Run a dependency extraction or vulnerability surfacing service against the repository or against individual directories under `fixtures/`.
2. Compare the tool output for each fixture with that fixture's `expected.json`.
3. Record mismatches such as missed dependencies, incorrect version resolution, wrong ecosystem mapping, or false positives.

This root README intentionally stays high-level. Language-specific behavior, edge cases, and scanner expectations are documented in `fixtures/<language>/README.md`.

## Contributing

Contributions are welcome if they improve the repository as a dependency-discovery test corpus.

Before opening a pull request:

- read [CONTRIBUTING.md](CONTRIBUTING.md)
- keep fixtures minimal and deterministic
- include or update the relevant `expected.json`
- update the appropriate language README when adding or changing fixture behavior
- update this root README if you add a new language under `fixtures/` or change the repository-wide fixture contract

## Security

If you find a problem in the repository itself, review [SECURITY.md](SECURITY.md) before opening a public report.

Please do not use this repository to report upstream vulnerabilities in third-party packages included as fixture data. Those belong with the relevant package maintainers or advisory databases.

For guidance on safe scanning posture and privacy-preserving usage, see [USAGE_BEST_PRACTISE.md](USAGE_BEST_PRACTISE.md).

## Community

- Contributor expectations are documented in [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).
- General support and project-usage guidance are documented in [SUPPORT.md](SUPPORT.md).
- Intended readers and non-target audiences are documented in [INTENDED AUDIENCE.md](<INTENDED AUDIENCE.md>).

## License

This repository is licensed under the MIT License. See [LICENSE.md](LICENSE.md).

## Notes

- These fixtures are intentionally synthetic and are not intended to represent production-ready applications.
- Vendored dependency samples are included to test scanners that inspect checked-in installed package metadata rather than only manifests and lockfiles.
- The repository does not currently depend on a single global test harness; `expected.json` is the source of truth per fixture.
