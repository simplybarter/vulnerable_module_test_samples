# Zig Fixtures

This directory contains Zig package metadata fixtures based on `build.zig.zon`.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `zig/` | `build.zig.zon` | dependency extraction from Zig package dependency declarations |

## Expected Detection Behavior

The `zig/` fixture uses `type: "manifest"` and expects:

- `zap` `0.8.0`
- `hzzp` `0.1.2`

The expected ecosystem label is `Zig`.

## Notes

- One dependency expresses its version through the tagged archive URL, while the other includes an explicit `.version` field.
- This fixture is useful for testing whether scanners can normalize Zig dependency declarations that combine URL-based source definitions with version metadata.
- There is currently no lockfile or vendored Zig sample in this directory.
