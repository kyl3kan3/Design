# Dialog choreography — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A native <dialog> whose entrance and exit are pure CSS — @starting-style in, allow-discrete out.

## Exact spec

- Use a real `<dialog>` + `showModal()`: focus trap, Esc, and `::backdrop` come free.
- Entry: `@starting-style { opacity: 0; transform: translateY(12px) scale(0.96); }` → settled state over **400ms** `--ease-out`; backdrop fades from transparent to `rgba(8,8,13,0.6)` with blur.
- Exit: transition `display` and `overlay` with `transition-behavior: allow-discrete` so close animates — **250ms** accelerate-out (faster than entry).
- No JS animation code at all — JS only calls `showModal()`/`close()`.

## Reduced motion

Opacity-only fades at short duration for both directions.

## Acceptance checklist

- [ ] Exit animates (allow-discrete works) — dialog does not vanish instantly
- [ ] Focus returns to the opener on close
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/dialog-choreography.html`](../../design-system/components/dialog-choreography.html).
