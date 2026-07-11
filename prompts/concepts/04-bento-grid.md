# 04 · Bento Grid Choreography — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A feature grid in the bento style — mixed tile sizes, each with its own ambient motion that intensifies on hover, plus a cursor-tracked spotlight that lights up card borders as you sweep across.

**Structure.** A bento grid (mixed tile spans, no holes at any breakpoint) where each tile has a cursor-tracked gradient ring masked to its 1px border, plus its own quiet ambient art (chart, orbit, grid…) that intensifies on hover.

## Why it works — design intent the implementation must serve

The spotlight border creates a moving point of light that follows the hand — motion + luminance contrast, the two strongest pre-attentive signals, locked to the cursor. Varied tile sizes break scanning monotony (Von Restorff effect), and hover intensification rewards exploration.

## Exact motion spec

- Spotlight is a 260px radial gradient masked to a 1px border ring, fading in over 350ms.
- Cards enter with a 90ms stagger over 650ms on cubic-bezier(.16,1,.3,1).
- Ambient loops run slow (1.4–9s) and snap to 3–4× speed on hover.
- Sparkline bars stagger 35ms per bar.

## Reduced motion

Cards render in place, the cable draws instantly, orbits/equalizer/keycaps freeze at a composed static state, and only color feedback remains on hover.

## Acceptance checklist

- [ ] Spotlight ring only updates on the hovered tile (single rAF, CSS vars)
- [ ] Grid has zero empty cells at desktop and tablet widths
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/04-bento-grid.html`](../../concepts/04-bento-grid.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
