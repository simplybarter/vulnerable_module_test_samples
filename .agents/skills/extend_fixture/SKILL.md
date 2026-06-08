---
name: extend_fixture
description: Use this skill when expanding fixture coverage in this repository. It is for adding or improving dependency-discovery fixtures under fixtures/, maintaining expected.json files and language READMEs, researching package-manager gaps online, and creating realistic manifest, lockfile, vendored, and malformed fixture scenarios with known historically vulnerable modules.
---

# Extend Fixture

Use this skill when the task is to extend fixture coverage for one or more languages in this repository.

The objective is not just to add files. The objective is to improve the repository as a dependency-discovery test corpus:

- broaden package-manager and dependency-format coverage
- keep fixtures minimal but realistic
- maintain `expected.json` as the contract
- update documentation with the fixture behavior being tested
- include both clean industry-standard cases and poorly formulated real-world variants

## Repository Contract

Before making changes, inspect:

- `README.md`
- `CONTRIBUTING.md`
- `templates/README.md`
- `templates/expected.template.json`
- `templates/expected.schema.json`
- `fixtures/<language>/README.md` for the language being changed

The fixture contract is:

- every fixture lives under `fixtures/<language>/<fixture>/`
- every fixture must include `expected.json`
- `expected.json` must remain valid against `templates/expected.schema.json`
- allowed discovery types are `manifest`, `locked`, and `installed`

For reporting, also inspect:

- `.agents/skills/extend_fixture/references/activity_report_template.md`

## Required Research Behavior

When deciding whether fixture coverage is incomplete, you must browse the web.

Use primary sources first:

- official package-manager documentation
- official language tooling documentation
- official lockfile or manifest format documentation
- official advisory sources or maintainer security advisories for vulnerable package examples

Use browsing for two separate questions:

1. Is there a meaningful coverage gap for this language?
2. Which package and version make a good historically vulnerable fixture candidate?

When reporting findings, include links to the sources used.

## Language Sweep

Cycle through each language directory already present under `fixtures/`:

- `csharp`
- `go`
- `java`
- `node`
- `php`
- `python`
- `ruby`
- `rust`
- `zig`

For each language:

1. Read `fixtures/<language>/README.md`.
2. Inventory the currently covered package managers, dependency files, and discovery types.
3. Research current and common dependency formats for that ecosystem.
4. Identify missing but meaningful fixture gaps.
5. Rank gaps by usefulness to dependency-discovery testing, not by novelty.

Good gaps usually include:

- a major package manager not yet represented
- a lockfile or vendored format not yet represented
- a widely used legacy format still seen in production
- a malformed or weakly normalized real-world declaration pattern that scanners often mishandle

Do not add coverage just to increase count. Add formats that improve test value.

## Fixture Selection Rules

Choose fixtures that are realistic, narrow, and defensible.

Prefer:

- historically important formats still encountered in repositories
- formats with materially different parsing behavior
- cases that test normalization, transitive discovery, or source attribution
- dependency names and versions that are stable and easy to document

Avoid:

- full runnable applications
- generated noise unrelated to dependency discovery
- speculative vulnerability claims
- fixtures that require network access, installation, or execution to understand

## Real-World Scenario Design

Each new fixture should simulate at least one of these patterns:

- standard well-formed manifest or lockfile usage
- mixed direct and transitive dependencies
- vendored or installed metadata checked into the repository
- old but still plausible enterprise format usage
- poorly formatted but parseable configuration
- comments, whitespace oddities, duplicate declarations, casing drift, or alternate section placement where that ecosystem allows it

Poorly formulated does not mean invalid garbage. It means realistic inputs that appear in real repositories and still deserve deterministic scanner behavior.

## Vulnerable Package Guidance

When choosing package examples, prefer a package with a clearly documented historical vulnerability or advisory trail.

Selection rules:

- use a real package from the ecosystem under test
- use a version that is widely recognized as vulnerable or included in a published advisory range
- verify the package/version relationship with primary or highly authoritative sources
- prefer packages that are common enough to be recognizable to tool authors

Do not overstate the claim. The fixture exists to test dependency discovery and vulnerable-package surfacing inputs, not to prove exploitability.

## expected.json Rules

`expected.json` is the contract. Treat it as carefully as code.

Requirements:

- every surfaced package entry must have `name`, `version`, `eco`, and `type`
- use the repository ecosystem label expected for that language
- use `manifest` only for direct declarations in manifests or project files
- use `locked` for pinned lockfile or resolved dependency data
- use `installed` for vendored or installed metadata committed into the repo
- keep entries deterministic and minimal
- do not include packages that cannot be defended from the checked-in files

When a fixture includes multiple discovery surfaces, record all intended entries explicitly and keep the distinction accurate.

Before finishing:

- compare the file to `templates/expected.template.json`
- validate the structure against `templates/expected.schema.json`
- confirm names and versions match the checked-in fixture files exactly

## Documentation Rules

Any fixture change must update documentation in the same turn.

Update:

- `fixtures/<language>/README.md` for language-specific behavior
- root `README.md` if coverage or repository-wide expectations change
- `templates/` docs only if the fixture contract itself changes

Document:

- the fixture name
- the files under test
- what behavior it exercises
- the expected ecosystem label
- any important edge cases or asymmetries

## Activity Logging

Every run of this skill must produce or update an activity report in:

- `logs/activity/`

File naming is required:

- `activity-[YYYY-MM-DD].md`

Logging rules:

- append to the file for the current date if it already exists
- create the file if it does not exist
- each entry must be timestamped in 24-hour format as `## [YYYY-MM-DD HH:MM]`
- use the structure from `.agents/skills/extend_fixture/references/activity_report_template.md`
- record both decisions and non-decisions, including when a language was reviewed and no worthwhile gap was selected

Each log entry must document:

- the language reviewed
- workflow assessment
- research sources
- vulnerable package selection or rejection
- implementation details
- `expected.json` assessment
- documentation impact
- repository impact and follow-up gaps

## Preferred Workflow

Use this sequence:

1. Audit the current fixture matrix locally.
2. Browse official docs to identify meaningful missing formats.
3. Browse authoritative advisory sources to choose a suitable package example.
4. Select one or more high-value gaps.
5. Create the smallest fixture that exercises the intended behavior.
6. Write or update `expected.json`.
7. Update the relevant language README and any root docs affected.
8. Validate file consistency and summarize remaining gaps.

## Quality Bar

A strong fixture:

- isolates one or two behaviors clearly
- is small enough to review quickly
- resembles something seen in real repositories
- has deterministic expected output
- distinguishes manifest, locked, and installed evidence correctly
- makes scanner failures easy to explain

A weak fixture:

- mixes too many behaviors at once
- requires guesswork to derive expected dependencies
- lacks documentation
- uses unrealistic placeholder packages
- cannot justify why the chosen format belongs in the corpus

## Output Expectations

When using this skill, produce two outputs:

1. A repository log entry in `logs/activity/activity-[YYYY-MM-DD].md` using the template in `.agents/skills/extend_fixture/references/activity_report_template.md`.
2. A user-facing summary that covers:

- the language reviewed
- the current coverage found
- the gap selected and why it matters
- the online sources used
- the package/version chosen as the vulnerable example
- the files added or changed
- the documentation updated
- any remaining high-value gaps for that language

If no worthwhile gap is found for a language, say so explicitly and move to the next language.
