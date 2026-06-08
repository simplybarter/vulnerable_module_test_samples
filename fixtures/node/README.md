# Node.js Fixtures

This directory contains JavaScript and TypeScript ecosystem fixtures that exercise lockfile parsing across multiple runtimes and package managers.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `bun/` | `bun.lock` | Bun lockfile parsing |
| `deno/` | `deno.lock` | Deno lock parsing for both npm-backed packages and remote imports |
| `npm/` | `package-lock.json` | npm package-lock parsing including transitive dependencies |
| `pnpm/` | `package.json`, `pnpm-lock.yaml` | pnpm lockfile parsing through importer and package sections |
| `yarn/` | `package.json`, `yarn.lock` | Yarn Classic lockfile parsing with grouped selectors and manifest-versus-lockfile comparison |

## Expected Detection Behavior

Most fixtures in this directory use `type: "locked"` only. `yarn/` intentionally mixes `manifest` and `locked` evidence in the same fixture.

- `bun/` expects `hono` `4.0.0` and `zod` `3.22.4`
- `npm/` expects `express` `4.18.2` and transitive `accepts` `1.3.8`
- `pnpm/` expects:
  - direct packages `express` `4.18.2` and `lodash` `4.17.20`
  - transitive package `accepts` `1.3.8`
- `deno/` expects:
  - npm packages `express` `4.18.2` and `accepts` `1.3.8`
  - `colors` `0.224.0`, derived from the remote import `https://deno.land/std@0.224.0/fmt/colors.ts`
- `yarn/` expects:
  - manifest packages `minimist` `^0.0.8` and `mkdirp` `0.5.1`
  - locked packages `minimist` `0.0.8` and `mkdirp` `0.5.1`

The expected ecosystem label is `node`.

## Notes

- `npm/` is useful for validating traversal of the `packages` object in lockfile version 3.
- `pnpm/` is useful for validating scanners that read both the `importers` block and the `packages` map from `pnpm-lock.yaml`.
- `pnpm/` intentionally includes the historically vulnerable `lodash` `4.17.20`, which is affected by the command-injection advisory patched in `4.17.21`.
- `deno/` intentionally mixes npm dependency metadata with a remote URL import so scanners can prove they handle both forms.
- `yarn/` uses the classic `yarn.lock` v1 format and a grouped selector entry so scanners can prove they normalize multiple selectors that resolve to the same locked package.
- `yarn/` intentionally includes `minimist` `0.0.8`, a historically vulnerable npm package, so scanners can compare the declared semver range in `package.json` with the exact vulnerable version pinned in `yarn.lock`.
- Only fixtures whose behavior depends on manifest-plus-lockfile comparison include `package.json`; the rest stay lockfile-centric on purpose.
