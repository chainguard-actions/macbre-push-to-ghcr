<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v17

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **macbre--push-to-ghcr/v17** was hardened automatically. 24 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The `run:` block in the composite action's single step directly interpolates numerous `${{ }}` expressions into shell commands without routing them through env vars. Attacker-controllable inputs and github context values are expanded by the YAML template engine before the shell ever sees them, enabling command injection. Offending lines include:
- `docker login ghcr.io -u "${{ github.actor }}"` (line ~90)
- `echo "Event received: '${{ github.event_name }}' ... '${{ github.ref }}' ... '${{ github.event.release.tag_name }}'"` (line ~97)
- `if [ "${{ github.event_name }}" = "release" ]` (line ~109)
- `export COMMIT_TAG="${{ github.event.release.tag_name }}"` (line ~110)
- `export GITHUB_URL=https://github.com/${{ github.repository }}` (line ~128)
- `GHCR_TAG_ARGS+=(--tag "${{ inputs.repository }}/${IMAGE_NAME}:${TAG}")` (line ~131)
- `--file ${{ inputs.dockerfile }}` (line ~136)
- `--build-arg ${{ inputs.build_arg }}` (line ~139)
- `${{ inputs.extra_args }}` (line ~142)
- `if [ -n "${{ inputs.platforms }}" ]` (line ~150)
- `export DOCKER_IO_USER="${{ github.actor }}"` (line ~156)
- `--platform ${{ inputs.platforms }}` and `--cache-from ${{ inputs.repository }}/...` and `${{ inputs.context }}` in docker buildx/build commands
All these must be moved to `env:` vars and then referenced as quoted `"$VAR"` in the shell.

Locations:

- `action.yml:90`
- `action.yml:97`
- `action.yml:109`
- `action.yml:110`
- `action.yml:128`
- `action.yml:131`
- `action.yml:136`
- `action.yml:139`
- `action.yml:142`
- `action.yml:150`
- `action.yml:156`

### unpinned-uses (severity: high)

Two `uses:` references in action.yml are pinned to mutable version tags instead of immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the upstream tag is moved or compromised:
- `uses: docker/setup-qemu-action@v3` (line 68)
- `uses: docker/setup-buildx-action@v3` (line 73)
These should be pinned to full SHA digests, e.g. `uses: docker/setup-qemu-action@<40-char-sha> # v3`.

Locations:

- `action.yml:68`
- `action.yml:73`

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

1. Pinned docker/setup-qemu-action@v3 to full SHA c7c53464625b32c7a7e944ae62b3e17d2b600130 and docker/setup-buildx-action@v3 to 8d2750c68a42422c14e847fe6c8ac0403b4cbd6f. 2. Moved all ${{ }} expressions from the run: block into the env: block: github.actor→ACTOR, github.event_name→EVENT_NAME, github.ref→GIT_REF, github.event.release.tag_name→RELEASE_TAG_NAME, github.repository→GITHUB_REPOSITORY, inputs.repository→INPUT_REPOSITORY, inputs.dockerfile→INPUT_DOCKERFILE, inputs.build_arg→INPUT_BUILD_ARG, inputs.extra_args→INPUT_EXTRA_ARGS, inputs.platforms→INPUT_PLATFORMS, inputs.context→INPUT_CONTEXT. All shell references now use quoted "${VAR_NAME}" syntax. The extra_args list input is tokenized with the xargs/while-read-NUL pattern to preserve argument boundaries and handle quoted sub-arguments correctly.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all 5 unquoted variable expansion instances in action.yml:
1. `for TAG in ${IMAGE_TAG_LIST}` → split into array with `read -ra IMAGE_TAG_ARRAY <<< "${IMAGE_TAG_LIST}"` then iterate with `for TAG in "${IMAGE_TAG_ARRAY[@]}"`  — prevents glob expansion while preserving word-splitting behavior
2. `echo ${IMAGE_NAME}` → `echo "${IMAGE_NAME}"` inside command substitution
3. `[ -z ${DOCKER_IO_USER} ]` (multi-platform section) → `[ -z "${DOCKER_IO_USER}" ]`
4. `DOCKER_IO_TAGS=(--tag docker.io/${IMAGE_NAME}:${COMMIT_TAG})` → `DOCKER_IO_TAGS=(--tag "docker.io/${IMAGE_NAME}:${COMMIT_TAG}")`
5. `[ -z ${DOCKER_IO_USER} ]` (single-platform section) → `[ -z "${DOCKER_IO_USER}" ]`

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection vulnerability in the COMMON_ARGS array in action.yml. All unquoted variable expansions were double-quoted: --build-arg BUILD_DATE=${BUILD_DATE} → --build-arg "BUILD_DATE=${BUILD_DATE}", --build-arg GITHUB_SHA=${GITHUB_SHA} → --build-arg "GITHUB_SHA=${GITHUB_SHA}", and all six --label arguments that used ${GITHUB_URL}, ${GITHUB_SHA}, and ${BUILD_DATE} unquoted. The critical fix is for GITHUB_URL (derived from GITHUB_REPOSITORY / ${{ github.repository }}), which is attacker-controlled and could contain shell metacharacters causing command injection when expanded unquoted into the docker build/buildx invocation.

