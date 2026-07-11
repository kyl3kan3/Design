# Stat counter — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A number that rolls up to its value on first sight — momentum you can read.

## Exact spec

- Roll 0 → target over **1.4s** with cubic ease-out (`v = target · (1 − (1−t)³)`), rendered via rAF.
- `font-variant-numeric: tabular-nums` so digits never jitter horizontally.
- Fire **once** on first viewport entry (IntersectionObserver, threshold ≈ 0.4, unobserve after).
- Support decimals (fixed precision) and suffixes (`M`, `ms`, `%`) via data attributes; format large values (41000000 → 41M).

## Reduced motion

Final value renders immediately; no roll.

## Acceptance checklist

- [ ] No digit jitter during the roll
- [ ] Scrolling away and back does not re-trigger
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/counter.html`](../../design-system/components/counter.html).
