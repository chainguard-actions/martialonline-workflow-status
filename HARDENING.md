<!-- markdownlint-disable -->

# Hardening Report: martialonline--workflow-status/v4.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **martialonline--workflow-status/v4.2** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell block. On line 17 of action.yml, `token="${{ github.token }}"` embeds the GitHub Actions expression directly into the shell script string. Any ${{ ... }} inside a run: block is a script-injection risk because the value is substituted into the shell command before the shell parses it. The token should instead be passed via an env: block and referenced as $TOKEN (already double-quoted).

Locations:

- `action.yml:17`

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell block in the workflow. On line 33 of test.yml, `echo "Workflow Status '${{ steps.test.outputs.status }}'"` embeds a steps output expression directly into the shell command string. steps.*.outputs.* is an untrusted-input source; it must be passed via an env: block and the shell variable must be double-quoted.

Locations:

- `.github/workflows/test.yml:33`

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v2` (a mutable version tag) instead of a pinned full 40-character commit SHA. A tag can be moved to point to a different, potentially malicious commit. It should be pinned to a specific SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`.

Locations:

- `.github/workflows/test.yml:27`

### missing-permissions (severity: medium)

The workflow file test.yml has no top-level `permissions:` key and the single job `test` also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions (which may be write-all), violating the principle of least privilege. A `permissions:` block with minimal required scopes should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings: (1) action.yml line 17: moved github.token expression into an env: block (TOKEN) and updated shell references from ${token} to ${TOKEN}; (2) test.yml line 33: moved steps.test.outputs.status into an env: block (WORKFLOW_STATUS) and updated the echo command to use ${WORKFLOW_STATUS}; (3) test.yml line 27: pinned actions/checkout@v2 to full SHA 0717577d45739eb3c851188b29f50ed6c0b2194e; (4) test.yml line 1: added top-level permissions block with actions: read and contents: read (minimum needed for checkout and reading workflow run jobs).

