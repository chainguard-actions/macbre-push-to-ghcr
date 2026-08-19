<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v18

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **macbre--push-to-ghcr/v18** was hardened automatically. 24 finding(s) were identified and resolved across 4 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Log in to the Container registry' run: block in action.yml directly interpolates numerous ${{ }} expressions into shell commands (sub-rule a), bypassing shell quoting and enabling command injection. Affected expressions include: `${{ github.actor }}` used unquoted in `docker login ghcr.io -u "${{ github.actor }}"`, `${{ github.event_name }}` in an `if [ "${{ github.event_name }}" = "release" ]` test, `${{ github.event.release.tag_name }}` assigned to a shell variable, `${{ github.ref }}` and `${{ github.repository }}` interpolated into echo/URL strings, and multiple `inputs.*` expressions (`${{ inputs.dockerfile }}`, `${{ inputs.build_arg }}`, `${{ inputs.extra_args }}`, `${{ inputs.platforms }}`, `${{ inputs.repository }}`, `${{ inputs.context }}`) interpolated directly into docker build command arguments without quoting. Any of these values can be attacker-controlled (e.g. via workflow_dispatch or a calling workflow), allowing shell metacharacter injection.

Locations:

- `action.yml:83`
- `action.yml:89`
- `action.yml:100`
- `action.yml:101`
- `action.yml:115`
- `action.yml:119`
- `action.yml:126`
- `action.yml:129`
- `action.yml:131`
- `action.yml:136`
- `action.yml:142`
- `action.yml:148`
- `action.yml:151`
- `action.yml:152`
- `action.yml:154`
- `action.yml:157`
- `action.yml:162`
- `action.yml:175`
- `action.yml:185`
- `action.yml:186`
- `action.yml:192`
- `action.yml:193`
- `action.yml:197`
- `action.yml:200`
- `action.yml:201`

### unpinned-uses (severity: high)

Two composite action steps use mutable version tags instead of pinned full-length SHA digests, making the action vulnerable to supply-chain attacks if the upstream tags are moved or compromised. Failing references: `docker/setup-qemu-action@v4` and `docker/setup-buildx-action@v4`. These should be pinned to their full 40-character commit SHAs (e.g. `docker/setup-qemu-action@<sha> # v4`).

Locations:

- `action.yml:65`
- `action.yml:69`

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

1. Pinned docker/setup-qemu-action@v4 to @96fe6ef7f33517b61c61be40b68a1882f3264fb8 # v4 and docker/setup-buildx-action@v4 to @bb05f3f5519dd87d3ba754cc423b652a5edd6d2c # v4.
2. Moved all ${{ }} expressions out of the run: block into the env: block: github.actor→ACTOR, github.event_name→EVENT_NAME, github.ref→GIT_REF, github.event.release.tag_name→RELEASE_TAG_NAME, github.repository→GH_REPOSITORY, inputs.repository→INPUT_REPOSITORY, inputs.dockerfile→INPUT_DOCKERFILE, inputs.build_arg→INPUT_BUILD_ARG, inputs.extra_args→INPUT_EXTRA_ARGS, inputs.platforms→INPUT_PLATFORMS, inputs.context→INPUT_CONTEXT.
3. All shell references in the run: block now use properly double-quoted ${VAR} environment variable references, preventing shell metacharacter injection.
4. The inputs.extra_args value is safely split using read -ra to handle word splitting without injection risk.

### Iteration 2

**Fixes applied:** broad-permissions, unpinned-uses, script-injection, github-env-injection

**Notes:**

Fixed all 5 findings:
1. broad-permissions: Replaced `permissions: write-all` with `permissions: {contents: read, packages: write}` in ci.yml
2. unpinned-uses: Pinned all 8 occurrences of `actions/checkout@v7` to SHA `3d3c42e5aac5ba805825da76410c181273ba90b1 # v7` in ci.yml
3. script-injection (ci.yml line 121): Moved `${{ github.repository_owner }}` into `env: REPO_OWNER:` block and referenced as `${REPO_OWNER}` in shell
4. github-env-injection (ci.yml line 130): Added `printf '%s' ... | tr -d '\n\r'` sanitization before writing to GITHUB_ENV
5. script-injection (action.yml lines 136,143,147,172,215): Fixed unquoted expansions: used `read -ra` array for IMAGE_TAG_LIST iteration, quoted IMAGE_NAME in command substitution, quoted GH_REPOSITORY in URL, and quoted DOCKER_IO_USER in both test expressions

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in action.yml by double-quoting all unquoted variable expansions in the COMMON_ARGS array. Specifically: --build-arg BUILD_DATE=${BUILD_DATE} → --build-arg "BUILD_DATE=${BUILD_DATE}", --build-arg GITHUB_SHA=${GITHUB_SHA} → --build-arg "GITHUB_SHA=${GITHUB_SHA}", and all five --label arguments (org.label-schema.build-date, org.label-schema.vcs-url, org.label-schema.vcs-ref, org.opencontainers.image.created, org.opencontainers.image.source, org.opencontainers.image.revision) were similarly double-quoted. This ensures that attacker-controlled values containing shell metacharacters cannot break out of the argument context.

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed all unquoted ${IMAGE} shell variable expansions in .github/workflows/ci.yml. The finding specifically called out lines 33, 40, 41, and 42, but there were many more instances throughout the file across 8 jobs. All occurrences of bare ${IMAGE} (and compound forms like ghcr.io/${IMAGE}:tag, docker.io/${IMAGE}:tag) have been wrapped in double quotes to prevent shell metacharacter injection. The file was rewritten in full to ensure comprehensive coverage.

