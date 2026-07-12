# 22 · Lumen Nebula — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: an editorial landing whose full-viewport hero is a real-time generative nebula — layered value-noise domain-warped three times in a fragment shader, drifting on its own and swirling around your cursor. Content scrolls up through it into deep space.

**Structure.** An editorial landing for a fictional generative-sound product whose fixed, full-viewport hero is a **live domain-warped fbm nebula** (single fragment shader) drifting on its own and swirling around the cursor; content scrolls up through it. Centered gradient-clipped headline, an equaliser cue, a moods marquee, an editorial split, a gradient-mesh "textures" grid, a roll-up stats band, closing CTA, footer.

## Why it works — design intent the implementation must serve

A luminous field that flows and reacts feels alive and weatherlike — the eye can't predict it, so it lingers. It sets a calm, atmospheric tone that suits a product about ambient sound, and answers the “static gradient hero” with something that actually moves.

## Exact motion spec

- Classic Perlin-style value-noise fbm (5 octaves), then Iñigo Quílez domain warping (fbm(p + 4·fbm(p + 4·fbm(p)))) for the filaments.
- Colour ramps from indigo→violet→cyan by warp magnitude with a warm core that tracks the pointer.
- ACES tonemap + interleaved-gradient-noise dither.
- Adaptive DPR by EMA frame time (cap 1.5). Type is a tight geometric grotesk.
- One gradient accent.

## Reduced motion

The field freezes to a single composed still (fixed time, no pointer swirl); the equaliser cue, marquee and reveals stop; the fixed render stays as a static backdrop.

## Acceptance checklist

- [ ] The nebula reads deep and luminous — voids near-black, filaments glowing (gate colour hard by density; an evenly bright field reads as flat fog, not a nebula)
- [ ] Domain warp is the iq pattern `fbm(p + 4·fbm(p + 4·fbm(p)))`; ACES tonemap + IGN dither; adaptive DPR (cap ~1.5)
- [ ] Reduced motion freezes to a single composed still (fixed time, no pointer swirl); equaliser, marquee and reveals stop
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/22-lumen-nebula.html`](../../concepts/22-lumen-nebula.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
