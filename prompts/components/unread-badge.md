# Bell + unread badge — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

The two rungs of quiet notification salience: a badge that pops in, a bell that physically rings.

## Exact spec

- Badge appears with **45% overshoot** (`scale 0 → 1.45 → 1`, `--ease-bounce`, ~400ms); count increments re-pop at reduced amplitude (~1.18).
- Bell rings **5 decaying swings** — rotate keyframes ≈ 14° → −10° → 6° → −3° → 0 over ~900ms, `transform-origin: top center`.
- Ring fires on new-notification events only — never loops idly.
- Badge count is inside the bell's accessible name (`aria-label="Notifications, 3 unread"`).

## Reduced motion

Badge appears/updates with color change only; the bell never swings.

## Acceptance checklist

- [ ] Bell ring decays naturally (no uniform wobble)
- [ ] Accessible name updates with the count
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/unread-badge.html`](../../design-system/components/unread-badge.html).
