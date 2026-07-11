# 07 · Dimensional Cards — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

Trading-card-style tiles that rotate in 3D toward the pointer. Inside each card, the starfield, artwork, and text sit at different translateZ depths, so tilting produces true parallax — plus a screen-blended glare and a holographic foil (a blurred rainbow conic field, color-dodge at 15%) whose hue rotates as the pointer crosses the card.

**Structure.** Three cards in a row: (1) pointer-driven 3D tilt with a gliding specular glare, (2) layered translateZ parallax depth, (3) a holographic foil whose gradient angle follows the pointer.

## Why it works — design intent the implementation must serve

Parallax under hand control is a depth cue the visual system can't ignore — the card stops being a picture and becomes an object. The moving specular highlight completes the material illusion (the brain reads gloss), and objects get attention that images don't.

## Exact motion spec

- Rotation is clamped to ±10° and eased with lerp 0.12/frame rather than snapping 1:1.
- Depth layers at 6 / 34 / 52 / 60px inside perspective: 1400px.
- On pointer-leave the same lerp carries the card home — one easing system, no transition fighting the loop (~400ms settle). Glare is a 360px radial at the pointer, mix-blend-mode: screen.

## Reduced motion

Tilt, parallax, and glare are fully disabled — cards render flat with their hover shadow as the only feedback.

## Acceptance checklist

- [ ] Tilt is clamped (≈ ±10°) and eases back on pointerleave
- [ ] Cards remain fully readable with no tilt (touch/keyboard)
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/07-tilt-cards.html`](../../concepts/07-tilt-cards.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
