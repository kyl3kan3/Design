# 23 · Strata Lattice — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: an architecture-studio landing whose fixed hero is a real-time raymarched gyroid — a minimal surface used in real structural engineering — carved into a slowly rotating specimen. It spins on its own; the pointer turns it.

**Structure.** An architecture-studio landing whose fixed hero is a real-time raymarched **gyroid specimen** — the gyroid isosurface `abs(dot(sin p, cos p.yzx)) − t` as a thin shell intersected with a bounding sphere, slowly rotating; the pointer turns it. Structural uppercase-grotesk type on a concrete palette, one terracotta accent. Sections: disciplines marquee, editorial split (practice), a gridded projects grid, a four-phase method, closing CTA, footer.

## Why it works — design intent the implementation must serve

A coherent, repeating structure turning in space reads as something built and physical — the eye follows its rotation and reads the depth through its tunnels. It says “spatial” before a word is read, which is exactly this studio's business.

## Exact motion spec

- The gyroid isosurface abs(dot(sin p, cos p.yzx)) − t as a thin shell, intersected with a bounding sphere and sphere-marched behind an analytic reach test.
- Steps-based ambient occlusion, a warm key with terracotta specular and fresnel rim, cool shadow fill.
- ACES + dither.
- Adaptive DPR. Type is structural uppercase grotesk on a concrete palette.
- Terracotta is the one accent.

## Reduced motion

The specimen stops rotating and holds a single composed still; marquee and reveals freeze; the fixed render stays as a static backdrop.

## Acceptance checklist

- [ ] The camera stays OUTSIDE the object (bound it with a sphere + analytic reach test) — marching an infinite gyroid traps the camera inside solid and renders as a flat near-wall
- [ ] Thin shell so tunnels/holes read; squared steps-based AO for crevice depth; warm key light + terracotta fresnel rim; ACES + dither; adaptive DPR
- [ ] Reduced motion stops the rotation and holds a composed still
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/23-strata-lattice.html`](../../concepts/23-strata-lattice.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
