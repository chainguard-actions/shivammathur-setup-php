<!-- markdownlint-disable -->

# Hardening Report: shivammathur--setup-php/2.37.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **shivammathur--setup-php/2.37.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (b) violation: In the 'Update' step of docs.yml, the env var `WIKI_REPOSITORY` (sourced from `${{ github.repository }}`, a `github.*` context value treated as untrusted) is expanded unquoted inside the `run:` shell command: `git push -f https://x-access-token:${GITHUB_TOKEN}@github.com/${WIKI_REPOSITORY}.wiki.git master`. Routing through an `env:` variable does not sanitize the value; the shell expansion must be double-quoted (`"${WIKI_REPOSITORY}"`) to prevent shell metacharacter injection.

Locations:

- `.github/workflows/docs.yml:149`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection vulnerability in .github/workflows/docs.yml at the 'Update' step. The `${WIKI_REPOSITORY}` variable (sourced from `${{ github.repository }}`) was expanded unquoted in the `git push` URL. Changed `${WIKI_REPOSITORY}` to `"${WIKI_REPOSITORY}"` to double-quote the shell expansion, preventing shell metacharacter injection from attacker-controlled repository names.

