# Go Fixtures

This directory contains Go module fixtures centered on `go.mod`, `go.sum`, workspace metadata, and vendored module resolution.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `mod/` | `go.mod`, `go.sum` | module discovery from direct and indirect dependencies recorded in Go module files |
| `workspace/` | `go.work`, workspace module `go.mod` and `go.sum` files | multi-module workspace discovery across more than one main module |
| `vendor/` | `go.mod`, `vendor/modules.txt`, vendored package paths | vendored module discovery from checked-in module metadata |

## Expected Detection Behavior

The `mod/` fixture uses `type: "locked"` in `expected.json` and expects scanners to surface:

- `github.com/bytedance/sonic` `1.15.0`
- `github.com/gin-contrib/sse` `1.1.0`
- `gopkg.in/yaml.v3` `3.0.1`
- `github.com/bytedance/gopkg` `0.1.3`
- `golang.org/x/net` `0.7.0`

The `vendor/` fixture uses `type: "installed"` in `expected.json` and expects scanners to surface:

- `golang.org/x/net` `0.6.0`
- `golang.org/x/text` `0.8.0`

The `workspace/` fixture uses `type: "locked"` in `expected.json` and expects scanners to surface:

- `golang.org/x/net` `0.6.0`
- `golang.org/x/text` `0.8.0`
- `github.com/google/uuid` `1.3.0`

The expected ecosystem label is `Go`.

## Notes

- The sample includes both direct and indirect requirements.
- `go.sum` is intentionally present so scanners can validate pinned module versions instead of only declared requirements.
- `workspace/` uses a checked-in `go.work` file with a `use` block to define multiple main modules in one repository, which scanners should treat as a single workspace context.
- `workspace/` intentionally spreads dependencies across two module roots so scanners can prove they aggregate locked modules across the workspace rather than inspecting only the first `go.mod` file they encounter.
- `workspace/` intentionally includes the historically vulnerable `golang.org/x/net` `0.6.0`, which is affected by GO-2023-1571, and a second workspace module with an unrelated dependency to make the cross-module behavior obvious.
- `vendor/` uses `vendor/modules.txt`, which the Go toolchain treats as vendored module version metadata when vendoring is enabled.
- `vendor/` intentionally includes the historically vulnerable `golang.org/x/net` `0.6.0`, which is affected by GO-2023-1571, and a transitive vendored `golang.org/x/text` module.
