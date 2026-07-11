# 05 · Velocity Marquee — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

Three infinite ticker bands drifting at different speeds and directions. Scrolling injects your velocity: bands accelerate, skew like they're under wind load, and follow your scroll direction, then relax back to their drift.

**Structure.** Three infinite ticker bands (alternating filled and outlined words) drifting at different base speeds and directions; scrolling injects velocity and skew, then both decay back to the drift.

## Why it works — design intent the implementation must serve

Continuous horizontal drift registers in peripheral vision without demanding focus — until it responds to you. Coupling the bands to scroll velocity turns passive ambience into a physics toy; the skew reads as momentum, which the brain finds irresistibly causal.

## Exact motion spec

- JS rAF loop advances each track by base drift (0.5–0.9 px/frame) + |scroll velocity| × 0.35, wrapped seamlessly at one content-unit width (the row is duplicated past 2× the viewport).
- SkewX = velocity × 0.05 clamped to ±12°, decayed with lerp 0.08/frame. Alternating filled and 1.5px-stroked outline words add depth on one type size.

## Reduced motion

Bands stop drifting entirely and become ordinary horizontally scrollable rows; the tilt is removed and no skew is applied.

## Acceptance checklist

- [ ] Loop wrap is seamless (row duplicated ≥ 2× viewport, wrapped at one content width)
- [ ] No horizontal page overflow from the bands
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/05-marquee.html`](../../concepts/05-marquee.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
