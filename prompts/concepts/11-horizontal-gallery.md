# 11 · Horizontal Gallery — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A section pins itself to the viewport and converts your vertical scroll into horizontal travel through four exhibition rooms, each with its own internal parallax and a segmented progress indicator.

**Structure.** A pinned horizontal gallery: a tall scroll region whose sticky viewport converts vertical scroll into sideways travel across rooms/panels, with internal parallax per panel and a progress indicator.

## Why it works — design intent the implementation must serve

The axis flip is a violated expectation — the page suddenly moves the “wrong” way, which re-engages attention exactly when scrolling has become automatic. Because travel is still scroll-driven, users keep full control; it surprises without hijacking.

## Exact motion spec

- A 420vh wrapper + position: sticky viewport.
- Track translateX maps wrapper progress with a 0.06 lerp so panels glide rather than track raw scroll (snapping exact at both ends).
- Titles lead the travel by up to ±60px and glow orbs by ±140px for foreground parallax.
- Progress dots fill per-panel. All motion is transform-only, on the compositor.

## Reduced motion

The pin is removed entirely — the gallery becomes an ordinary horizontally scrollable strip with no scroll capture, no parallax.

## Acceptance checklist

- [ ] Scroll math is bidirectional and resumes correctly mid-gallery on reload
- [ ] Reduced motion / narrow viewports fall back to a normal vertical (or natively scrollable horizontal) list
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/11-horizontal-gallery.html`](../../concepts/11-horizontal-gallery.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
