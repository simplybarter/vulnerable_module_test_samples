# PHP Fixtures

This directory contains Composer-based fixtures for both lockfile-driven and vendor-metadata-driven dependency detection.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `composer/` | `composer.json`, `composer.lock` | standard Composer manifest and lockfile parsing |
| `composer-custom-vendor/` | `composer.json`, `deps/composer/installed.json` | installed package detection when Composer `vendor-dir` is moved away from the default `vendor/` path |
| `composer-vendor/` | `composer.json`, `vendor/composer/installed.json` | comparison of declared constraints versus installed package metadata checked into the repo |

## Expected Detection Behavior

- `composer/` uses `type: "locked"` and expects:
  - `guzzlehttp/guzzle` `7.0.1`
  - `laravel/framework` `8.12.0`
  - `phpunit/phpunit` `9.3.0`
- `composer-custom-vendor/` mixes types:
  - `manifest`: `symfony/http-foundation` `6.4.13`
  - `installed`: `symfony/http-foundation` `6.4.13`
  - `installed`: `symfony/deprecation-contracts` `3.5.1`
- `composer-vendor/` mixes types:
  - `manifest`: `guzzlehttp/guzzle` `7.0`
  - `installed`: `guzzlehttp/guzzle` `7.8.1`
  - `installed`: `laravel/framework` `11.0.0`

The expected ecosystem label is `Packagist`.

## Notes

- `composer/` verifies that scanners inspect both `packages` and `packages-dev` sections of `composer.lock`.
- `composer-custom-vendor/` uses Composer's `config.vendor-dir` setting to place installed package metadata under `deps/composer/installed.json` instead of the conventional `vendor/composer/installed.json` path.
- `composer-custom-vendor/` intentionally keeps `symfony/http-foundation` `6.4.13` as both a declared and installed package and adds `symfony/deprecation-contracts` as an installed-only package so scanners must handle both path relocation and source attribution.
- `composer-vendor/` is intentionally asymmetric: `guzzlehttp/guzzle` appears as both a declared manifest constraint and an installed package version.
- `laravel/framework` is expected only from installed metadata in `composer-vendor/`, which helps test source attribution.
