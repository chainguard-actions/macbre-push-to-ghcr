<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v16

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **macbre--push-to-ghcr/v16** was hardened automatically. 14 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The single composite step in action.yml directly interpolates numerous ${{ }} expressions inside the run: shell block (rule a). This means GitHub Actions performs YAML template substitution before the shell ever sees the string, allowing an attacker to inject arbitrary shell commands via attacker-controlled values. Offending expressions include: `${{ github.actor }}` (line 68, 128), `${{ github.event_name }}` (lines 74, 76), `${{ github.ref }}` (line 74), `${{ github.event.release.tag_name }}` (lines 74, 77), `${{ github.repository }}` (line 86), `${{ inputs.repository }}` (lines 88, 93, 94, 100, 115, 118, 121), `${{ inputs.dockerfile }}` (lines 88, 93), `${{ inputs.context }}` (lines 88, 109), `${{ inputs.build_arg }}` (line 98), `${{ inputs.extra_args }}` (line 102). All of these should be moved to env: variables and referenced as quoted shell variables (e.g. "$VAR") instead.

Locations:

- `action.yml:68`
- `action.yml:74`
- `action.yml:76`
- `action.yml:77`
- `action.yml:86`
- `action.yml:88`
- `action.yml:93`
- `action.yml:94`
- `action.yml:98`
- `action.yml:100`
- `action.yml:102`
- `action.yml:109`
- `action.yml:115`
- `action.yml:118`
- `action.yml:121`
- `action.yml:128`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:102`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.dockerfile }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:102`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.context }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:102`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.dockerfile }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:108`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:109`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.build_arg }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:115`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:117`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.extra_args }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:119`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.context }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:128`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:137`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:140`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:144`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:145`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved all ${{ }} expressions from the run: shell block into the step's env: block. Added new env vars: GITHUB_ACTOR, GITHUB_EVENT_NAME, GITHUB_REF, GITHUB_RELEASE_TAG_NAME, GITHUB_REPOSITORY, INPUT_REPOSITORY, INPUT_DOCKERFILE, INPUT_CONTEXT, INPUT_BUILD_ARG, INPUT_EXTRA_ARGS. All references in the run: block now use plain shell variable syntax (e.g. "$GITHUB_ACTOR", "$INPUT_REPOSITORY"). The optional INPUT_EXTRA_ARGS uses ${INPUT_EXTRA_ARGS:+"$INPUT_EXTRA_ARGS"} to avoid passing an empty argument when the value is unset/empty.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all unquoted shell variable expansions in action.yml that could allow shell metacharacter injection: (1) `${COMMIT_TAG//v/}` → `"${COMMIT_TAG//v/}"` (line ~98), (2) `[ -z ${IMAGE_TAG} ]` → `[ -z "${IMAGE_TAG}" ]` (line ~100), (3) `${IMAGE_TAG}` → `"${IMAGE_TAG}"` (line ~103), (4) `echo ${IMAGE_NAME}` → `echo "${IMAGE_NAME}"` (line ~108), (5) `[ -z ${DOCKER_IO_USER} ]` → `[ -z "${DOCKER_IO_USER}" ]` (line ~148). All variables derived from untrusted inputs (inputs.* and github.*) are now properly double-quoted to prevent shell metacharacter injection.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in action.yml (line 136) by double-quoting all unquoted shell expansions in the `docker build` command. Specifically, wrapped the following arguments in double quotes: `--build-arg "BUILD_DATE=${BUILD_DATE}"`, `--build-arg "GITHUB_SHA=${GITHUB_SHA}"`, `--label "org.label-schema.build-date=${BUILD_DATE}"`, `--label "org.label-schema.vcs-url=${GITHUB_URL}"`, `--label "org.label-schema.vcs-ref=${GITHUB_SHA}"`, `--label "org.opencontainers.image.created=${BUILD_DATE}"`, `--label "org.opencontainers.image.source=${GITHUB_URL}"`, and `--label "org.opencontainers.image.revision=${GITHUB_SHA}"`. This prevents shell metacharacters in these values from being interpreted by the shell.

