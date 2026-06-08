# Node.js Fixtures

This directory contains JavaScript and TypeScript ecosystem fixtures that exercise manifest, lockfile, and installed-package parsing across multiple runtimes and package managers.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `bun/` | `bun.lock` | Bun lockfile parsing |
| `deno/` | `deno.lock` | Deno lock parsing for both npm-backed packages and remote imports |
| `node-modules/` | `package.json`, `node_modules/**/package.json` | checked-in installed package metadata parsing from a nested `node_modules` tree |
| `npm/` | `package-lock.json` | npm package-lock parsing including transitive dependencies |
| `npm-shrinkwrap/` | `package.json`, `npm-shrinkwrap.json`, `package-lock.json` | npm shrinkwrap parsing and documented precedence over a conflicting `package-lock.json` |
| `pnpm/` | `package.json`, `pnpm-lock.yaml` | pnpm lockfile parsing through importer and package sections |
| `yarn-berry/` | `package.json`, `.yarnrc.yml`, `yarn.lock` | Yarn Berry lockfile parsing with modern YAML metadata and Plug'n'Play configuration |
| `yarn/` | `package.json`, `yarn.lock` | Yarn Classic lockfile parsing with grouped selectors and manifest-versus-lockfile comparison |

## Expected Detection Behavior

Most fixtures in this directory use `type: "locked"` only. `yarn/` intentionally mixes `manifest` and `locked` evidence in the same fixture.

- `bun/` expects `hono` `4.0.0` and `zod` `3.22.4`
- `node-modules/` expects:
  - manifest package `mkdirp` `^0.5.1`
  - installed package `mkdirp` `0.5.1`
  - installed transitive package `minimist` `0.0.8`
- `npm/` expects `express` `4.18.2` and transitive `accepts` `1.3.8`
- `npm-shrinkwrap/` expects:
  - manifest package `mkdirp` `^0.5.1`
  - locked package `mkdirp` `0.5.1`
  - locked transitive package `minimist` `0.0.8`
- `pnpm/` expects:
  - direct packages `express` `4.18.2` and `lodash` `4.17.20`
  - transitive package `accepts` `1.3.8`
- `yarn-berry/` expects:
  - manifest packages `lodash` `^4.17.20` and `mkdirp` `0.5.1`
  - locked packages `lodash` `4.17.20`, `mkdirp` `0.5.1`, and transitive `minimist` `0.0.8`
- `deno/` expects:
  - npm packages `express` `4.18.2` and `accepts` `1.3.8`
  - `colors` `0.224.0`, derived from the remote import `https://deno.land/std@0.224.0/fmt/colors.ts`
- `yarn/` expects:
  - manifest packages `minimist` `^0.0.8` and `mkdirp` `0.5.1`
  - locked packages `minimist` `0.0.8` and `mkdirp` `0.5.1`

The expected ecosystem label is `node`.

## Notes

- `npm/` is useful for validating traversal of the `packages` object in lockfile version 3.
- `node-modules/` is useful for validating scanners that inspect checked-in installed package metadata rather than only manifests and lockfiles.
- `node-modules/` intentionally places `minimist` under `node_modules/mkdirp/node_modules/` so scanners can prove they recurse through nested installed dependency trees instead of only reading the first level.
- `npm-shrinkwrap/` is useful for validating npm's documented lockfile precedence: when both files exist, `npm-shrinkwrap.json` wins and `package-lock.json` is ignored.
- `npm-shrinkwrap/` intentionally includes a conflicting `package-lock.json` that resolves `mkdirp` and `minimist` to different versions so scanners can prove they prefer the shrinkwrap tree.
- `pnpm/` is useful for validating scanners that read both the `importers` block and the `packages` map from `pnpm-lock.yaml`.
- `pnpm/` intentionally includes the historically vulnerable `lodash` `4.17.20`, which is affected by the command-injection advisory patched in `4.17.21`.
- `yarn-berry/` uses the modern Yarn lockfile format with a top-level `__metadata` block plus per-package `resolution`, `checksum`, and `linkType` fields, which differ materially from Yarn Classic's v1 syntax.
- `yarn-berry/` sets `nodeLinker: pnp` in `.yarnrc.yml` and includes the `packageManager` field in `package.json` so scanners can distinguish a modern Yarn Plug'n'Play project from a classic lockfile-only fixture.
- `yarn-berry/` intentionally includes the historically vulnerable `lodash` `4.17.20` and keeps `minimist` `0.0.8` as a transitive lockfile entry so scanners can prove they traverse both direct and transitive Yarn Berry package blocks.
- `deno/` intentionally mixes npm dependency metadata with a remote URL import so scanners can prove they handle both forms.
- `yarn/` uses the classic `yarn.lock` v1 format and a grouped selector entry so scanners can prove they normalize multiple selectors that resolve to the same locked package.
- `yarn/` intentionally includes `minimist` `0.0.8`, a historically vulnerable npm package, so scanners can compare the declared semver range in `package.json` with the exact vulnerable version pinned in `yarn.lock`.
- Only fixtures whose behavior depends on manifest-plus-lockfile comparison include `package.json`; the rest stay lockfile-centric on purpose.
