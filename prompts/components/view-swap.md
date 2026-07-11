# View Transitions swap — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A grid whose shuffles and filters morph — items physically travel to their new slots via the View Transitions API.

## Exact spec

- Wrap the DOM mutation in `document.startViewTransition(() => mutate())`.
- Give each item a stable, unique `view-transition-name` (e.g. `card-7`) so reorders track identity and morph positions.
- Tune `::view-transition-group(*) { animation-duration: 450ms; animation-timing-function: var(--ease-out); }`; removed items fade via their old snapshot.
- Feature-detect: without `document.startViewTransition`, mutate instantly — the API call is the only difference.

## Reduced motion

Skip the transition entirely (mutate directly) when reduced motion is set.

## Acceptance checklist

- [ ] Shuffle morphs items along paths (not a crossfade of the whole grid)
- [ ] Unsupported browsers still work with instant swaps
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`design-system/components/view-swap.html`](../../design-system/components/view-swap.html).
