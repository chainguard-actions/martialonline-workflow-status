# Hardening Report: martialonline--workflow-status/v4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

Action **martialonline--workflow-status/v4.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

In action.yml, the `run:` block directly interpolates `${{ github.token }}` into the shell script string (assigned to the shell variable `token`). Per security policy, all `github.*` expressions must be passed via an `env:` block and referenced as shell environment variables (e.g., `TOKEN: ${{ github.token }}` in `env:`, then `$TOKEN` in the script) rather than being interpolated directly into the `run:` string. Direct interpolation allows the expression value to be interpreted as shell code if it contains special characters.

Locations:

- `action.yml:17`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in action.yml: moved `${{ github.token }}` out of the `run:` shell string and into an `env:` block as `GITHUB_TOKEN: ${{ github.token }}`. Updated all references in the shell script from the local `token` variable to the environment variable `${GITHUB_TOKEN}`. The intermediate `token` shell variable assignment was also removed as it is no longer needed.

