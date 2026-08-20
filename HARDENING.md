<!-- markdownlint-disable -->

# Hardening Report: thehanimo--pr-title-checker/v1.4.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **thehanimo--pr-title-checker/v1.4.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file references three GitHub Actions using mutable version tags instead of pinned full-length SHA digests. This exposes the workflow to supply-chain attacks if the referenced tags are moved or overwritten. Failing references: `actions/checkout@v3` (line 22), `actions/setup-node@v3` (line 24), `actions/upload-artifact@v2` (line 37).

Locations:

- `.github/workflows/check-release-drift.yml:22`
- `.github/workflows/check-release-drift.yml:24`
- `.github/workflows/check-release-drift.yml:37`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the single job `check-release-drift` also has no job-level `permissions:` key. Without explicit permissions, the job inherits the default repository token permissions, which may be broader than necessary (e.g. write access to contents). A minimal explicit permissions block should be added.

Locations:

- `.github/workflows/check-release-drift.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed .github/workflows/check-release-drift.yml: (1) Pinned all three action references to full commit SHAs — actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, actions/upload-artifact@v2 → 82c141cc518b40d92cc801eee768e7aafc9c2fa2 — with original tags preserved as inline comments. (2) Added top-level `permissions: contents: read` block, which is the minimum required for a workflow that only checks out code and uploads artifacts.

