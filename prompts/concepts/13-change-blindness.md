# 13 · Change Blindness Lab — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A self-experiment. One of nine metrics changes each round. In Cut mode the change happens during a 140ms blank flicker — the classic lab paradigm. In Motion mode the same change is animated. Try both and compare your hit rates.

**Structure.** An interactive perception experiment: rounds alternate between a flicker cut (change hidden by an interruption) and an animated change; the player clicks what changed; a scoreboard contrasts detection speed, teaching why motion beats cuts for attention.

## Why it works — design intent the implementation must serve

Why it matters: vision is functionally blind during saccades, blinks, and flickers; an instant swap that coincides with one goes unseen (lab subjects needed 20–40 flicker cycles to find a single altered detail). Animation defeats change blindness because peripheral vision catches motion automatically — this is the strongest functional argument for UI animation, beyond aesthetics.

## Exact motion spec

- The flicker mask shows for 140ms (in the 60–250ms range used in the literature).
- The animated variant moves only the changed region — value rolls over 600ms, bar eases on cubic-bezier(.16,1,.3,1), one 1.1s glow pulse, everything else stays static so the one change owns the contrast.

## Reduced motion

The animated mode substitutes a static highlight ring and instant value swap — change remains findable through color contrast rather than movement, and the flicker in Cut mode still works (it contains no motion).

## Acceptance checklist

- [ ] Changes are genuinely randomized per round
- [ ] The lesson (motion detected faster than cuts) is stated in the results copy
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/13-change-blindness.html`](../../concepts/13-change-blindness.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
