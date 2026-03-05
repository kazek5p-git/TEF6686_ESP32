# Issue Draft: Accessibility integration in upstream TEF6686_ESP32

## Summary

Request to integrate an accessibility layer (audio cues) for blind and low-vision users with minimal impact on core radio logic.

## Scope

- Add/keep accessibility logic in dedicated module: `src/accessibility.h/.cpp`.
- Add small hook calls in existing input paths (rotary/touch/menu/BW selector).
- Keep original tuner and menu architecture intact.

## Functional requirements

1. Menu navigation cues (position-based pitch)
2. Distinct cues for enter/back/exit/confirm
3. ON/OFF two-tone pattern for toggles
4. BW selector transactional behavior:
   - filter/iMS/EQ changes apply immediately (preview)
   - save only after `OK`
   - `MENU/BACK` exits without saving and restores previous state

## Technical requirement for BW selector

Use snapshot + temporary state:

- Snapshot: `BWsetRecall`, `iMSsetRecall`, `EQsetRecall`
- Temp: `BWtemp`, `BWsettemp`, `iMSsettemp`, `EQsettemp`

Rules:

- While selector is open (`BWtune`): runtime preview is allowed, EEPROM commit is not.
- On `OK`: write EEPROM and commit.
- On cancel: restore from snapshot, no EEPROM commit.

## Acceptance tests

- No duplicate/overlapping menu cues
- BW selector preview/save/cancel behavior exactly as above
- No regression in band switching stability
- Stereo/mono cue still consistent with ON/OFF pattern
- Firmware size remains within 4MB flash profile constraints

## Notes

A detailed technical guide is included in:
`docs/ACCESSIBILITY_UPSTREAM_INTEGRATION_GUIDE.md`
