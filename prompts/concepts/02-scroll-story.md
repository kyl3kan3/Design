# 02 · Scroll-Driven Story — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A narrative page where scroll position drives everything: a progress bar, staggered section reveals, a text passage that “fills” with brightness as you read, a pinned parallax scene, and counters that roll up on arrival.

**Structure.** A tall scroll narrative: fixed top progress bar driven by scroll, a pinned scene where layers translate at different rates while pinned, a paragraph whose text "fills" with color as it crosses the viewport, and a stats row whose numbers roll up on first view.

## Why it works — design intent the implementation must serve

Tying motion to scroll makes the reader the animator — the page responds 1:1 to their hand, which sustains engagement far longer than autoplay. Withholding each chapter until it enters view builds a curiosity loop: every swipe is rewarded.

## Exact motion spec

- Progress bar uses native CSS animation-timeline: scroll() with a rAF fallback.
- Reveals enter over 700ms on cubic-bezier(.16,1,.3,1) with 80ms stagger, triggered at 18% visibility.
- The pinned scene maps 320vh of scroll to parallax offsets (back ridge 4vh, front ridge 16vh).
- Counters ease out over 1.4s with tabular numerals so digits don't jitter.

## Reduced motion

All content is visible immediately, parallax is frozen, the text fill renders fully bright, and counters show final values without rolling.

## Acceptance checklist

- [ ] Progress bar uses CSS scroll-timeline where supported with a JS fallback
- [ ] Counters fire exactly once (IntersectionObserver, unobserve after trigger)
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/02-scroll-story.html`](../../concepts/02-scroll-story.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
