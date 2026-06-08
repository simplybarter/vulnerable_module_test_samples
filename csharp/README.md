# C# Fixtures

This directory contains NuGet-oriented fixture samples for C# repositories. The current focus is manifest parsing rather than lockfile or installed-package detection.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `legacy/` | `packages.config` | legacy NuGet package declaration parsing |
| `msbuild/` | `App.csproj` | SDK-style MSBuild `PackageReference` parsing |

## Expected Detection Behavior

Both fixtures use `type: "manifest"` in `expected.json`.

- `legacy/` expects `EntityFramework` `6.4.4` and `Newtonsoft.Json` `13.0.1`
- `msbuild/` expects `Newtonsoft.Json` `12.0.1` and `Microsoft.Extensions.Logging` `5.0.0`

The expected ecosystem label is `NuGet`.

## Notes

- `legacy/` covers the older `packages.config` format where package IDs and versions are declared as XML attributes.
- `msbuild/` covers package references embedded directly in the project file.
- There is intentionally no NuGet lockfile or vendored package metadata fixture here yet.
