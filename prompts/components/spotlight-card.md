# Spotlight card — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A card whose 1px border lights up under the cursor — a gradient ring that follows you, masked so only the border shows.

## Exact spec

- A `::before` overlay carries `radial-gradient(240px circle at var(--mx) var(--my), --accent, --accent-2 45%, transparent 65%)`.
- Mask it to the border: `padding: 1px` + two-layer mask with `mask-composite: exclude` (WebKit: `-webkit-mask-composite: xor`) so only a **1px ring** renders.
- Pointer position writes `--mx/--my` **only on the hovered card**, coalesced in one rAF for the whole grid; ring fades in/out over 350ms.
- `:focus-within` shows a centered static ring so keyboard users get the same reward.

## Reduced motion

Ring becomes a static border-color change on hover/focus.

## Acceptance checklist

- [ ] Non-hovered cards do zero per-frame work
- [ ] Focus shows the ring without a pointer
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/spotlight-card.html`](../../design-system/components/spotlight-card.html).
