# 21 · Meridian Studio — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: an editorial studio landing whose centrepiece is a real-time raymarched liquid-metal object — three metaballs sphere-traced per pixel, chrome-shaded by reflecting a procedural warm/cool studio environment, morphing on its own and bending toward your cursor. Everything scrolls over it into black.

**Structure.** An editorial studio landing page (fictional design-and-technology studio) whose fixed, full-viewport centerpiece is a **live raymarched liquid-chrome object** — content scrolls over it into black. Sections: hero (oversized display-serif headline overlapping the metal, mono eyebrow, magnetic CTA), an italic-serif marquee, an editorial split (approach), a gradient-mesh "selected work" grid (generated, no images), an honest numbered process (four ordered steps), a giant closing CTA, footer.

## Why it works — design intent the implementation must serve

A hero that reflects and reacts in real time reads as “impossible object, physically lit” — the brain can’t file it as a picture, so it keeps looking. It answers the template-market 3D-render hero with something their static previews can’t be: alive.

## Exact motion spec

- Raymarch ≤ 96 steps behind an analytic reach test.
- Normalized cubic smin k≈0.17.
- Chrome = env(reflect(rd,n)) with Schlick fresnel rim.
- Procedural env is a vertical plum→warm gradient plus an amber key and a cool counter-rim.
- Narkowicz ACES + interleaved-gradient-noise dither.
- Adaptive DPR by EMA frame time. Type is Georgia set huge with italic accents against system-ui.
- One loud colour (molten amber).

## Reduced motion

The object stops morphing and holds a composed still (no auto-drift, no cursor bend); marquee, scroll cue and reveals freeze; the fixed render stays as a static backdrop.

## Acceptance checklist

- [ ] Chrome reads as reflective metal, not matte — the reflected environment must be high-contrast (crisp bright studio strips on near-black), which is what separates chrome from a pearl blob
- [ ] Normalized cubic smin k≈0.17 (influence = 6k), analytic reach test before marching, ACES + IGN dither, adaptive DPR by EMA frame time
- [ ] Reduced motion holds ONE composed still (fixed time, no drift, no cursor bend) — the fixed render stays as a static backdrop; marquee and reveals freeze
- [ ] Display type is a genuine serif set huge with italic accents against a grotesk UI face; one loud color only
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/21-molten-studio.html`](../../concepts/21-molten-studio.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
