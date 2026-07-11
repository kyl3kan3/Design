# Spring toggle — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A switch that answers with body language: squash on press, overshoot on travel.

## Exact spec

- On pointerdown the thumb **squashes 15%** (scaleX 1.15 / scaleY 0.85 — volume preserved) against the near wall.
- On release the thumb travels with `--ease-bounce` (**cubic-bezier(0.34, 1.56, 0.64, 1)**, ≈10% overshoot) over ~350ms.
- Track color crossfades `--bg-elev` → `--accent-2` in sync; label states swap.
- `role="switch"`, `aria-checked`, Space/Enter toggles, focus-visible ring.

## Reduced motion

Thumb position and track color swap instantly; no squash, no overshoot.

## Acceptance checklist

- [ ] State is announced correctly (aria-checked flips)
- [ ] Squash preserves volume (x up ⇒ y down)
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/spring-toggle.html`](../../design-system/components/spring-toggle.html).
