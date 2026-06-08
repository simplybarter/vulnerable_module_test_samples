# Ruby Fixtures

This directory contains Ruby dependency fixtures for Bundler lockfiles and vendored gem metadata.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `bundler/` | `Gemfile.lock` | lockfile parsing across multiple gem sources and transitive dependencies |
| `bundle-vendor/` | `Gemfile`, vendored `.gemspec` files | comparison of declared gem constraints against installed gem specifications checked into the repository |

## Expected Detection Behavior

- `bundler/` uses `type: "locked"` and expects:
  - `activesupport` `7.1.2`
  - `concurrent-ruby` `1.2.2`
  - `i18n` `1.14.1`
  - `minitest` `5.20.0`
  - `tzinfo` `2.0.6`
  - `rails-assets-jquery` `3.7.1`
- `bundle-vendor/` mixes types:
  - `manifest`: `nokogiri` `1.15`
  - `manifest`: `rails` `7.1`
  - `installed`: `nokogiri` `1.16.2`
  - `installed`: `rails` `7.1.3`

The expected ecosystem label is `RubyGems`.

## Notes

- `bundler/` includes two gem sources in `Gemfile.lock`: `rubygems.org` and `rails-assets.org`.
- `bundle-vendor/` is designed to test scanners that inspect vendored gemspec files under `vendor/bundle/.../specifications`.
- The manifest expectations in `bundle-vendor/` intentionally preserve simplified version constraints rather than the fully installed versions.
