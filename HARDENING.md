<!-- markdownlint-disable -->

# Hardening Report: docker--login-action/v4.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--login-action/v4.1.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Check' step in the `registry-auth-exclusive` job directly interpolates `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` inside a `run:` shell command string. GitHub Actions expressions are substituted by the template engine before the shell parses the command, so any value containing shell metacharacters could alter execution. The offending line is: `if [ "${{ steps.login.outcome }}" != "failure" ] || [ "${{ steps.login.conclusion }}" != "success" ]; then`. These should be moved to an `env:` block and referenced as quoted shell variables (e.g., `env: OUTCOME: ${{ steps.login.outcome }}` then `if [ "$OUTCOME" != "failure" ]`).

Locations:

- `.github/workflows/ci.yml:248`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the 'Check' step of the 'registry-auth-exclusive' job in .github/workflows/ci.yml. Moved ${{ steps.login.outcome }} and ${{ steps.login.conclusion }} out of the run: shell command and into an env: block as OUTCOME and CONCLUSION variables. The shell script now references these as plain environment variables ($OUTCOME and $CONCLUSION) instead of directly interpolating GitHub Actions expressions.

