# Intended Audience

This repository is designed for people evaluating or building systems that detect dependencies in source repositories and surface package-version information that may later be matched against vulnerability data.

It is primarily a test corpus, not a tutorial repository or a general programming-learning resource.

## Who This Repository Is For

The most relevant audience includes:

- security professionals evaluating software composition analysis, dependency discovery, or vulnerable-package surfacing behavior
- application security engineers testing how scanners interpret manifests, lockfiles, and vendored dependency metadata
- product engineers building repository scanners, dependency parsers, or package intelligence systems
- seasoned developers validating how tooling behaves across multiple ecosystems and package managers
- researchers comparing dependency extraction accuracy, normalization, and coverage across tools
- QA and test engineers maintaining fixture-based regression suites for dependency-analysis products

## Expected Background

Readers will get the most value from this repository if they are already comfortable with:

- software supply chain or dependency-analysis concepts
- package manifests and lockfiles
- reading repository structures without needing full runnable applications
- interpreting package names, version constraints, and transitive dependency data
- working with isolated environments when evaluating third-party tools

## Typical Use Cases

This repository is well suited for:

- benchmarking whether a scanner finds the expected packages in each fixture
- validating whether a tool distinguishes `manifest`, `locked`, and `installed` dependency sources correctly
- regression testing parser behavior across ecosystems such as Node.js, Python, Java, Go, Ruby, Rust, PHP, C#, and Zig
- reviewing how a tool handles intentionally minimal or synthetic repository structures
- comparing scanner output to the `expected.json` contract in each fixture directory

## Who This Repository Is Not For

This repository is not intended for:

- beginners learning a programming language through complete example applications
- users looking for production-ready starter projects
- people seeking current upgrade advice for real dependencies
- users expecting a live vulnerability feed or authoritative advisory database
- people who need a hands-off introduction to package managers without prior context
- anyone expecting every sample to build, install, or run as a full application

## Practical Interpretation

If you are evaluating a scanner, building one, testing one, or reviewing dependency-discovery behavior as part of a security or engineering workflow, this repository is likely relevant.

If you are trying to learn programming fundamentals, application architecture, or general dependency management from first principles, this repository is probably the wrong starting point.
