# 25 · Cirrus Clouds — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: a climate-intelligence landing whose fixed hero is a real-time volumetric cloudscape — density raymarched through an fbm field, self-shadowed by a second march toward a low sun, at dawn. The clouds drift on wind; the cursor pushes the weather.

**Structure.** A climate-intelligence landing whose fixed hero is a **real-time volumetric cloudscape** — cloud density raymarched through an fbm field and self-shadowed by a second short march toward a low dawn sun; clouds drift on wind, the cursor pushes the weather. Airy grotesk type, dawn palette (peach accent). Sections: data-types marquee, editorial split (the model), a roll-up stats band, an industries grid, closing CTA, footer.

## Why it works — design intent the implementation must serve

Real volumetric light — soft edges, sun bleeding through thinning cloud, warm tops and cool undersides — reads as a photograph of the sky that happens to be moving. It's atmosphere you can feel, which is the whole promise of the product.

## Exact motion spec

- ~48-step density march with a Henyey-Greenstein-ish forward glow and Beer's-law transmittance.
- A short 5-step light march per sample for self-shadowing.
- Front-to-back compositing with early-out at full opacity.
- Dawn sky gradient with a sun-disc glow behind. ACES + dither.
- DPR capped low with EMA adaptive scaling. Type is an airy grotesk.
- Dawn peach is the one accent.

## Reduced motion

The wind stops and the sky holds a single composed still (no drift, no cursor push); marquee and reveals freeze; the fixed render stays as a static backdrop.

## Acceptance checklist

- [ ] Clouds read as billowing volume, not flat haze — gate density with contrast (×~2.8 after threshold), erode edges with a second octave, keep ambient low so cores shadow; add a forward-scatter (HG-ish) silver lining
- [ ] Per-sample light march (≈5 steps) toward the sun for self-shadowing; front-to-back compositing with early-out at full opacity; Beer-law transmittance
- [ ] DPR capped low (~1.25) with EMA adaptive scaling — volumetric marching is the most expensive hero in the set
- [ ] Reduced motion freezes wind + cursor to a single composed still
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/25-cirrus-clouds.html`](../../concepts/25-cirrus-clouds.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
