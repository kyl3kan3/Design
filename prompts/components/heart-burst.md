# Heart burst — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A like button whose confirmation is a small celebration — felt, brief, and cheap.

## Exact spec

- On like: the heart **pops with ~25% overshoot** (`scale 0.8 → 1.25 → 1`, `--ease-bounce`, ~450ms) and fills `--hot`.
- **12 particles** launch on an even circle (± jitter), each flying 24–42px outward while fading, over **650ms** with decelerating easing; remove each on `animationend`.
- Count increments with a small roll (old digit up, new digit in).
- Unlike is quiet: 150ms color/scale-down only — celebrations are for positive actions.

## Reduced motion

Fill color changes and count updates instantly; no pop, no particles.

## Acceptance checklist

- [ ] Particles are removed from the DOM after flight (no accumulation)
- [ ] Rapid toggling never double-spawns or leaks particles
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/heart-burst.html`](../../design-system/components/heart-burst.html).
