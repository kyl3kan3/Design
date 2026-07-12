# 29 · Flux Swarm — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: a realtime-data landing whose hero couples two GPU simulations — a fluid velocity field you stir with the cursor, and a swarm of 65,536 particles that read that field and ride its currents. Two systems, one feeding the other, both running on the GPU every frame.

**Structure.** A realtime-data landing whose hero **couples two GPU simulations** — a cursor-stirred fluid velocity field, and a swarm of ~65k particles that read that field and ride its currents. Dark indigo + aurora (violet→cyan→green). Sections: pipeline-stage marquee, editorial split, stats band, use-case grid, closing CTA, footer.

## Why it works — design intent the implementation must serve

Thousands of points organising into flowing filaments — and reorganising the instant you move — reads as something alive and responsive. It's the product as a picture: streams of data finding their path in real time.

## Exact motion spec

- The fluid is an RG16F ping-pong field at 1/8 res (semi-Lagrangian advection + Gaussian cursor splats + dissipation).
- The particles live in an RGBA32F ping-pong texture (256²), each reading texture(fluid, pos) for its velocity plus a little ambient curl, respawning on death.
- Rendered bufferless via gl_VertexID → texelFetch, additive, coloured by speed (violet slow → cyan-green fast). Float-buffer feature-checked with a static fallback.

## Reduced motion

The simulation runs a few warm-up steps then holds a single composed still — no ongoing motion, no cursor stirring; marquee and reveals freeze.

## Acceptance checklist

- [ ] Fluid is an RG16F ping-pong field at 1/8 res (advect + Gaussian cursor splats + dissipation); particles live in an RGBA32F ping-pong texture (256²), each reading texture(fluid, pos) for velocity + a little ambient curl, respawning on death
- [ ] Particles render bufferless via gl_VertexID → texelFetch, additive, coloured by speed; the particle sim samples the SAME fluid texture the cursor stirs (that is the coupling)
- [ ] Feature-check EXT_color_buffer_float with a static fallback; reduced motion warms up then holds a composed still
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/29-flux-swarm.html`](../../concepts/29-flux-swarm.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
