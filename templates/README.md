# Templates

This directory contains reusable scaffolding files that illustrate repository conventions without acting as live fixtures.

## Purpose

Use these files as a starting point when creating new fixture directories under `fixtures/`.

The goal is to make it easier to copy the expected structure and field formatting used by this repository without pulling example data from an unrelated language fixture.

## Included Templates

- `expected.template.json`: a formatted example of the standard `expected.json` structure, including representative `manifest`, `locked`, and `installed` entries
- `expected.schema.json`: a machine-readable JSON Schema describing the valid `expected.json` structure and allowed discovery types

## Usage Notes

- `expected.template.json` is intended to be copied to `fixtures/<language>/<fixture>/expected.json` when creating a new fixture
- use `expected.schema.json` when validating generated or AI-authored `expected.json` files
- replace the placeholder names, versions, ecosystem label, and discovery types with values that match the fixture you are adding
- remove any example entries that do not apply to the fixture

Templates in this directory are documentation aids. They should not be treated as scanner test cases or included in fixture result comparisons.
