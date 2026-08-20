<!-- markdownlint-disable -->

# Hardening Report: thehanimo--pr-title-checker/v1.4.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **thehanimo--pr-title-checker/v1.4.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file .github/workflows/check-release-drift.yml has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). A minimal permissions block should be added.

Locations:

- `.github/workflows/check-release-drift.yml:1`

### unpinned-uses (severity: high)

Three `uses:` references in .github/workflows/check-release-drift.yml use mutable version tags instead of full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the tag is moved or the action is compromised:
- `actions/checkout@v3` (mutable tag)
- `actions/setup-node@v3` (mutable tag)
- `actions/upload-artifact@v4` (mutable tag)
Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/check-release-drift.yml:22`
- `.github/workflows/check-release-drift.yml:24`
- `.github/workflows/check-release-drift.yml:36`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Added top-level `permissions: contents: read` block to restrict the workflow token to the minimum needed. Pinned all three mutable action tags to full 40-character commit SHAs: actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, actions/upload-artifact@v4 → ea165f8d65b6e75b540449e92b4886f43607fa02. Original tag names preserved as inline comments.

