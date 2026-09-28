<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v17

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **macbre--push-to-ghcr/v17** was hardened automatically. 24 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The single `run:` block in action.yml directly interpolates numerous `${{ }}` expressions into shell commands without routing them through env vars with proper quoting. This allows an attacker-controlled value to break out of the intended shell context.

Violations include (sub-rule a — direct expression interpolation):
- `echo "${GITHUB_TOKEN}" | docker login ghcr.io -u "${{ github.actor }}" --password-stdin` — github.actor injected directly into shell
- `echo "Event received: '${{ github.event_name }}' (with a reference '${{ github.ref }}' / tag name '${{ github.event.release.tag_name }}')"` — multiple github context values injected
- `if [ "${{ github.event_name }}" = "release" ]` — github.event_name injected into a conditional
- `export COMMIT_TAG="${{ github.event.release.tag_name }}"` — release tag injected directly
- `export GITHUB_URL=https://github.com/${{ github.repository }}` — repository injected into URL
- `--file ${{ inputs.dockerfile }}` — user-controlled path injected into docker build args
- `--build-arg ${{ inputs.build_arg }}` — user-controlled build arg injected
- `${{ inputs.extra_args }}` — arbitrary extra args injected into docker build
- `--platform ${{ inputs.platforms }}` — platforms injected
- `--cache-from ${{ inputs.repository }}/${IMAGE_NAME}:latest` — repository injected
- `--tag ${{ inputs.repository }}/${IMAGE_NAME}:${COMMIT_TAG}` — repository injected (multiple occurrences)
- `${{ inputs.context }}` — build context path injected (multiple occurrences)
- `if [ -n "${{ inputs.platforms }}" ]` — platforms injected into conditional
- `export DOCKER_IO_USER="${{ github.actor }}"` — github.actor injected
- `docker image inspect ${{ inputs.repository }}/${IMAGE_NAME}:${COMMIT_TAG}` — repository injected
- `docker push ${{ inputs.repository }}/${IMAGE_NAME}:${TAG}` — repository injected (multiple occurrences)

All of these `${{ }}` expressions are expanded by the Actions template engine before the shell ever sees the string, enabling shell metacharacter injection.

Locations:

- `action.yml:76`
- `action.yml:83`
- `action.yml:97`
- `action.yml:98`
- `action.yml:112`
- `action.yml:120`
- `action.yml:130`
- `action.yml:132`
- `action.yml:140`
- `action.yml:148`
- `action.yml:155`
- `action.yml:163`
- `action.yml:168`
- `action.yml:172`
- `action.yml:175`
- `action.yml:178`
- `action.yml:185`
- `action.yml:192`
- `action.yml:200`
- `action.yml:210`
- `action.yml:218`
- `action.yml:222`
- `action.yml:228`
- `action.yml:232`
- `action.yml:237`
- `action.yml:244`
- `action.yml:248`
- `action.yml:255`
- `action.yml:261`

### unpinned-uses (severity: high)

Two `uses:` references in action.yml use mutable version tags instead of pinned 40-character SHA digests. This means the action could silently pull in a different (potentially malicious) version of these dependencies if the tag is moved.

Failing references:
- `uses: docker/setup-qemu-action@v3` (mutable tag `v3`)
- `uses: docker/setup-buildx-action@v3` (mutable tag `v3`)

These should be pinned to their full commit SHA, e.g.:
- `uses: docker/setup-qemu-action@29109295f81e9208d7d86ff9c098d4ee7ab27b57 # v3`
- `uses: docker/setup-buildx-action@b5ca514318bd6ebac0fb2aedd5d36ec1b5c232a2 # v3`

Locations:

- `action.yml:62`
- `action.yml:66`

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

1. Pinned docker/setup-qemu-action@v3 to @c7c53464625b32c7a7e944ae62b3e17d2b600130 and docker/setup-buildx-action@v3 to @8d2750c68a42422c14e847fe6c8ac0403b4cbd6f.
2. Moved all ${{ }} expressions from the run: block into the step's env: block: github.actor→GITHUB_ACTOR, github.event_name→GITHUB_EVENT_NAME, github.ref→GITHUB_REF_VALUE, github.event.release.tag_name→RELEASE_TAG_NAME, github.repository→GITHUB_REPOSITORY, inputs.dockerfile→INPUT_DOCKERFILE, inputs.build_arg→INPUT_BUILD_ARG, inputs.extra_args→INPUT_EXTRA_ARGS, inputs.platforms→INPUT_PLATFORMS, inputs.repository→INPUT_REPOSITORY, inputs.context→INPUT_CONTEXT.
3. The extra_args list input is properly tokenized via xargs into a bash array (EXTRA_ARGS_ARRAY) with the guarded while/read/printf pattern to handle quoted arguments correctly.
4. All other inputs (single values/paths) are referenced as double-quoted shell variables in the run: block.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed 6 unquoted variable expansion violations in action.yml:
1. `for TAG in ${IMAGE_TAG_LIST}` → converted IMAGE_TAG_LIST to array with `read -ra IMAGE_TAG_ARRAY <<< "${IMAGE_TAG_LIST}"` and iterated with `for TAG in "${IMAGE_TAG_ARRAY[@]}"`
2. `echo ${IMAGE_NAME}` inside subshell → `echo "${IMAGE_NAME}"`
3. First `if [ -z ${DOCKER_IO_USER} ]` (multi-platform section) → `if [ -z "${DOCKER_IO_USER}" ]`
4. `DOCKER_IO_TAGS=(--tag docker.io/${IMAGE_NAME}:${COMMIT_TAG})` → `DOCKER_IO_TAGS=(--tag "docker.io/${IMAGE_NAME}:${COMMIT_TAG}")`
5. `--tag docker.io/${IMAGE_NAME}:${COMMIT_TAG}` in docker build → `--tag "docker.io/${IMAGE_NAME}:${COMMIT_TAG}"`
6. Second `if [ -z ${DOCKER_IO_USER} ]` (single-platform section) → `if [ -z "${DOCKER_IO_USER}" ]`

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed all unquoted shell variable expansions in action.yml:
1. Quoted GITHUB_URL assignment: `export GITHUB_URL="https://github.com/${GITHUB_REPOSITORY}"`
2. Quoted build-arg values: `--build-arg "BUILD_DATE=${BUILD_DATE}"` and `--build-arg "GITHUB_SHA=${GITHUB_SHA}"`
3. Quoted array expansions in COMMON_ARGS: `"${GHCR_TAG_ARGS[@]}"` and `"${DOCKER_IO_TAG_ARGS[@]}"`
4. Quoted all 6 --label values containing variables (BUILD_DATE, GITHUB_URL, GITHUB_SHA)
5. Quoted docker push command: `docker push "docker.io/${IMAGE_NAME}:${TAG}"`

