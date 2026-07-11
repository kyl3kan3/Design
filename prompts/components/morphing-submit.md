# Morphing submit — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

One button that is the whole submission story: label → spinner → success check, with no jump cuts and no element swaps.

## Exact spec

- Stage 1→2: the button animates `width` (grid-template-columns or fixed measured widths — this is the one sanctioned layout animation, ~300ms `--ease-out`) to a circle; label fades/scales out 150ms; a conic/border spinner fades in, rotating 800ms/turn.
- Stage 2→3: spinner stops, an inline SVG check **draws via stroke-dashoffset** over 400ms `--ease-out`; circle fills `--accent-2` with dark text/icon.
- The `<button>` never leaves the DOM; it is `disabled` + `aria-busy="true"` during pending; final state announces via `aria-live="polite"` text.
- Auto-reset (or reset button) after ~1.8s for demo purposes.

## Reduced motion

No width morph or spin: label text swaps "Submit → Sending… → Sent ✓" with color changes only.

## Acceptance checklist

- [ ] No layout jump in surrounding content during the morph
- [ ] Screen reader hears the state change (aria-live)
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/morphing-submit.html`](../../design-system/components/morphing-submit.html).
