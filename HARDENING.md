<!-- markdownlint-disable -->

# Hardening Report: macbre--push-to-ghcr/v16

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **macbre--push-to-ghcr/v16** was hardened automatically. 16 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): the composite step 'Log in to the Container registry' puts GitHub expressions straight into its run: script. Examples: `docker login ghcr.io -u "${{ github.actor }}"`, `echo "Event received: '${{ github.event_name }}' (with a reference '${{ github.ref }}' / tag name '${{ github.event.release.tag_name }}')"`, `export COMMIT_TAG="${{ github.event.release.tag_name }}"`, `export GITHUB_URL=https://github.com/${{ github.repository }}`, `--file ${{ inputs.dockerfile }}`, `--cache-from ${{ inputs.repository }}/...`, `--build-arg ${{ inputs.build_arg }}`, `${{ inputs.extra_args }}`, `${{ inputs.context }}`, and `docker push ${{ inputs.repository }}/...`. Inputs and the release tag name are substituted into the script before the shell runs, so they can inject shell commands.

Locations:

- `action.yml:73`
- `action.yml:80`
- `action.yml:83`
- `action.yml:100`
- `action.yml:102`
- `action.yml:108`
- `action.yml:115`
- `action.yml:119`

### script-injection (severity: high)

Rule (b): the same step expands env vars that hold workflow-controlled data without quotes. Examples: `if [ -z ${IMAGE_TAG} ]; then`, `export COMMIT_TAG=${IMAGE_TAG}`, `export IMAGE_NAME=$(echo ${IMAGE_NAME} | tr ...)`, `--tag docker.io/${IMAGE_NAME}:${COMMIT_TAG}`, `if [ -z ${DOCKER_IO_USER} ]; then`, and `docker push docker.io/${IMAGE_NAME}:${COMMIT_TAG}`. These values come from inputs.image_tag, inputs.image_name and inputs.docker_io_user.

Locations:

- `action.yml:86`
- `action.yml:89`
- `action.yml:94`
- `action.yml:118`

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

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 1360567..bf3b66c 100644
--- a/action.yml
+++ b/action.yml
@@ -5,49 +5,39 @@ description: "This action simplifies pushes of Docker images ot ghcr.io reposito
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
     description: Image tag, e.g. latest. Will overwrite the tag latest tag on a push, and have no effect on a release.
     required: false
-
   # https://docs.docker.com/build/guide/build-args/
   build_arg:
     description: Additional build argument to be passed to the Dockerfile (e.g. NODE_AUTH_TOKEN=s3cr3t)
     required: false
     default: "FOO=bar"
-
   extra_args:
     description: Additional arguments to be passed to "docker build", you can pass custom tags here for instance.
     required: false
@@ -55,6 +45,23 @@ inputs:
 runs:
   using: "composite"
   steps:
     # https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry
     - name: Log in to the Container registry
       shell: bash
@@ -65,67 +72,85 @@ runs:
         DOCKER_IO_TOKEN: ${{ inputs.docker_io_token }}
         IMAGE_NAME: ${{ inputs.image_name }}
         IMAGE_TAG: ${{ inputs.image_tag }}
-
+        GITHUB_ACTOR: ${{ github.actor }}
+        GITHUB_EVENT_NAME: ${{ github.event_name }}
+        GITHUB_REF: ${{ github.ref }}
+        GITHUB_RELEASE_TAG_NAME: ${{ github.event.release.tag_name }}
+        GITHUB_REPOSITORY: ${{ github.repository }}
+        INPUT_DOCKERFILE: ${{ inputs.dockerfile }}
+        INPUT_REPOSITORY: ${{ inputs.repository }}
+        INPUT_CACHE_FROM_REPO: ${{ inputs.repository }}
+        INPUT_BUILD_ARG: ${{ inputs.build_arg }}
+        INPUT_EXTRA_ARGS: ${{ inputs.extra_args }}
+        INPUT_CONTEXT: ${{ inputs.context }}
       run: |
         echo "Using $(docker -v)"
 
         echo "::group::Logging into the GitHub Container registry (ghcr.io) ..."
-        echo "${GITHUB_TOKEN}" | docker login ghcr.io -u "${{ github.actor }}" --password-stdin
+        echo "${GITHUB_TOKEN}" | docker login ghcr.io -u "$GITHUB_ACTOR" --password-stdin
         echo "::endgroup::"
 
         echo "::group::Set commit tag and image name"
         # github.ref: For workflows triggered by pull_request, this is the pull request merge branch.
         #             For workflows triggered by release, this is the release tag created.
         # https://docs.github.com/en/actions/reference/contexts-reference
-        echo "Event received: '${{ github.event_name }}' (with a reference '${{ github.ref }}' / tag name '${{ github.event.release.tag_name }}')"
+        echo "Event received: '$GITHUB_EVENT_NAME' (with a reference '$GITHUB_REF' / tag name '$GITHUB_RELEASE_TAG_NAME')"
 
-        if [ "${{ github.event_name }}" = "release" ]; then
-          export COMMIT_TAG="${{ github.event.release.tag_name }}"
+        if [ "$GITHUB_EVENT_NAME" = "release" ]; then
+          export COMMIT_TAG="$GITHUB_RELEASE_TAG_NAME"
           ex
... (truncated)
```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, prior-fix

**Notes:**

Rewrote action.yml's single composite step so the run: script has no ${{ }} expressions left. The github context values (actor, event_name, ref, release tag_name, repository) and the inputs (dockerfile, repository, build_arg, extra_args, context) now go through the step's env: block and are double-quoted wherever they expand. The env vars that were already there (IMAGE_TAG, IMAGE_NAME, DOCKER_IO_USER) are now quoted in tests and assignments. extra_args was pasted into the script by upstream, so it is split with the guarded, quote-aware xargs pattern into a bash array. build_arg is passed as one --build-arg value and dropped when empty; context uses ${VAR:+"$VAR"}. All other behavior is unchanged.

### Iteration 1

**Fixes applied:** publish-gate

**Notes:**

Added '|| true' to the BUILD_DATE assignment at action.yml:110. If date fails, the step now carries on as it did upstream instead of stopping.

