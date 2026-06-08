# Agent Guidance

This repository is a dependency-discovery fixture corpus. Treat it as test data, not as a runnable application monorepo.

## Repository Rules

- keep all fixtures under `fixtures/<language>/<fixture>/`
- keep fixtures minimal, deterministic, and reviewable
- do not add full applications, unrelated generated files, or unnecessary noise
- preserve the repository contract defined in `README.md` and `CONTRIBUTING.md`

## Guardrails

- never install a programming language runtime, SDK, package manager, module, dependency, or system tool for work in this repository
- never run package restore, dependency install, build, compile, or execution steps unless the user explicitly asks for that and the action is necessary
- never fetch packages from external registries just to construct or validate fixtures
- prefer static fixture authoring and documentation over environment setup
- do not modify files outside the repository scope unless the user explicitly requests it
- do not treat the presence of vulnerable package versions as permission to execute exploit code or proof-of-concept payloads

## Git Rules

- keep each fixture expansion as a discrete git commit
- do not bundle multiple unrelated fixture additions, language expansions, or major documentation changes into one commit
- scope each commit to one coherent unit of work, ideally a single language and fixture scenario
- include the corresponding documentation and activity-log updates in the same commit as the fixture change they describe
- if multiple fixture expansions are performed in one session, stage and commit them separately

## expected.json Contract

Every fixture must include `expected.json`.

Each entry must contain:

- `name`
- `version`
- `eco`
- `type`

Allowed discovery types:

- `manifest`
- `locked`
- `installed`

Use `templates/expected.template.json` as the example shape and `templates/expected.schema.json` as the validation contract.

## Documentation Rules

When fixture behavior changes:

- update `fixtures/<language>/README.md`
- update `README.md` if repository-wide coverage or structure changes
- update templates only if the fixture contract itself changes

## Activity Logging

When an agent performs fixture expansion work, append a timestamped entry to:

- `logs/activity/activity-[YYYY-MM-DD].md`

## Skill Usage

For fixture expansion, gap analysis, package-manager research, vulnerable package selection, and activity reporting, use:

- `.agents/skills/extend_fixture/SKILL.md`

That skill is the authoritative workflow for extending fixture coverage in this repository.
