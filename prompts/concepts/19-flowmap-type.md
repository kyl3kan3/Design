# 19 · Flowmap Type — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A live fluid velocity field follows your cursor: each move splats momentum into a half-float simulation texture that advects itself forward. The giant poster type samples through that field — displaced, smeared, and chromatically split wherever you've stirred, healing as the flow decays.

**Structure.** Raw WebGL2 poster: a fluid flowmap (RG16F ping-pong at 1/8 resolution — semi-Lagrangian self-advection, Gaussian pointer splats, dissipation ≈ 0.975, magnitude clamp) displaces and chromatically splits large canvas-rendered typography along the cursor's wake.

## Why it works — design intent the implementation must serve

The flowmap cursor trail is the single most copied signature of award-winning WebGL sites — because it gives the page a memory. The type doesn't just react under the cursor; it remembers the path your hand took and lets it dissolve slowly, which reads as material, not effect.

## Exact motion spec

- Two RG-half-float ping-pong buffers at 1/8 resolution.
- Per frame: semi-Lagrangian self-advection, Gaussian splat of pointer velocity (radius scales with speed), dissipation 0.975. Composite pass displaces the poster texture by the field (strength 0.34, field clamped at 0.30), samples R/G/B at ±35% of the offset for chromatic fringe, and tints the wake violet→green by flow direction. Poster is a 2048px offscreen 2D canvas re-rendered on resize.

## Reduced motion

The simulation never runs — the poster renders undistorted, once. No WebGL2/half-float support → static message.

## Acceptance checklist

- [ ] Type texture is drawn once at DPR resolution and only the composite runs per frame
- [ ] Splat gain / displacement are tuned to be unmissable (this fails silently by being too subtle — verify visually)
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/19-flowmap-type.html`](../../concepts/19-flowmap-type.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
