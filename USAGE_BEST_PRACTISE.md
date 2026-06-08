# Usage Best Practise

This document describes the safest way to use this repository when testing dependency scanners, repository analysis systems, and vulnerable-package surfacing workflows.

The fixtures in this repository are static and synthetic, but the safest operating model is still to treat any repository analysis workflow as potentially sensitive. That is especially true when the scanner is proprietary, uploads source material, executes helper tooling, or stores results outside your control.

## Core Principle

Scan this repository in an isolated environment with the minimum access needed to complete the test.

The goal is to reduce the chance that:

- local credentials are exposed to the scanner or its runtime
- unrelated source code or files are collected accidentally
- build tools or package managers contact external services unnecessarily
- scan output containing internal metadata is shared too broadly

## Recommended Environment

Use one of the following:

- a disposable virtual machine
- a locked-down container
- a short-lived cloud workspace or sandbox
- a dedicated CI job running in an isolated project or account

Prefer environments that are:

- ephemeral and easy to destroy after the scan
- separated from your normal development workstation
- free of unrelated repositories, tokens, SSH keys, browser sessions, and personal files
- configured with explicit outbound network rules

## Repository Handling

Before scanning:

- clone only this repository into the isolated environment
- avoid placing it alongside private or proprietary repositories
- do not copy unrelated configuration files into the workspace
- verify that the scanner target path is limited to this repository only

If the scanner supports archive upload instead of full workspace upload, prefer uploading only the repository contents required for the test.

## Credentials And Secrets

Do not scan this repository from an environment that contains:

- production cloud credentials
- long-lived API tokens
- SSH keys used for private infrastructure
- authenticated browser sessions
- package-manager credentials for private registries unless absolutely required

If credentials are needed for the scanner itself:

- use short-lived tokens where possible
- scope them only to the minimum project or service required
- revoke or rotate them after testing if they were created specifically for this workflow

## Network And Execution Controls

Dependency scanners vary widely. Some only parse files. Others may invoke package-manager logic, build-system helpers, or remote lookups.

To maximize protection:

- prefer scanners that can run in a read-only analysis mode
- disable automatic build, install, restore, or dependency-fetch steps unless they are explicitly part of the test
- restrict outbound network access to only what the scanner requires
- block access to internal package registries, source control systems, and metadata services unless intentionally needed
- avoid mounting sensitive host directories into containers used for scanning

If you do need network access, use a dedicated egress path that is monitored and limited.

## Data Privacy

Assume that a scanner may collect some combination of:

- file names
- directory structure
- manifest contents
- package names and versions
- scan results and derived metadata

Before using a third-party service:

- review whether repository contents are uploaded, cached, or retained
- review whether results are used for product training, analytics, or shared benchmarking
- review where data is stored and which region or jurisdiction applies
- confirm whether uploaded data can be deleted on demand

If data privacy is a priority, prefer self-hosted or locally executed scanning workflows.

## Output Handling

Treat scan output as data that may still require care.

- store results in a controlled location
- avoid pasting raw outputs into public systems unless you intend them to be public
- sanitize logs if they include hostnames, usernames, internal file paths, or account identifiers
- separate public benchmark results from any internal notes about scanner configuration or access patterns

## CI And Automation Guidance

If you scan this repository in CI:

- run the scan in a dedicated pipeline or isolated job
- use repository-scoped or short-lived credentials only
- disable access to secrets not required by the job
- avoid reusing runners that also process sensitive internal repositories unless isolation is strong
- expire artifacts and logs on a short retention schedule where possible

For vendor evaluations, create a dedicated test project rather than scanning from a shared production CI environment.

## Local Workstation Guidance

Avoid running unreviewed scanning tools directly on a primary workstation when a disposable environment is practical.

If local execution is necessary:

- use a separate OS user account or isolated container runtime
- close unrelated development tools and browser sessions
- confirm the scanner is pointed only at this repository
- monitor what files, processes, and network destinations the tool accesses

## Multi-Tenant And SaaS Scanners

When using hosted scanning platforms:

- confirm the repository visibility setting before upload
- use private projects unless you intentionally want public results
- review organization-sharing defaults
- verify who can access historical scans and exported reports
- understand whether vendor staff can access uploaded materials for support or debugging

## What This Repository Does Not Change

This repository being open source does not remove the need for safe scanning practices.

Even though the fixture content is intentionally synthetic, your scanner configuration, account metadata, access tokens, IP addresses, and result history may still be sensitive.

## Minimum Safe Checklist

Before scanning, verify:

- the scan runs in an isolated environment
- the target path contains only this repository
- unnecessary secrets and credentials are not present
- network access is restricted where possible
- automatic install or build steps are disabled unless intentionally required
- result retention and sharing settings are understood

## Final Recommendation

For the strongest privacy and safety posture, use a short-lived isolated environment, keep the repository as the only scanned content, minimize credentials, and prefer local or self-hosted analysis over broad third-party upload workflows whenever possible.
