<!-- markdownlint-disable -->

# Hardening Report: thehanimo--pr-title-checker/v1.3.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **thehanimo--pr-title-checker/v1.3.7** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file .github/workflows/check-dist.yml has no top-level `permissions:` key and the single job `check-dist` also has no `permissions:` key. Without explicit permissions, the workflow inherits the default repository permissions (which may include write access), violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:7`

### unpinned-uses (severity: high)

Three `uses:` references in .github/workflows/check-dist.yml are pinned to mutable version tags rather than immutable full 40-character SHA digests, making the workflow vulnerable to supply-chain attacks if those tags are moved:
- `uses: actions/checkout@v3` (tag ref)
- `uses: actions/setup-node@v3` (tag ref)
- `uses: actions/upload-artifact@v2` (tag ref)
Each should be replaced with a full commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/check-dist.yml:22`
- `.github/workflows/check-dist.yml:25`
- `.github/workflows/check-dist.yml:43`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Added `permissions: {}` at the workflow top level to enforce least privilege. Pinned all three `uses:` references to full 40-character commit SHAs: actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, actions/upload-artifact@v2 → 82c141cc518b40d92cc801eee768e7aafc9c2fa2. Original tag names preserved as inline comments.

