# AGENTS.md — layer-android-sdk

Standalone candy repo for the `android-sdk` layer. The candy lives in `charly.yml`
at the repo root: the AUR/distro package lists, the pinned `apkeep` download, the
`sdkmanager` system-image `run:` step, an ordered `plan:` of build-time `check:`
steps, and the embedded `skill:` entity (when present). There is no source tree
and no service of its own.

Canonical files:

- `charly.yml` — the `android-sdk:` candy entity (and the `android-sdk-skill:`
  skill entity, when present).
- `.github/workflows/` — the org-wide `charly/pr-validator` gate; there is no per-repo candy gate.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-check:android` — the closest owning skill: the `kind: android` device
  substrate and the `apk:` layer package format this SDK backs. Load before
  editing or troubleshooting the candy.
- `/charly-check:adb` / `/charly-check:appium` — the device-interaction verbs
  (`adb:` / `appium:`) that drive a running device built on this SDK. Load when
  changing what the device can do.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`/`run:`/`download:`, package sections, AUR).
  Load before editing any entity field or plan step.

There is no dedicated `/charly-*:android-sdk` owning skill yet — this repo's
candy carries no `skill:` entity. When one is authored, add it here.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the org-wide `charly/pr-validator` (required check
  `validate / validate`); there is no per-repo candy gate.
- There is no live bed in this repo: the candy's `plan:` `check:` steps assert
  the installed paths (`sdkmanager`, `adb`, `aapt2`, the emulator, the `android-36`
  platform, the system image) and run `adb version`; the emulator bring-up is
  proven by the consuming `check-android-emulator-pod` bed in `distro-cachyos`.

## Modify this repo

- Edit the `android-sdk:` candy entity (and its `skill:` entity together, when
  one exists). The skill is the projected usage source, so an install or behaviour
  change that is not mirrored in the skill leaves the corpus stale.
- Package/AUR changes go under `distro:`; the `apkeep` and system-image versions
  are `var:` consumed by the `download:`/`run:` steps. Behaviour claims go in
  `plan:` as observable `check:` steps.
- Keep `version:` at the schema stamp the pinned CI charly supports.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
