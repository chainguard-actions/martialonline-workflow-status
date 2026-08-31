<!-- markdownlint-disable -->

# Hardening Report: martialonline--workflow-status/v4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **martialonline--workflow-status/v4** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): `${{ github.token }}` is directly interpolated inside a `run:` shell command string in action.yml. Any `${{ ... }}` expression inside a `run:` block is a script-injection risk because YAML template substitution occurs before the shell ever sees the value. The token should be passed via an `env:` variable instead (e.g., `env: TOKEN: ${{ github.token }}`) and referenced as `$TOKEN` in the script.

Locations:

- `action.yml:14`

### script-injection (severity: high)

Sub-rule (a): `${{ steps.test.outputs.status }}` is directly interpolated inside a `run:` shell command string. `steps.*.outputs.*` is a workflow-controllable context and must not be interpolated directly into shell commands. The value should be passed via an `env:` variable and double-quoted in the script.

Locations:

- `.github/workflows/test.yml:23`

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v2`, which is pinned to a mutable tag rather than an immutable 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit. Pin to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`.

Locations:

- `.github/workflows/test.yml:19`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the `test` job also has no `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. Add a `permissions:` block with the minimum required scopes (e.g., `contents: read`).

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed 4 findings across 2 files: (1) action.yml: moved `${{ github.token }}` into an `env: TOKEN:` block and referenced it as `${TOKEN}` in the shell script to prevent script injection; (2) test.yml: added top-level `permissions: contents: read` block; pinned `actions/checkout@v2` to full SHA `0717577d45739eb3c851188b29f50ed6c0b2194e # v2`; moved `${{ steps.test.outputs.status }}` into an `env: WORKFLOW_STATUS:` block and referenced it as `$WORKFLOW_STATUS` in the shell script.

