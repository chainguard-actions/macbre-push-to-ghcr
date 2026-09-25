<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v17

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **macbre--push-to-ghcr/v17** was hardened automatically. 24 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The composite action's single `run:` block directly interpolates numerous `${{ }}` expressions into shell commands (sub-rule a). Attacker-controllable values include: `${{ github.actor }}` used as a docker login username; `${{ github.event_name }}`, `${{ github.ref }}`, `${{ github.event.release.tag_name }}` used in conditionals and variable assignments; `${{ github.repository }}` embedded in a URL; and user-supplied inputs `${{ inputs.dockerfile }}`, `${{ inputs.build_arg }}`, `${{ inputs.extra_args }}`, `${{ inputs.platforms }}`, `${{ inputs.context }}`, `${{ inputs.repository }}` passed directly as shell arguments. Any of these can contain shell metacharacters (`;`, `|`, `&`, `$(...)`, etc.) that will be interpreted by the shell before quoting takes effect, enabling command injection. For example: `--file ${{ inputs.dockerfile }}`, `--build-arg ${{ inputs.build_arg }}`, `${{ inputs.extra_args }}`, `--platform ${{ inputs.platforms }}`, and `${{ inputs.context }}` are all unquoted and directly expanded into shell.

Locations:

- `action.yml:75`
- `action.yml:83`
- `action.yml:84`
- `action.yml:85`
- `action.yml:97`
- `action.yml:103`
- `action.yml:104`
- `action.yml:121`
- `action.yml:148`
- `action.yml:155`
- `action.yml:157`
- `action.yml:159`
- `action.yml:161`
- `action.yml:163`

### unpinned-uses (severity: high)

Two `uses:` references in action.yml use mutable version tags instead of pinned 40-character SHA digests, making the action vulnerable to supply-chain attacks if those tags are moved: `docker/setup-qemu-action@v3` and `docker/setup-buildx-action@v3`.

Locations:

- `action.yml:63`
- `action.yml:68`

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

Fixed all findings in action.yml:
1. Pinned docker/setup-qemu-action@v3 to @c7c53464625b32c7a7e944ae62b3e17d2b600130 # v3
2. Pinned docker/setup-buildx-action@v3 to @8d2750c68a42422c14e847fe6c8ac0403b4cbd6f # v3
3. Moved all ${{ }} expressions out of the run: block into the env: block: inputs.dockerfile→INPUT_DOCKERFILE, inputs.build_arg→INPUT_BUILD_ARG, inputs.extra_args→INPUT_EXTRA_ARGS, inputs.platforms→INPUT_PLATFORMS, inputs.context→INPUT_CONTEXT, inputs.repository→INPUT_REPOSITORY, github.event.release.tag_name→EVENT_RELEASE_TAG_NAME
4. Replaced inline github.actor/event_name/ref/repository with standard GitHub env vars (GITHUB_ACTOR, GITHUB_EVENT_NAME, GITHUB_REF, GITHUB_REPOSITORY)
5. inputs.extra_args (a list/args input) is tokenized using the xargs+read loop pattern into EXTRA_ARGS_ARR to preserve argument boundaries
6. All other input variables are double-quoted in the shell script to prevent word-splitting

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all 6 unquoted shell variable expansion locations in action.yml:
1. Replaced `for TAG in ${IMAGE_TAG_LIST}` with a safe xargs-based tokenization loop (`while IFS= read -r -d '' TAG; done < <(printf '%s' "${IMAGE_TAG_LIST}" | xargs printf '%s\0')`) to properly handle the space-separated tag list without unquoted expansion.
2. Quoted `${IMAGE_NAME}` inside the echo command substitution: `echo "${IMAGE_NAME}"`.
3. Quoted `${DOCKER_IO_USER}` in both `[ -z ]` tests: `[ -z "${DOCKER_IO_USER}" ]`.
4. Quoted the array element in DOCKER_IO_TAGS: `(--tag "docker.io/${IMAGE_NAME}:${COMMIT_TAG}")`.

