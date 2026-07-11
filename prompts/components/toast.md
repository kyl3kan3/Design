# Toast stack — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

Notifications that arrive with a spring, wait politely, and leave without drama.

## Exact spec

- Entrance: translateY(16px→0) + scale(0.97→1) + fade over **500ms** `--ease-bounce`; exit: **250ms** accelerate-out (exits are ~30% faster and never bounce).
- A **lifetime bar** (scaleX 1→0, linear, ~4.5s) shows remaining time and **pauses on hover AND focus-within**; timer uses the bar's actual elapsed fraction, not a naive setTimeout.
- The stack reflows with transform translations (FLIP or measured offsets), not layout snaps.
- Container is `aria-live="polite"`; each toast has a real close `<button>`.

## Reduced motion

Fade-only entrance/exit; lifetime is longer and the bar is static (or hidden) — time-based dismissal must not depend on animation.

## Acceptance checklist

- [ ] Hovering pauses the countdown and it resumes correctly
- [ ] Three rapid toasts stack and reflow smoothly without overlap
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/toast.html`](../../design-system/components/toast.html).
