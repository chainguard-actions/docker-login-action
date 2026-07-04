<!-- markdownlint-disable -->

# Hardening Report: docker--login-action--/v4.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **docker--login-action--/v4.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Check' step in the 'registry-auth-exclusive' job directly interpolates GitHub Actions expressions inside a run: shell command string. Specifically, `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` are embedded directly in the shell script: `if [ "${{ steps.login.outcome }}" != "failure" ] || [ "${{ steps.login.conclusion }}" != "success" ]`. Any ${{ ... }} expression inside a run: block is a script-injection risk because the value is substituted into the shell command string before the shell parses it. These values should be passed via env: variables and referenced as quoted shell variables instead.

Locations:

- `.github/workflows/ci.yml:271`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the 'Check' step of the 'registry-auth-exclusive' job in .github/workflows/ci.yml. Moved ${{ steps.login.outcome }} and ${{ steps.login.conclusion }} expressions out of the run: shell command and into an env: block as LOGIN_OUTCOME and LOGIN_CONCLUSION variables. The shell script now references these as plain quoted environment variables ($LOGIN_OUTCOME and $LOGIN_CONCLUSION) instead of directly interpolating GitHub Actions expressions.

