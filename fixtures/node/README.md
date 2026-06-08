# Node.js Fixtures

This directory contains JavaScript and TypeScript ecosystem fixtures that exercise lockfile parsing across multiple runtimes and package managers.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `bun/` | `bun.lock` | Bun lockfile parsing |
| `deno/` | `deno.lock` | Deno lock parsing for both npm-backed packages and remote imports |
| `npm/` | `package-lock.json` | npm package-lock parsing including transitive dependencies |

## Expected Detection Behavior

All fixtures currently use `type: "locked"` in `expected.json`.

- `bun/` expects `hono` `4.0.0` and `zod` `3.22.4`
- `npm/` expects `express` `4.18.2` and transitive `accepts` `1.3.8`
- `deno/` expects:
  - npm packages `express` `4.18.2` and `accepts` `1.3.8`
  - `colors` `0.224.0`, derived from the remote import `https://deno.land/std@0.224.0/fmt/colors.ts`

The expected ecosystem label is `node`.

## Notes

- `npm/` is useful for validating traversal of the `packages` object in lockfile version 3.
- `deno/` intentionally mixes npm dependency metadata with a remote URL import so scanners can prove they handle both forms.
- There is no top-level `package.json` in these samples; the fixtures are intentionally minimal and centered on the lockfiles themselves.
