<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v16

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **macbre--push-to-ghcr/v16** was hardened automatically. 16 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The run: block in action.yml directly interpolates multiple ${{ inputs.* }} and ${{ github.* }} expressions inside shell commands (sub-rule a). This is a composite action, so any calling workflow can supply attacker-controlled values that are substituted directly into the shell before execution, enabling command injection.

Offending lines include:
- `echo "${GITHUB_TOKEN}" | docker login ghcr.io -u "${{ github.actor }}" --password-stdin`
- `echo "Event received: '${{ github.event_name }}' (with a reference '${{ github.ref }}' / tag name '${{ github.event.release.tag_name }}')"`
- `if [ "${{ github.event_name }}" = "release" ]; then`
- `export COMMIT_TAG="${{ github.event.release.tag_name }}"`
- `export GITHUB_URL=https://github.com/${{ github.repository }}`
- `--file ${{ inputs.dockerfile }}`
- `--cache-from ${{ inputs.repository }}/${IMAGE_NAME}:latest`
- `--build-arg ${{ inputs.build_arg }}`
- `--tag ${{ inputs.repository }}/${IMAGE_NAME}:${COMMIT_TAG}`
- `${{ inputs.extra_args }}`
- `${{ inputs.context }}`
- `docker image inspect ${{ inputs.repository }}/${IMAGE_NAME}:${COMMIT_TAG}`
- `docker push ${{ inputs.repository }}/${IMAGE_NAME}:${COMMIT_TAG}`
- `export DOCKER_IO_USER="${{ github.actor }}"`

All these values should be passed via env: variables and then referenced as quoted shell variables (e.g., "$VAR") instead of being interpolated directly.

Locations:

- `action.yml:68`
- `action.yml:74`
- `action.yml:77`
- `action.yml:78`
- `action.yml:87`
- `action.yml:89`
- `action.yml:95`
- `action.yml:96`
- `action.yml:99`
- `action.yml:101`
- `action.yml:103`
- `action.yml:110`
- `action.yml:120`
- `action.yml:123`
- `action.yml:126`
- `action.yml:136`

### broad-permissions (severity: medium)

The workflow file .github/workflows/ci.yml sets `permissions: write-all` at the top level, granting all jobs overly broad write access to every GitHub API scope. This should be replaced with specific minimal permissions (e.g., `packages: write`, `contents: read`) required by each job.

Locations:

- `.github/workflows/ci.yml:10`

### unpinned-uses (severity: high)

All four jobs in .github/workflows/ci.yml use `actions/checkout@v4`, which is a mutable tag reference rather than a pinned full-length commit SHA. If the tag is moved (e.g., by a supply-chain compromise), the workflow will silently execute different code. Each reference should be pinned to a full 40-character commit SHA, e.g., `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:40`
- `.github/workflows/ci.yml:60`
- `.github/workflows/ci.yml:80`

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

**Fixes applied:** script-injection, static-inline-injection, broad-permissions, unpinned-uses

**Notes:**

Fixed action.yml: moved all ${{ github.* }} and ${{ inputs.* }} expressions from run: blocks into the step's env: block. Added env vars: GITHUB_ACTOR, GITHUB_EVENT_NAME, GITHUB_REF, GITHUB_RELEASE_TAG_NAME, GITHUB_REPOSITORY, INPUT_DOCKERFILE, INPUT_REPOSITORY, INPUT_CACHE_FROM_REPO, INPUT_BUILD_ARG, INPUT_EXTRA_ARGS, INPUT_CONTEXT. The extra_args input is tokenized via xargs into a bash array to handle quoted sub-commands safely. Fixed ci.yml: replaced 'permissions: write-all' with 'contents: read' and 'packages: write'; pinned all four actions/checkout@v4 references to SHA 11d5960a326750d5838078e36cf38b85af677262 # v4.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all unquoted shell variable expansions in both action.yml and .github/workflows/ci.yml:

**action.yml fixes:**
- Quoted `${IMAGE_TAG}` in `if [ -z ... ]` test and assignment
- Quoted `${IMAGE_NAME}` in `echo ... | tr` pipeline
- Quoted `docker.io/${IMAGE_NAME}:${COMMIT_TAG}` in `--tag` argument to docker build
- Quoted `${DOCKER_IO_USER}` in `if [ -z ... ]` test
- Quoted `docker.io/${IMAGE_NAME}:${COMMIT_TAG}` in `docker push` command

**ci.yml fixes:**
- Quoted all `${IMAGE}` expansions across all four jobs (ghcr, ghcr_with_extra_args, ghcr_with_dockerfile_in_subdir, ghcr_and_docker_io) in grep, docker rmi, docker pull, and docker image inspect commands

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variable expansions in the docker build command in action.yml. All --build-arg and --label arguments that used unquoted ${VAR} expansions have been wrapped in double quotes: --build-arg "BUILD_DATE=${BUILD_DATE}", --build-arg "GITHUB_SHA=${GITHUB_SHA}", --label "org.label-schema.build-date=${BUILD_DATE}", --label "org.label-schema.vcs-url=${GITHUB_URL}", --label "org.label-schema.vcs-ref=${GITHUB_SHA}", --label "org.opencontainers.image.created=${BUILD_DATE}", --label "org.opencontainers.image.source=${GITHUB_URL}", and --label "org.opencontainers.image.revision=${GITHUB_SHA}". The highest-risk fix was quoting ${GITHUB_URL} since it is constructed from GITHUB_REPOSITORY (a workflow-controllable value via github.repository), which could contain shell metacharacters that would be interpreted by the shell.

