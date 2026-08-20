<!-- markdownlint-disable -->

# Hardening Report: thehanimo--pr-title-checker/v1.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **thehanimo--pr-title-checker/v1.4.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses tag-based (mutable) action references instead of pinned SHA digests, making the workflow vulnerable to supply-chain attacks if the referenced tags are moved or compromised. Failing references: `actions/checkout@v3` (line 23), `actions/setup-node@v3` (line 25), `actions/upload-artifact@v2` (line 38). Each should be replaced with a full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/check-release-drift.yml:23`
- `.github/workflows/check-release-drift.yml:25`
- `.github/workflows/check-release-drift.yml:38`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/check-release-drift.yml` has no top-level `permissions:` key, and the only job (`check-release-drift`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. write access to contents). A minimal permissions block such as `permissions: { contents: read }` should be added at the top level or job level.

Locations:

- `.github/workflows/check-release-drift.yml:7`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed .github/workflows/check-release-drift.yml: (1) Pinned all three action references to full commit SHAs — actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610, actions/upload-artifact@v2 → @82c141cc518b40d92cc801eee768e7aafc9c2fa2 — each with the original tag preserved as a comment. (2) Added a top-level `permissions: contents: read` block to restrict the workflow token to the minimum required permissions.

