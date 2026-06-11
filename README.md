# Tonotype

> Voice as type specimen — a single-page web tool that listens to the pitch of your voice and sets each spoken word in a font that matches its tone.

Low rumbles land in heavy display type. Mid registers in editorial serifs. High notes in flowing scripts. Read something aloud — with feeling — and watch your voice become a typeface.

---

## What it does

Tonotype runs entirely in your browser. It uses the Web Audio API to analyse the pitch and loudness of your voice in real time, and the Web Speech API to capture the words you say. Each captured word is rendered in a font picked from a curated specimen pool, modulated by the *shape* of how you said it.

Four session modes. Six pitch buckets. Twenty-five-plus specimen fonts plus one variable face. One terracotta accent. No build step, no dependencies, no server.

### Modes

| Mode | What happens |
|------|--------------|
| **Free** | Speak freely; each word lands in the specimen feed in a font matching its pitch. |
| **Reading** | Pick a literary passage (Shakespeare, Rumi, Whitman, Sappho, or paste your own). Words light up in their specimen font as you read them aloud. |
| **Variable** | One typeface (Fraunces) instead of many. Pitch drives optical size, loudness drives weight, within-word variance drives softness, vibrato drives wonkiness. Every word is a unique font instance. |
| **Practice** | Pick a target pitch range (Masculinizing, Feminizing, Androgynous, or Custom). A translucent band on the pitch scale shows the zone. Live hit-rate, countdown timer, end-of-session metrics, local history with a sparkline trend. |

### Other features

- **Voice calibration** — first 8 seconds map your personal pitch range across the six buckets, so every voice reaches every bucket.
- **Sub-word letter morphing** — when pitch varies enough mid-word, each letter picks its own font.
- **Octave-error correction** — keeps the autocorrelation from jumping an octave on harmonic-rich voices.
- **Multi-speaker detection** — two voices in a conversation get different ink shades automatically.
- **Phoneme-aware spacing** — vowels breathe, consonants tighten, in morphed letter rendering.
- **Tonal-language presets** — English, Mandarin, Vietnamese, Cantonese, Spanish, Japanese pitch envelopes.
- **Voice fingerprint** — your calibration persists across sessions; the 8-second warm-up only runs once per ~month.
- **Shareable specimen URLs** — `Share` button copies a base64-encoded link; opening it elsewhere reconstructs the specimen without a microphone.
- **Live performance / stage mode** — chrome-less, near-black canvas with huge cream-on-black type, designed for projection.
- **Export to PNG** — the current specimen renders to a 1200 × 1600 poster you can save and share.
- **Live demo word** — a giant Fraunces "hello" in the hero modulates weight and optical size in real time from your mic, before you commit to a session.

---

## How it works

The engine is around 1,500 lines of vanilla JavaScript:

1. **Pitch detection** — ACF2+ autocorrelation (Chris Wilson's well-known implementation) running at ~25Hz, with exponential smoothing and an octave-correction guard against the classic harmonic-jump bug.
2. **Calibration** — the 5th and 95th percentiles of the first 8 seconds of speech become your range. The six font buckets are then mapped log-proportionally across that range, so every voice reaches every bucket regardless of speaker.
3. **Specimen rendering** — speech recognition gives final words; each word's font is picked at random from the pool of its assigned bucket. Loudness (RMS) modulates font-size via a CSS custom property. Sub-word morphing fires when letter pitches span two or more buckets.
4. **Variable mode** — one font (Fraunces) replaces the lookup. Four voice signals drive four font axes:
    - Pitch → `opsz` (inverse log, 144 display → 9 caption)
    - Loudness → `wght` (200 → 900)
    - Within-word pitch variance → `SOFT` (0 → 100)
    - Direction changes in pitch contour → `WONK` (0 → 1)
5. **Practice mode** — same pitch detection, plus a target band overlaid on the scale and per-frame hit/miss counting. Sessions are saved to localStorage; history view renders a sparkline of hit-rates over time.

---

## Run locally

The microphone needs a secure context, so `file://` won't work. From this directory:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000` in Chrome or Edge. Allow the microphone when prompted.

There's no build step. The entire app is one `index.html` file with inline CSS and JavaScript. Just `index.html` plus a Google Fonts `<link>` for the specimen pool.

---

## Privacy

Tonotype is **local-first by design.**

- Audio is processed in your browser and never leaves the device.
- Nothing is recorded. The audio buffer is consumed for pitch/loudness analysis and discarded each frame.
- Speech recognition uses the browser's built-in Web Speech API. In Chrome this routes through Google's servers — the words you say (but not raw audio) are sent there for transcription. If that's a concern, run the page in a browser with on-device speech recognition.
- Session summaries (calibrated pitch range, practice hit-rates) are saved only to `localStorage` in your own browser. They never sync, never upload. `forget all history` in the Practice History panel deletes everything immediately.
- Shareable specimen URLs encode the captured *words and fonts* into the URL hash — nothing is uploaded; the bytes travel only when you copy and paste the link.

---

## A note on intent

Tonotype is an exploratory creative tool. The Practice mode includes pitch-range presets that may be useful for people working on their voice (singers, public speakers, those exploring voice modulation in any direction). **It is not a medical or therapeutic device** and makes no clinical claims. If you're working on your voice as part of medical treatment or transition, please work with a qualified speech-language pathologist or voice coach. Tonotype can be one piece of practice; it isn't the whole picture.

---

## Credits

- **Pitch detection** — ACF2+ implementation adapted from Chris Wilson's PitchDetect.
- **Specimen pool** — Anton, Archivo Black, Black Ops One, Bodoni Moda, Bowlby One, Bungee, Bungee Shade, Caveat, Cormorant Garamond, DM Serif Display, EB Garamond, Faster One, Inter, Italianno, Lora, Major Mono Display, Manrope, Monoton, Parisienne, Playfair Display, Rubik Glitch, Space Grotesk, Spectral, Work Sans, Yeseva One — all from Google Fonts.
- **Variable face** — Fraunces (Undercase Type · Phaedra Charles & Flavia Zimbardi), Google Fonts.
- **Chrome typography** — Instrument Serif (display), Geist (body), JetBrains Mono (telemetry).
- **Literary passages** — Sonnet XVIII (Shakespeare), The Field (Rumi), Song of Myself (Whitman), Fragment 31 (Sappho) — all public domain.

---

## License

[MIT](./LICENSE). Use it, fork it, learn from it.

---

## Documentation

The full design system — colour palette, typography rules, component anatomy, motion table, anti-patterns, every feature documented — lives in [DESIGN.md](./DESIGN.md).
