# 20 · Applied: Signal SaaS — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A complete SaaS landing page at template-market polish density — glass nav, glowing hero with a live product mockup, logo marquee, glass bento, counters, testimonial wall, pricing with billing toggle, FAQ — every section from Attention Lab components.

**Structure.** A complete SaaS landing page for a fictional attention-analytics product ("Signal") at commercial polish density: glass sticky nav that gains a floor on scroll, hero with shimmer headline and conic-ring CTA, a 3D-tilting product mockup that draws its chart on first view, logo marquee, spotlight bento (with a JS-generated attention heatmap), roll-up stats band, testimonials, monthly/yearly pricing with rolling prices, FAQ accordion (grid-template-rows 0fr→1fr), footer.

## Why it works — design intent the implementation must serve

This is the genre prompt libraries sell (“what agencies charge $5,000 for”). The difference is under the hood: one attention budget (sky + one ring loop, everything else at rest until interaction), documented timings, and a full reduced-motion contract — the things a generated template never carries.

## Exact motion spec

- Hero copy staggers ~140ms apart on cubic-bezier(.16,1,.3,1).
- The mockup bobs 10px over 7s, tilts ±3° to the pointer, and draws its chart (1.6s stroke, then area fade) on first view.
- Marquee loops 30s and pauses on hover.
- Spotlight bento updates only the hovered tile.
- Counters roll 1.4s cubic ease-out.
- The billing toggle rolls prices over 500ms.
- FAQ animates grid-template-rows 0fr→1fr over 400ms.

## Reduced motion

Orbs, grain, rings, marquee, bob/tilt, shimmer, reveals, and chart draws all freeze to composed static states; prices and accordions swap instantly; smooth scroll turns off.

## Acceptance checklist

- [ ] Bento grid has zero holes at every breakpoint
- [ ] Billing toggle is a real role="switch" and prices roll (not jump) between monthly/yearly
- [ ] Chart draw, counters, and reveals all fire once via IntersectionObserver
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/20-signal-landing.html`](../../concepts/20-signal-landing.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
