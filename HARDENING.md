<!-- markdownlint-disable -->

# Hardening Report: docker--login-action/v4.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **docker--login-action/v4.1.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: GitHub Actions expressions `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` are directly interpolated inside a `run:` shell command in the `registry-auth-exclusive` job's "Check" step. These values flow through YAML template substitution before the shell sees them, allowing an attacker to inject arbitrary shell commands. The offending line is: `if [ "${{ steps.login.outcome }}" != "failure" ] || [ "${{ steps.login.conclusion }}" != "success" ]; then`. These should be moved to an `env:` block and referenced as shell variables (e.g., `$OUTCOME`, `$CONCLUSION`) instead.

Locations:

- `.github/workflows/ci.yml:299`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the `registry-auth-exclusive` job's "Check" step in `.github/workflows/ci.yml`. Moved `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` expressions out of the `run:` shell command into an `env:` block as `OUTCOME` and `CONCLUSION` variables. The shell script now references these as `$OUTCOME` and `$CONCLUSION` instead of directly interpolating the GitHub Actions expressions.

