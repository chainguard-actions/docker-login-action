<!-- markdownlint-disable -->

# Hardening Report: docker--login-action/v4.5.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--login-action/v4.5.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: The 'Check' step in the 'registry-auth-exclusive' job directly interpolates GitHub Actions expressions inside a run: shell command. The offending line is: `if [ "${{ steps.login.outcome }}" != "failure" ] || [ "${{ steps.login.conclusion }}" != "success" ]`. Both `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` are substituted by the Actions template engine before the shell parses the command, meaning a maliciously crafted value could inject arbitrary shell commands. These should be moved to an env: block and referenced as environment variables (e.g., `env: OUTCOME: ${{ steps.login.outcome }}` then `if [ "$OUTCOME" != "failure" ]`).

Locations:

- `.github/workflows/ci.yml:305`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the 'Check' step of the 'registry-auth-exclusive' job in .github/workflows/ci.yml. Moved ${{ steps.login.outcome }} and ${{ steps.login.conclusion }} expressions out of the run: shell command and into an env: block as LOGIN_OUTCOME and LOGIN_CONCLUSION. The shell script now references these as plain environment variables ($LOGIN_OUTCOME and $LOGIN_CONCLUSION) instead of directly interpolating GitHub Actions expressions, preventing potential shell command injection.

