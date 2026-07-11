# Redaction reveal — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A line of classified text that un-redacts itself — blocks shimmer, then lift character by character.

## Exact spec

- Text is real mono text; redaction blocks are per-character overlays (spans or a clipped cover) shimmering via a background-position loop at ~**33ms** steps.
- Reveal runs **left → right, 40–60ms per character**: block scales/fades away, glyph beneath is already in place (no reflow).
- A stamp (rotate ≈ −8°) lands after the line completes: `scale 1.6 → 1` + fade, `--ease-bounce`, single bounce.
- Replayable via a button; text is selectable and present in the DOM throughout.

## Reduced motion

Text fully visible immediately; blocks render as static strikethrough-style marks or are omitted.

## Acceptance checklist

- [ ] Copy/paste works mid-animation (real text underneath)
- [ ] Stamp lands exactly once per replay
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/redaction-line.html`](../../design-system/components/redaction-line.html).
