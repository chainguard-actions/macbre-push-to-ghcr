<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v17

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **macbre--push-to-ghcr/v17** was hardened automatically. 24 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml references two actions using mutable version tags instead of pinned SHA digests. 'docker/setup-qemu-action@v3' and 'docker/setup-buildx-action@v3' are both tag-based references that can be silently updated to point to different (potentially malicious) code. They must be pinned to full 40-character commit SHAs.

Locations:

- `action.yml:68`
- `action.yml:72`

### script-injection (severity: high)

The run: block in action.yml directly interpolates numerous ${{ ... }} expressions into shell commands (rule a), including attacker-controllable inputs and github context values. These are expanded by the Actions template engine before the shell sees them, allowing an attacker to inject arbitrary shell commands. Offending expressions include: '${{ github.actor }}' (used unquoted in docker login command), '${{ github.event_name }}' (used in if-condition string comparison), '${{ github.ref }}' (echoed into shell), '${{ github.event.release.tag_name }}' (assigned to shell variable), '${{ github.repository }}' (used in URL construction), '${{ inputs.dockerfile }}' (passed unquoted to --file flag), '${{ inputs.build_arg }}' (passed unquoted to --build-arg), '${{ inputs.extra_args }}' (expanded unquoted into argument array), '${{ inputs.platforms }}' (used in if-condition and passed unquoted to --platform), '${{ inputs.repository }}' (used unquoted in multiple docker commands), and '${{ inputs.context }}' (passed unquoted as build context path).

Locations:

- `action.yml:82`

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

Fixed all security findings in action.yml:
1. Pinned docker/setup-qemu-action@v3 to full SHA @c7c53464625b32c7a7e944ae62b3e17d2b600130 # v3
2. Pinned docker/setup-buildx-action@v3 to full SHA @8d2750c68a42422c14e847fe6c8ac0403b4cbd6f # v3
3. Moved all ${{ }} expressions out of the run: block into the env: block. Added env vars: GITHUB_ACTOR, GITHUB_EVENT_NAME, GITHUB_REF, GITHUB_RELEASE_TAG_NAME, GITHUB_REPOSITORY, INPUT_DOCKERFILE, INPUT_BUILD_ARG, INPUT_EXTRA_ARGS, INPUT_PLATFORMS, INPUT_REPOSITORY, INPUT_CONTEXT. All shell commands now reference these as plain environment variables (e.g. $GITHUB_ACTOR instead of ${{ github.actor }}). Also improved quoting throughout the shell script for correctness and safety.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed four unquoted shell variable expansions of untrusted data in action.yml:
1. Line ~133: Changed `for TAG in ${IMAGE_TAG_LIST}` to use `read -ra IMAGE_TAG_ARRAY <<< "${IMAGE_TAG_LIST}"` and `for TAG in "${IMAGE_TAG_ARRAY[@]}"` — safely splits the comma-converted space-separated tag list while preventing word-splitting/glob injection.
2. Line ~141: Changed `echo ${IMAGE_NAME}` to `echo "${IMAGE_NAME}"` inside the command substitution to properly quote the caller-controlled image name.
3. Line ~165: Changed `if [ -z ${DOCKER_IO_USER} ]` to `if [ -z "${DOCKER_IO_USER}" ]` in the multi-platform build section.
4. Line ~205: Changed `if [ -z ${DOCKER_IO_USER} ]` to `if [ -z "${DOCKER_IO_USER}" ]` in the single-platform build section.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variable expansions of workflow-controllable data in action.yml:
1. Quoted the GITHUB_URL assignment: `export GITHUB_URL="https://github.com/${GITHUB_REPOSITORY}"` (was unquoted RHS)
2. Quoted all --label array elements in COMMON_ARGS that contained variable expansions: `--label "org.label-schema.vcs-url=${GITHUB_URL}"`, `--label "org.opencontainers.image.source=${GITHUB_URL}"`, and the other label/build-arg entries for consistency.
This prevents word splitting on whitespace in attacker-controlled values like github.repository.

