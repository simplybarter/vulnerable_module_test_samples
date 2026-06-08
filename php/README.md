# PHP Fixtures

This directory contains Composer-based fixtures for both lockfile-driven and vendor-metadata-driven dependency detection.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `composer/` | `composer.json`, `composer.lock` | standard Composer manifest and lockfile parsing |
| `composer-vendor/` | `composer.json`, `vendor/composer/installed.json` | comparison of declared constraints versus installed package metadata checked into the repo |

## Expected Detection Behavior

- `composer/` uses `type: "locked"` and expects:
  - `guzzlehttp/guzzle` `7.0.1`
  - `laravel/framework` `8.12.0`
  - `phpunit/phpunit` `9.3.0`
- `composer-vendor/` mixes types:
  - `manifest`: `guzzlehttp/guzzle` `7.0`
  - `installed`: `guzzlehttp/guzzle` `7.8.1`
  - `installed`: `laravel/framework` `11.0.0`

The expected ecosystem label is `Packagist`.

## Notes

- `composer/` verifies that scanners inspect both `packages` and `packages-dev` sections of `composer.lock`.
- `composer-vendor/` is intentionally asymmetric: `guzzlehttp/guzzle` appears as both a declared manifest constraint and an installed package version.
- `laravel/framework` is expected only from installed metadata in `composer-vendor/`, which helps test source attribution.
