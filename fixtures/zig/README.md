# Zig Fixtures

This directory contains Zig package metadata fixtures based on `build.zig.zon`.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `zig/` | `build.zig.zon` | dependency extraction from Zig package dependency declarations |
| `zig-pkg/` | `build.zig.zon`, fetched package `build.zig.zon` files under `zig-pkg/` | comparison of declared Zig package dependencies against checked-in fetched package metadata |

## Expected Detection Behavior

The `zig/` fixture uses `type: "manifest"` and expects:

- `zap` `0.8.0`
- `hzzp` `0.1.2`

The `zig-pkg/` fixture mixes types and expects:

- `manifest`: `zap` `0.8.0`
- `manifest`: `hzzp` `0.1.2`
- `installed`: `zap` `0.8.0`
- `installed`: `hzzp` `0.1.2`

The expected ecosystem label is `Zig`.

## Notes

- One dependency expresses its version through the tagged archive URL, while the other includes an explicit `.version` field.
- This fixture is useful for testing whether scanners can normalize Zig dependency declarations that combine URL-based source definitions with version metadata.
- `zig-pkg/` models checked-in fetched package directories using the project-local `zig-pkg/` layout now used by Zig, with package identity coming from nested `build.zig.zon` files.
- The `zig-pkg/` fixture intentionally mirrors the same dependency names and versions in both the root manifest and fetched package metadata so scanners can distinguish declaration evidence from installed-package evidence without requiring a separate Zig lockfile concept.
