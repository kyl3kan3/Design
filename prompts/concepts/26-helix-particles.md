# 26 · Helix Particles — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: a genomics-platform landing whose fixed hero is a real-time double helix built from ~24,000 GPU particles — two strands and their base-pair rungs, positions computed on the GPU from each particle's index, rotating and shimmering with curl-noise wander. The pointer tilts the whole molecule.

**Structure.** A genomics-platform landing whose fixed hero is a **live double helix built from ~24k GPU particles** — two strands plus base-pair rungs, positions derived on the GPU from each particle index, rotating with per-point wander; the pointer tilts the molecule. Clinical near-black + bio-green. Sections: capabilities marquee, editorial split (the platform), a stats band, a programs grid, closing CTA, footer.

## Why it works — design intent the implementation must serve

A recognisable structure made of thousands of glowing points that drift yet hold their shape reads as “alive and precise at once” — the eye follows the rotation and the twinkle. It is the company's subject rendered as its hero.

## Exact motion spec

- Bufferless rendering — each particle's position is derived from gl_VertexID in the vertex shader (helix parameter → two strands + rungs), with per-point simplex wander and a subtle breathing radius.
- Additive blending, no depth test, a hot-core + wide-skirt sprite so overlaps read as bloom without a post pass. Pointer eases the tilt.
- ACES-free additive space kept in gamma. ~24k points at 60fps, DPR-capped.

## Reduced motion

The helix stops rotating and holds a single composed still (no wander, no tilt); marquee and reveals freeze; the fixed render stays as a static backdrop.

## Acceptance checklist

- [ ] Bufferless rendering: each particle position computed from `gl_VertexID` in the vertex shader (helix parameter → two strands + rungs) — no vertex buffers, no ping-pong needed for a fixed shape
- [ ] Additive blending, depth test OFF, hot-core + wide-skirt sprite (`exp(-r²·28)+exp(-r²·6)·0.35`) so overlaps read as bloom without a post pass; perspective-scaled point size
- [ ] Reduced motion stops rotation + wander and holds a composed still
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/26-helix-particles.html`](../../concepts/26-helix-particles.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
