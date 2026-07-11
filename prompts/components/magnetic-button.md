# Magnetic button — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A primary CTA that is pulled toward the cursor inside a proximity field, then springs back — the flagship "the interface noticed you" microinteraction.

## Exact spec

- Within a 120px radius of the button center, translate the button toward the pointer by **35% of the offset**; outside the radius, no pull.
- On leave, spring back with an exponential settle — **lerp factor 0.12 per frame** toward rest (no CSS transition fighting the rAF).
- While magnetized: scale ≈ 1.03 and a **shine sweep** (a skewed translucent gradient) crosses the face once per entry.
- One passive `pointermove` listener + one rAF for the whole effect; transform-only.

## Reduced motion

No pull, no sweep — hover feedback becomes a border/brightness change. Keyboard focus shows the same state.

## Acceptance checklist

- [ ] Button is a real `<button>` and fully usable with keyboard only
- [ ] Motion is transform-only; no layout reads inside the rAF loop
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/magnetic-button.html`](../../design-system/components/magnetic-button.html).
