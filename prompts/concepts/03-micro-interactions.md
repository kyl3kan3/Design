# 03 · Micro-Interactions — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

Six interaction stations: a magnetic button, a particle-burst like, a squash-and-stretch toggle, a morphing submit, an error shake, and a cascading star rating.

**Structure.** A grid of six labeled "stations", each one micro-interaction: magnetic button, particle-burst like, squash-and-stretch toggle, morphing submit (button → spinner → check), error shake on an input, and a five-star cascade rating.

## Why it works — design intent the implementation must serve

Instant, physical feedback makes an interface feel alive — each response arrives inside the ~100ms window where cause and effect feel simultaneous. Overshoot and squash borrow real-world physics, so the eye reads the element as an object, not a pixel.

## Exact motion spec

- Magnet pulls at 35% of cursor offset within a 120px radius and springs home with a lerp of 0.12/frame.
- Pops use cubic-bezier(.34,1.56,.64,1) overshoot at 250–450ms.
- The submit morph runs 450ms on an emphasized curve, then draws its check via stroke-dashoffset in 400ms.
- The error shake is ±4px for 400ms — enough to notice, too brief to annoy.

## Reduced motion

Magnetism and particle bursts are disabled; state changes (like, toggle, success, error) become instant color/fill swaps, and the spinner slows rather than spins fast.

## Acceptance checklist

- [ ] Every station is keyboard-operable with visible focus rings
- [ ] The submit morph never leaves the DOM or jumps layout
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/03-micro-interactions.html`](../../concepts/03-micro-interactions.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
