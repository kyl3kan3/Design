# 09 · Shared-Element Morph — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

Click any specimen: its artwork physically travels from the grid into the detail sheet and settles there — one object moving between two layouts, rather than a cut between two screens.

**Structure.** A card grid where clicking a card FLIP-morphs it into a full detail view and back: measure first/last rects, apply inverted transform, play. Background gets `inert` while open; focus is trapped and restored on close.

## Why it works — design intent the implementation must serve

Continuity. Jump cuts force the brain to re-find its place; a shared element carries the eye across the layout change, so attention never drops. The motion also explains the interface: this became that — model-building for free.

## Exact motion spec

- Classic FLIP — First/Last rects measured, the element Inverted with a transform, then Played over 550ms on cubic-bezier(.32,.72,0,1). Non-uniform scale is applied to width/height factors so corner radii stay sane.
- Detail copy staggers in 60ms apart only after the morph is underway (220ms delay).
- The return trip runs 420ms — exits faster than entrances.

## Reduced motion

The sheet opens and closes with a plain instant swap — no flying element, no staggered copy.

## Acceptance checklist

- [ ] Return trip animates (force a style flush so the start state commits) and a safety timeout guarantees cleanup if transitionend never fires
- [ ] Overlay never blocks clicks after close
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/09-shared-element.html`](../../concepts/09-shared-element.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
