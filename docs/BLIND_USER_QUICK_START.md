# Blind User Quick Start

This short guide is for non-visual first use of the accessibility fork.
It focuses only on the parts that differ from upstream behavior or are easy to get wrong without sight.

## 1) After flashing

On a fresh install, accessibility cues are OFF by default.
That is intentional.

If you flash the fork and hear no accessibility sounds, do not assume the firmware is wrong. Enable accessibility first.

## 2) Fastest way to enable accessibility

While powering on the radio, hold:

- `BW + MODE + BAND`

This boot shortcut toggles accessibility quickly without needing menu navigation first.

## 3) First non-visual checks

After enabling accessibility, verify these three behaviors:

1. open a menu and rotate through items
2. listen for changing pitch as your position changes
3. use a simple toggle and confirm you hear:
   `ON = low -> high`
   `OFF = high -> low`

If those cues are present, the accessibility layer is active.

## 4) Important button behavior

The most important fork-specific shortcuts are:

- `Short press BW` on the main screen: opens the `BW` selector
- `Long press BW` on `FM/OIRT`: toggles `Stereo/Mono`
- `Long press BW` on `LW/MW/SW`: keeps BW selector behavior instead of stereo toggle

Practical meaning:

- on FM / OIRT, long `BW` is a quick accessibility-friendly stereo toggle
- on AM bands, do not expect the same long-press stereo action

## 5) BW selector: safe mental model

This is the part that matters most for non-visual use.

When the selector is open:

- moving through items gives you audible cursor feedback
- changing filter / `iMS` / `EQ` previews immediately
- nothing is saved until you confirm with `OK`
- leaving with `MENU/BACK` cancels and restores the previous values

## 6) BW selector order

### FM / OIRT

The order is:

`56 kHz` ... `311 kHz` -> `Auto BW` -> `iMS` -> `EQ` -> `OK`

Rules:

- the highest cursor tone is the last item: `OK`
- `iMS` and `EQ` are toggles
- you must end on `OK` and confirm there if you want the change saved

### AM

The AM selector is simpler:

AM filter values -> `OK`

## 7) What to expect when saving or canceling

If you confirm on `OK`:

- the new BW / `iMS` / `EQ` values are saved

If you leave with `MENU/BACK`:

- preview stops
- previous values come back
- nothing is saved

If a change "disappears", that usually means you exited instead of confirming on `OK`.

## 8) Quick recovery when something seems wrong

### No accessibility sounds after flashing

- enable accessibility with `BW + MODE + BAND` during boot

### A BW change did not stay

- reopen the selector
- make the change again
- move to `OK`
- confirm there

### Long `BW` does not toggle stereo

- check the current band
- that shortcut is for `FM/OIRT`, not `LW/MW/SW`

## 9) What is still not documented enough

These areas still rely more on upstream wiki or trial-and-error than they should:

- a fully non-visual first-run walkthrough from power-on to daily use
- a hardware-agnostic explanation of the physical controls for different TEF6686 builds
- a non-visual workflow for memory channels and scanner features
- an audio-first explanation of menu layout beyond the BW selector

If you maintain this fork, these are the highest-value next documentation targets for blind users.
