<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v17

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **macbre--push-to-ghcr/v17** was hardened automatically. 24 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): The `run:` block in action.yml directly interpolates multiple GitHub Actions expressions inside shell commands without routing them through env vars. Attacker-controllable values include: `${{ github.actor }}` (used in `docker login ghcr.io -u "${{ github.actor }}"`), `${{ github.event_name }}` (used in an `if` comparison), `${{ github.event.release.tag_name }}` (assigned to COMMIT_TAG), `${{ github.ref }}` (echoed), `${{ github.repository }}` (embedded in a URL), `${{ inputs.dockerfile }}` (passed to `--file`), `${{ inputs.build_arg }}` (passed to `--build-arg`), `${{ inputs.extra_args }}` (expanded directly into the docker build argument array), `${{ inputs.platforms }}` (passed to `--platform`), `${{ inputs.context }}` (passed as the build context path), and `${{ inputs.repository }}` (used in image tag strings and docker push commands). Any of these values can contain shell metacharacters that will be interpreted by bash before the shell ever sees them, enabling command injection.

Locations:

- `action.yml:80`
- `action.yml:88`
- `action.yml:101`
- `action.yml:113`
- `action.yml:115`
- `action.yml:131`
- `action.yml:148`
- `action.yml:151`
- `action.yml:153`
- `action.yml:155`
- `action.yml:158`
- `action.yml:163`

### unpinned-uses (severity: high)

Two `uses:` references in action.yml use mutable version tags instead of immutable 40-character commit SHA pins, making the action vulnerable to supply-chain attacks if the upstream tag is moved or the repository is compromised: `docker/setup-qemu-action@v3` and `docker/setup-buildx-action@v3`. These should be pinned to their full SHA digests, e.g. `docker/setup-qemu-action@<40-char-sha> # v3`.

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

Fixed all security findings in hardened/action/action.yml:

1. **unpinned-uses**: Pinned `docker/setup-qemu-action@v3` to SHA `c7c53464625b32c7a7e944ae62b3e17d2b600130` and `docker/setup-buildx-action@v3` to SHA `8d2750c68a42422c14e847fe6c8ac0403b4cbd6f`.

2. **script-injection / static-inline-injection**: Moved all `${{ }}` expressions from the `run:` block into the `env:` block. Added env vars: `GITHUB_ACTOR`, `GITHUB_EVENT_NAME`, `GITHUB_REF`, `GITHUB_RELEASE_TAG_NAME`, `GITHUB_REPOSITORY`, `INPUT_DOCKERFILE`, `INPUT_BUILD_ARG`, `INPUT_EXTRA_ARGS`, `INPUT_PLATFORMS`, `INPUT_CONTEXT`, `INPUT_REPOSITORY`. The `run:` block now only uses `${VAR_NAME}` shell variable references.

3. For `inputs.extra_args` (a list-type input), used the xargs-based tokenization pattern (`while IFS= read -r -d '' t; do ... done < <(printf '%s' "$VAR" | xargs printf '%s\0')`) to properly split it into an array while preserving quoted arguments and preventing injection.

4. All `${{ }}` expressions in `if:` conditions and `env:` blocks were left as-is since those are safe (evaluated by GitHub Actions, not the shell).

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all 5 script injection locations in action.yml:
1. Replaced `for TAG in ${IMAGE_TAG_LIST}` with `IFS=',' read -ra IMAGE_TAG_LIST <<< "${IMAGE_TAG}"` and `for TAG in "${IMAGE_TAG_LIST[@]}"` — properly splits comma-separated tags into an array and iterates with quoting.
2. Added double quotes around `${IMAGE_NAME}` in the `echo` command substitution: `echo "${IMAGE_NAME}"`.
3. Added double quotes around both occurrences of `${DOCKER_IO_USER}` in `[ -z ${DOCKER_IO_USER} ]` tests: `[ -z "${DOCKER_IO_USER}" ]`.
4. Added double quotes around the `--build-arg` values for `BUILD_DATE` and `GITHUB_SHA`: `--build-arg "BUILD_DATE=${BUILD_DATE}"` and `--build-arg "GITHUB_SHA=${GITHUB_SHA}"`.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed all unquoted variable expansions in the run: shell script in action.yml:
1. `export GITHUB_URL="https://github.com/${GITHUB_REPOSITORY}"` - added quotes around value containing GITHUB_REPOSITORY
2. `export COMMIT_TAG="${COMMIT_TAG//v/}"` - added quotes around bash parameter substitution
3. `export COMMIT_TAG="${IMAGE_TAGS[0]}"` - added quotes around array element access
4. All six `--label` arguments in COMMON_ARGS array now have their values double-quoted (org.label-schema.build-date, org.label-schema.vcs-url, org.label-schema.vcs-ref, org.opencontainers.image.created, org.opencontainers.image.source, org.opencontainers.image.revision)
5. `DOCKER_IO_TAGS=(--tag "docker.io/${IMAGE_NAME}:${COMMIT_TAG}")` - added quotes around the tag value
6. `docker push "docker.io/${IMAGE_NAME}:${TAG}"` - added quotes around the image reference

All workflow-controllable values (GITHUB_REPOSITORY, IMAGE_NAME, COMMIT_TAG, GITHUB_URL, GITHUB_SHA, BUILD_DATE) are now properly double-quoted to prevent shell word-splitting and glob expansion.

