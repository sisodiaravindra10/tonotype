# Product

## Register

brand

## Users

Two overlapping audiences, both arriving at the same single page:

1. **Type-curious creatives** (designers, poets, performers, font nerds) who land via a shared link or social post. Context: desktop Chrome, headphones maybe, thirty seconds of patience. Job: understand what this is, try it, capture something worth sharing. Emotion to evoke: delight and the itch to perform.
2. **Voice-practice users** (people training pitch range, including trans voice training; vocal coaches and their students; Indian-language speakers across eleven supported languages). Context: returning regularly, often privately, sometimes about something personally sensitive. Job: run a target-range session, see honest numbers, watch progress over weeks. Emotion to evoke: dignity, calm, and trust that nothing leaves the device.

The first audience funds attention; the second is the durable use case. The landing must serve the first without embarrassing the second.

## Product Purpose

Tonotype turns the pitch and loudness of a voice into live typography: every spoken word renders in a typeface matched to how it was said. Free speech, passage reading, a continuously-deforming variable face, and a practice mode with pitch targets and local history. Single HTML file, browser APIs only, no accounts, no cloud. Success: a first-time visitor speaks within sixty seconds of landing; a practice user returns weekly and can see their trend.

## Brand Personality

Editorial, dignified, instrument-like. A type foundry's specimen book that happens to listen. Matter-of-fact copy (`say something.`), foundry-catalog numbering, instrument-panel telemetry. Playful through the typography itself, never through chrome, mascots, or exclamation marks.

## Anti-references

- Clinical voice-app aesthetics: line-chart spectrograms, medical blues, dashboard density.
- SaaS gradient slop: purple-blue gradients, glassmorphism cards, hero-metric templates, emoji-studded copy.
- Gamified habit apps: streaks, badges, leaderboards, guilt loops, confetti.
- Therapeutic condescension: "Let's work on your voice together!" tone, wellness pastels.
- Anything where a viewer could say "AI made that" without doubt.

## Design Principles

1. **The word is the loudest thing on screen.** Chrome never competes with the captured specimen; the product's output is the page's hero.
2. **Show the mechanism, don't explain it.** A live demo word and a six-register specimen strip beat paragraphs of copy. Telemetry (Hz, axis values) is exposed, never hidden, because the readout is the trust.
3. **Honest signal only.** No fabricated metrics, no fake calibration, no decorative data. If the system didn't measure it, the page doesn't show it.
4. **Private by default, stated plainly.** Local-only processing is a feature; say it in the interface, repeatedly, in plain words.
5. **Tool chrome appears when the tool engages.** The landing is a brand surface; instruments (pitch scale, telemetry) earn their place onscreen only once a session starts.

## Accessibility & Inclusion

- `prefers-reduced-motion` honored everywhere: entry choreography and loops collapse to simple fades or nothing.
- Keyboard: Esc layering (overlays, stage, stop), focus-visible states on all interactive elements, 44px minimum touch targets.
- Multilingual by design: English plus ten Indian languages with script-correct typography; chrome stays English (honest scope), voice and specimens are multilingual.
- Mic permission is always explicit (no auto-grab); blocked-mic recovery is guided inline.
- Color is never the only signal (bucket names, labels, and position carry meaning alongside the terracotta accent).
