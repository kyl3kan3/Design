# 17 · Particle Field — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

262,144 particles simulated entirely on the GPU: positions and velocities live in floating-point textures, a fragment shader integrates curl-noise flow, spring-to-shape forces and your pointer every frame, and the points render with additive blending and depth-attenuated sprites. No libraries.

**Structure.** Raw WebGL2, no libraries: 262,144 particles (512² RGBA32F ping-pong position+velocity textures, MRT via drawBuffers) advected by curl noise with spring forces toward morph targets (sphere → the word "LOOK" → torus knot), pointer repulsion, additive-blended sprite rendering via gl_VertexID (bufferless), and UI chips to switch shapes.

## Why it works — design intent the implementation must serve

A quarter-million things obeying one hand is the signature move of award-tier WebGL sites — mass, physics, and immediate response at a scale DOM animation cannot fake. The morphs exploit the same open-loop pull as the redaction reveal: you watch until the shape finishes becoming.

## Exact motion spec

- MRT ping-pong at 512×512 RGBA32F (position + velocity).
- Forces per frame: spring 12 toward the target shape, Bridson curl noise 0.9 (divergence-free — swirls, never clumps), pointer repulsor with Gaussian falloff (radius 1.4 world units), click impulse ×6 decaying at 0.9/frame.
- Velocity damping 0.93, speed clamp 3.
- Color ramps violet → green by speed with depth fade.
- DPR capped at 1.75, simulation halves on coarse-pointer devices (256² = 65k).

## Reduced motion

The simulation runs 200 warm-up steps invisibly, renders one settled frame, and stops — shape buttons still work but swap as stills. No WebGL2/float-buffer support → static message, no broken canvas.

## Acceptance checklist

- [ ] Use the canonical Ashima/stegu snoise verbatim — one wrong constant silently explodes positions
- [ ] Text morph target uses near-zero noise and a camera that settles frontal so glyphs stay legible
- [ ] Falls back gracefully (static poster) without WebGL2/float-color support
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/17-particle-field.html`](../../concepts/17-particle-field.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
