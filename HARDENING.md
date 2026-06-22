<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v14

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **macbre--push-to-ghcr/v14** was hardened automatically. 13 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): Multiple ${{ }} expressions are interpolated directly inside the run: shell block in action.yml, allowing attacker-controlled values to be injected as shell code before the shell processes them. Affected expressions include: ${{ github.actor }} (lines 64, 117), ${{ github.event_name }} (line 67), ${{ github.repository }} (line 80), ${{ inputs.repository }} (lines 82, 88, 93, 107, 108, 111, 112), ${{ inputs.dockerfile }} (lines 82, 87), ${{ inputs.context }} (lines 82, 101), and ${{ inputs.build_arg }} (line 91). These should be moved to env: variables and referenced as properly quoted shell variables. Rule (b): Several env vars holding untrusted input are expanded without double-quotes: ${IMAGE_TAG} (line 70, in [ -z ${IMAGE_TAG} ]), ${IMAGE_NAME} (line 76, in echo ${IMAGE_NAME}), and ${DOCKER_IO_USER} (line 116, in [ -z ${DOCKER_IO_USER} ]), allowing shell metacharacter injection.

Locations:

- `action.yml:64`
- `action.yml:67`
- `action.yml:70`
- `action.yml:76`
- `action.yml:80`
- `action.yml:82`
- `action.yml:87`
- `action.yml:88`
- `action.yml:91`
- `action.yml:93`
- `action.yml:101`
- `action.yml:107`
- `action.yml:108`
- `action.yml:111`
- `action.yml:112`
- `action.yml:116`
- `action.yml:117`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:93`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.dockerfile }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:93`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.context }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:93`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.dockerfile }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:99`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:100`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.build_arg }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:106`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:108`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.context }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:118`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:127`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:130`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:134`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.repository }}" appears directly in run: block of step "Log in to the Container registry"; move to env: map

Locations:

- `action.yml:135`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all script injection vulnerabilities in action.yml by: (1) Moving all ${{ }} expressions (${{ github.actor }}, ${{ github.event_name }}, ${{ github.repository }}, ${{ inputs.repository }}, ${{ inputs.dockerfile }}, ${{ inputs.context }}, ${{ inputs.build_arg }}) from the run: shell block into the step's env: block as GITHUB_ACTOR, GITHUB_EVENT_NAME, GITHUB_REPOSITORY, INPUT_REPOSITORY, INPUT_DOCKERFILE, INPUT_CONTEXT, and INPUT_BUILD_ARG respectively. (2) Replacing all inline ${{ }} references in the run: block with the corresponding environment variable references. (3) Adding double-quotes around all shell variable expansions that were previously unquoted: ${IMAGE_TAG} → "${IMAGE_TAG}", ${IMAGE_NAME} → "${IMAGE_NAME}", ${DOCKER_IO_USER} → "${DOCKER_IO_USER}", and all other variable expansions used in command arguments.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed 7 unquoted shell variable expansions in action.yml's run block:
1. `export COMMIT_TAG=${GITHUB_REF:10}` → `export COMMIT_TAG="${GITHUB_REF:10}"`
2. `export COMMIT_TAG=${IMAGE_TAG}` → `export COMMIT_TAG="${IMAGE_TAG}"`
3. `--build-arg GITHUB_SHA=${GITHUB_SHA}` → `--build-arg "GITHUB_SHA=${GITHUB_SHA}"`
4. `--label org.label-schema.vcs-url=${GITHUB_URL}` → `--label "org.label-schema.vcs-url=${GITHUB_URL}"`
5. `--label org.label-schema.vcs-ref=${GITHUB_SHA}` → `--label "org.label-schema.vcs-ref=${GITHUB_SHA}"`
6. `--label org.opencontainers.image.source=${GITHUB_URL}` → `--label "org.opencontainers.image.source=${GITHUB_URL}"`
7. `--label org.opencontainers.image.revision=${GITHUB_SHA}` → `--label "org.opencontainers.image.revision=${GITHUB_SHA}"`

All workflow-controllable values are now double-quoted, preventing shell metacharacter injection.

