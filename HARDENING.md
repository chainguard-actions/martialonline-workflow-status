# Hardening Report: martialonline--workflow-status/v4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **martialonline--workflow-status/v4.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The `run:` block in action.yml directly interpolates `${{ github.token }}` inside the shell script string. This embeds the GitHub Actions expression value directly into the shell command rather than passing it through an `env:` variable. The correct pattern is to expose the value via `env: TOKEN: ${{ github.token }}` and then reference `$TOKEN` in the script. While `github.token` is a system-controlled value (not directly attacker-supplied), direct expression interpolation in `run:` blocks is flagged as script-injection per policy, and the pattern establishes a dangerous precedent if other `github.*` values are added similarly.

Locations:

- `action.yml:14`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Moved `${{ github.token }}` out of the `run:` block and into an `env:` block as `WORKFLOW_TOKEN: ${{ github.token }}`. Updated the shell script to reference `${WORKFLOW_TOKEN}` instead of the direct expression interpolation `${{ github.token }}`. This eliminates the script-injection pattern while preserving the same functionality.

