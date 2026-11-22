# Publishing a new version

This repository only hosts public APK builds. The application source stays private.

When a new signed APK is published in the private source repository:

1. **Create a release here** — Releases → Draft a new release
   - Tag: same as the source release, e.g. `v1.1.71-beta`
   - Title: `1.1.71-beta`
   - **Do not** mark it as a pre-release (the stable `/releases/latest/download/...` link needs it)
   - Upload three files:
     - the APK, renamed exactly **`Ashke-Shadek.apk`**
     - **`update.json`** — copy it from the source release and change only `apkUrl` to:
       `https://github.com/aamiqram/ashke-sadek-downloads/releases/download/<tag>/Ashke-Shadek.apk`
     - **`SHA256SUMS.txt`** — `<sha256 of apk>  Ashke-Shadek.apk`
2. **Update the `update-mirror` branch** (used by the in-app updater when the GitHub API is rate-limited):
   - open the branch, replace `update.json` with the same patched file
   - replace `Ashke-Shadek.apk` with the new APK (Add file → Upload files → commit to `update-mirror`)
3. **Update the page** — edit `index.html` on `main` and change the version badge text.
4. Verify:
   - `https://aamiqram.github.io/ashke-sadek-downloads/` shows the new version
   - `https://github.com/aamiqram/ashke-sadek-downloads/releases/latest/download/Ashke-Shadek.apk` downloads the new file

The in-app updater checks this repository first and falls back to the old source repository.
