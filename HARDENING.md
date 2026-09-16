<!-- markdownlint-disable -->

# Hardening Report: shivammathur--setup-php/2.37.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **shivammathur--setup-php/2.37.0** was hardened automatically. 7 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Direct ${{ }} expression interpolation inside run: shell commands in docs.yml. Sub-rule (a) violations:
- "Create final file for Windows" step (line ~72): `Write-Output "## PHP ${{ matrix.php-versions }}`n"` — matrix context interpolated directly into shell.
- "Configure Git" step (line ~107): `git config --local user.email "${{ secrets.email }}"` and `git config --local user.name "${{ github.repository_owner }}"` — secrets/github context interpolated directly into shell.
- "Update" step (line ~122): `git push -f https://${{ github.repository_owner }}:${{ secrets.GITHUB_TOKEN }}@github.com/${{ github.repository }}.wiki.git master` — github context and secrets interpolated directly into shell command.

Locations:

- `.github/workflows/docs.yml:72`
- `.github/workflows/docs.yml:107`
- `.github/workflows/docs.yml:108`
- `.github/workflows/docs.yml:122`

### script-injection (severity: high)

Direct ${{ }} expression interpolation inside run: shell commands in php.yml. Sub-rule (a) violations:
- "Stage php-version-file" step: `echo ${{ env.default-php-version }} > php-version-file` — env context interpolated directly into shell.
- "Testing PHP version" step: `php -r "if(strpos(phpversion(), '${{ matrix.php-versions || env.default-php-version }}') === false)..."` — matrix/env context interpolated directly into shell string.

Locations:

- `.github/workflows/php.yml:52`
- `.github/workflows/php.yml:60`

### unpinned-uses (severity: high)

All uses: references in codeql.yml use mutable version tags instead of pinned 40-character SHA hashes:
- `actions/checkout@v6`
- `github/codeql-action/init@v4`
- `github/codeql-action/autobuild@v4`
- `github/codeql-action/analyze@v4`

Locations:

- `.github/workflows/codeql.yml:16`
- `.github/workflows/codeql.yml:21`
- `.github/workflows/codeql.yml:26`
- `.github/workflows/codeql.yml:30`

### unpinned-uses (severity: high)

All uses: references in docs.yml use mutable version tags instead of pinned 40-character SHA hashes:
- `shivammathur/setup-php@v2` (×2)
- `actions/upload-artifact@v7`
- `actions/checkout@v6`
- `actions/download-artifact@v8`

Locations:

- `.github/workflows/docs.yml:23`
- `.github/workflows/docs.yml:42`
- `.github/workflows/docs.yml:83`
- `.github/workflows/docs.yml:91`
- `.github/workflows/docs.yml:96`

### unpinned-uses (severity: high)

All uses: references in node.yml use mutable version tags instead of pinned 40-character SHA hashes:
- `actions/checkout@v6`
- `actions/setup-node@v6`
- `codecov/codecov-action@v5`

Locations:

- `.github/workflows/node.yml:27`
- `.github/workflows/node.yml:31`
- `.github/workflows/node.yml:47`

### unpinned-uses (severity: high)

All uses: references in php.yml use mutable version tags/branches instead of pinned 40-character SHA hashes:
- `actions/checkout@v6`
- `shivammathur/cache-extensions@develop` (branch reference)
- `actions/cache@v5`

Locations:

- `.github/workflows/php.yml:38`
- `.github/workflows/php.yml:43`
- `.github/workflows/php.yml:49`

### unpinned-uses (severity: high)

All uses: references in publish.yml use mutable version tags instead of pinned 40-character SHA hashes:
- `actions/checkout@v6` (×2)
- `actions/setup-node@v6` (×2)

Locations:

- `.github/workflows/publish.yml:16`
- `.github/workflows/publish.yml:21`
- `.github/workflows/publish.yml:28`
- `.github/workflows/publish.yml:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed all findings across 5 workflow files:

**script-injection fixes:**
- docs.yml 'Create final file for Windows': replaced `${{ matrix.php-versions }}` with `$env:version` (already in env block)
- docs.yml 'Configure Git': moved secrets.email and github.repository_owner to env block (GIT_USER_EMAIL, GIT_USER_NAME)
- docs.yml 'Update': moved github.repository_owner, secrets.GITHUB_TOKEN, github.repository to env block (REPO_OWNER, GITHUB_TOKEN_VAL, REPO)
- php.yml 'Stage php-version-file': moved env.default-php-version to env block as DEFAULT_PHP_VERSION
- php.yml 'Testing PHP version': moved matrix.php-versions||env.default-php-version to env block as MATRIX_PHP_VERSION

**unpinned-uses fixes (all pinned to full 40-char SHA):**
- actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803
- shivammathur/setup-php@v2 → @f3e473d116dcccaddc5834248c87452386958240
- actions/upload-artifact@v7 → @043fb46d1a93c77aae656e7c1c64a875d1fc6a0a
- actions/download-artifact@v8 → @3e5f45b2cfb9172054b4087a40e8e0b5a5461e7c
- github/codeql-action/init@v4 → @ff2f1c621b7f889edc0d3c761ac2e6a3f8cdb0dd
- github/codeql-action/autobuild@v4 → @ff2f1c621b7f889edc0d3c761ac2e6a3f8cdb0dd
- github/codeql-action/analyze@v4 → @ff2f1c621b7f889edc0d3c761ac2e6a3f8cdb0dd
- actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38
- codecov/codecov-action@v5 → @0fb7174895f61a3b6b78fc075e0cd60383518dac
- shivammathur/cache-extensions@develop → @de3c642a5fce0ef91581a1c9831e229f525196d6
- actions/cache@v5 → @caa296126883cff596d87d8935842f9db880ef25

