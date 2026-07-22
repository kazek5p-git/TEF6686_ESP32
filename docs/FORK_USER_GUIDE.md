# Fork User Guide

This guide is for the accessibility-focused fork at `kazek5p-git/TEF6686_ESP32`.
It complements the upstream wiki with fork-specific behavior, release notes, and flashing advice.

If you want the shortest non-visual walkthrough first, start with `docs/BLIND_USER_QUICK_START.md`.

## 1) What this fork changes

Compared with upstream `PE5PVB/TEF6686_ESP32`, this fork adds or documents:

- optional audio accessibility cues for blind and low-vision users
- a boot shortcut to toggle accessibility quickly
- BW selector behavior with instant preview, save on `OK`, and rollback on `MENU/BACK`
- FlashWizard UI language auto-detection (`PL/EN/DE/ES/FR/IT`, with `EN` fallback)
- fork-specific release notes and upstream integration notes

## 2) Which release to use

Use GitHub Releases, not the branch tip, when you want a tested package.

Current public package:

- Tag: `v2026.03.06-a11y-final-r7`
- Title: `TEF6686 Accessibility Final Review Pack (Fork Cleanup)`

Important:

- `r7` is a documentation cleanup release.
- Firmware behavior is the same as `r6` (`505a655`).
- The `README_PUBLIC.txt` inside the `r7` ZIP may still identify `r6` / `505a655`. That is expected because the firmware payload did not change between `r6` and `r7`.

## 3) Package contents

The public full package includes:

- `TEF6686_ESP32.ino.bin`
- `bootloader.bin`
- `partitions.bin`
- `boot_app0.bin`
- `format_Spiffs.ino.bin`
- `esptool.exe`
- `flash.bat`
- `flash.sh`
- `FlashWizard.ps1`
- `FlashWizard.cmd`

After using `FlashWizard`, you may also see `backup_update_*` folders. Those are local backup folders created during update operations.

## 4) How to flash

### Recommended on Windows

Run one of these from the release folder:

- `FlashWizard.cmd`
- `FlashWizard.ps1`

Why this is the easiest path:

- guided UI
- multilingual interface
- safer update workflow for fork users

### Direct flashing on Windows

Run `flash.bat` if you prefer a simpler script-driven flow.

What `flash.bat` does:

1. asks for the radio COM port
2. asks whether your radio needs the `BOOT` button held for flashing
3. formats the filesystem with `format_Spiffs.ino.bin`
4. flashes the firmware image and core ESP32 binaries

Notes:

- treat this as a full flash workflow, because the script formats first
- if `TEF6686_ESP32.spiffs.bin` is not present, the script skips SPIFFS upload and still flashes the main firmware

### Shell script path

`flash.sh` is the shell-script variant for non-Windows environments.

## 5) Updating from older builds

### From an older build of this fork

Recommended path:

- download the latest full release package
- flash from that package
- re-enable accessibility if needed after flashing

If you are already on `r6`, moving to `r7` does not change firmware behavior. `r7` mainly cleans up fork documentation and release indexing.

### From upstream firmware

Use a full fork release package instead of mixing random binaries by hand.
That avoids confusion around:

- accessibility settings
- BW selector behavior
- FlashWizard fork protections

### FlashWizard update behavior

This fork's `FlashWizard.ps1` can check for PE5PVB releases and update base files.
When doing that, it only replaces `TEF6686_ESP32.ino.bin` if accessibility markers are detected (`Accessibility` + `Voice Lite`).

That behavior is intentional. It reduces the chance of silently replacing the fork firmware with a non-accessibility upstream binary.

## 6) Accessibility quick start

Fresh installs keep accessibility cues OFF until you enable them.

Fastest enable path:

- hold `BW + MODE + BAND` while powering on

What you get after enabling:

- menu position cues
- distinct `enter`, `back`, `exit`, and `confirm` cues
- ON/OFF two-tone cues for toggles
- Voice Lite cues for selected actions

## 7) BW selector behavior

This is one of the most important fork-specific behavior changes.

Expected behavior:

1. open selector
2. move through filter / `Auto BW` / `iMS` / `EQ`
3. hear cursor cues while previewing
4. press `OK` to save
5. leaving with `MENU/BACK` restores the previous values

### FM / OIRT selector order

`56 kHz` ... `311 kHz` -> `Auto BW` -> `iMS` -> `EQ` -> `OK`

Important:

- the highest cursor tone is the last selector item: `OK`
- `iMS` and `EQ` are toggle items
- preview is immediate, but persistence happens only on `OK`

### AM selector order

AM filter values -> `OK`

## 8) Button shortcuts that matter in this fork

- `Short press BW` on the main screen opens the `BW` selector
- `Long press BW` on `FM/OIRT` toggles `Stereo/Mono`
- `Long press BW` on `LW/MW/SW` keeps the BW selector flow
- boot shortcut: hold `BW + MODE + BAND` while powering on to toggle accessibility

## 9) Troubleshooting

### I flashed successfully but hear no accessibility cues

That can be expected on a fresh install.
This fork keeps cues OFF by default until you enable them.
Use the boot shortcut or the normal settings path to turn them on.

### My BW / iMS / EQ change disappeared after leaving the selector

That is expected if you left the selector with `MENU/BACK`.
Use `OK` to commit the new values.

### The `r7` ZIP says `r6` in `README_PUBLIC.txt`

That is expected for the public `r7` package.
`r7` is a docs cleanup pack built on top of the unchanged `r6` firmware payload.

### `flash.bat` warns that `TEF6686_ESP32.spiffs.bin` is missing

That is informational.
The script can still flash the main firmware without a SPIFFS image.

## 10) For developers and maintainers

This fork keeps the same basic build/tooling flow as upstream.

Useful references in this repo:

- `README.md` for the user-facing summary
- `docs/FORK_RELEASES.md` for public pack history
- `docs/ACCESSIBILITY_UPSTREAM_INTEGRATION_GUIDE.md` for a low-risk upstreaming approach
- `docs/ISSUE_DRAFT_ACCESSIBILITY_PLAN.md` for the upstream issue template

General board/toolchain setup still follows upstream conventions and upstream wiki guidance.
