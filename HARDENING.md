<!-- markdownlint-disable -->

# Hardening Report: docker--login-action/v4.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--login-action/v4.6.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: In the `registry-auth-exclusive` job's "Check" step, the `run:` block directly interpolates `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` inside a shell `if` command. These expressions are substituted by the GitHub Actions template engine before the shell ever sees the command, allowing an attacker who can influence step outcomes (or a malicious action) to inject arbitrary shell metacharacters. The offending line is: `if [ "${{ steps.login.outcome }}" != "failure" ] || [ "${{ steps.login.conclusion }}" != "success" ]; then`. These values should be passed via environment variables and referenced as `"$STEP_OUTCOME"` / `"$STEP_CONCLUSION"` instead.

Locations:

- `.github/workflows/ci.yml:364`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the `registry-auth-exclusive` job's "Check" step in `.github/workflows/ci.yml`. Moved `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` out of the `run:` shell block and into an `env:` block as `STEP_OUTCOME` and `STEP_CONCLUSION` respectively. The shell `if` statement now references these as plain environment variables (`"$STEP_OUTCOME"` and `"$STEP_CONCLUSION"`), preventing template engine substitution from injecting shell metacharacters.

