# 24 · Atelier Noir — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: a fragrance-house landing whose hero wordmark is drawn into a live fluid field — a GPU flow simulation you stir with the cursor, smearing and chromatically splitting the letters like ink in water, then letting them heal.

**Structure.** A fragrance-house landing whose hero wordmark is drawn into a **live fluid field the cursor stirs** — the serif letters smear and chromatically split like ink in water, then heal. Dark oxblood-rose luxury; high-contrast serif; fashion-editorial layout. Sections: notes marquee, editorial split (the house), a "collection" grid of numbered scents, a four-step "making" process, closing CTA, footer.

## Why it works — design intent the implementation must serve

Type that responds to touch like a liquid is unexpected and tactile — you keep moving just to watch it bleed and recover. For a scent brand it stands in for the thing itself: something you disturb and that lingers.

## Exact motion spec

- Two RG16F ping-pong buffers at 1/8 resolution — semi-Lagrangian self-advection, Gaussian pointer splats (radius grows with speed), dissipation 0.975/frame, magnitude clamp so hard stirs smear instead of wrapping.
- The composite displaces the wordmark texture and samples R/G/B at staggered offsets for a rose-and-gold chromatic fringe. Type is a high-contrast serif.
- Oxblood rose is the one accent.

## Reduced motion

The wordmark renders once, undistorted and still; no fluid simulation runs; marquee and reveals freeze.

## Acceptance checklist

- [ ] Two `RG16F` ping-pong buffers at 1/8 resolution: semi-Lagrangian self-advection, Gaussian pointer splats (radius grows with speed), dissipation 0.975, magnitude clamp; the composite displaces a canvas-rendered wordmark texture and samples R/G/B at staggered offsets for the chromatic fringe
- [ ] Splat gain / displacement tuned to be clearly visible (this effect fails silently by being too subtle — verify by stirring)
- [ ] Reduced motion renders the wordmark once, undistorted, and runs no simulation
- [ ] Requires WebGL2 + float/half-float colour buffers — ship a static fallback when unavailable
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/24-atelier-noir.html`](../../concepts/24-atelier-noir.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
