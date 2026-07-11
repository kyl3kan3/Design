# 01 · Kinetic Typography — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

Oversized display type that enters with a staggered mask reveal, reacts to the cursor, and cycles key words through a clip mask.

**Structure.** Three stacked full-height sections: (1) a masked headline whose lines rise out of `overflow:hidden` clips on load, (2) a headline whose letters individually repel the cursor and spring back, (3) a static sentence with one word slot cycling through alternatives via a vertical mask.

## Why it works — design intent the implementation must serve

Type at this scale is itself the image, and motion in text is doubly salient — the eye is drawn to movement pre-attentively, and to words automatically. Staggering the lines creates a reading rhythm the eye follows like a cue.

## Exact motion spec

- Lines rise 900ms on cubic-bezier(.16,1,.3,1) with a 120ms stagger.
- Supporting copy waits until the headline lands (~1.15s). Letters ease back over 450ms after cursor displacement. Word rotator: 700ms in / 500ms out — exits faster than entrances.

## Reduced motion

All text renders in place immediately, cursor repulsion is disabled, and the word rotator swaps instantly instead of sliding.

## Acceptance checklist

- [ ] Letters repel smoothly with no per-letter layout thrash (transform only, one rAF)
- [ ] Word cycle never shows two words at once mid-transition
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/01-kinetic-typography.html`](../../concepts/01-kinetic-typography.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
