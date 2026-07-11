# Award-Site WebGL: Techniques Behind the Winners

*Research addendum — July 2026. Three parallel research passes: a technical teardown of
Awwwards SOTY/SOTD-class 3D sites (2023–2026), a production raymarching recipe book, and a
raw-WebGL2 GPGPU particle guide. This document backs the Volume II concepts
(17 Particle Field, 18 Liquid Raymarch, 19 Flowmap Type).*

## Who wins, and with what

Confirmed Site-of-the-Year lineage: **Lusion v3** (2023), **Igloo Inc** by abeto (2024),
**Lando Norris** by OFF+BRAND (2025), with **Messenger** (abeto) a top annual winner.
Cross-cutting findings:

- **three.js dominates** every confirmed winner; the raw-WebGL holdout is Active Theory's
  in-house Hydra engine. WebGPU arrives 2025–26 via three.js TSL dual-compile, not raw.
- **Physics is almost always baked, not simulated live**: Lusion pre-calculates cloth drag in
  Houdini and blends four stored directions by cursor (220 KB); Igloo compresses VDB volume
  data for its footer particles; Messenger's "gravity" is a constant pull to a sphere core.
- **Perf discipline is the real differentiator**: Messenger ships 5.7 MB initial / 17.5 MB
  peak with custom LOD; Shopify's Spring '26 uses 4 device tiers and skips entire render
  passes for off-screen sections; "Sketching the Impossible" killed all runtime lights and
  baked tints into textures.
- Igloo renders its **entire UI in WebGL** — text scramble is done by swapping SDF-atlas
  texture offsets per glyph, never touching DOM layout.

### The recurring signature moments (ranked by impact)

1. **Fluid / flowmap cursor trail** distorting images or the whole frame — the single most
   copied award-site tell *(→ concept 19)*
2. **GPGPU ping-pong particles** morphing into logos/shapes/volumes *(→ concept 17)*
3. Scroll-scrubbed cinematic camera on a baked path, with velocity-driven chromatic
   aberration/DOF
4. Shader-native typography (MSDF atlases displaced, scrambled, dissolved on the GPU)
5. **Raymarched SDF blobs/volumes** with fresnel rim and refraction *(→ concept 18)*
6. Render-target scene transitions (threshold-texture reveals, zoom blur, scene-B-as-texture)
7. Infinite WebGL galleries with bend distortion and wrap-around
8. The house post chain: bloom + chromatic aberration + film grain + vignette
9. Full UI in WebGL, or DOM-synced WebGL planes (r3f-scroll-rig pattern)
10. Baked-sim playback masquerading as physics (Houdini VATs, precalced ArrayBuffers)

## Raymarching: the production recipe (used in concept 18)

- **March loop**: 80–128 steps, t-scaled epsilon `0.0005·t`, tight far plane (20–30), and an
  **analytic bounding sphere** before marching — most sky pixels never march at all.
- **Smooth minimum**: use iq's *normalized* forms, where the parameter k is the blend
  inflation in world units and the influence range is 4k (quadratic) / **6k (cubic)** — the
  classic tuning mistake is treating k as the influence radius and welding the scene into one
  potato. Cubic smin is C2-continuous, so specular highlights don't crawl across seams.
- **Normals**: 4-tap tetrahedron trick, loop form with a uniform-based `ZERO` so drivers
  can't inline `map()` four times.
- **Soft shadows**: iq's improved penumbra method (triangulated closest approach), 16–32
  steps; square the result for filmic falloff. **AO**: the canonical 5-tap along the normal
  with early exit.
- **Look**: Schlick fresnel (exponent 2 for a wide "expensive" rim), fake SSS by probing the
  SDF a few steps into the surface along the light, three-light studio rig
  (warm key 1.6–1.9, cool sky fill 0.6·occ, back light 0.4·occ).
- **Output**: exposure → Narkowicz ACES → gamma 1/2.2 → **interleaved-gradient-noise dither
  last** (±1/255). Also jitter the ray start by IGN to break shadow/step banding.
- **Perf**: DPR cap ≤ 2 (biggest single lever), adaptive quality via EMA frame time with
  hysteresis (render scale 1 → 0.75 → 0.5 above ~22 ms), pause off-screen, `highp` mandatory.

## GPGPU particles: the architecture (used in concept 17)

- **Ping-pong float textures beat transform feedback** in raw WebGL2: same GPU-resident
  state, but you get random access to any particle's state, morph targets as textures with
  identical texel layout, WebGL2 MRT to write position+velocity in one pass, and the entire
  battle-tested FBO-particles recipe ecosystem. TF is the fallback when float color buffers
  are unavailable (vanishingly rare).
- **Bufferless everywhere**: fullscreen-triangle sim pass and a `gl_VertexID` render pass —
  zero vertex buffers in the app.
- **Curl noise** (Bridson 2007): the velocity field v = ∇×ψ is divergence-free by
  construction — particles swirl forever and never clump. Three decorrelated simplex fields,
  central differences. Use the canonical Ashima/stegu `snoise` transcription: a single wrong
  constant produces silent garbage (verified the hard way — positions hit ±7·10⁹ in three
  steps with one wrong swizzle).
- **Force balance** for legible shape morphs: spring 8–16 toward targets, curl strength the
  same order as spring pull at rest distance, damping 0.90–0.96, and a max-speed clamp so
  pointer forces can't launch rockets. Text targets need near-zero noise (±0.1 wander blurs
  0.15-unit glyph strokes) and a camera that settles face-on.
- **Render**: additive blending, no depth test (order-independent), tight hot core + wide dim
  skirt in the sprite (`exp(-r²·28)` + `exp(-r²·6)·0.3`) so overlaps read as bloom without a
  post pass. 512² = 262k particles is the desktop sweet spot; 256² on coarse pointers.

## Fluid flowmap: the signature (used in concept 19)

- Two **RG half-float ping-pong buffers at 1/8 resolution** with LINEAR filtering.
- Per frame: semi-Lagrangian self-advection (`texture(field, uv − vel·dt)`), Gaussian splat
  of pointer velocity (radius grows with speed), dissipation ~0.975, and a magnitude clamp so
  violent stirs smear instead of wrapping. The full Jacobi pressure solve is only needed for
  smoke-like incompressibility — the flowmap look skips it.
- Composite: displace the content texture by the field, sample R/G/B at staggered distances
  along the offset for chromatic fringe, tint the wake by flow direction. One sim instance
  app-wide; only the active section may splat (Shopify's rule).

## Sources

Primary case studies: Awwwards case studies for Igloo Inc, Messenger, Lusion, Unseen,
Immersive Garden; Codrops case studies for Anderson Moss, Rogier de Boevé, The Monolith,
Shader.se, Shopify Spring '26, The Sleepers, Sketching the Impossible; Active Theory's
Hydra engine history on Medium. Technique canon: iq's articles (smin, distfunctions,
rmshadows, normalsSDF, fog), the Xds3zN reference shader, Narkowicz ACES, Jimenez IGN,
Bridson's curl-noise paper, stegu/webgl-noise, Barradeau FBO particles, webgl2fundamentals
GPGPU, Pavel Dobryakov's fluid sim, and the Codrops flowmap/infinite-gallery lineage.
Full URL lists live in the session research transcripts.
