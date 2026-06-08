# C# Fixtures

This directory contains C# dependency fixtures across NuGet and Paket dependency surfaces.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `legacy/` | `packages.config` | legacy NuGet package declaration parsing |
| `lockfile/` | `App.csproj`, `packages.lock.json` | SDK-style `PackageReference` with checked-in NuGet lockfile parsing |
| `msbuild/` | `App.csproj` | SDK-style MSBuild `PackageReference` parsing |
| `central-packages/` | `Directory.Packages.props`, `src/App/App.csproj` | NuGet Central Package Management with versions declared outside the project file |
| `paket/` | `paket.dependencies`, `paket.lock`, `src/App/paket.references` | Paket dependency and lockfile parsing for NuGet-backed package resolution |
| `packages-folder/` | `packages.config`, `nuget.config`, extracted `.nuspec` files under `packages/` | comparison of declared NuGet package entries against checked-in installed package metadata |

## Expected Detection Behavior

- `legacy/` uses `type: "manifest"` and expects `EntityFramework` `6.4.4` and `Newtonsoft.Json` `13.0.1`
- `msbuild/` uses `type: "manifest"` and expects `Newtonsoft.Json` `12.0.1` and `Microsoft.Extensions.Logging` `5.0.0`
- `central-packages/` uses `type: "manifest"` and expects `Newtonsoft.Json` `12.0.3` and `Microsoft.Extensions.Logging` `5.0.0`
- `lockfile/` uses `type: "locked"` and expects:
  - `Newtonsoft.Json` `12.0.3`
  - `Microsoft.Extensions.Logging` `5.0.0`
  - `Microsoft.Extensions.Logging.Abstractions` `5.0.0`
  - `Microsoft.Extensions.DependencyInjection.Abstractions` `5.0.0`
  - `Microsoft.Extensions.Options` `5.0.0`
  - `Microsoft.Extensions.Primitives` `5.0.0`
- `paket/` mixes types and expects:
  - `manifest`: `Newtonsoft.Json` `12.0.3`
  - `manifest`: `Microsoft.Extensions.Logging` `5.0.0`
  - `locked`: `Newtonsoft.Json` `12.0.3`
  - `locked`: `Microsoft.Extensions.Logging` `5.0.0`
  - `locked`: `Microsoft.Extensions.Logging.Abstractions` `5.0.0`
  - `locked`: `Microsoft.Extensions.DependencyInjection.Abstractions` `5.0.0`
  - `locked`: `Microsoft.Extensions.Options` `5.0.0`
  - `locked`: `Microsoft.Extensions.Primitives` `5.0.0`
- `packages-folder/` mixes types and expects:
  - `Newtonsoft.Json` `12.0.3` as both `manifest` and `installed`
  - `Microsoft.Extensions.Logging.Abstractions` `5.0.0` as `installed`

The expected ecosystem label is `NuGet`.

## Notes

- `legacy/` covers the older `packages.config` format where package IDs and versions are declared as XML attributes.
- `msbuild/` covers package references embedded directly in the project file.
- `central-packages/` covers NuGet Central Package Management, where `PackageReference` items omit versions in the project file and the actual versions live in `Directory.Packages.props`.
- `central-packages/` intentionally keeps the historically vulnerable `Newtonsoft.Json` `12.0.3` in the shared props file so scanners must join project-level package IDs with centrally managed versions.
- `lockfile/` models an application-style project with a checked-in `packages.lock.json`, which is useful for scanners that need to distinguish resolved locked packages from direct project-file declarations.
- `lockfile/` intentionally includes the historically vulnerable `Newtonsoft.Json` `12.0.3` along with transitive logging-related packages so scanners can validate locked dependency extraction beyond a single direct package.
- `paket/` covers Paket's three core file types: `paket.dependencies` for repository-wide declarations, `paket.lock` for resolved direct and transitive packages, and `paket.references` for the per-project direct package subset.
- `paket/` intentionally keeps `Newtonsoft.Json` `12.0.3` as both a direct declaration and a locked package while using `Microsoft.Extensions.Logging` to surface a small transitive NuGet graph through Paket's lockfile syntax.
- `packages-folder/` uses `nuget.config` `repositoryPath` to point `packages.config`-style installs at a local `packages/` directory, then exposes installed package identity through extracted `.nuspec` metadata.
- `packages-folder/` intentionally keeps `Newtonsoft.Json` `12.0.3` as a declared and installed package and includes an additional installed-only `Microsoft.Extensions.Logging.Abstractions` package to simulate a stale or asymmetric packages folder that scanners should still inventory.
