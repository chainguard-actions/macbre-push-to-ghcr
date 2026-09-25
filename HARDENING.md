<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v18

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **macbre--push-to-ghcr/v18** was hardened automatically. 24 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two `uses:` references in action.yml are pinned to mutable version tags instead of immutable 40-character SHA digests, making the action vulnerable to supply-chain attacks if the upstream tag is moved or compromised.

Failing references:
- `uses: docker/setup-qemu-action@v4` (line 66)
- `uses: docker/setup-buildx-action@v4` (line 70)

These should be replaced with full SHA-pinned references such as:
`uses: docker/setup-qemu-action@29109295f81a9208d4e8f4a3e0a5c8e5f1c5a3b # v4`

Locations:

- `action.yml:66`
- `action.yml:70`

### script-injection (severity: high)

The `run:` block in the 'Log in to the Container registry' step (action.yml) directly interpolates numerous `${{ }}` GitHub Actions expressions into shell command strings (sub-rule a). This allows an attacker who controls any of these values — via a crafted release tag, PR, or workflow_dispatch input — to inject arbitrary shell commands.

Violating expressions found directly inside the run: shell script:
- `${{ github.actor }}` — used unquoted in `docker login ghcr.io -u "${{ github.actor }}"` (line ~84)
- `${{ github.event_name }}`, `${{ github.ref }}`, `${{ github.event.release.tag_name }}` — interpolated into an echo command (line ~90)
- `${{ github.event_name }}` — used in an `if [ "${{ github.event_name }}" = "release" ]` condition (line ~103)
- `${{ github.event.release.tag_name }}` — assigned via `export COMMIT_TAG="${{ github.event.release.tag_name }}"` (line ~104)
- `${{ github.repository }}` — used in `export GITHUB_URL=https://github.com/${{ github.repository }}` (line ~120)
- `${{ inputs.repository }}` — used unquoted in tag arguments and docker commands (multiple lines ~124, ~148, ~152, ~154, ~163, ~168, ~172, ~175)
- `${{ inputs.dockerfile }}` — used unquoted as `--file ${{ inputs.dockerfile }}` (line ~129)
- `${{ inputs.build_arg }}` — used unquoted as `--build-arg ${{ inputs.build_arg }}` (line ~132)
- `${{ inputs.extra_args }}` — used unquoted directly in the args array (line ~134)
- `${{ inputs.platforms }}` — used unquoted in `--platform ${{ inputs.platforms }}` and in `if [ -n "${{ inputs.platforms }}" ]` (lines ~140, ~150)
- `${{ inputs.context }}` — used unquoted as the final positional argument to docker build/buildx (lines ~155, ~163)

All of these must be moved to `env:` variables and then referenced as properly double-quoted shell variables (e.g., `"$VAR"`) inside the run script.

Locations:

- `action.yml:84`
- `action.yml:90`
- `action.yml:103`
- `action.yml:104`
- `action.yml:120`
- `action.yml:124`
- `action.yml:129`
- `action.yml:132`
- `action.yml:134`
- `action.yml:140`
- `action.yml:150`
- `action.yml:155`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:155`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.dockerfile }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:163`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.build_arg }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:167`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.extra_args }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:170`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platforms }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:179`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:195`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platforms }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:195`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platforms }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:198`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:199`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:201`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.context }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:204`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:209`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.dockerfile }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:209`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.context }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:209`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:215`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:217`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.context }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:219`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:228`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:231`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:234`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:236`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:236`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, static-inline-injection

**Notes:**

Fixed all findings in hardened/action/action.yml:
1. Pinned docker/setup-qemu-action@v4 → @99012661954931238ded8c8b007157a8430204e1 # v4
2. Pinned docker/setup-buildx-action@v4 → @f87e5991a6d7451dcb8d9637bfbc97413f497069 # v4
3. Moved all ${{ }} expressions (github.actor, github.event_name, github.ref, github.event.release.tag_name, github.repository, inputs.repository, inputs.dockerfile, inputs.build_arg, inputs.extra_args, inputs.platforms, inputs.context) from the run: block into the env: block as named environment variables (ACTOR, EVENT_NAME, GIT_REF, RELEASE_TAG_NAME, GH_REPOSITORY, INPUT_REPOSITORY, INPUT_DOCKERFILE, INPUT_BUILD_ARG, INPUT_EXTRA_ARGS, INPUT_PLATFORMS, INPUT_CONTEXT). The run: script now references only plain shell variables. inputs.extra_args (a list-style input) is tokenized via xargs into a bash array to preserve argument boundaries.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all five unquoted variable expansions in action.yml:
1. Line 141: Replaced unquoted `for TAG in ${IMAGE_TAG_LIST}` with a bash array (`read -ra IMAGE_TAG_ARRAY <<< "${IMAGE_TAG_LIST}"`) and `for TAG in "${IMAGE_TAG_ARRAY[@]}"`.
2. Line 155: Added double-quotes around `${IMAGE_NAME}` inside the `echo` subshell: `echo "${IMAGE_NAME}"`.
3. Lines 186-187: Added double-quotes to array expansions in COMMON_ARGS: `"${GHCR_TAG_ARGS[@]}"` and `"${DOCKER_IO_TAG_ARGS[@]}"`.
4. Lines ~202 and ~218: Added double-quotes to both `[ -z ${DOCKER_IO_USER} ]` test expressions: `[ -z "${DOCKER_IO_USER}" ]`.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed all unquoted variable expansions in the COMMON_ARGS array and DOCKER_IO_TAGS construction in action.yml. The --build-arg and --label arguments now properly quote the entire KEY=VALUE string (e.g., --build-arg "GITHUB_SHA=${GITHUB_SHA}") to prevent shell metacharacter injection from workflow-controllable values like IMAGE_NAME (from inputs.image_name), GITHUB_SHA, and GITHUB_URL (derived from github.repository). The DOCKER_IO_TAGS construction was also fixed to quote the full image reference string.

