# Design System: Tonotype

> Voice as type specimen — a single-page tool that listens to the pitch of your voice and sets each spoken word in a font that matches its tone. This document governs the **chrome** (the page Tonotype lives in). The **specimen pool** (the 25+ fonts that get shown to the user) is intentionally diverse and operates under separate rules — they are the content, not the system.

---

## 1. Visual Theme & Atmosphere

Tonotype sits where a type-foundry catalog meets a recording studio. The atmosphere is **gallery-airy** (density 3), **asymmetric-editorial** (variance 7), and **fluid-restrained** (motion 4). Creativity index 9 — the voice-to-type concept itself carries the gesture, so the chrome stays quiet.

The page reads like a printed specimen book: warm cream paper, near-black ink, a single restrained terracotta accent, generous whitespace around oversized type. Live telemetry reads like instrument panel copy — small mono, sparse, precise. Words drift onto the page like a typesetter pulling sorts from a case: each word rises into place, settles, captions itself in technical mono beneath a hairline divider.

The interface never competes with the captured word. The word is always the loudest thing on screen.

Two modes share this chrome: **free speech** (your words flow onto the page as a foundry-style specimen feed) and **reading** (a literary passage from the in-page library, where each word lights up in its captured font as you read it aloud). Both modes begin with an **8-second voice calibration** — the system samples your pitch range and rebuilds the six buckets around your personal Hz spectrum, so every voice reaches every register.

---

## 2. Color Palette & Roles

All values warm-biased. No pure black, no neon, no purple, no gradients.

- **Paper Cream** (`#efe9de`) — Primary background. Warm off-white that reads as printed paper, not screen. Carries a subtle radial-dot grain (3px raster, `rgba(26,24,21,0.04)`) on a fixed pseudo-element below z-index 2 for tactile texture.
- **Paper Cream Soft** (`#e7e0d3`) — Secondary surface, used sparingly for layered panels (currently reserved; not yet on screen).
- **Charcoal Ink** (`#1a1815`) — Primary text and ink. Near-black with a warm bias. **Never use `#000000`.**
- **Soft Ink** (`#4a443c`) — Secondary body text, sub-copy, italic tagline.
- **Mute Ink** (`#8a8278`) — Caption text, telemetry numbers, axis labels, idle status, ghost-button text.
- **Hairline** (`rgba(26,24,21,0.12)`) — 1px structural dividers: between word-text and word-meta, between scale bucket cells, top/bottom of header/footer bars.
- **Hairline Strong** (`rgba(26,24,21,0.22)`) — Ghost-button borders, slash separators in telemetry copy.
- **Terracotta Accent** (`#c0431f`) — The single accent. HSL(12°, 71%, 44%) — saturation 71% (below the 80% ceiling). Used for: live state (dot, pulse, status text), active bucket highlight, italicized accent word in the hero (`typeface`), BUCKET label in word captions, scale marker. Coverage capped at ~10% of any frame.
- **Terracotta Wash** (`rgba(192,67,31,0.12)`) — Active-bucket cell fill on the pitch scale.

**Banned colors:** any purple, any cyan, any neon. No `#000000`. No multi-stop gradients on text or surfaces.

---

## 3. Typography Rules

Three chrome typefaces hold the page steady. A separate specimen pool of 25 Google Fonts is what the tool **shows**, and is governed by sonic appropriateness, not chrome rules.

### Chrome (governed by this system)

- **Display** — `'Instrument Serif', 'Cormorant Garamond', Georgia, serif`
  - High-contrast editorial display serif. Weight 400 is its natural display weight.
  - Used for: hero headline, brand tagline italic, placeholder copy.
  - Specs: hero `clamp(56px, 8.2vw, 116px)`, line-height 0.96, letter-spacing -0.02em.
  - Italic permitted for emphasis (one accented word per headline maximum).

- **Body** — `'Geist', system-ui, -apple-system, sans-serif`
  - Neutral geometric sans for sub-copy.
  - Specs: 17px, line-height 1.6, color Soft Ink, max-width ~560px (≈56ch), weight 400.

- **Mono** — `'JetBrains Mono', ui-monospace, Menlo, monospace`
  - All telemetry, captions, button labels, brand wordmark, status, bucket metadata, footer copy.
  - Sizes 9–13px. Uppercase with letter-spacing 0.10–0.20em for label-style copy.

### Specimen pool (NOT governed by chrome rules — these are the product)

Six pitch buckets, each with 4–6 fonts loaded from Google Fonts. Specimens are deliberately varied. **Inter is permitted only here**, as one ALTO option, because the purpose of a specimen is to expose the character of a face in use.

The Hz ranges in the table below are the **pre-calibration absolutes** (60 → 1200 Hz, log-equally divided across the six buckets). After the 8-second calibration runs, the buckets become log-equal segments of the *speaker's* range (5th → 95th percentile of their captured pitches, lightly padded). The font pool stays the same; only the Hz boundaries shift. See §11.

| Bucket    | Default Hz range (pre-calibration) | Font pool |
|-----------|------------------------------------|-----------|
| BASS      | ~60–109     | Anton, Archivo Black, Bungee, Bowlby One, Black Ops One |
| BARITONE  | ~109–199    | Playfair Display 900, DM Serif Display, Bodoni Moda 900, Yeseva One |
| TENOR     | ~199–362    | Lora 600, EB Garamond 700, Cormorant Garamond 700, Spectral 700 |
| ALTO      | ~362–659    | Inter 800, Space Grotesk 700, Manrope 800, Work Sans 800 |
| SOPRANO   | ~659–1200 (log-equal slot) | Cormorant Italic 500, Playfair Italic 900, Caveat 700, Parisienne |
| PICCOLO   | (top log-equal slot) | Italianno, Monoton, Faster One, Bungee Shade, Rubik Glitch, Major Mono Display |

### Scale

| Element            | Size                          | Notes |
|--------------------|-------------------------------|-------|
| Hero h1            | `clamp(56px, 8.2vw, 116px)`   | Instrument Serif 400, line-height 0.96 |
| Captured word      | `clamp(72px, 11vw, 168px)`    | Variable face, line-height 0.88, letter-spacing -0.015em |
| Body sub           | 17px / line-height 1.6        | Geist 400, max-width 560px |
| Bucket glyph (Aa)  | 32px                          | Live face preview in scale row |
| Brand tag          | 15px                          | Instrument Serif italic |
| Telemetry / meta   | 9–11px                        | JetBrains Mono uppercase, tracking 0.10–0.20em |

### Banned in chrome
- **Inter** as a chrome face.
- **Times New Roman, Georgia, Garamond, Palatino** as primary faces (system fallbacks only).
- Any monospace other than JetBrains Mono.
- Decorative display faces from the specimen pool used for chrome.

---

## 4. Component Stylings

### Word block — the centerpiece

The captured word and its caption form one `inline-block` unit that flows on the page like type set in a paragraph. **Never wrapped in a card.** Never a tile. The word IS the unit.

- Structure: `N° 003` index (mono 9px, mute) → giant word in its specimen font → 1px hairline divider → `BUCKET · FONT NAME · 247 Hz` meta line in mono 9px uppercase.
- Spacing: 14px between word and divider, 10px between divider and meta, 64px bottom margin, 38px right margin between blocks.
- Reveal: opacity 0 + translateY(10px) animated to settled over 0.55s with `cubic-bezier(0.22, 0.61, 0.36, 1)`. One-shot. No bounce, no overshoot. Words **land**, they don't bounce.
- Punctuation in spoken word is stripped at terminal positions (commas, periods, exclamation marks at the end of a token are removed before render).

### Pitch scale — top-of-page instrument

- Flex row of six bucket cells separated by 1px hairlines. Each cell shows an `Aa` glyph in one of that bucket's specimen fonts (32px), a mono label, and the bucket's current Hz range.
- The scale **rebuilds dynamically**: pre-calibration each cell takes one-sixth of the log range from 60–1200 Hz; post-calibration each cell takes one-sixth of the log range from the speaker's calibrated min → max. Bucket fonts never change — only the Hz boundaries on the labels and the cell widths' implied range.
- Below cells: 2px hairline track with a 2px terracotta marker that slides logarithmically as `smoothedPitch` updates. An 8px terracotta dot sits above the track to mark the position.
- Active bucket gets Terracotta Wash background. Quiet, not screaming.
- Axis labels (6 values across the active range, log-distributed) sit in mute mono 9px below the track. Pre-calibration reads `60 Hz · 109 · 199 · 362 · 659 · 1200 Hz`; post-calibration reads the same shape but in the speaker's range.
- The `.scale-label` heading swaps text accordingly: `pitch spectrum · 60 → 1200 hz · logarithmic` → `your voice · {pMin} → {pMax} hz · logarithmic`.

### Status bar

Top-right of header. The only perpetual motion in the chrome.

- 7px dot: Mute Ink when idle, Terracotta Accent with 1.2s ease-in-out infinite pulse when live. Pulse pairs opacity 1→0.4→1 with an expanding box-shadow ring (0→6px, terracotta-soft fading to transparent).
- Format: `STATUS / pitch HZ / BUCKET` — slash separators in Hairline Strong.
- All copy: mono 10px uppercase, tracking 0.14em.

### Primary CTA — `Start listening`

- Charcoal Ink fill, Paper Cream text, no border, no radius, no shadow.
- 18px vertical / 36px horizontal padding. Mono 12px uppercase, letter-spacing 0.18em, weight 600.
- Hover: `translateY(-2px)` + background fades to Terracotta Accent over 0.25s.
- Disabled (`Connecting…`): opacity 0.7, cursor wait. **No spinner — the copy is the spinner.**

### Ghost button — `Clear specimen`

- Transparent fill, 1px Hairline Strong border, Soft Ink text.
- Hover: inverts to Charcoal Ink fill, Paper Cream text. No translate. 0.2s ease.

### Placeholder — `say something.`

- Instrument Serif italic, `clamp(40px, 5vw, 72px)`, Mute Ink. Shown only between starting listening and the first captured word. Disappears once `main.has-words` is set.

### Header / footer bars

- Paper Cream background, 1px Hairline top/bottom borders, 20px / 40px padding.
- Both: mono 10–11px uppercase, tracking 0.08–0.18em.

### Loaders, errors, empty states

- **No spinners.** **No skeletons.** Loading copy IS the loader (`Connecting…`, `say something.`).
- **Errors render inline** in the status bar as plain mono text (`mic blocked — refresh and allow`). No icon. No toast. No modal.
- **Empty state is the hero.** Reset state is the placeholder.

---

## 5. Layout Principles

- **Vertical band stack, no overlap.** Header → pitch scale → main (hero OR specimen) → footer. Each band owns its row. Nothing absolutely positioned over content. The only fixed-position element is the paper-grain pseudo-element, decorative only, never above interactive content.
- **Max-width 1500px** on main, centered, with 56px horizontal padding on desktop (20px on mobile).
- **Hero is left-aligned.** Max-width 820px. **Never centered.** Variance 7 forbids centered heroes.
- **Specimen feed is inline-block flow** — words wrap naturally like a typeset paragraph. No grid. No columns. No card containers. Asymmetry emerges naturally from each word's intrinsic width (different fonts × different character counts).
- **Pitch scale uses log-proportional `flex-basis`** computed in JS — each bucket cell's visual width matches its Hz range on a log axis. The marker position then aligns naturally with the named bucket.
- **No `calc()` percentage hacks.** Computed widths set directly via inline `flex-basis: Npct;`.
- **No `h-screen`.** Use `min-height: 100vh` on body with `flex-direction: column` and `flex: 1` on main to fill remaining space safely on iOS.

---

## 6. Responsive Rules

Single breakpoint at 720px.

- **< 720px:**
  - Header collapses to vertical stack (brand row, then status row).
  - Italic brand-tag (`voice becomes type specimen`) hides.
  - Scale cells lose Hz range labels (only bucket name remains).
  - Bucket glyph drops 32px → 22px; cells lose padding.
  - Main padding tightens: 40px / 20px.
  - Word block margin tightens: 20px right / 40px bottom.
  - Footer padding: 12px / 18px.
- **All viewports:**
  - No horizontal scroll. Ever.
  - Touch targets ≥44px (CTA and ghost button both exceed).
  - Typography uses `clamp()` with viewport-width middle term. Specimen words top out at 168px, floor at 72px — still aggressively large, because the tool is about type at scale.
- **`prefers-reduced-motion`:** word-rise animation disabled; live-dot pulse disabled. Marker slide retained (it's a status indicator, not decoration).

---

## 7. Motion & Interaction

Tonotype is a tool, not a film. Motion is **load-bearing** — each animation communicates state, never decorates.

| Element                | Property            | Duration | Easing                                 | Loop |
|------------------------|---------------------|----------|----------------------------------------|------|
| Word block reveal      | opacity + translateY| 0.55s    | `cubic-bezier(0.22, 0.61, 0.36, 1)`     | once |
| Live dot pulse         | opacity + box-shadow| 1.2s     | ease-in-out                            | ∞    |
| Scale marker slide     | left                | 0.12s    | ease-out                               | —    |
| Marker opacity         | opacity             | 0.3s     | ease                                   | —    |
| Active bucket fill     | background          | 0.25s    | ease                                   | —    |
| CTA hover              | transform + bg      | 0.25s    | ease                                   | —    |
| Ghost button hover     | all                 | 0.2s     | ease                                   | —    |
| Calibration progress   | width               | 0.12s    | linear                                 | once |
| Reading slot reveal    | opacity             | 0.45s    | ease                                   | once |
| Reading slot color     | color               | 0.45s    | ease                                   | once |
| Reading slot caption   | opacity             | 0.5s     | ease                                   | once |
| Passage card hover     | bg + color + border + transform | 0.22s | ease                       | —    |

- **No spring physics on chrome.** This is editorial typography, not a consumer toy. Easings stay quiet — `cubic-bezier(0.22, 0.61, 0.36, 1)` for the only featured curve (a soft settle, no overshoot).
- **Hardware-accelerated properties only:** `transform`, `opacity`, `background-color`, `box-shadow`. The scale marker's `left` is the documented exception — it's a fixed-track status indicator, 0.12s, single property, negligible layout cost.
- **Only one infinite loop:** the live-dot pulse. It communicates "we're listening" — anything else looping would be decorative noise.

---

## 8. Anti-Patterns (Banned)

Enforced. Anything matching this list is a failure of the system.

### Visual / typography
- ❌ Emojis anywhere
- ❌ `Inter` font in chrome (allowed only as one ALTO specimen)
- ❌ Generic serifs (Times New Roman, Georgia, Garamond, Palatino) as chrome
- ❌ Pure black `#000000` — use Charcoal Ink (`#1a1815`)
- ❌ Neon glows / static outer glows (the live-dot's expanding ring is animated, not a static aura)
- ❌ Oversaturated accents (Terracotta caps at 71%; anything ≥80% rejected)
- ❌ Gradient text on headlines (solid Terracotta on one italic word, that's the limit)
- ❌ Custom mouse cursors
- ❌ Multi-stop gradients on surfaces

### Layout
- ❌ Centered hero (variance 7 forbids it)
- ❌ Overlapping elements (every element gets its own spatial zone; paper-grain is decorative-only)
- ❌ Card containers around word blocks (the word IS the unit)
- ❌ 3-column equal grids
- ❌ `h-screen` (use `min-height: 100vh` + flex)
- ❌ `calc()` percentage hacks (use computed `flex-basis`)
- ❌ Horizontal scroll on mobile
- ❌ `position: absolute` content stacking on other content

### Motion
- ❌ Spring overshoot in chrome
- ❌ Infinite loops on anything except the live-dot pulse
- ❌ Bouncing arrows, scroll indicators, "scroll to explore"
- ❌ `top` / `left` animations on layout content (scale marker is the documented exception)

### Copy
- ❌ AI clichés: "Elevate," "Seamless," "Unleash," "Next-Gen," "Revolutionary," "Unlock the power of"
- ❌ Filler UI text: "Scroll to explore," "Swipe down," bouncing chevrons
- ❌ Fabricated metrics, fake percentages, invented uptime/response numbers
- ❌ `LABEL // YEAR` formatting (`SYSTEM // 2026`)
- ❌ Generic placeholder names ("John Doe," "Acme," "Nexus")
- ❌ Fake round numbers (99.99%, 50%)
- ❌ Broken Unsplash links — none, since this design ships with zero images

---

## 9. Voice & Copy

- **Mono labels:** lowercase, sparse, precise. `listening`, `offline`, `connecting`, `pitch spectrum · 60 → 1200 hz · logarithmic`.
- **Narrative copy:** sentence-case Geist for body, Instrument Serif italic for tagline / placeholder.
- **Tone:** matter-of-fact. `say something.` not `Begin your voice journey.`
- **Telemetry separators:** `·` (middot) and `/` (slash). Never em-dashes or commas in mono telemetry.
- **Captions:** `N° 003` numeric notation, foundry-catalog style. Zero-padded to 3 digits.
- **Pitch:** integer Hz only. Never decimals. Always followed by `Hz` with a space.
- **Bucket names:** uppercase. BASS / BARITONE / TENOR / ALTO / SOPRANO / PICCOLO. Never abbreviated.

---

## 10. File & Asset Conventions

- **Single-file delivery.** `index.html` with inline CSS and JS. No build step, no bundler, no framework, no `node_modules`.
- **Fonts:** one Google Fonts `<link>` with `display=swap`. All specimen + chrome faces preloaded via a hidden `.preload` div on boot so the first captured word never falls back to a system face.
- **No image assets.** All visual character comes from typography and a single radial-dot CSS pattern.
- **Browser APIs only:** Web Audio API (mic + analyser) + Web Speech API (`webkitSpeechRecognition`) + vanilla DOM. No external JS dependencies.
- **Serving:** mic requires a secure context. Use `python3 -m http.server 8000` and open `http://localhost:8000`. `file://` will not work.
- **Path:** `/Users/zop.dev/rav/tonotype/index.html`

---

## 11. Calibration, Modes & the Reading Library

Tonotype runs a small state machine. The chrome shows different surfaces depending on which phase the session is in. The phase transitions are deliberate and one-directional — there is no back button.

### Session phases

| Phase           | Trigger                       | UI shown                              | Speech captured? |
|-----------------|-------------------------------|---------------------------------------|------------------|
| `idle`          | page load                     | Hero + passage selector chips         | no               |
| `connecting`    | user starts a session         | "Connecting…" on the CTA              | no               |
| `calibrating`   | 350ms after audio open        | Calibration banner (8s progress)      | no — discarded   |
| `capturing`     | calibration completes         | Specimen feed (free) or passage (reading) | yes          |

The transition `calibrating → capturing` is the moment the pitch scale rebuilds and the speech engine's output starts flowing into the active mode. There is no other state-changing transition during the session.

### Two modes

- **`free`** — Default. Triggered by clicking `Start listening`. After calibration, the placeholder reads `say something.`; every spoken word commits a new `word-block` into the specimen feed.
- **`reading`** — Triggered by clicking a passage card in the hero. After calibration, the chosen passage renders as a sequence of `r-slot` units laid out as type. As the speaker reads each word, the next unmatched slot transforms from a muted serif into its captured specimen font.

### Passage card (hero)

The library lives directly in the hero, below the `Start listening` CTA, separated by a 1px hairline.

- Label above the cards: `— or read aloud —` in mono 10px uppercase, tracking 0.2em, Mute Ink.
- Cards: 4 of them, in a wrapping flex row with 10px gap. Each card is a button with:
  - Border: 1px Hairline Strong.
  - Padding: 14px / 20px. Minimum 64px tall.
  - Top line: source in mono 9px uppercase, tracking 0.18em, Mute Ink (`SHAKESPEARE`).
  - Bottom line: title in Instrument Serif italic 19px, Charcoal Ink (`Sonnet XVIII`).
  - Hover: inverts to Charcoal Ink fill, Paper Cream text. Source line goes to Paper Cream at 60% opacity. `translateY(-2px)`. 0.22s ease on all properties.
- Click commits the user to reading mode for that passage. The hero hides (`.listening` class), then calibration begins, then the passage renders.

### Calibration banner

Shown only when `main.calibrating`. Max-width 620px, 56px / 64px vertical margin.

- **Label:** mono 10px uppercase, tracking 0.22em, **Terracotta Accent**. Default text `Warming the type case`. In reading mode: `Warming up · then {source} — {title}`.
- **Hint:** Instrument Serif italic `clamp(28px, 3.6vw, 44px)`, line-height 1.15, Soft Ink. Default: `Talk naturally for a few seconds — anything at all. Tonotype is learning the shape of your voice.` In reading mode: `Talk for a few seconds — anything at all. After this the page switches into reading mode.`
- **Progress bar:** 2px-tall Hairline track. Inside, a Terracotta fill grows from 0% → 100% over 8 seconds (`transition: width 0.12s linear`).
- **Foot:** mono 9px uppercase, tracking 0.18em, Mute Ink. Reads `Calibrating · {N.N}s / 8.0s` and updates every frame.
- During calibration the placeholder, specimen feed, and reading container are all hidden — only the banner is visible in main.

### Reading slot — `r-slot`

The reading-mode equivalent of a `word-block`. Inline-flex column, vertical-align top, margin `0 22px 36px 0`. Three states, mutually exclusive:

| State     | Word color | Word style                    | Opacity | Caption |
|-----------|-----------|-------------------------------|---------|---------|
| pending   | Mute Ink  | Instrument Serif italic 400   | 0.38    | hidden  |
| matched   | Charcoal Ink | specimen font (per pitch)   | 1.00    | shown — bucket + font + Hz |
| skipped   | Mute Ink  | Instrument Serif italic 400   | 0.18    | hidden  |

- **Word size:** `clamp(36px, 5.2vw, 76px)` — smaller than free-mode (which goes up to 168px) because a whole passage needs to fit on screen.
- **Color and opacity transitions:** 0.45s ease on `color` and `opacity` when state changes. The slot fades in to its specimen font as it matches; there's no scale, no slide, no overshoot.
- **Caption:** identical structure to `word-meta` (BUCKET tag in terracotta, font name in ink, Hz in mute), revealed via opacity 0 → 1 over 0.5s when the slot matches.
- **`pending` is the default visual** — it sets the editorial tone of the passage before the reader gets to each word.
- **`skipped`** is reserved for slots the speaker skipped over within the 3-word lookahead window. The slot stays in place, visually present but more recessed. This is intentional — the reader can *see* what they missed.

### Reading header — `reading-header`

Sits above the passage. Mono 10px uppercase, tracking 0.2em, with a 1px Hairline bottom border and 36px margin-bottom.

- `<source>` — source name in Charcoal Ink (`SHAKESPEARE`).
- `<title>` — Instrument Serif italic 17px, normal-case, Soft Ink (`Sonnet XVIII`).
- `<progress>` — right-aligned via `margin-left: auto`. Reads `{matched} / {total}`. Updates after each slot match.

### Word-matching logic

Spoken-to-slot matching is governed by three rules:

1. **Normalization:** both spoken word and slot word are lowercased and stripped to `[a-z']` for comparison. `summer's` → `summer's`; `I'll` → `i'll`.
2. **Levenshtein tolerance:** distance ≤0 for ≤2-char words, ≤1 for 3–5 chars, ≤2 for 6+ chars. Short words must match exactly; longer words forgive typos.
3. **3-word lookahead:** if the spoken word doesn't match the cursor slot, the matcher checks the next 2 slots. A match at offset N marks slots `cursor..N-1` as `skipped` and advances the cursor to `N+1`. The reader can mis-read or skip without breaking the flow, within a small window.

### Mode-related CSS class hooks

| Class on `main`     | Meaning                          | Effect                                          |
|---------------------|----------------------------------|-------------------------------------------------|
| `.listening`        | Session is active (any phase ≠ idle) | Hides hero, keeps everything else available |
| `.calibrating`      | Inside the 8s calibration phase  | Shows `.calibration`; hides placeholder, specimen, reading |
| `.has-words`        | At least one word landed in free mode | Hides placeholder                          |
| `.reading-mode`     | Reading mode session             | Shows `.reading`; hides specimen + placeholder  |

### Anti-patterns specific to modes

- ❌ No "skip calibration" button. The 8 seconds are non-negotiable and load-bearing — without them the user's voice doesn't fit the buckets.
- ❌ No multi-mode toggling mid-session. Switching from free → reading requires starting a new session (clearing state).
- ❌ No fake calibration. The progress bar fills at exactly real elapsed time; the percentiles are computed from real samples. If fewer than 30 samples come in (the speaker stayed silent), the system falls back to the absolute 60→1200 range rather than fabricating a calibration.
- ❌ No passage cards filled with fake metadata. Source + title only. No author bio, no year, no AI-generated descriptions.
- ❌ No passages outside the public domain or used without attribution. Current four (Shakespeare, Rumi, Whitman, Sappho) are all PD.

---

## 12. Loudness, Letter Morphing, Custom Text & Poster Export

Four extensions to the core system. Each adds something concrete the user can feel without re-architecting the chrome.

### 12.1 Loudness → font size (second axis)

Pitch picks the **font**; loudness now picks the **size**. The two axes are orthogonal — same voice, same word, different scale depending on how loud you said it.

- **Signal:** RMS amplitude of the time-domain buffer, computed every detection frame (≈25 Hz), smoothed via exponential moving average (`α = 0.4`), stored in `rmsHistory` alongside `pitchHistory` for the last 3 seconds.
- **Snapshot:** at word commit, take the median RMS over the last 1000ms (`getRecentMedianRMS`).
- **Mapping:** log-scaled from `0.005` (whisper floor) to `0.25` (shout ceiling), producing a multiplier in `[0.55, 1.65]`. Applied via a CSS custom property `--size` on the word element.
- **CSS hook:** `.word-text` and `.r-word` use `font-size: calc(var(--size, 1) * clamp(...))`. Default `--size` is `1` (no effect when calibration isn't running yet, or when commit happens without a snapshot — defensive fallback).
- **Range rationale:** 0.55× → 1.65× is a 3× ratio between whisper and shout. Larger than that breaks the page layout; smaller than that doesn't read as a different word.
- **Reading-mode behavior:** identical mapping in both modes. A whispered word in a passage shrinks; a shouted one expands the row.
- **Banned:** mapping loudness to color, weight, or any second visual property. The size axis is one channel — adding more turns the page into a slot machine.

### 12.2 Sub-word letter morphing

When pitch varies enough mid-word, the **letters** within the word land in different fonts — not the whole word.

- **Trigger condition:** word has ≥3 alphabetic characters AND the per-letter pitches resolve to ≥2 distinct buckets. Words below the threshold render single-font as before. About 25–40% of multi-syllable words morph in practice.
- **Per-letter pitch estimation:** speech recognition gives only word-level timing. To get per-letter pitches, we estimate the word's spoken duration as `clamp(300, 110 × len, 1200)` milliseconds, anchored to "now" (the moment the word arrives). Each letter's pitch is the closest sample in `pitchHistory` to the center of its slot.
- **Rendering:** the word `<span class="word-text">` is filled with `<span class="letter">…</span>` per character. Each letter gets its own `font-family / weight / style`. Non-alphabetic characters (apostrophes, punctuation) get a letter span too but inherit the default font (no bucket assignment).
- **Caption when morphed:** `MORPH · BUCKET → BUCKET · minHz → maxHz`. The `MORPH` tag replaces the bucket-name tag in the caption (still terracotta — uses the `.bucket-tag.morph` modifier). The font name slot becomes the arrow-joined bucket sequence (`BASS → ALTO`).
- **Reading mode:** identical treatment. `transformSlot` clears the slot's word text and rebuilds it from letter spans when morphing.
- **Punctuation handling:** when the raw token is something like `summer's?`, the apostrophe and question mark get inert letter spans (no font set) sandwiched between the alphabetic letters. The morph keeps reading correctly.
- **Limit:** the system never renders more than 6 distinct fonts in one word (because there are only 6 buckets). In practice 2–3 is typical.
- **Banned:** morphing for cosmetic reasons. The threshold is a real signal threshold; morphing on every word would turn each word into a circus.

### 12.3 Custom passage modal

A 5th card in the passage selector — labeled `CUSTOM · Paste your text` — opens a modal where users can supply their own text for reading mode.

- **Trigger:** click the `.passage-card-custom` card (always rendered last in the row, with a dashed border to distinguish it from the literary cards).
- **Modal structure:** fixed overlay (`.cm-overlay`) with `rgba(26,24,21,0.5)` backdrop, centered cream panel (`.cm-panel`, 1px Charcoal Ink border, deep diffused shadow). Closes on Cancel click, on backdrop click, on Escape key. Commits on `Cmd/Ctrl + Enter` or "READ THIS ALOUD" click.
- **Panel content:**
  - **Label** (`.cm-label`): mono 10px uppercase, terracotta — `YOUR TEXT`
  - **Hint** (`.cm-hint`): Instrument Serif italic, `clamp(22px, 2.6vw, 30px)` — `Paste a poem, a passage, a vow, a lyric — anything you want to read aloud.`
  - **Textarea** (`#customText`): Geist 16px, 180px tall, no border except a 1px Charcoal Ink underline that turns terracotta on focus. Placeholder uses Instrument Serif italic 18px — Rumi quote by default.
  - **Error line** (`.cm-err`): mono 10px uppercase, terracotta — `NEEDS AT LEAST THREE WORDS.` Hidden until validation fails.
  - **Actions:** ghost Cancel + primary "READ THIS ALOUD" (slightly tighter padding than the main CTA).
- **Validation:** minimum 3 words (whitespace-split, non-empty). Below threshold, the error line fades in. Submitting clears the error on next input.
- **Flow on submit:** modal closes, `state.passage` is built directly from the user's text (no PASSAGES lookup), then `startSession('reading')` runs without a `passageId` — `startSession` treats a missing passageId in reading mode as "passage already provided externally."
- **Header in reading view:** `CUSTOM` source, `Your text` title.
- **Banned:** persisting custom text across sessions (no localStorage). Treat each session as ephemeral — the user can re-paste if they want it again. No fake "Save as a card" UI.

### 12.4 Save-as-poster (canvas PNG export)

A second footer button — `Export poster` — renders the current captured words onto a 1200×1600 PNG and triggers a download.

- **Trigger:** click `#exportBtn` in the footer (left of `Clear specimen`).
- **Output:** PNG, 1200 × 1600 pixels (3:4 ratio, prints cleanly to letter or A4). Filename: `tonotype-{YYYY-MM-DD}-{NNN}.png`.
- **Rendering path:** native `<canvas>` 2D context. No external libraries. `await document.fonts.ready` runs first so canvas can resolve the specimen fonts. Subtle grain is drawn as ~1800 random 1px dots at `rgba(26,24,21,0.035)`.
- **Layout:**
  - **Top:** 80px margin → `TONOTYPE` mono bold + `SPECIMEN N° NNN · YYYY-MM-DD` mono mute on the next line → 1px Hairline Strong divider
  - **Body:** each captured word at 92px font-size in its actual specimen font (morphed words render letter-by-letter via `getComputedStyle` on each `.letter` span) → 1px Hairline divider under the word (sized to the word's measured width + 24px) → mono caption beneath
  - **Footer:** 1px Hairline Strong → `VOICE → TYPE SPECIMEN` left, `TONOTYPE` right, both mono mute
- **Overflow:** if words exceed canvas height, the renderer stops and writes `+ N more` in Instrument Serif italic at the cut-off point. No silent clipping.
- **Loudness on export:** **ignored**. All words on the poster render at the same 92px size — the canvas is a static deliverable, and size variance would make the layout impossible to compose. The captured RMS data is intentionally dropped here.
- **Empty state:** if no words have been captured yet, the button label briefly flips to `Speak something first` for 1.6 seconds, no download triggered.
- **Banned:** watermarks, URLs, branding chrome beyond the existing TONOTYPE wordmark and footer label. No QR codes. No "Made with…" footer. The poster reads as a foundry catalog page.

### Updated motion entries

Add to §7 motion table (already done in the file):

| Element                | Property            | Duration | Easing                                 | Loop |
|------------------------|---------------------|----------|----------------------------------------|------|
| Custom modal overlay   | opacity             | 0.22s    | ease                                   | once |
| Custom modal panel     | transform (translateY) | 0.3s | `cubic-bezier(0.22, 0.61, 0.36, 1)`    | once |
| Error line             | opacity             | 0.2s    | ease                                    | —    |

### Updated mode CSS class hooks

| Class on `main`     | Meaning                          | Effect                                          |
|---------------------|----------------------------------|-------------------------------------------------|
| `.cm-overlay.open`  | (on overlay, not main) — modal visible | Backdrop + panel shown, panel translates in |

---

## 13. Variable Specimen Mode (Fraunces, 4 axes)

The platonic version of the voice-to-type idea. The bucket system in modes free + reading is a discrete approximation — a 6-position rotary switch. Variable mode replaces those 6 switches with **4 continuous dials**, each driven by a different signal in the voice, all routed into the axes of a single variable typeface. Same word, same speaker, same pitch range — but every utterance produces a *literally unique* font instance.

### Font choice — Fraunces

`Fraunces` (Undercase Type, Phaedra Charles & Flavia Zimbardi via Google Fonts) is the typeface. Editorial serif, in the same lineage as Instrument Serif (the chrome display face) so the variable rendering doesn't feel foreign next to the rest of the page. It exposes four interpolation axes:

| Axis    | Range          | Visual effect                                                          |
|---------|----------------|------------------------------------------------------------------------|
| `wght`  | 100 → 900      | Thin hairline → heavy black weight                                     |
| `opsz`  | 9 → 144        | Caption-grade (open, low contrast) → display-grade (refined, high contrast) |
| `SOFT`  | 0 → 100        | Sharp Didone-style terminals → rounded humanist softening              |
| `WONK`  | 0 → 1          | Geometric calm → twisted "wonky" alternate forms                       |

Loaded via `Fraunces:opsz,wght,SOFT,WONK@9..144,100..900,0..100,0..1`. Three sample instances (light-caption, mid, heavy-display-soft-wonky) are preloaded on boot so the first rendered word lands in the real face, not a fallback.

### Mapping — voice signals → axes

Four independent signals, four independent axes. Each mapping is a closed-form function of one input signal; no axes share inputs.

| Signal                                | Function                                      | → Axis  |
|---------------------------------------|-----------------------------------------------|---------|
| **Pitch snapshot** at word commit     | inverse-log mapping over calibrated range     | `opsz`  |
| **RMS amplitude** (median, 1000ms)    | log mapping, `[0.005, 0.25]` → `[200, 900]`   | `wght`  |
| **Per-word pitch variance** (coefficient of variation) | `cv × 350`, capped at 100      | `SOFT`  |
| **Pitch direction-change count** within the word | `Math.min(1, changes / 3)`         | `WONK`  |

#### Pitch → opsz (inverse)

```
t = (log(pitch) - log(min)) / (log(max) - log(min))   // 0 at speaker's lowest, 1 at highest
opsz = 144 − t × 135                                  // 144 (display) → 9 (caption)
```

Low pitch = full display-size letterforms (massive contrast, refined). High pitch = caption-size letterforms (open, lower contrast). This is **inverted from intuition** — louder/lower voice gets the "important" letterforms — but it matches how editorial type works: display sizes for headlines, caption sizes for footnotes. A whisper at high pitch feels caption-like; a bass declaration feels headline-like.

#### RMS → wght

Same log mapping as the loudness-to-font-size axis in free/reading mode. Whispered words get `wght 200`, shouted words get `wght 900`.

#### Pitch variance → SOFT

The within-word pitch coefficient of variation (standard deviation / mean) over the alphabetic letters' sampled pitches. A monotone "the" has `cv ≈ 0` → `SOFT 0` (sharp Didone terminals). A wavering "well…" has `cv ≈ 0.15` → `SOFT 50` (rounded edges). Cap at 100.

The signal is the **same data** used by sub-word morph detection, but expressed as a continuous magnitude instead of a discrete trigger.

#### Direction changes → WONK

Count the times the pitch contour reverses direction within the word, ignoring movements smaller than 4 Hz to filter noise. 0 changes = `WONK 0`. 3+ changes (true vibrato) = `WONK 1`. Words said with a single rising or falling contour stay prim; words said with a wobble go wonky.

### Caption — axis readout

Variable mode words show their axis instance values instead of a font name:

```
FRAUNCES    wght 540 · opsz 36 · SOFT 22 · WONK 0.20    ·    247 Hz
```

- `FRAUNCES` in terracotta (replacing the bucket label) — anchors the family.
- Axis keys (`wght`, `opsz`, `SOFT`, `WONK`) in Mute Ink, values in Charcoal Ink. Tabular numerals (`font-feature-settings: "tnum" 1`) so the digits don't shift width as values change.
- `·` separators in Hairline Strong.
- Pitch in Hz at the end, same format as other modes.

The caption reads like instrument telemetry — exactly the four numbers needed to reproduce the font's geometry. No font name to memorize; the readout *is* the font.

### Secondary CTA

The hero's "Start listening" button is wrapped in a `.start-row` flex container. Beside it sits a `.start-link` button:

```
[ START LISTENING ]   or start in italic-variable-italic mode →
```

- Style: `'Instrument Serif' italic 18px`, Soft Ink, no border, no background. The word `variable` itself is wrapped in `<em>` for italic-within-italic emphasis (same trick as the brand tagline). Hover shifts color to Terracotta and slides the arrow +4px right.
- Function: identical to the primary CTA but starts `startSession('variable')` instead of `startSession('free')`. Same 8-second calibration. Same continuous-flow specimen feed after. Only the per-word rendering differs.

### Trade-offs

- **Poster export preserves `wght` only.** Canvas 2D doesn't reliably support `font-variation-settings`. Variable words export to PNG in Fraunces at the correct weight (because `wght` is also set via the standard `font-weight` property), but `opsz`, `SOFT`, and `WONK` are dropped. The on-page rendering is the live specimen; the poster is the static summary. **The caption on the poster still shows the full axis readout** — so the four numbers survive even if the rendering doesn't.
- **No bucket label on variable words.** The word doesn't "belong" to a bucket — it belongs to a position in 4D axis space. The pitch scale's bucket highlighting still tracks the live pitch (the scale is global state), but variable words themselves don't carry a `BUCKET` tag.
- **Variable mode + reading mode are mutually exclusive.** The reading library uses bucket fonts; variable is a separate session type. To read a passage in variable type, you'd need to extend either mode to call `renderVariableWord` per matched slot — not done in v0.4, left as future work.

### Anti-patterns specific to variable mode

- ❌ No spring-animated transitions between word instances. Each word lands at its computed axis values and stays there. Interpolating between sequential words would look like a synth glissando — pretty but signal-destroying.
- ❌ No "axis sliders" UI letting the user manually override values. The whole point is *voice → axes*. Manual control turns it into a font editor.
- ❌ No additional axes beyond the four. Fraunces also has italic, but mixing in a 5th channel (e.g., "tilt detection on phone gyro → ital") muddies the conceptual cleanliness. Four voice signals, four font axes. One-to-one.
- ❌ No font swap mid-session. Once variable mode starts, it's Fraunces until the session ends. Swapping typefaces during a session would defeat the "one instrument the voice plays" idea.

### CSS hooks

| Selector                       | Purpose                                                    |
|--------------------------------|------------------------------------------------------------|
| `.start-row`                   | Flex container wrapping primary + secondary CTAs           |
| `.start-link`                  | Secondary CTA — italic Instrument Serif, hover-shifts arrow|
| `.word-text.variable`          | Variable-mode word rendering — `font-family: 'Fraunces'`   |
| `.r-word.variable`             | Reserved for future reading-mode integration               |
| `.word-meta .axis-readout`     | Container for the four axis key+value pairs                |
| `.word-meta .axis-key`         | Lowercase axis label (`wght`, `opsz`, etc.), Mute Ink      |
| `.word-meta .axis-sep`         | `·` separator between axis pairs, Hairline Strong          |

---

## 14. v0.5 — Distribution, Performance, Multi-speaker, Continuity

Six extensions that move Tonotype from "fun demo" toward real applications: live performance, shareable artifacts, returning-user continuity, multi-speaker conversations, and language presets for tonal voices.

### 14.1 Stage mode (live performance)

A chrome-less black-canvas full-screen mode for projection, gallery installation, and stage backdrop.

- **Toggle:** `Stage mode` button in footer-actions (leftmost). Click swaps the page to `body.stage-mode`. Esc key or the floating `Exit stage · esc` chip (top-right, 1px border, mono caps) restores normal mode.
- **Palette in stage mode:** background `#0c0b09` (deeper than Charcoal Ink, true near-black for projection contrast), ink `#efe9de` (Paper Cream as cream-on-black), Terracotta Accent unchanged. Paper grain pseudo-element inverts to cream dots on black at `rgba(239, 233, 222, 0.04)`.
- **Hidden in stage:** `.topbar`, `.scale`, `.bottombar`, `.hero`, `.lang-picker`, `.returning-banner`, calibration foot copy. Specimen and reading slots remain.
- **Word sizing:** `clamp(96px, 14vw, 220px)` in free + variable; `clamp(56px, 7.5vw, 110px)` for matched reading slots. Larger than the page-mode sizes — the viewer is further away.
- **Mouse-idle behavior:** after 2.5s of no mouse movement, the `Exit stage` chip fades over 1.2s. Any mouse-move brings it back. This is the only "auto-hide" in the whole system — justified because the affordance would otherwise compete with the projected typography.
- **Banned:** no chrome reappears in stage mode for any reason. No notification toasts, no calibration UI text, no progress bars. The performer sees ONLY their captured words.

### 14.2 Phoneme-aware letter spacing

Sub-word morphed and variable-mode words include heuristic phoneme classification per letter, surfaced as breathing room around vowels.

- **Classifier:** `/[aeiouAEIOU]/.test(ch) → 'vowel'`, else `[a-zA-Z] → 'consonant'`, else `null` (no class). Heuristic only — true phoneme detection needs a small ML model; this captures the visual intent.
- **CSS:** `.letter.vowel { padding-left: 0.02em; padding-right: 0.04em; }` opens vowels slightly. `.letter.consonant { letter-spacing: -0.005em; }` tightens consonant clusters. Effect is subtle but readable — looks like the typographer breathing where the voice breathes.
- **Where it applies:** only when a word renders letter-by-letter (sub-word morph in free + reading modes). Single-span words and variable-mode words don't get per-letter classes — they have other expressive channels (axis or font swap).
- **Not real phonemes:** `y` is always consonant here (vs. its sometimes-vowel role); `e` at the end of a silent word still counts. This is intentional — visual intent over linguistic accuracy.

### 14.3 Tonal-language presets

A 6-option language picker bar sits between the pitch scale and the main column. Each preset adjusts the *pre-calibration envelope* (the assumed Hz range for that language's typical voice).

| ID  | Language    | minHz | maxHz | Why |
|-----|-------------|-------|-------|-----|
| EN  | English     | 60    | 1200  | Default. Wide range, equally distributed. |
| ZH  | Mandarin    | 75    | 1500  | Tonal — wider top end captures 4-tone contours. |
| VI  | Vietnamese  | 75    | 1500  | Tonal — six tones, similar envelope. |
| YUE | Cantonese   | 75    | 1500  | Tonal — nine tones, similar envelope. |
| ES  | Spanish     | 55    | 900   | Narrower range, deeper average. |
| JA  | Japanese    | 80    | 1100  | Pitch accent rather than tone. Less expansive. |

- **UI:** `.lang-picker` bar — `LANGUAGE · EN ZH VI YUE ES JA` in mono 10px caps. Active language gets Terracotta Accent text + `--accent-soft` background. Hover lifts to Charcoal Ink.
- **Disabled during session:** language can't be changed while `state.listening`. Switching forces a recalibration (clears `pMin/pMax`).
- **Scale label reflects language:** `mandarin · 75 → 1500 hz · logarithmic` pre-calibration; flips to `your voice · {pMin} → {pMax} hz · logarithmic` after.
- **Banned:** language-specific font choices (e.g., switching to CJK serifs when ZH is picked). The specimen pool stays the same — *only* the pitch envelope shifts. The tool is voice-shape-as-type, not script-as-type.

### 14.4 Voice fingerprint persistence

The calibrated range from any session is saved to `localStorage` (`tonotype.fingerprint.v1`) with a 30-day TTL. On return, the page picks up where you left off — no recalibration needed.

- **Stored shape:** `{ pMin, pMax, lang, savedAt, sessions }`. ~80 bytes. No PII, no audio, no recordings.
- **On boot:** if a fresh fingerprint exists, restore `state.calibration.pMin/pMax` + `state.language`, show the **Returning voice banner** in the hero.
- **Returning banner:** dashed-border block under the CTAs. `Returning voice 142 – 380 Hz   last seen 3d ago · 4 sessions   forget & recalibrate`. Terracotta key, Mute Ink metadata, underlined ghost link to forget. Display is `none` by default, swapped to `flex` via `.show`.
- **Skip calibration on return:** when starting a session with an existing fingerprint, the 8s calibration step is bypassed entirely — straight from `connecting → capturing`. Scale rebuilds with the cached range so the marker positions correctly.
- **Forget link:** clicking `forget & recalibrate` removes the localStorage entry, clears `pMin/pMax`, hides the banner, and rebuilds the scale to defaults. Next session runs the full 8s calibration.
- **Banned:** persisting word data, audio buffers, transcripts, or any per-session content. Only the voice-range *envelope* persists. No "history" tab. No "your past specimens." Each session is ephemeral except for calibration.

### 14.5 Shareable session URL

A `Share` button in the footer encodes the current specimen feed into the URL hash. Opening that URL elsewhere reconstructs the page without needing a microphone.

- **Encoding:** `btoa(unescape(encodeURIComponent(JSON.stringify(payload))))` → URL hash `#s=<base64>`. The payload includes mode, language, calibrated range, and per-word `{ word, speaker, variable flag, font props, variation settings, --size, caption text }`. ~500–2000 bytes for a typical specimen.
- **Trigger:** click `Share` in `.footer-actions`. The URL is copied to clipboard via `navigator.clipboard.writeText`. A `.share-toast` confirmation slides up from the bottom: `— Specimen link copied`. Em-dash separator, not a checkmark (no emojis).
- **Empty-state guard:** if there are no captured words yet, the toast reads `Speak something first`. No copy happens.
- **Restore on load:** `tryRestoreFromHash` runs at boot. If `location.hash.startsWith('#s=')`, decode and call `restoreSpecimen(payload)`. The page hides the hero, marks `.listening + .has-words`, sets the language pill, restores the calibrated range to the scale, and renders each word block from the payload.
- **Restoration limits:** restored specimens are read-only artifacts — clicking `Start listening` works to add new words, but the existing words came from someone else's voice. Status reads `viewing shared specimen` instead of `listening`.
- **Banned:** server-side persistence, account systems, "save to gallery." The URL hash IS the share — no database, no auth. The receiver gets the bytes; they don't get to leak.

### 14.6 Multi-speaker detection (2 speakers)

A second voice talking on the same mic gets distinguished automatically. Each detected speaker carries a different ink shade.

- **Algorithm:**
  1. Maintain a 2.5s rolling median of pitch samples (median of `pitchHistory.filter(s => s.t >= ts - 2500)`).
  2. Track `baselineA` and `baselineB` — slow EMAs (`α = 0.03`) of the median when each speaker is active.
  3. If the current median is >0.30 in log-space distance from the current speaker's baseline (≈ a perfect fourth interval) AND closer to the other speaker's baseline, register a switch.
  4. Cooldown: 1.2s between switches, so a single wobble doesn't trigger.
  5. First time a sustained baseline >0.30 away from A appears, register speaker B and set `state.speakers.detectedTwo = true`.
- **Hard cap of 2 speakers.** A conversation is 2 voices in this system; 3+ would dilute the visual differentiation and complicate the bucket logic.
- **Visual differentiation:** Speaker A uses Charcoal Ink (`var(--ink)`) for word text. Speaker B uses Soft Ink (`var(--ink-soft)`) — lighter ink without changing color. A Terracotta em-dash prefix sits before B's word index (`— N° 014`).
- **CSS:** `.word-block.speaker-b .word-text` overrides color. Same for `.r-slot.speaker-b .r-word`. Stage mode applies analogous lighter cream for B (`rgba(239, 233, 222, 0.6)` instead of full cream).
- **Banned:** speaker A vs B can't both use Terracotta (that's a reserved accent). No additional fonts per speaker — the differentiation is *only* the ink shade and the em-dash prefix. Loudness and pitch still drive the same axes for both speakers.

### Deferred (next session, not v0.5)

- **Per-word audio replay** — needs `MediaRecorder` with per-word time slicing, audio buffer slicing on each commit, click handler on each word for playback. ~250 LOC and substantial test surface (mic + speech recog + audio playback overlapping).
- **Animated session export (MP4/GIF)** — needs `canvas.captureStream()` + audio mixing + WebM-to-MP4 fallback. ~400 LOC and browser-compatibility headaches. Worth a dedicated session.

### Updated mode CSS class hooks

| Class on `body` or `main`     | Effect                                                  |
|-------------------------------|---------------------------------------------------------|
| `body.stage-mode`             | Chrome-less near-black canvas, huge cream type          |
| `body.stage-mode.idle-mouse`  | Exit chip fades after 2.5s of no mouse movement         |
| `.word-block.speaker-b`       | Speaker B styling — Soft Ink word, em-dash before index |
| `.r-slot.matched.speaker-b`   | Speaker B styling in reading mode                       |
| `.returning-banner.show`      | Fingerprint-restored returning banner visible           |
| `.share-toast.show`           | Copy-confirmation toast visible (auto-hides 2.2s)       |
| `.lang-option.active`         | Current language preset highlighted                     |

### Updated motion table additions

| Element              | Property            | Duration | Easing                                 | Loop |
|----------------------|---------------------|----------|----------------------------------------|------|
| Stage exit fade-out  | opacity             | 1.2s     | ease (triggered by 2.5s mouse idle)    | once |
| Share toast slide-in | transform + opacity | 0.3s     | `cubic-bezier(0.22, 0.61, 0.36, 1)`    | once |
| Language pill        | bg + color          | 0.2s     | ease                                   | —    |
| Word color (Speaker B transition) | color  | 0.4s     | ease                                   | once |

---

## 15. Practice Mode — Pitch-Target Vocal Training

The product pivot. Tonotype as creative tool becomes Tonotype as **practice tool**: a 4th session type alongside free / reading / variable, designed for vocal training (trans voice, Parkinson's rehab, vocal coaching, mood journaling). The same editorial chrome, the same engine, but framed around a concrete goal: *stay in a target pitch range for a set time, see how you did, track progress over sessions.*

### Why this exists

Vocal training apps today are dominated by line-chart spectrograms — clinical, intimidating, ugly. Trans-voice practice apps in particular are an underserved, paying market. Tonotype's existing pitch-detection + bucket-scale infrastructure already does 90% of what's needed; Practice mode wires a target band + a hit-rate counter + session history on top.

The pivot preserves what Tonotype *is* (an editorial-grade creative tool), and extends what it can *do* (a real training instrument). The chrome doesn't shift toward "clinical" — practice users get the same paper, the same ink, the same terracotta. The framing is dignified, never therapeutic-app-condescending.

### Entry point

In the hero, a third action chip joins `Start listening` and `or start in variable mode →`:

```
[ START LISTENING ]   or start in italic variable italic mode →   practice with a italic pitch target italic →
```

Same `.practice-link` styling as the variable mode link — Instrument Serif italic 18px, terracotta on hover, arrow slides right.

### Setup overlay (`.ps-overlay`)

Clicking the chip opens a modal:

1. **Header:** `PRACTICE MODE` in terracotta mono caps, `Speak inside a target pitch range. Tonotype tracks how much of your time stays in the zone.` in Instrument Serif italic.

2. **Target range section** — 4 preset cards in a 2-column grid:

   | Preset       | Range (Hz) | Use case |
   |--------------|-----------|----------|
   | Masculinizing | 80 – 165  | Trans-masc voice training. Lower-register practice. |
   | Feminizing    | 165 – 255 | Trans-fem voice training. Default selection. |
   | Androgynous   | 145 – 200 | Voice-neutralizing practice. |
   | Custom range  | user-set  | Two number inputs for min/max Hz. |

   Active card flips to Charcoal Ink fill, Paper Cream text. Custom card reveals two `<input type="number">` fields with an italic `→` between them.

3. **Session length section** — 3 duration buttons: `3 MIN / 5 MIN / 10 MIN`. Same active-state pattern.

4. **Privacy notice** — bordered-left block in mono caps, terracotta left border. Reads: *"Private by default. Your voice never leaves this device. No recording, no cloud, no account. Session summaries (pitch range, hit-rate) are saved only to this browser, and you can clear them anytime from the History panel."*

   The privacy framing is **deliberately repeated** in the UI rather than buried in a separate Terms page. Clinical/health users care about this; Tonotype's local-only architecture lets us say it plainly.

5. **Actions:** Cancel + `START PRACTICE` (primary CTA).

### Target band on the pitch scale

A translucent terracotta band overlaid on the scale, positioned by `targetMin/targetMax` mapped to log-scale percentages. Dashed 1px Terracotta border-left and border-right mark the bounds. A tiny mono caption above (`TARGET · 165–255 HZ`) labels the band.

- **Lives inside `.scale-buckets`** so it aligns with the bucket cells. `rebuildScale()` re-attaches it after the cells are rebuilt so the band survives a calibration finish.
- **Shows only during practice sessions.** Hidden in free, reading, variable modes — the band would be meaningless without a target.
- **Logarithmically scaled** like everything else on the spectrum, via `pctOnScale()`.

### Status bar during practice

The topbar status switches to two practice-specific cells (replacing `Hz` and bucket name):

```
●  practice  /  4:32  /  in-target 73%
```

- **Timer** — countdown `MM:SS`, updates 10×/sec, Charcoal Ink.
- **Hit-rate** — `in-target NN%`, Terracotta. Computed as `hitFrames / totalFrames`.

Driven by `status.practice-active` class which hides the normal Hz/bucket cells and shows the timer/hit-rate cells.

### Hit-rate math

In `tickPractice(ts, pitch)` called per detection frame (≈25 Hz):

```js
totalFrames++;
pitches.push(pitch);
const inTarget = pitch >= targetMin && pitch <= targetMax;
if (inTarget) {
  hitFrames++;
  currentStreakFrames++;
  if (currentStreakFrames > longestStreakFrames) longestStreakFrames = currentStreakFrames;
} else {
  currentStreakFrames = 0;
}
```

Frames are 40ms each (the detection throttle), so 1 second ≈ 25 frames. `longestStreakFrames × 0.04` converts back to seconds for the results card.

### End of session — results overlay (`.pr-overlay`)

When the timer hits zero (`elapsed >= durationS`), `endPractice()`:

1. Tears down mic + speech recognition (`teardownAudio()`).
2. Removes `.practice-active` from the status, hides the target band.
3. Computes final metrics and saves to localStorage.
4. Shows the results overlay.

The results card features one big headline:

```
Spent 73% in your target range (165–255 Hz · Feminizing)
```

— with `73%` colored Terracotta. Below, a **3×2 metrics grid**:

| Avg pitch | Your range | Longest streak |
| Duration  | Words captured | Target |

Each metric is a Hairline-bordered cell with mono key + Instrument Serif value.

Actions: `Done` (closes overlay), `Practice again` (clears specimen + re-opens setup).

### Session storage (`tonotype.practice.v1`)

Each completed session saves to localStorage as JSON in an array. Capped at 60 sessions (`HISTORY_MAX`). Shape:

```js
{
  id: 1716817200000,
  date: '2026-05-27T14:30:00.000Z',
  durationS: 300,
  targetMin: 165, targetMax: 255,
  presetId: 'fem',
  presetLabel: 'Feminizing',
  hitPct: 73,
  avgPitch: 198,
  rangeMin: 142,
  rangeMax: 244,
  longestStreakS: 18.2,
  wordCount: 47,
}
```

~200 bytes per session × 60 = ~12 KB total. No audio, no transcripts, no PII.

### History view (`.hist-overlay`)

Footer `History` button (leftmost) opens an overlay with:

1. **Header:** `PRACTICE HISTORY · {count} SESSIONS` (count in terracotta) + close link.
2. **Sparkline** — 80px-tall canvas with hit-rates plotted chronologically (oldest left, newest right). Terracotta line + dots, dashed mid-line at 50%, latest dot bigger with its percentage labeled in mono. DPR-scaled for crispness.
3. **Axis labels** — `0% · 50% · 100%` along the top.
4. **Session list** — scrollable list of rows: `date · preset · range · duration · hit-rate%`. Hover tints with Terracotta Wash.
5. **Actions:** `forget all history` link (requires `confirm()` dialog, deletes the localStorage key).

### Mode integration

| State key | Purpose |
|-----------|---------|
| `state.mode === 'practice'` | Routes commit to `renderFreeWord` (practice words go into the specimen feed normally) |
| `state.phase === 'capturing' + state.mode === 'practice'` | Loop calls `tickPractice(ts, smoothedPitch)` per frame |
| `state.practice.{ targetMin, targetMax }` | Target range bounds (resolved from preset at session start) |
| `state.practice.{ hitFrames, totalFrames }` | Hit-rate numerator/denominator |
| `state.practice.pitches[]` | Collected for avg/range computation at end |
| `state.practice.longestStreakFrames` | Tracked alongside `currentStreakFrames` |
| `els.status.classList.toggle('practice-active')` | Swaps the topbar cells |

### Anti-patterns specific to practice mode

- ❌ No clinical positioning. We don't claim Tonotype is a medical device, a speech-therapy tool, or a diagnostic instrument. Marketing copy must stay in the "training" / "practice" register.
- ❌ No leaderboards, no streaks-as-gamification (avoiding the "didn't practice today" guilt loop common in habit apps). The sparkline shows trend, not pressure.
- ❌ No social sharing of practice sessions (only of creative specimens). Practice data is private by design — no `Share` button in the results overlay.
- ❌ No "your voice score" or single quality metric. Hit-rate is the headline number, but the metrics grid surfaces several complementary signals so users don't fixate on a single hit-rate.
- ❌ No persistent recordings, transcripts, or audio playback in v0.6 (audio replay is deferred to a later session). The captured *words* go into the specimen feed but the underlying audio is discarded after each detection frame.
- ❌ No condescending or therapeutic copy. `Speak inside a target pitch range` — not `Let's work on your voice together!`

### CSS hooks summary

| Selector                          | Purpose                                              |
|-----------------------------------|------------------------------------------------------|
| `.practice-link`                  | Tertiary hero CTA (italic Instrument Serif)          |
| `.target-band`                    | Translucent terracotta band on `.scale-buckets`      |
| `.target-band-label`              | Mono caption above the band                          |
| `.status.practice-active`         | Swaps Hz/bucket cells for timer/hit-rate cells       |
| `.ps-overlay`, `.ps-panel`        | Practice setup modal                                 |
| `.preset-card`, `.preset-card.active` | Target-range preset selection                    |
| `.duration-card`                  | Session-length selection                             |
| `.ps-privacy`                     | Privacy notice with terracotta left border           |
| `.pr-overlay`, `.pr-panel`        | End-of-session results modal                         |
| `.metrics-grid`, `.metric-cell`   | 3×2 metrics grid in results                          |
| `.hist-overlay`, `.hist-panel`    | History view modal                                   |
| `.sparkline-wrap`                 | Hit-rate trend visualization                         |
| `.session-row`                    | Individual history list entry                        |

---

## 16. Live Demo (Fraunces, real-time wght + opsz)

The hero now opens with a giant Fraunces word — `hello` by default — that visitors can talk *at* before deciding to start a session. Loudness drives `wght`, pitch drives `opsz`, both in real-time. It's a live preview of what variable mode does, no calibration required.

### Why this exists

Variable mode's `loudness → wght` mapping is the most impressive single move in Tonotype, but you only see it *after* committing to an 8s calibration and speaking whole words. That's a lot of friction for a first-time visitor who's just trying to understand what the tool *does*. The live demo collapses that to: see a word → tap it → talk → see the word morph in your hand.

It's also the cleanest possible statement of the project's premise: *one font, your voice, look what happens.*

### How it works

- **Idle state:** word renders in Fraunces at neutral settings (`wght 400`, `opsz 60`, `SOFT 0`, `WONK 0`). A small mute dot + mono caption below reads `tap and speak — your loudness modulates this letterform live`.
- **Activated (tap):** opens its own lightweight mic stream via `getUserMedia` (separate `AudioContext` from the main session). Starts a `requestAnimationFrame` loop throttled to ~30fps. Dot turns terracotta with the pulse animation; caption shifts to live readout `· wght 540 · opsz 36 · 247 Hz`.
- **Loop math:** RMS smoothed (`α = 0.45`) → `mapRMSToWght` (200 → 900). Pitch from the same `autoCorrelate` ACF2+ function used by the main loop, smoothed (`α = 0.3`) → `mapPitchToOpsz`. Applied as inline `font-weight` (so canvas-based export could capture it) and `font-variation-settings` for the non-weight axes.
- **Releases the mic** when a real session starts — `deactivateLiveDemo()` is called at the top of `startSession()` before requesting the session-grade stream. Browsers don't reliably support two concurrent `getUserMedia` consumers, and the main loop needs exclusive access.
- **Hidden during sessions** via `main.listening .live-demo { display: none; }`. The real specimen feed takes over.
- **No CSS transition on variation-settings** — would feel laggy against live audio. Updates fire at ~30Hz which is already smoother than the eye perceives.

### CSS hooks

| Selector | Purpose |
|----------|---------|
| `.live-demo` | Container, only visible in idle state |
| `.live-demo.active` | Active state — dot pulses, caption shifts color |
| `.live-demo-word` | The big Fraunces word itself |
| `.live-demo-hint .ldh-dot` | Status indicator (mute / terracotta + pulse) |
| `.live-demo-hint .ldh-key` | "Tap and speak" / "Mic blocked" copy slot |
| `.live-demo-hint .ldh-readout` | Live axis readout when speaking |

### Anti-patterns specific to the demo

- ❌ No auto-grab of the mic on page load. Even with persisted permission, requires explicit tap. Privacy-respecting.
- ❌ No CSS animations on the variation axes. They are driven directly from audio; smoothing happens in JS (EMA), not via CSS transitions.
- ❌ No fallback to a faked oscillator if the mic is blocked. The word just sits in its neutral state and the caption reads `mic blocked`. Truth over theater.
- ❌ The demo word never modulates `SOFT` or `WONK`. Those axes need within-word pitch trajectory data which only makes sense for committed words — applying them to a single sustained demo would be dishonest signal.

---

## 17. v0.7 — Usability Pass (Session Lifecycle & Recovery)

A product-register pass over the chrome. No new capability; every change closes a usability gap found by auditing the session lifecycle end-to-end. The visual system is untouched — same paper, ink, terracotta, hairlines.

### The gaps this closes

1. **No way to stop.** Once a session started, the mic ran until page reload. Privacy problem and a trust problem.
2. **Shared links were dead ends.** Opening a `#s=` URL hid the hero with no path to starting your own session.
3. **Modes were buried.** One block button plus two italic links in three different styles; variable and practice were easy to miss entirely.
4. **Silent failure.** A blocked mic produced a 10px status caption and nothing else.
5. **Ambush calibration.** Nothing warned first-timers about the 8-second warm-up.
6. **Dead controls looked alive.** Share and Export were clickable with zero words on the page.
7. **No guidance while listening.** The empty state said `say something.` and nothing more.

### Mode grid (replaces the CTA row)

Four equal cards in a 2×2 grid (`max-width: 640px`), one per session mode. Same visual DNA as the passage and preset cards — 1px Hairline Strong border, mono uppercase label, Instrument Serif italic description, hover inverts to Charcoal Ink with a -2px lift.

| Card | Label | Description line |
|------|-------|------------------|
| free (primary) | SPEAK FREELY | Every word lands in a font matched to your pitch |
| reading | READ A PASSAGE | Words light up as you read them aloud |
| variable | VARIABLE TYPE | One typeface, bent live by four voice signals |
| practice | PRACTICE | Hold a target pitch range, track your hit-rate |

- The free card carries `.primary` — Charcoal Ink fill at rest (the old `Start listening` weight), hover shifts to Terracotta. One primary per view.
- Reading toggles the passage list open rather than starting a session — the passage choice IS the session start.
- During `connecting`, all cards set `disabled` (opacity 0.5, `cursor: wait`). No spinner; the status bar carries the state.
- Collapses to a single column below 720px.

### Passage list — collapsed by default

The five passage cards now live behind the READ A PASSAGE card. Collapsed state: `max-height: 0`, opacity 0, no border. `.open`: `max-height: 440px` (700px mobile), 0.4s `cubic-bezier(0.22, 0.61, 0.36, 1)`. Label reads `pick a passage to read aloud`. This halves the idle hero's vertical bulk.

### Stop control

A terracotta-bordered `STOP` button at the end of the topbar status cluster. Hidden until a session is live (`.show`). Also bound to Esc (lowest priority in the Esc chain — overlays and stage mode resolve first).

- Free / variable / reading: tears down mic + recognition + loop, keeps the specimen on the page, shows the ended bar.
- Practice mid-session: routes to `endPractice()` — partial metrics are computed honestly (`durationS` = real elapsed, not planned) and the results overlay shows as usual.
- `teardownAudio()` now also halts the rAF detection loop (`loopRunning` guard) so a stopped session costs zero CPU.

### Ended bar

A 1px-bordered mono bar at the top of main, shown after any stop:

```
SESSION ENDED · 014 WORDS            [ KEEP SPEAKING ] [ NEW SESSION ]
```

- **Keep speaking** — restarts the same mode (fingerprint skips recalibration). Reading mode resets passage progress first.
- **New session** — full reset: clears specimen, restores the hero, releases everything, scrolls to top.
- Practice completion shows it without the resume button (`practice complete · 73% in target`); the results overlay owns the practice-again flow.
- **Shared-specimen restore reuses it** as the escape hatch: `shared specimen · 012 words` + `Start your own`. A share link is never a dead end.

### Mic-blocked recovery banner

Terracotta-left-bordered banner (same anatomy as `.ps-privacy`) shown on `getUserMedia` failure or a `not-allowed` speech-recognition error:

```
MICROPHONE BLOCKED. Click the lock icon in your address bar, allow the microphone, then reload this page.   dismiss
```

Failure also fully unwinds the session state (classes, busy cards, teardown) so the page never wedges in a half-started state.

### Rotating tips

While listening with zero words captured, a mono caption under `say something.` cycles every 4s:

`try a low rumble · now whisper something · sing one note and hold it · shout a single word · say a long word, slowly`

The tips teach the pitch→font mapping by prompting the extremes. Hidden in reading mode (the passage is the prompt), practice (the band is the prompt), and once the first word lands.

### Disabled footer actions

`Share` and `Export poster` are `disabled` until at least one word exists (`.ghost:disabled` — opacity 0.35, `cursor: not-allowed`, hover suppressed). `Clear specimen` stays enabled and now confirms via the toast (`Specimen cleared`).

### Calibration pre-notice

The hero hint now reads: `allow microphone access · chrome or edge · first run includes an 8-second voice warm-up`. The warm-up is no longer a surprise.

### Anti-patterns specific to this pass

- ❌ No confirmation dialogs on stop. Stopping is non-destructive (the specimen stays); a confirm would punish the most important control on the page.
- ❌ No auto-restart after mic failure. The banner explains recovery; retrying in a loop would re-trigger the permission prompt and read as nagging.
- ❌ No pulsing/bouncing attention-getters on the stop button. It appears when relevant and sits still.
- ❌ Tips never instruct posture, breathing, or technique — they prompt play, not pedagogy.
