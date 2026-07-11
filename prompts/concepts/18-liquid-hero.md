# 18 · Liquid Raymarch — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A signed-distance scene sphere-traced per pixel in one fragment shader: five metaballs on incommensurate orbits blend through a cubic smooth-minimum, one chases your cursor, and a click sends a merge-pulse through the blend radius. No geometry, no libraries — the surface exists only as math.

**Structure.** Raw WebGL2 fragment raymarcher: five drifting metaballs blended with iq's normalized cubic smin, sphere-traced with soft shadows (penumbra method), 5-tap AO, Schlick fresnel rim, three-light studio rig, ACES tonemap + IGN dither; pointer repels the blobs; a "pulse" control briefly raises the blend radius.

## Why it works — design intent the implementation must serve

Raymarched liquid is the flagship material of award-tier WebGL (Igloo's ice, Lusion's blobs): light behaves like it does on real gloss — fresnel rim, soft shadows, creamy specular — on a surface that visibly has no polygons. The brain reads “impossible object, physically lit” and stares.

## Exact motion spec

- 100-step sphere trace inside an analytic bounding sphere (most sky pixels never march), t-scaled epsilon 0.0005·t with IGN-jittered ray starts.
- Normalized cubic smin k=0.16 — influence 6k (C2, highlights don't crawl).
- Iq's penumbra soft shadows (w 0.1), 5-tap AO, Schlick fresnel².
- Three-light studio rig with warm key / cool sky fill / back light, ACES tonemap, gamma, then 1/255 dither. Pointer camera parallax lerps at λ=8.
- Adaptive quality drops render scale 1 → 0.75 → 0.5 if frame time passes 22ms.

## Reduced motion

The simulation freezes at a composed moment and renders exactly one frame; pointer parallax and the chase-blob are disabled. No WebGL2 → static message.

## Acceptance checklist

- [ ] Remember smin influence = 6k for the cubic form — k is blend inflation in world units, NOT the influence radius (k ≈ 0.16 here, not 0.5+)
- [ ] Adaptive render scale (EMA frame time with hysteresis) keeps frame time under ~22ms
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/18-liquid-hero.html`](../../concepts/18-liquid-hero.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
