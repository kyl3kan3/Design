# 28 · Eclipse Corona — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: a privacy-infrastructure landing whose fixed hero fuses four techniques in one fragment shader — an analytic dark sphere (the occulting body), a blazing corona with chromatic dispersion around its rim, volumetric god-rays streaming past it, and a drifting starfield + nebula behind. The pointer moves the light; the eclipse breathes.

**Structure.** A privacy-infrastructure landing whose fixed hero **combines four techniques in one fragment shader** — an analytic dark occulting sphere, a blazing corona with chromatic dispersion around its rim, volumetric god-rays streaming past, and a drifting starfield + nebula behind; the pointer moves the light. Refined grotesk, one gold accent. Sections: capabilities marquee, editorial split, stats band, use-case grid, closing CTA, footer.

## Why it works — design intent the implementation must serve

An eclipse is pure focal tension — a black disc ringed in fire, the eye pinned to the edge where light escapes. It literally pictures the product: everything happening behind something you can't see through.

## Exact motion spec

- Ray–sphere intersection gives the body and the perpendicular distance d to its centre.
- The corona is exp(−(d−1)) with a hot ring at d≈1 tinted through the spectrum by angle (the dispersion).
- God-rays are angular fbm streaks masked outside the disc.
- Stars are a hashed grid twinkling over an fbm nebula. ACES + dither.
- Adaptive DPR. One gold accent.
- Everything else near-black.

## Reduced motion

The corona, rays and stars hold a single composed still (no breathing, no pointer light); marquee and reveals freeze; the fixed render stays as a static backdrop.

## Acceptance checklist

- [ ] Ray–sphere intersection gives the body and the perpendicular distance d to its centre; corona = exp(−(d−1)) plus a hot ring at d≈1 tinted through the spectrum by angle (that is the dispersion)
- [ ] God-rays are angular-fbm streaks masked to outside the disc; stars are a hashed grid twinkling over an fbm nebula — all layered in ONE pass, no FBOs
- [ ] ACES + dither; adaptive DPR; reduced motion holds a single composed still (no breathing, no pointer light)
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/28-eclipse-corona.html`](../../concepts/28-eclipse-corona.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
