# assaheeh-build

Public GitHub Actions runner for the **private** AsSaheeh Android source repository.

No application source code, resources, Gradle files or signing material are stored
in this repository. The only content here is this README and
`.github/workflows/build-private-apk.yml`.

## How it works

1. `Build AsSaheeh (private source)` is a manually triggered (`workflow_dispatch`)
   workflow.
2. It checks out `sketchnoobdeveloper/AsSaheeh` (private) using the repository
   secret `PRIVATE_REPO_TOKEN` — a fine-grained PAT restricted to that repository
   with **Contents: read** only. The token is never written to the public repo,
   logs or artifacts.
3. The workflow detects the Java, Gradle, Android SDK and module requirements from
   the checked-out source, builds `:app:assembleDebug` and uploads the resulting
   ABI-split APKs as the `assaheeh-debug-apks` Actions artifact.

## Required secret

`PRIVATE_REPO_TOKEN` — fine-grained PAT, resource owner `sketchnoobdeveloper`,
repository access: **Only select repositories → AsSaheeh**, permissions:
**Contents → Read-only**.

## Privacy notes

- The source repo stays private; its visibility is never changed.
- The workflow never prints source files. Only discovered version numbers, the
  build task and produced APK filenames/sizes go to the public step summary.
- The published artifact contains debug APKs only.
