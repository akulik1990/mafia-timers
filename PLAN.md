# Mafia Timers — plan / tracker

Single-file mod timer for Mafia, streamed to a Zoom cam via OBS. Replaces
advancedmod.com/mafia (saved as `reference-advancedmod.html`). Vanilla HTML/JS,
no build, no dependencies, system monospace font so it works offline in an OBS
browser source.

## Decisions
- **Single view, hideable controls** (H / "Hide UI"). Broadcast = banner + 2 clocks.
- **Formal counts DOWN** from its set duration (reference counted up), so text goes
  white → yellow → red as it nears the end. Thresholds configurable (default 60s / 20s).
- **Dropped "formals pause time"** — day keeps running during formals (never used).
- **Formal label on the timer**: type dropdown (Formal / EOD / GTD / RNG 1 / RNG 2) +
  name → "EOD Formal on Morris" (plain "Formal" → "Formal on Morris").
- **10 sec** = manual overlay countdown; just don't press it on F3.
- **Vote** = configurable seconds, progress bar then full-red "VOTE" flash; press again to reset.
- Settings (gear) persist in localStorage: day default, formal default, vote secs, warn/danger.
- All action buttons in one place (Pause / 10 sec / Vote). Space = pause.

## Done (v2 — redesigned to real Zoom-tile feedback)
- **Fixed-size box** (~540px wide, a bit taller), does NOT stretch to window; scales down only if window is smaller. Name text sits under the clocks.
- Clocks **fused into one block**, dominant; active phase greens, DAY stays visible (dims during a formal).
- **10-sec** shows as a cooldown in the FORMAL area; **Vote** is a 3s "time to vote" bar in the FORMAL area ending on "TIME". DAY never covered.
- **Single Start↔Stop toggle** per timer; inputs collapse when running.
- Subtle buttons; **Settings under the ⚙ menu**; **Hide UI removed**.
- Day countdown white → orange <60s → red negative; formal warn/danger colours; Pause; localStorage settings.

## Done (v3 — layout + polish)
- Fixed 16:9 box (~560px), three bands: **top** (Day start/stop + ⚙), **middle** (fused clocks, digits dominant), **bottom** (left = formal type/name/Start, right = Pause/10 sec/Vote).
- Digits use embedded **DSEG7** LCD font (base64 data URI, offline-safe). System monospace fallback.
- **Formal counts up by default**; Settings → "Formal direction" toggles up/down. Colour still tracks time-left vs the configured limit either way.
- Default day length **18 min**. Formal duration lives in Settings (off the main display).
- Formal label spacing fixed ("GTD Formal on Cyclone").

## Done (v4 — layout/behavior polish)
- Fills viewport at 16:9 (no letterbox); OBS browser source at 16:9 = edge to edge.
- Bottom is a 2×2: left [type|name] over [Start/Stop full width]; right [Pause full width] over [10 sec|Vote].
- DAY/FORMAL labels bigger, lifted to top of panel (clock centred below). Digits `min(26cqh,11cqw)`.
- Font swapped to **DSEG7 Modern** (no ghost segments; OFL). Old app used "digital-7 mono" (Style-7 freeware) — embeddable on request.
- Formal label space **pre-reserved** (min-height) so starting a formal doesn't resize the clocks.
- **10 sec holds on 0** when done; **vote reset → formal 00:00**. Both are end-of-formal actions. Starting a formal clears any held 10s/vote.

## Done (v5 — fonts, day tracker, gating)
- Fills the OBS browser source exactly (100vw/vh); set source W×H to control size.
- Embedded fonts + **Clock font** selector in Settings: Plain mono (default), Digital-7 (old app), DSEG7 Modern, DSEG7 Classic.
- Idle day clock previews the default (18:00), not a hardcoded value.
- **Day # tracker** in top bar → text line `D{n} --- {3|4} min formals` (+ ` : EOD Formal on Morris` when a formal runs).
- **Formal length** (3/4 min) selector in the top bar, default 4.
- Formal/action controls **disabled unless day is running and not paused**.
- **Vote** counts UP with a ms counter that never stops (past the 3s window) for exact screenshot timing; bar centered in the formal panel.
- Settings modal moved out of #stage (container-type was trapping it), px-styled, warn/danger on two rows.

## Backlog / next
- [ ] F3 quick-switch: one-tap 4:00 ↔ 3:00 formal duration (ELO F3 rule)
- [ ] Optional sound cues (60s / zero / vote) — off by default
- [ ] Formal number tracker (F1/F2/F3…) if useful on screen
- [ ] Publish to GitHub Pages (like theme-mod / mafia-tracker)
- [ ] OBS browser-source sizing notes (1920×1080 canvas)
- [ ] Confirm colour palette / font with Lou
- [ ] Night / defense timers — only if the ruleset needs them (currently out of scope)
