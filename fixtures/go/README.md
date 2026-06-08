# Go Fixtures

This directory contains Go module fixtures centered on `go.mod` and `go.sum` resolution.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `mod/` | `go.mod`, `go.sum` | module discovery from direct and indirect dependencies recorded in Go module files |

## Expected Detection Behavior

The `mod/` fixture uses `type: "locked"` in `expected.json` and expects scanners to surface:

- `github.com/bytedance/sonic` `1.15.0`
- `github.com/gin-contrib/sse` `1.1.0`
- `gopkg.in/yaml.v3` `3.0.1`
- `github.com/bytedance/gopkg` `0.1.3`
- `golang.org/x/net` `0.7.0`

The expected ecosystem label is `Go`.

## Notes

- The sample includes both direct and indirect requirements.
- `go.sum` is intentionally present so scanners can validate pinned module versions instead of only declared requirements.
- This directory currently has one fixture focused on module-based dependency resolution rather than vendored packages.
