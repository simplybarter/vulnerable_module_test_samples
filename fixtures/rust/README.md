# Rust Fixtures

This directory contains Cargo-based fixtures for manifest parsing and vendored crate metadata detection.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `cargo/` | `Cargo.toml` | dependency extraction across multiple Cargo dependency sections |
| `cargo-lock/` | `Cargo.toml`, `Cargo.lock` | comparison of broad manifest declarations against exact resolved crate versions in Cargo lock state |
| `cargo-vendor/` | `Cargo.toml`, vendored crate `Cargo.toml` files | comparison of manifest constraints against vendored installed crate versions |

## Expected Detection Behavior

- `cargo/` uses `type: "manifest"` and expects:
  - `serde` `1.0.197`
  - `tokio` `1.36.0`
  - `anyhow` `1.0.81`
  - `criterion` `0.5.1`
  - `cc` `1.0.90`
  - `openssl` `0.10.64`
- `cargo-lock/` mixes types:
  - `manifest`: `idna` `0.5`
  - `locked`: `idna` `0.5.0`
  - `locked`: `tinyvec` `1.6.0`
  - `locked`: `tinyvec_macros` `0.1.0`
  - `locked`: `unicode-bidi` `0.3.13`
  - `locked`: `unicode-normalization` `0.1.22`
- `cargo-vendor/` mixes types:
  - `manifest`: `serde` `1.0`
  - `manifest`: `serde_json` `1.0`
  - `installed`: `serde` `1.0.197`
  - `installed`: `serde_json` `1.0.114`

The expected ecosystem label is `crates.io`.

## Notes

- `cargo/` is useful for validating parsing across `[workspace.dependencies]`, `[dependencies]`, `[dev-dependencies]`, `[build-dependencies]`, and target-specific dependency sections.
- `cargo-lock/` uses a broad dependency requirement in `Cargo.toml` and a checked-in `Cargo.lock` to exercise the distinction between declared crate intent and the exact versions Cargo resolved.
- `cargo-lock/` intentionally centers on `idna` `0.5.0`, which has a published RustSec advisory, and includes its transitive Unicode-related lockfile dependencies so scanners can verify recursive `Cargo.lock` extraction.
- `cargo-vendor/` is intended to verify that scanners can inspect vendored crate manifests under `vendor/` in addition to the top-level `Cargo.toml`.
- The manifest expectations in `cargo-vendor/` intentionally preserve the broad declared versions while installed expectations use the precise vendored crate versions.
