# Publishing a new version

This repository hosts the public APK builds. The application source stays private.

## Automatic path

A maintainer dispatches **Build Android APK** on `main` in the source repository. The signed **Publish signed Android release** workflow then:

1. Publishes `Ashke-Shadek.apk`, `update.json`, and `SHA256SUMS.txt` here as a non-prerelease release tagged `v1.1.<run>-beta`.
2. Writes `update.json.apkUrl` against this repository, not the source repository.
3. Force-pushes the APK and `update.json` to the `update-mirror` branch.

The workflow needs the `DOWNLOADS_REPO_TOKEN` Actions secret in the source repository: a token with write access to this repository's contents. Prefer a fine-grained token limited to this repository with **Contents: Read and write**.

## Why `update-mirror` is separate

`update-mirror` is an **orphan branch** with no shared history with `main`. It contains only `update.json` and `Ashke-Shadek.apk` and is force-pushed on every release. The in-app updater reads those files directly from `raw.githubusercontent.com`.

Do not merge it into `main`: the unrelated history is intentional, it keeps the ~44 MB APK out of the site branch, and the next release force-pushes the branch anyway. GitHub's compare page will always say the histories are unrelated.

## Verify after publishing

- `releases/latest/download/Ashke-Shadek.apk` returns the new APK; size and SHA-256 match.
- `releases/latest/download/update.json` has the matching `versionCode` and an `apkUrl` under `aamiqram/ashke-sadek-downloads`.
- `raw.githubusercontent.com/aamiqram/ashke-sadek-downloads/update-mirror/update.json` matches, and the mirror APK is the same size.
- Update the version badge and SHA-256 footer in `index.html`, then check the live page.

## Manual fallback

If the workflow is unavailable, create the release here by hand:

1. Tag it like the source release, e.g. `v1.1.73-beta`; do **not** mark it pre-release.
2. Upload the APK renamed exactly **`Ashke-Shadek.apk`**.
3. Copy `update.json` from the source release and change only `apkUrl` to
   `https://github.com/aamiqram/ashke-sadek-downloads/releases/download/<tag>/Ashke-Shadek.apk`.
4. Create `SHA256SUMS.txt` as `<sha256 of apk>  Ashke-Shadek.apk`.
5. Replace `update.json` and `Ashke-Shadek.apk` on the `update-mirror` branch.
6. Update the page badge and SHA-256 footer.
