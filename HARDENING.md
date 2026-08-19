<!-- markdownlint-disable -->

# Hardening Report: martialonline--workflow-status/v3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **martialonline--workflow-status/v3** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The expression `${{ github.token }}` is interpolated directly inside a `run:` shell command string in action.yml. Any `${{ ... }}` expression embedded directly in a run block undergoes YAML template substitution before the shell sees it, making it a script-injection risk. The offending line is: `token="${{ github.token }}"`

Locations:

- `action.yml:17`

### script-injection (severity: high)

Sub-rule (a): The expression `${{ steps.test.outputs.status }}` is interpolated directly inside a `run:` shell command string in the workflow file. `steps.*.outputs.*` is a workflow-controllable context and must not be embedded directly in a run block. The offending line is: `run: echo "Workflow Status '${{ steps.test.outputs.status }}'"` 

Locations:

- `.github/workflows/test.yml:24`

### unpinned-uses (severity: high)

The workflow references `actions/checkout@v2`, which uses a mutable tag ref instead of a pinned 40-character commit SHA. This is vulnerable to supply-chain attacks if the tag is moved. It should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`.

Locations:

- `.github/workflows/test.yml:21`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the `test` job also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions (which may be `write-all`). A minimal `permissions:` block should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings: (1) action.yml script-injection: moved ${{ github.token }} to env block as GITHUB_TOKEN; (2) test.yml script-injection: moved ${{ steps.test.outputs.status }} to env block as WORKFLOW_STATUS; (3) test.yml unpinned-uses: pinned actions/checkout@v2 to full SHA 0717577d45739eb3c851188b29f50ed6c0b2194e; (4) test.yml missing-permissions: added top-level 'permissions: {}' block.

