# layer-android-sdk

Android SDK toolchain for OpenCharly images.

The `android-sdk` candy installs the Android SDK under `/opt/android-sdk`:
cmdline-tools (`sdkmanager`/`avdmanager`), platform-tools (`adb`), build-tools 36
(`aapt2`), the `android-36` platform, the emulator, and the
`google_apis_playstore` x86_64 system image — plus `apkeep`, the by-package-name
app downloader the `charly check adb install-app` verb drives in the pod. Every
artifact is a fixed path under `/opt/android-sdk`, so the SDK is verifiable by
probing those paths and running `adb`.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `android-sdk` |
| Install root | `/opt/android-sdk` |
| Components | cmdline-tools (`sdkmanager`/`avdmanager`), platform-tools (`adb`), build-tools 36 (`aapt2`), `android-36` platform, emulator, `google_apis_playstore` x86_64 system image |
| Extra binary | `/usr/local/bin/apkeep` (pinned v0.18.0) |
| Requires | `layer-java-openjdk` |
| Environment | `ANDROID_HOME=/opt/android-sdk`, `ANDROID_SDK_ROOT=/opt/android-sdk` |
| PATH append | `/opt/android-sdk/cmdline-tools/latest/bin`, `/opt/android-sdk/platform-tools`, `/opt/android-sdk/emulator` |
| Service / port | none |

The SDK tools come from CachyOS/AUR packages; the API-36 system image — the one
component with no package anywhere — is fetched by `sdkmanager` into a persistent
build cache and copied into the install root.

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
android-emulator:
  candy:
    base: cachyos
    candy:
      - '@github.com/opencharly/layer-android-sdk:v2026.250.1813'
```

Then, inside the built image:

```bash
adb version
sdkmanager --list_installed
avdmanager list avd
```

## Layout

- `charly.yml` — the candy manifest: the AUR/distro package lists, the pinned
  `apkeep` download, the `sdkmanager` system-image `run:` step, an ordered
  `plan:` of build-time `check:` steps, and the embedded `skill:` entity (when
  present).
- `.github/workflows/` — the org-wide `charly/pr-validator` gate; no per-repo candy gate.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: none yet (see `/charly-check:android` for the `kind: android`
  device and `apk:` package-format model this SDK backs)
- Device interaction: `/charly-check:adb`, `/charly-check:appium`
- Requires: `/charly-coder:java-openjdk`
- Consumed by: `box/android-emulator`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
