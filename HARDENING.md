<!-- markdownlint-disable -->

# Hardening Report: martialonline--workflow-status/v4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **martialonline--workflow-status/v4.1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): `${{ github.token }}` is directly interpolated inside a `run:` shell block in action.yml. Even though github.token is not attacker-controlled, any ${{ ... }} expression inside a run: block flows through YAML template substitution before the shell sees it, making it a script-injection risk. The token value should be passed via an `env:` variable (e.g., `TOKEN: ${{ github.token }}`) and referenced as `$TOKEN` in the shell script.

Locations:

- `action.yml:14`

### script-injection (severity: high)

Sub-rule (a): `${{ steps.test.outputs.status }}` is directly interpolated inside a `run:` shell command: `echo "Workflow Status '${{ steps.test.outputs.status }}'"`. The `steps.*.outputs.*` context is workflow-controllable and must not be interpolated directly into shell — it should be passed via an `env:` variable and double-quoted in the script.

Locations:

- `.github/workflows/test.yml:24`

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v2`, which is pinned to a mutable tag rather than a full 40-character commit SHA. This is vulnerable to supply-chain attacks if the tag is moved. Use a full SHA pin such as `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`.

Locations:

- `.github/workflows/test.yml:20`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted its default (often broad) permissions. Add a top-level `permissions: {}` block and grant only the minimum required scopes.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings: (1) action.yml: moved `${{ github.token }}` to an `env:` block as `TOKEN` and updated shell references from `${token}` to `${TOKEN}`; (2) test.yml: moved `${{ steps.test.outputs.status }}` to an `env:` block as `WORKFLOW_STATUS` and referenced it as `$WORKFLOW_STATUS` in the run step; (3) test.yml: pinned `actions/checkout@v2` to full SHA `0717577d45739eb3c851188b29f50ed6c0b2194e # v2`; (4) test.yml: added top-level `permissions: {}` block.

