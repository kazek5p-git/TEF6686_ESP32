# Fork Release Index

This index tracks public release packages published on this fork.

## Latest

- Tag: `v2026.03.06-a11y-final-r7`
- Title: `TEF6686 Accessibility Final Review Pack (Fork Cleanup)`
- Full package:
  - `TEF6686_ESP32_Release_Public_latest_20260306_r7_fork_cleanup_full.zip`
- Firmware payload:
  - same runtime behavior as `r6`
  - firmware commit: `505a655`
- Focus:
  - fork documentation cleanup and organization
  - README cleanup and dedicated fork-doc links
  - added release index for this fork
- SHA256:
  - `2ea0dac0aa81f26e9757615e16e8081e4c6b115815d71acc34b0903e5e9128fb`
- Package note:
  - `README_PUBLIC.txt` inside the `r7` ZIP still identifies `r6` / `505a655`
  - this is expected because `r7` reuses the unchanged `r6` firmware payload
- Includes:
  - compiled firmware (`TEF6686_ESP32.ino.bin`)
  - `bootloader.bin`, `partitions.bin`, `boot_app0.bin`
  - `format_Spiffs.ino.bin`
  - `esptool.exe`
  - `flash.bat`, `flash.sh`
  - `FlashWizard.ps1`, `FlashWizard.cmd`

## Previous Public Packs

- `v2026.03.06-a11y-final-r6`
  - Full package: `TEF6686_ESP32_Release_Public_latest_20260306_bw_preview_fix_full.zip`
  - Focus: BW selector instant preview, save on `OK`, rollback on `MENU/BACK`
- `v2026.03.05-a11y-final-r5`
  - Full package: `TEF6686_ESP32_Release_Public_latest_20260305_bw_save_fix_full.zip`
  - Focus: BW selector save-on-OK hotfix
- `v2026.03.05-a11y-final-r4`
  - Full package: `TEF6686_ESP32_Release_Public_latest_20260305_wizard_newline_fix.zip`
  - Focus: FlashWizard newline rendering fix in translated messages
- `v2026.03.05-a11y-final-r1`
  - Accessibility final review pack baseline

## Earlier Accessibility Milestones

- `v2026.03.03-accessibility-r1`
  - first merged accessibility package
- `stable_a11y_public_2026-03-04`
  - stable accessibility baseline before the final review packs

## Notes

- This index describes this fork only.
- Releases `r6` and `r7` share the same firmware behavior; choose `r7` if you want the cleaned-up fork docs.
- Upstream repository: `PE5PVB/TEF6686_ESP32`.
