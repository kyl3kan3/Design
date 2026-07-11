# Star cascade — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A rating control where choosing 4 stars feels like lighting 4 candles in a row.

## Exact spec

- Choosing rating n lights stars 1…n **sequentially, 60ms apart**, each with a pop (`scale 0.7 → 1.18 → 1`, `--ease-bounce` ~260ms) and fill `--gold`.
- Re-rating lower dims the excess stars simultaneously (dimming is not a celebration).
- Semantics: `role="radiogroup"` with roving tabindex — Arrow keys move selection, Space/Enter confirms; hover previews with a lighter fill.
- Clear any in-flight cascade timeouts when a new rating arrives (no overlapping cascades).

## Reduced motion

All n stars light simultaneously with a color change only.

## Acceptance checklist

- [ ] Arrow-key navigation works and is visible
- [ ] Spamming different ratings never leaves stray lit stars (timeouts cleared)
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/star-cascade.html`](../../design-system/components/star-cascade.html).
