<!-- markdownlint-disable -->

# Hardening Report: docker--login-action/v4.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--login-action/v4.5.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: The `registry-auth-exclusive` job's "Check" step in ci.yml interpolates `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` directly inside a `run:` shell command string. Any `${{ ... }}` expression interpolated directly into a `run:` block is a script injection risk — the value is substituted by the YAML template engine before the shell ever sees it, allowing an attacker who can influence the step outcome/conclusion values to inject arbitrary shell commands. The offending line is: `if [ "${{ steps.login.outcome }}" != "failure" ] || [ "${{ steps.login.conclusion }}" != "success" ]; then`. These should be moved to `env:` variables and referenced as `"$STEP_OUTCOME"` / `"$STEP_CONCLUSION"` in the shell script.

Locations:

- `.github/workflows/ci.yml:330`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the `registry-auth-exclusive` job's "Check" step in .github/workflows/ci.yml. Moved `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` out of the `run:` shell string and into an `env:` block as `STEP_OUTCOME` and `STEP_CONCLUSION`. The shell script now references these as plain environment variables `"$STEP_OUTCOME"` and `"$STEP_CONCLUSION"` instead of interpolating the template expressions directly.

