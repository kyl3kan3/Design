# 14 · Spring & Easing Lab — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

Five identical moves, five different easing curves, raced side by side — then a designer that samples a real damped-spring simulation into CSS's native linear() function, with copyable output.

**Structure.** A motion-craft tool in two halves: (1) five easing curves race identical dots over one duration with their curves plotted; (2) a damped-spring designer (stiffness/damping/mass sliders) that previews the motion and emits a copyable native CSS `linear()` approximation.

## Why it works — design intent the implementation must serve

Why it matters: easing is where users perceive quality without knowing why. Deceleration into place reads as physical; a slight overshoot (≤15%) reads as alive; pure linear motion reads as robotic. Since linear() shipped in all engines, real spring physics needs zero JavaScript at runtime.

## Exact motion spec

- The race runs one duration (adjustable 600–2000ms) so only the curve differs.
- Plots are evaluated from the exact bézier control points (bisection solve) and spring function that drive the motion. The spring designer solves the closed-form underdamped oscillator x(t)=1−e^(−ζω₀t)(cos ω_d t + (ζω₀/ω_d) sin ω_d t), samples 49 stops (48 intervals), and emits linear(). Damping ratio 0.7–1.0 feels premium.
- Below 0.5 becomes cartoon.

## Reduced motion

Runs complete instantly (curve plots — the educational content — carry the comparison), and nothing animates without an explicit button press either way.

## Acceptance checklist

- [ ] The emitted linear() string actually reproduces the previewed spring when applied
- [ ] Race can be replayed; curves are labeled with their cubic-bezier values
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/14-easing-lab.html`](../../concepts/14-easing-lab.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
