<!-- markdownlint-disable -->

# Hardening Report: docker--login-action/v4.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--login-action/v4.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Check' step in the `registry-auth-exclusive` job directly interpolates `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` inside a `run:` shell script. These `steps.*` context values are substituted by the GitHub Actions template engine before the shell executes the script, allowing an attacker who can influence step outputs to inject arbitrary shell commands. The offending line is: `if [ "${{ steps.login.outcome }}" != "failure" ] || [ "${{ steps.login.conclusion }}" != "success" ]; then`. These values should be passed via an `env:` variable and referenced as `"$ENV_VAR"` inside the script instead.

Locations:

- `.github/workflows/ci.yml:233`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the `registry-auth-exclusive` job's `Check` step in `.github/workflows/ci.yml`. Moved `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` out of the `run:` shell script into an `env:` block as `LOGIN_OUTCOME` and `LOGIN_CONCLUSION`. The shell script now references these as plain environment variables (`$LOGIN_OUTCOME` and `$LOGIN_CONCLUSION`) instead of directly interpolating GitHub Actions expressions.

