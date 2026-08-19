<!-- markdownlint-disable -->

# Hardening Report: martialonline--workflow-status/v2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **martialonline--workflow-status/v2** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses actions/checkout@v2, which is pinned to a mutable tag rather than a full 40-character commit SHA. This means the referenced action could be silently replaced with a different (potentially malicious) version without any change to the workflow file.

Locations:

- `.github/workflows/test.yml:24`

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression is interpolated directly inside a run: shell command. The step 'Get Test Output' uses `${{ steps.test.outputs.status }}` directly in the run: string. If the action output contains shell metacharacters, they will be interpreted by the shell before quoting can protect them. The offending line is: `run: echo "Workflow Status '${{ steps.test.outputs.status }}'"`. The value should be passed via an env: variable and then referenced as a quoted shell variable (e.g., `"$STATUS"`).

Locations:

- `.github/workflows/test.yml:29`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the single job ('test') also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). A minimal permissions block such as `permissions: read-all` or specific scopes should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings in hardened/action/.github/workflows/test.yml: (1) Pinned actions/checkout@v2 to full commit SHA 0717577d45739eb3c851188b29f50ed6c0b2194e with a # v2 comment for readability. (2) Moved ${{ steps.test.outputs.status }} out of the run: shell string into an env: block as STATUS, then referenced it as $STATUS in the shell command to prevent script injection. (3) Added a top-level permissions: block with contents: read, which is the minimum permission needed for the checkout step.

