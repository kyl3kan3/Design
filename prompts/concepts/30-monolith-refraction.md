# 30 · Monolith Refraction — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

What this is: a platform landing that fuses three of the lab's techniques — a live nebula field, a raymarched dark-glass monolith that refracts that nebula through its body, and an editorial kinetic-typography entrance where the headline rises line by line out of masks on load. The monolith drifts and parallaxes to the pointer.

**Structure.** A platform landing that **fuses three techniques** — a live fbm nebula, a raymarched dark-glass monolith (rounded-box SDF) that refracts that nebula through its body, and an editorial kinetic-typography entrance where the headline rises line-by-line out of masks on load. The monolith drifts and parallaxes to the pointer. Uppercase grotesk, warm amber accent on a cool nebula. Sections: layers marquee, editorial split, feature grid, closing CTA, footer.

## Why it works — design intent the implementation must serve

A black slab that bends the colour behind it is an “impossible object” the eye interrogates, and the type arriving in choreographed lines gives the load a sense of intent. Two kinds of motion — ambient (the glass) and staged (the words) — that don't compete.

## Exact motion spec

- A rounded-box SDF sphere-marched over a domain-warped fbm nebula.
- At the hit, the background nebula is re-sampled offset by the refracted ray (refract(rd,n,1/1.3)) and darkened, with a Schlick fresnel reflection and a bright edge.
- The headline uses overflow:hidden line masks with inner translateY(110%→0) staggered ~90ms on cubic-bezier(.76,0,.24,1). ACES + dither.
- Adaptive DPR. One warm amber accent against the cool nebula.

## Reduced motion

The headline is shown in place (no line rise), the monolith holds a composed still (no drift/parallax), marquee and reveals freeze; the fixed render stays as a static backdrop.

## Acceptance checklist

- [ ] The nebula is a reusable function of a 2D coord; the monolith re-samples it offset by the refracted ray (refract(rd,n,1/1.3)) and darkens/tints it — that is the refraction of the background through the glass
- [ ] Headline uses overflow:hidden line masks with inner translateY(110%→0), staggered ~90ms on cubic-bezier(.76,0,.24,1), triggered by a .loaded class on the next frame
- [ ] Two motions that do not compete: ambient (glass drift) + staged (type entrance); reduced motion shows type in place and freezes the monolith to a still
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/30-monolith-refraction.html`](../../concepts/30-monolith-refraction.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
