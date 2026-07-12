# 27 · Prism Crystal — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: a decision-intelligence landing whose fixed hero is a real-time raymarched faceted crystal — an octahedral gem that refracts a studio environment through its faces with true chromatic dispersion (red, green and blue bend by different amounts), plus a bright fresnel rim. It turns on its own; the pointer spins it.

**Structure.** A decision-intelligence landing whose fixed hero is a **real-time refractive faceted crystal** — an octahedron SDF raymarched behind an analytic reach test, refracting a procedural studio environment through its faces with true chromatic dispersion; it turns on its own, the pointer spins it. Near-monochrome UI so the gem supplies the only colour; gradient-clipped headline. Sections: inputs marquee, editorial split (the idea), stats band, use-case grid, closing CTA, footer.

## Why it works — design intent the implementation must serve

Refraction with rainbow fringing is the visual signature of “premium and precise” — the brain reads cut glass and expects light to behave, then watches it split into spectrum along every edge. It literally shows the product's promise: many inputs, refracted into one clear view.

## Exact motion spec

- Octahedron SDF (|x|+|y|+|z|−s)·0.577 sphere-marched behind an analytic reach test.
- At the surface, a Schlick fresnel mixes a reflection of the procedural studio env with a refraction sampled at three IORs (1.44 / 1.47 / 1.50) for the dispersion fringe.
- Bright grazing rim + spectral edge glow. ACES + dither.
- Adaptive DPR. UI stays near-monochrome so the gem supplies the only colour.

## Reduced motion

The crystal stops turning and holds a single composed still (no auto-spin, no pointer spin); marquee and reveals freeze; the fixed render stays as a static backdrop.

## Acceptance checklist

- [ ] Dispersion is real: sample the environment along THREE refracted directions at different IORs (≈1.44 / 1.47 / 1.50), one per channel, so edges fringe into spectrum
- [ ] Schlick fresnel mixes an environment reflection with the refraction; bright grazing rim + a sharp specular sparkle; the procedural env must be high-contrast (bright strips + key spots on dark) or the gem reads flat/dark
- [ ] Octahedron SDF `(|x|+|y|+|z|−s)·0.577`; ACES + dither; adaptive DPR; reduced motion stops the spin and holds a still
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/27-prism-crystal.html`](../../concepts/27-prism-crystal.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
