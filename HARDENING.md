<!-- markdownlint-disable -->

# Hardening Report: thehanimo--pr-title-checker/v1.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **thehanimo--pr-title-checker/v1.4.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file references three Actions using mutable version tags instead of full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if the upstream tag is moved or the repository is compromised. Failing references:
- `actions/checkout@v3` (line 22)
- `actions/setup-node@v3` (line 24)
- `actions/upload-artifact@v2` (line 37)
Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/check-release-drift.yml:22`
- `.github/workflows/check-release-drift.yml:24`
- `.github/workflows/check-release-drift.yml:37`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the only job (`check-release-drift`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary (e.g. write access to contents). A minimal permissions block such as `permissions: read-all` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/check-release-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both findings in .github/workflows/check-release-drift.yml: (1) Pinned all three action references to full 40-character SHA digests with original tags preserved as comments: actions/checkout@a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3, actions/setup-node@3235b876344d2a9aa001b8d1453c930bba69e610 # v3, actions/upload-artifact@82c141cc518b40d92cc801eee768e7aafc9c2fa2 # v2. (2) Added a top-level `permissions: contents: read` block — the minimum required for a workflow that checks out code and reads repository contents.

