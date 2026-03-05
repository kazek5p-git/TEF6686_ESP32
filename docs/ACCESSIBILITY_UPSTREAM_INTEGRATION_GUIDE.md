# Accessibility Integration Guide (for PE5PVB mainline)

This document explains how to integrate accessibility features into upstream TEF6686_ESP32 with minimal core-code impact.

## 1) Goals

- Keep original radio logic intact as much as possible.
- Isolate accessibility in a dedicated module (`src/accessibility.*`).
- Add only small hook calls in existing code paths.
- Preserve expected UX in critical flows (especially BW selector behavior).

## 2) Minimal-change architecture

### Keep in one module

- `src/accessibility.h`
- `src/accessibility.cpp`

Module responsibilities:

- cue generation (beeps / two-tone on/off)
- menu position cue mapping
- runtime feature toggles and defaults
- thin wrappers callable from legacy code

### Keep outside module

- tuner logic, EEPROM layout, menu routing
- radio state machines (seek, scan, tune)
- existing rendering logic

Only add hook calls where needed.

## 3) Hook points (small inserts only)

Main file hooks (examples):

- `ButtonPress()`:
  - menu move/confirm cues
  - BW selector cursor/confirm/toggle cues
- `ModeButtonPress()`:
  - menu enter/back/exit cues
- `BWButtonPress()`:
  - selector open cue
  - stereo/mono cue
- rotary handlers:
  - position cue for menu/submenu navigation

Touch hooks:

- `src/touch.cpp` selector and menu touch actions
- keep same cue API as rotary paths for consistency

Rendering hooks:

- `src/gui.cpp` only for reading temporary selector state while selector is open

## 4) BW selector behavior (required)

Expected behavior (original UX with safe persistence):

1. Enter selector: snapshot current values.
   - `BWsetRecall`, `iMSsetRecall`, `EQsetRecall`
2. Selecting filter/iMS/EQ: apply immediately to tuner (instant preview).
3. `OK`: persist new values to EEPROM.
4. `MENU/BACK` exit: rollback to snapshot, do not persist.

### Recommended transaction model

- Temporary state:
  - `BWtemp`, `BWsettemp`, `iMSsettemp`, `EQsettemp`
- Snapshot state:
  - `BWsetRecall`, `iMSsetRecall`, `EQsetRecall`

Pseudo-flow:

```text
onSelectorOpen:
  snapshot = current
  temp = current

onSelectorChange:
  temp = changedValue
  applyPreview(temp)        // tuner only, no EEPROM commit

onOK:
  current = temp
  persist(current)          // EEPROM write + commit

onCancel:
  current = snapshot
  applyPreview(snapshot)    // restore runtime state, no EEPROM commit
```

Important:

- During selector preview (`BWtune == true`), `doBW()` must not commit EEPROM.
- Commit only on `OK` path.

## 5) Regression checklist (must pass)

- No duplicate tones in submenu navigation.
- Menu enter/back/exit cues remain distinct.
- BW selector:
  - immediate preview works for BW, iMS, EQ
  - `OK` saves
  - `MENU/BACK` reverts
- Band switching remains stable (no lockups).
- Stereo/mono cue still uses ON/OFF two-tone pattern.
- Memory/flash usage remains inside board limits.

## 6) Suggested upstream process

Use an Issue first (implementation plan), then optional PR.

Issue should include:

- user-facing benefit for blind/low-vision operation
- exact behavior requirements (especially BW selector transaction behavior)
- module boundary (`src/accessibility.*`) and low-risk hook strategy
- regression checklist

## 7) Why this works for maintainability

- Core logic stays familiar for maintainer.
- Accessibility evolves mostly in one module.
- Future merges with upstream become easier (small, explicit hook diffs).
- Lower chance of regressions from broad edits.
