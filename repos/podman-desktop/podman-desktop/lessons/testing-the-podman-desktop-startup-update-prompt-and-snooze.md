---
topic: Testing the Podman Desktop startup update prompt and snooze
description: "How to manually verify the startup update prompt, Remind me tomorrow and preferences.update.nextReminderTimestamp in Podman Desktop: downgrading package.json version with version-util.cjs, compile:current win-unpacked build, fresh PODMAN_DESKTOP_HOME_DIR, settings.json edits, updater disabled in dev and on Linux."
sources:
  - https://github.com/podman-desktop/podman-desktop/pull/18491#issuecomment-6419397139
last_verified: 2026-10-26
confidence: medium
---
- The updater only runs in production builds and not on Linux (`packages/main/src/plugin/updater.ts`: `!isLinux() && import.meta.env.PROD`). Dev mode (`pnpm watch`) never shows the prompt.
- To get an update offered, build an older version: `node tests/playwright/scripts/version-util.cjs ./package.json --update` (lowers the minor version by 2, falls back to 1.20.0), then `pnpm compile:current --win nsis`. Without `GH_TOKEN` the installer step fails, but `dist/win-unpacked/Podman Desktop.exe` is already built and is enough. Revert `package.json` afterwards.
- The CI reference is job `win-update-e2e-test` in `.github/workflows/e2e-main.yaml`. It uses env `UPDATE_TARGET_OWNER`, `UPDATE_TARGET_REPO` and `UPDATE_PRERELEASE` to pick the release feed. The bot verified against the real podman-desktop release feed.
- Use a fresh `PODMAN_DESKTOP_HOME_DIR` for every case, with `settings.json` containing `"preferences.update.reminder": "startup"`. The startup dialog only appears with that value.
- `preferences.update.nextReminderTimestamp` is hidden: edit `settings.json` directly (epoch ms), then restart the app. Expected: future and at most 24h ahead means the dialog is suppressed; past means shown; more than 24h ahead, non-numeric or <= 0 is ignored, so shown. Choosing `Later` > `Remind me tomorrow` writes now+24h (compare with `date`). The status bar update entry still shows during the snooze.
- Screenshots of each case go in the PR thread. The Playwright update spec (`update-install.spec.ts`) and a real 24h wait were not run.
