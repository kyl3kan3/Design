# 15 · Intro Sequence — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

The signature opening of award-site portfolios: a percentage counter builds anticipation, curtain panels wipe upward in a stagger, and the hero text rises through masks — three acts, one continuous gesture.

**Structure.** A three-act entrance: preloader with a rolling percentage counter → a four-panel curtain wipe → the hero arriving as masked lines with staggered rises. Total choreography lands inside ~2.5s and can be replayed.

## Why it works — design intent the implementation must serve

Anticipation. The counter is a promise (“something is being prepared for you”), the curtain is a theatrical reveal, and the staggered text pays it off. The chain manufactures a moment of arrival that a page which simply appears can never have — first impressions form in under a second, and this owns that second.

## Exact motion spec

- The counter runs ~2.1s with deliberately uneven pacing (fast start, mid-stall, final sprint — real loading rhythms).
- Curtain panels wipe 850ms on cubic-bezier(.76,0,.24,1) with a 70ms stagger.
- Hero lines start 100ms into the wipe so the reveal overlaps the exit (choreography, not a queue). Total: under 3.5s, replayable, and only ever shown once per visit in production.

## Reduced motion

The entire intro is skipped — loader and curtains never render, hero text appears immediately. An intro is the definition of non-essential motion.

## Acceptance checklist

- [ ] A "skip" affordance (click/keypress) jumps straight to the settled hero
- [ ] The sequence never blocks interaction after it settles
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/15-intro-sequence.html`](../../concepts/15-intro-sequence.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
