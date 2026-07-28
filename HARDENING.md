<!-- markdownlint-disable -->

# Hardening Report: docker--login-action/v4.5.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--login-action/v4.5.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: The 'Check' step in the `registry-auth-exclusive` job directly interpolates GitHub Actions expressions `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` inside a `run:` shell command string. Any `${{ ... }}` expression inside a `run:` block is a script-injection risk because the value is substituted by the YAML template engine before the shell ever sees it, allowing special characters to be interpreted by the shell. The offending line is: `if [ "${{ steps.login.outcome }}" != "failure" ] || [ "${{ steps.login.conclusion }}" != "success" ]; then`. These should be moved to an `env:` block and referenced as quoted shell variables: `env: OUTCOME: ${{ steps.login.outcome }}` and then `if [ "$OUTCOME" != "failure" ]`.

Locations:

- `.github/workflows/ci.yml:330`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the 'Check' step of the `registry-auth-exclusive` job in `.github/workflows/ci.yml`. Moved `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` expressions from the `run:` shell command into an `env:` block as `LOGIN_OUTCOME` and `LOGIN_CONCLUSION` respectively. The shell script now references these as plain environment variables (`$LOGIN_OUTCOME` and `$LOGIN_CONCLUSION`), eliminating the risk of shell injection via template substitution.

