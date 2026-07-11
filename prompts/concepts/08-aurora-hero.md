# 08 · Aurora Ambient Hero — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A hero lit by a real-time aurora: canvas-drawn curtains of light undulating over a starfield and ridge line, under stepped film grain, with a shimmer through the headline and a CTA ringed by a rotating conic gradient.

**Structure.** A full-viewport hero: canvas aurora curtains (layered sine fields, blurred) over a starfield, stepped film grain, centered display copy, and a CTA wearing a slowly rotating conic-gradient ring via a registered `@property`.

## Why it works — design intent the implementation must serve

Slow, organic light movement is ambient salience — enough change to keep peripheral vision engaged, too slow to fight the copy. Unlike stock gradient blobs, curtain physics read as a place, not a texture; the one fast element — the CTA ring — inherits the attention the sky primes.

## Exact motion spec

- Four ribbons drawn per frame as single paths whose edges follow three layered sine fields (periods ~37s/13s/7s).
- Vertical gradients run violet → green → transparent and a whole-canvas GPU blur(14px) + mix-blend-mode: screen melts them into light. Rendering is capped at 30fps and skipped entirely off-tab.
- ~160 stars and the ridge are drawn once on a separate static canvas.

## Reduced motion

The aurora renders exactly one composed frame and never animates; grain, shimmer, and pulse stop; all copy appears immediately.

## Acceptance checklist

- [ ] Aurora canvas renders at reduced internal resolution and upscales (GPU blur does the smoothing)
- [ ] Grain is steppy (8–12fps) not per-frame — it should feel like film, not noise
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/08-aurora-hero.html`](../../concepts/08-aurora-hero.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
