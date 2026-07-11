# 06 · Cursor & Spotlight — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

The cursor becomes a designed object: a dot with a spring-lagged ring that morphs by context (grows on links, becomes a “VIEW” pill over work items) — and a section where it turns into a literal flashlight, uncovering a generative canvas night scene: a harbor skyline, a moon, and eleven windows still lit.

**Structure.** Two scenes: (1) a custom cursor with lag and mass that changes state over links, text, and drag zones; (2) a "flashlight" — a mask-image hole in a dark overlay revealing a generative canvas night scene beneath the beam.

## Why it works — design intent the implementation must serve

The cursor is the one pixel users already watch, and the mask creates a genuine curiosity gap — the dark layer shows faint outlines of something, so you sweep to find out what. Discovery only works when there is something to discover; the payoff is a scene, not more text.

## Exact motion spec

- Dot tracks 1:1.
- Ring follows with lerp 0.16/frame for a ~90ms lag that implies weight.
- State morphs run 300ms on cubic-bezier(.16,1,.3,1). Spotlight is a CSS mask-image radial circle whose radius springs 0→220px on entry and position lerps at 0.2/frame. The scene is drawn once per resize on two canvases from one seeded generator — full color for the lit layer, 5%-alpha wireframe for the teaser.

## Reduced motion

Reduced motion (and touch devices): the native cursor is kept, the ring is not rendered, and the night scene displays fully unmasked — no content is gated behind pointer motion.

## Acceptance checklist

- [ ] Native cursor remains available/visible for keyboard and touch users
- [ ] Nothing essential is only discoverable inside the flashlight beam
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/06-cursor-spotlight.html`](../../concepts/06-cursor-spotlight.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
