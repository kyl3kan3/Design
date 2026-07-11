# Curtain wipe — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A four-panel curtain that wipes away to reveal content — the theatrical reveal, kept fast.

## Exact spec

- Four vertical panels (`--bg-elev`, hairline gaps) covering the stage; each collapses `scaleY(1 → 0)` with `transform-origin: top`.
- Duration **850ms**, easing **cubic-bezier(0.76, 0, 0.24, 1)**, stagger **70ms** left → right (total ≈ 1.06s).
- Content beneath begins its own entrance (fade + 12px rise) as the last panel clears — overlap, don't queue.
- Panels get `pointer-events: none` and are display-noned after the wipe.

## Reduced motion

Single crossfade (250ms opacity) — no panels, no scaling.

## Acceptance checklist

- [ ] Nothing is clickable through the curtain while it is visible, and everything is after
- [ ] Replay resets cleanly
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/curtain-wipe.html`](../../design-system/components/curtain-wipe.html).
