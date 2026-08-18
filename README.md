# Lunch Voice Levels & Cleanup Timer

A full-screen cafeteria display for Matchbook Learning at Wendell Phillips School 63. It runs the whole 30-minute lunch block: shows the expected voice level, counts down to cleanup with a draining hourglass, plays a calm neo-soul music bed, throws a settle-down alert on demand, and runs a gain-only homeroom points competition.

Built as a single self-contained `index.html`. No build step, no backend, no dependencies. Open it in a browser and press Start.

## Run it

- **Quick look:** open `index.html` in any modern browser, press **Start Lunch Timer**, then **Fullscreen**.
- **Real classroom use:** run it from a real browser tab on the projector machine (not inside a sandboxed preview) so the microphone (Auto-listen) and score saving work.
- **Publish with GitHub Pages:** merges to `main` deploy automatically via the included workflow (`.github/workflows/deploy.yml`), which also enables Pages on its first run — no manual setting needed. Once it succeeds the tool is live at `https://marlithaw.github.io/lunch/`. (If an org policy blocks auto-enable, set **Settings → Pages → Source** to **GitHub Actions** once and re-run.)

## What it does

- **Voice levels as apples.** Red = Level 0 (silent), Yellow = Level 1 (whisper), Green = Level 2 (table talk). The apple, the big label, an explicit expectation sentence, a persistent 0/1/2 scale, and the whole background color all move together.
- **Stages across the full 30 minutes.** Each stage has its own name, minutes, and voice level, editable at setup. The total flags red if it does not sum to 30.
- **Hourglass wind-down.** Sand drains through the current stage, tinted to the current level, emptying exactly as the big countdown hits zero.
- **Cleanup countdown.** A live "Cleanup in mm:ss" line during earlier stages; a distinct three-bell alarm when cleanup begins.
- **Alarms.** A bright start alarm, a soft ding at each stage change, the cleanup alarm, and a closing chord. All synthesized (Web Audio), no audio files.
- **Music bed (six options).** Pick an ambient track in setup or from the display's dropdown: the original neo-soul / R&B loop (Cmaj9, Am9, Dm9, G13 over a rounded bass), Rainforest, Ocean Waves, Gentle Rain, Meditation Bowls, or Lo-Fi Piano. All synthesized live with Web Audio — no audio files. The choice is remembered, and the bed can be switched mid-lunch. Muteable.
- **Too Loud alert.** A manual button (or the spacebar) fires a settle-down chime plus a full-screen "BRING IT DOWN" overlay naming the level to return to.
- **Auto-listen.** Optional microphone monitor with a live room meter and sensitivity slider; fires the same alert automatically when the room stays too loud, with a cooldown. Requires mic permission, so it only works when the file is served from a real browser tab.
- **Homeroom competition.** Tap a homeroom for +1, the star button for +5. The current leader wears a crown. A separate **Redirect** action plays a soft settle cue — a chime plus a spoken "let's get back on track" (browser voice, no audio file) — and a visible banner, with no point change. Points are gain-only by default; a setup toggle, **"Allow taking a point away,"** adds a **−1** button (floors at 0) that plays the same settle cue.
- **Teacher names stay set.** Names typed in setup save per lunch group to `localStorage` and reload automatically — enter them once and they persist (per device/browser). Scores persist the same way until you tap **Reset scores**.
- **Five lunch groups in one file.** A setup dropdown switches between Kinder & Dream, 1st & 2nd, 3rd & 4th, 5th & 6th, and 7th & 8th. Each group keeps its own scoreboard, saved separately in `localStorage`.
- **Matchbook styling.** Ember and flame palette with an original flame mark and wordmark, kept large and rounded so it reads as a kid tool.

## How it is built

- One HTML file. Inline CSS and JS, no external libraries.
- Audio is fully synthesized with the Web Audio API (oscillators, gains, filters, an LFO). No media files to license or ship.
- Visuals are inline SVG (apples, hourglass, flame) plus CSS.
- Sizing uses `vw` / `vh` so it fills a projector at full screen. It looks small in a narrow preview pane; that is expected.
- Scores persist in `localStorage` under keys `lunchScores_<groupId>`. Wrapped in try/catch so a sandbox that blocks storage degrades gracefully instead of crashing.
- No `alert()` / `confirm()` (sandboxes block them). Reset uses a two-tap confirm.
- Still one self-contained `index.html`, now with an inline SVG flame favicon and social/`theme-color` meta tags. Deployment is a small GitHub Actions workflow that publishes the repo root to Pages — no build step, no bundler.

## Open items for the next build session

1. **Teacher names.** Names are now entered in the setup screen and saved per group in the browser, so each band just needs to be filled in once on the machine that runs the display. 3rd and 4th grade ship with real homerooms (Jemison, Smallwood, Dr. Romeril / Prince, Shehadeh, Milton) as starting defaults; the other four groups start blank. Each band is fixed at three slots for now. Still open to confirm: whether "Dream" is its own set of homerooms, and whether any band needs more or fewer than three rooms (add/remove slots is a possible future enhancement).
2. **Default stage timings.** Currently Get Settled 5 / Eat & Connect 15 / Finish Up 4 / Cleanup 6 (sums to 30). Replace with the real routine so staff do not re-enter it daily.
3. **Auto-listen sensitivity.** Default threshold is a guess; calibrate against the real cafeteria noise floor.
4. **Exact Matchbook brand.** Palette is matched by eye to the "ignite / spark" identity. Swap in official hex codes or the real logo if available.

## Possible enhancements

- Add / remove homeroom buttons per band instead of fixed slots.
- Weekly cumulative scoring with a Friday winner and an auto-reset schedule.
- Optional teacher or homeroom photos on the cards.
- Export or print the day's scores.
- A settings screen to reorder the voice-level arc (quiet-to-loud vs loud-to-quiet).

## Handoff prompt

Paste this into Claude Code after opening the repo:

> This repo is a single-file cafeteria display (`index.html`) for a K-8 lunch block: voice-level apples, staged countdown with a draining hourglass, synthesized R&B music bed, a Too Loud alert with optional mic auto-listen, and a gain-only homeroom points competition with five selectable lunch groups saved per group in localStorage. Read the README "Open items" section first. Start by filling real teacher names into the `GROUPS` array and setting the real default stage timings. Keep it a single self-contained HTML file with no external dependencies.
