# 16 · Applied: Drift Landing — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A complete landing page assembled from the Attention Lab design system: aurora canvas hero, spotlight-border bento, roll-up counters, hover rows, and a magnetic CTA — all on the shared tokens.

**Structure.** A complete landing page for a fictional sleep-audio product ("Drift") assembled from the system: aurora hero, spotlight bento of features, roll-up stats, testimonial, magnetic CTA, footer — every pattern quoted from the component library.

## Why it works — design intent the implementation must serve

Why it matters: concepts prove patterns; a landing page proves the budget. One ambient layer (the sky), one looping element (the CTA ring), everything else silent until the user acts. Attention spending is what a system is for.

## Exact motion spec

- Hero copy staggers 150ms apart on cubic-bezier(.16,1,.3,1).
- Scroll reveals trigger at 15% visibility, 80ms siblings.
- Counters roll 1.4s cubic ease-out on arrival.
- The magnet pulls 35% within 120px.
- Ambient tile loops run 2.6–14s and speed ~3× on hover.

## Reduced motion

Aurora renders one static frame; entrances, counters, magnetism, spotlight rings, and ambient loops are all replaced with immediate static states.

## Acceptance checklist

- [ ] One loud element per view — ambient motion never competes with the CTA
- [ ] All section entrances are IntersectionObserver-staggered and fire once
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/16-drift-landing.html`](../../concepts/16-drift-landing.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
