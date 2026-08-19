<!-- markdownlint-disable -->

# Hardening Report: docker--login-action/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **docker--login-action/v4.0.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation of `steps.*` context values inside a `run:` shell block. The 'Check' step in the `registry-auth-exclusive` job interpolates `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` directly into the shell command string. These are `steps.*.outputs.*`-style context values that flow through YAML template substitution before the shell sees them, enabling script injection. Fix: move the values into `env:` variables and reference those with double-quoted `"$VAR"` expansions.

Locations:

- `.github/workflows/ci.yml:233`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions and Docker images using mutable version tags instead of immutable 40-character SHA digests, making the workflows vulnerable to supply-chain attacks if the tag is moved. Failing references include: ci.yml — `actions/checkout@v6` (×20 occurrences), `aws-actions/configure-aws-credentials@v6` (×2), `docker://docker` (mutable image tag, no digest); codeql.yml — `actions/checkout@v6`, `github/codeql-action/init@v4`, `github/codeql-action/autobuild@v4`, `github/codeql-action/analyze@v4`; publish.yml — `actions/checkout@v6`, `actions/publish-immutable-action@v0.0.4`; test.yml — `actions/checkout@v6`, `docker/bake-action@v6`, `codecov/codecov-action@v5`; update-dist.yml — `actions/create-github-app-token@v2`, `actions/checkout@v6`, `docker/bake-action@v6`; validate.yml — `actions/checkout@v6`, `docker/bake-action/subaction/list-targets@v6`, `docker/bake-action@v6`. All should be pinned to full 40-hex-char commit SHAs.

Locations:

- `.github/workflows/ci.yml:22`
- `.github/workflows/codeql.yml:28`
- `.github/workflows/publish.yml:14`
- `.github/workflows/test.yml:17`
- `.github/workflows/update-dist.yml:16`
- `.github/workflows/validate.yml:21`

### missing-permissions (severity: medium)

Four workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, workflows inherit the default repository token permissions (which may be broad), violating the principle of least privilege. Affected files: ci.yml (14 jobs, none with permissions), test.yml (1 job, no permissions), update-dist.yml (1 job, no permissions), validate.yml (2 jobs, none with permissions). Each file should declare a top-level `permissions:` block with the minimal scopes required (e.g. `contents: read`).

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/update-dist.yml:1`
- `.github/workflows/validate.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings across 6 workflow files:

1. script-injection (ci.yml line 233): Moved `${{ steps.login.outcome }}` and `${{ steps.login.conclusion }}` from the `run:` shell string into the step's `env:` block as LOGIN_OUTCOME and LOGIN_CONCLUSION, referenced as "$LOGIN_OUTCOME" and "$LOGIN_CONCLUSION" in the shell.

2. unpinned-uses: Pinned all action references to full 40-char SHAs with tag comments: actions/checkout@v6→d23441a4, aws-actions/configure-aws-credentials@v6→e6de0542, github/codeql-action/*@v4→ff2f1c62, actions/publish-immutable-action@v0.0.4→4bc8754f, docker/bake-action@v6→5be5f02f, codecov/codecov-action@v5→0fb71748, actions/create-github-app-token@v2→fee1f7d6, docker/bake-action/subaction/list-targets@v6→5be5f02f. Also pinned the mutable `docker://docker` image to `docker://docker:latest@sha256:12e683a1...`.

3. missing-permissions: Added top-level `permissions: contents: read` to ci.yml, test.yml, and validate.yml. For update-dist.yml, added top-level `permissions: contents: read` with job-level `permissions: contents: write` to allow the push. codeql.yml and publish.yml already had permissions blocks.

