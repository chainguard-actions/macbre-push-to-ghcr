<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v17

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **macbre--push-to-ghcr/v17** was hardened automatically. 25 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml composite steps use actions pinned to mutable tags rather than full commit SHAs: 'uses: docker/setup-qemu-action@v3' and 'uses: docker/setup-buildx-action@v3'.

Locations:

- `action.yml:68`
- `action.yml:73`

### script-injection (severity: high)

Step 'Log in to the Container registry' breaks sub-rule (a): GitHub expressions are pasted straight into the run: script. Examples: 'docker login ghcr.io -u "${{ github.actor }}"', 'export COMMIT_TAG="${{ github.event.release.tag_name }}"', 'echo "Event received: '${{ github.event_name }}' ... '${{ github.ref }}'"', 'export GITHUB_URL=https://github.com/${{ github.repository }}', '--file ${{ inputs.dockerfile }}', '--build-arg ${{ inputs.build_arg }}', '${{ inputs.extra_args }}', 'if [ -n "${{ inputs.platforms }}" ]', '--platform ${{ inputs.platforms }}', '--cache-from ${{ inputs.repository }}/${IMAGE_NAME}:latest' and '${{ inputs.context }}'. The step also breaks sub-rule (b): variables holding workflow-controlled data are expanded without quotes. Examples: 'export IMAGE_NAME=$(echo ${IMAGE_NAME} | tr ...)', 'for TAG in ${IMAGE_TAG_LIST}; do', 'if [ -z ${DOCKER_IO_USER} ]', '--tag docker.io/${IMAGE_NAME}:${COMMIT_TAG}', 'docker push docker.io/${IMAGE_NAME}:${TAG}', and '${GHCR_TAG_ARGS[@]}' / '${DOCKER_IO_TAG_ARGS[@]}' inside the COMMON_ARGS array. No shellcheck SC2086/SC2206 directive is present.

Locations:

- `action.yml:86`
- `action.yml:90`

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

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 6cf2490..835843c 100644
--- a/action.yml
+++ b/action.yml
@@ -5,73 +5,76 @@ description: "This action simplifies pushes of Docker images ot ghcr.io reposito
 branding:
   icon: "arrow-up-circle"
   color: "blue"
-  
 inputs:
-  github_token: 
-     description: "Your secrets.GITHUB_TOKEN"
-     required: true
-
+  github_token:
+    description: "Your secrets.GITHUB_TOKEN"
+    required: true
   image_name:
     description: "Image name, e.g. my-user-name/my-repo (will be lowercased)"
     required: true
-
   dockerfile:
     description: "A path to the Dockerfile (if it's not in the repository's root directory)"
     required: false
     default: "./Dockerfile"
-
   context:
     description: "A path to the context in which the build will happen, see https://docs.docker.com/engine/reference/commandline/build/"
     required: false
     default: "."
-
   repository:
     description: "Docker repository to push an image to, defaults to ghcr.io"
     required: true
     default: "ghcr.io"
-
   docker_io_user:
     description: "A username to use when pushing an image to docker.io (defaults to the github.actor)"
     required: false
-
   docker_io_token:
     description: "Your docker.io token created via https://hub.docker.com/settings/security"
     required: false
-  
   image_tag:
     description: Image tag or tags, e.g. latest or "1.0.0,latest". Only tag names are accepted. Will overwrite the latest tag on a push, and have no effect on a release.
     required: false
-
   # https://docs.docker.com/build/guide/build-args/
   build_arg:
     description: Additional build argument to be passed to the Dockerfile (e.g. NODE_AUTH_TOKEN=s3cr3t)
     required: false
     default: "FOO=bar"
-
   extra_args:
     description: Additional arguments to be passed to "docker build".
     required: false
     default: ""
-
   # https://docs.docker.com/build/building/multi-platform/
   platforms:
     description: "Comma-separated list of target platforms for the build (e.g. linux/amd64,linux/arm64). When set, uses docker buildx for multi-platform builds and pushes directly to the registry."
     required: false
     default: ""
-
 runs:
   using: "composite"
   steps:
     # https://docs.docker.com/build/drivers/docker-container/#qemu
     - name: Set up QEMU for multi-platform builds
       if: ${{ inputs.platforms != '' }}
       uses: docker/setup-qemu-action@v3
-
     # https://docs.docker.com/build/ci/github-actions/multi-platform/
     - name: Set up Docker Buildx
       if: ${{ inputs.platforms != '' }}
       uses: docker/setup-buildx-action@v3
-
     # https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry
     - name: Log in to the Container registry
       shell: bash
@@ -82,19 +85,29 @@ runs:
         DOCKER_IO_TOKEN: ${{ inputs.docker_io_token }}
         IMAGE_NAME: ${{ inputs.image_name }}
         IMAGE_TAG: ${{ inputs.image_tag }}
-
+        ACTOR: ${{ github.actor }}
+        EVENT_NAME: ${{ github.event_name }}
+        GIT_REF: ${{ github.ref }}
+        RELEASE_TAG_NAME: ${{ github.event.release.tag_name }}
+        GITHUB_REPOSITORY: ${{ github.repository }}
+        INPUT_REPOSITORY: ${{ inputs.repository }}
+        INPUT_DOCKERFILE: ${{ inputs.dockerfile }}
+        INPUT_BUILD_ARG: ${{ inputs.build_arg }}
+        INPUT_EXTRA_ARGS: ${{ inputs.extra_args }}
+        INPUT_PLATFORMS: ${{ inputs.platforms }}
+        INPUT_CONTEXT: ${{ inputs.context }}
       run: |
         echo "Using $(docker -v)"
 
         echo "::group::Logging into the GitHub Container registry (ghcr.io) ..."
-        echo "${GITHUB_TOKEN}" | docker login ghcr.io -u "${{ github.actor }}" --password-stdin
+        echo "${GITHUB_TOKEN}" | docker login ghcr.io -u "${ACTOR}" --password-stdin
         echo "::endgroup::"
 
         echo "::group::Set commit tag and image name"
         # github.ref: For workflows triggered by pull_request, this is the pull request merge branch.
         #             F
... (truncated)
```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, static-inline-injection, prior-fix

**Notes:**

Fixed in action.yml:
- Pinned the two unpinned actions to full commit SHAs: docker/setup-qemu-action@c7c53464625b32c7a7e944ae62b3e17d2b600130 (v3) and docker/setup-buildx-action@8d2750c68a42422c14e847fe6c8ac0403b4cbd6f (v3).
- Removed every ${{ }} expression from the run: script. The github.* values and every input now come in through the step's env: block (ACTOR, EVENT_NAME, GIT_REF, RELEASE_TAG_NAME, GITHUB_REPOSITORY_NAME and INPUT_*), and the script reads them as quoted shell variables.
- Single values (dockerfile, context, repository, platforms) are passed as one quoted argument each.
- extra_args and build_arg were pasted straight into the script before, so the shell split them and honoured quotes. They are now split the same way, using the guarded 'xargs -r' loop with the 'xargs -r true' check for unmatched quotes. --build-arg is only added when build_arg is not empty.
- Quoted the variable expansions the finding flagged: the DOCKER_IO_USER tests, the docker.io tags, the tag arrays, the labels and the build args. The image name is lowercased with printf instead of an unquoted echo.
- The image_tag list is still split on commas and whitespace (and globbed) like before. It now goes through an unquoted array assignment with a shellcheck SC2206 directive.
- Re-applied the previous harden's changes from the prior-fix finding: moving expressions into env and using ACTOR. I left the input formatting alone.

No tests were run on these changes.

### Iteration 1

**Fixes applied:** publish-gate

**Notes:**

Added '|| true' to the BUILD_DATE capture at action.yml:174, so a failing date no longer stops the step, as it didn't upstream.

