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

## Release build
Send `release_build: true` with the manual dispatch to also produce a signed release APK
(`assaheeh-release-signed` artifact). Signing uses these repository secrets:
`RELEASE_KEYSTORE_BASE64`, `RELEASE_STORE_PASSWORD`, `RELEASE_KEY_ALIAS`, `RELEASE_KEY_PASSWORD`.

The upload key was generated locally and is stored only as encrypted GitHub secrets. Save a copy
from GitHub Settings → Secrets before rotating/removing them; losing it prevents future app updates.
