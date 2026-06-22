<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v18

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **macbre--push-to-ghcr/v18** was hardened automatically. 24 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The single `run:` block in action.yml (step 'Log in to the Container registry') directly interpolates numerous ${{ }} expressions into shell commands, violating rule (a). Attacker-controlled inputs and github context values are substituted by the YAML template engine before the shell ever sees them, enabling command injection. Offending lines include:
- `docker login ghcr.io -u "${{ github.actor }}"` — github.actor injected directly into shell
- `echo "Event received: '${{ github.event_name }}' (with a reference '${{ github.ref }}' / tag name '${{ github.event.release.tag_name }}')"` — multiple github context values injected
- `if [ "${{ github.event_name }}" = "release" ]` — github.event_name in shell conditional
- `export COMMIT_TAG="${{ github.event.release.tag_name }}"` — release tag injected directly
- `export GITHUB_URL=https://github.com/${{ github.repository }}` — github.repository injected
- `GHCR_TAG_ARGS+=(--tag "${{ inputs.repository }}/${IMAGE_NAME}:${TAG}")` — inputs.repository injected
- `--file ${{ inputs.dockerfile }}` — inputs.dockerfile injected into docker build args
- `--build-arg ${{ inputs.build_arg }}` — inputs.build_arg injected (can contain shell metacharacters)
- `${{ inputs.extra_args }}` — inputs.extra_args injected directly into shell array
- `if [ -n "${{ inputs.platforms }}" ]` — inputs.platforms injected into shell conditional
- `--platform ${{ inputs.platforms }}` — inputs.platforms injected into docker buildx command
- `${{ inputs.context }}` — inputs.context injected into docker build/buildx commands
All of these should be routed through env: variables and then double-quoted in the shell.

Locations:

- `action.yml:84`
- `action.yml:91`
- `action.yml:109`
- `action.yml:110`
- `action.yml:126`
- `action.yml:138`
- `action.yml:143`
- `action.yml:145`
- `action.yml:147`
- `action.yml:155`
- `action.yml:175`
- `action.yml:181`

### unpinned-uses (severity: high)

Two `uses:` references in action.yml are pinned to mutable version tags rather than immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if those tags are moved:
- `uses: docker/setup-qemu-action@v4`
- `uses: docker/setup-buildx-action@v4`
These should be pinned to full SHA digests, e.g. `uses: docker/setup-qemu-action@<40-char-sha> # v4`.

Locations:

- `action.yml:65`
- `action.yml:70`

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
1. Pinned docker/setup-qemu-action@v4 to full SHA @06116385d9baf250c9f4dcb4858b16962ea869c3 # v4
2. Pinned docker/setup-buildx-action@v4 to full SHA @d7f5e7f509e45cec5c76c4d5afdd7de93d0b3df5 # v4
3. Moved all ${{ }} expressions from the run: block into the env: block: github.actor→GITHUB_ACTOR, github.event_name→GITHUB_EVENT_NAME, github.ref→GITHUB_REF, github.event.release.tag_name→GITHUB_RELEASE_TAG_NAME, github.repository→GITHUB_REPOSITORY, inputs.repository→INPUT_REPOSITORY, inputs.dockerfile→INPUT_DOCKERFILE, inputs.build_arg→INPUT_BUILD_ARG, inputs.extra_args→INPUT_EXTRA_ARGS, inputs.platforms→INPUT_PLATFORMS, inputs.context→INPUT_CONTEXT. All shell references now use ${VAR_NAME} syntax. The inputs.extra_args value is safely expanded into an array (EXTRA_ARGS_ARRAY) to preserve argument boundaries.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all 5 unquoted shell variable expansions in action.yml: (1) Changed `for TAG in ${IMAGE_TAG_LIST}` to use `IFS=' ' read -ra TAG_ARRAY <<< "${IMAGE_TAG_LIST}"` with `for TAG in "${TAG_ARRAY[@]}"` for safe word-splitting; (2) Quoted `${IMAGE_NAME}` inside echo command substitution as `"${IMAGE_NAME}"`; (3) Changed `EXTRA_ARGS_ARRAY=(${INPUT_EXTRA_ARGS})` to `IFS=' ' read -ra EXTRA_ARGS_ARRAY <<< "${INPUT_EXTRA_ARGS}"` for safe array population; (4) & (5) Quoted both occurrences of `${DOCKER_IO_USER}` in `[ -z ]` test expressions as `"${DOCKER_IO_USER}"`.

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed 5 unquoted shell variable expansions in action.yml's run: block:
1. Line ~121: Quoted `export COMMIT_TAG="${COMMIT_TAG//v/}"` (derived from github.event.release.tag_name)
2. Line ~127: Quoted `export IMAGE_TAG_LIST="${IMAGE_TAG//,/ }"` (derived from inputs.image_tag)
3. Line ~136: Quoted `export COMMIT_TAG="${IMAGE_TAGS[0]}"` (derived from inputs.image_tag)
4. Line ~164: Quoted docker label `--label "org.label-schema.vcs-url=${GITHUB_URL}"` (GITHUB_URL built from github.repository)
5. Line ~167: Quoted docker label `--label "org.opencontainers.image.source=${GITHUB_URL}"` (same GITHUB_URL)

All github context values were already properly moved into the step's env: block; only the shell-level quoting was missing. The fixes ensure attacker-controlled values containing shell metacharacters cannot cause command injection.

