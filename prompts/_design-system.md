# Attention Lab — design-system preamble

> **How to use:** paste this entire file at the top of your prompt, then paste one
> generation prompt from [`concepts/`](concepts/) or [`components/`](components/) after it.
> Every prompt in this library assumes these tokens and rules are already in context.

You are building a piece of the **Attention Lab** design system: a dark, quiet laboratory
aesthetic where motion — not color — is the loud element. Follow everything below exactly.

## Output contract

- Produce a **single self-contained HTML file**: vanilla CSS and JS in `<style>`/`<script>`
  tags, system font stack, inline SVG only, **zero external requests** (no fonts, no CDNs,
  no images). It must work opened straight from `file://`.
- If the request asks for a different stack (React + Tailwind + Framer Motion, Vue, Svelte),
  keep every token value, duration, easing, and behavior identical — only the syntax changes.

## Design tokens

```css
:root {
  --bg: #08080d;            /* page — near-black, blue-violet cast */
  --bg-elev: #10101a;       /* raised surfaces: cards, rails, terminals */
  --ink: #f4f3f7;           /* primary text */
  --ink-dim: #8f8ea0;       /* secondary text — the default voice */
  --accent: #7c5cff;        /* violet — THE loud color; use #6f4dff under white text (4.5:1) */
  --accent-2: #29e6a7;      /* mint — success, live, data */
  --hot: #ff4d6d;           /* alerts/errors only — always dark text on it */
  --gold: #ffb74d;          /* warm highlight, ratings, warnings */
  --line: rgba(244, 243, 247, 0.09);  /* hairline borders */
  --glass: rgba(16, 16, 26, 0.55);    /* frosted panels (pair with backdrop-blur) */
  --font-ui: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  --font-mono: ui-monospace, "SF Mono", "Cascadia Code", Menlo, monospace;
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);    /* entrances — fast start, long settle */
  --ease-bounce: cubic-bezier(0.34, 1.56, 0.64, 1);  /* playful overshoot ~10% */
}
```

Typography: display headlines are `--font-ui` at weight 750–850, letter-spacing −0.02 to
−0.04em, `clamp()`-sized. Labels/kickers are `--font-mono`, 10–12px, letter-spacing
0.12–0.2em, uppercase, `--ink-dim`. Body is 14–16px at line-height 1.55–1.7.

## Motion craft rules (non-negotiable)

1. **Entrances decelerate** (`--ease-out`), **exits accelerate** — and exits run ~30%
   faster than entrances.
2. **Duration ramp:** micro-feedback 100–200ms · standard transitions 250–400ms ·
   hero/entrance choreography 600–1000ms. Nothing a user waits on exceeds ~1s.
3. **Staggers of 30–80ms** between siblings; a whole choreography settles inside ~1s.
4. **Compositor-only on the hot path:** animate `transform`, `opacity`, `filter`,
   `clip-path`/`mask` — never top/left/width/height/margin. rAF loops are passive
   (`{ passive: true }` listeners) and coalesced (one rAF, not one per element).
5. **Attention is a budget:** one loud element per view; everything else whispers.
   If two things move at once, one of them is wrong.
6. **Springs read as physical:** overshoot ≤ 25% for playful elements, ≤ 10% for
   professional ones; squash-and-stretch preserves volume (scaleX up ⇒ scaleY down).

## Reduced motion (required, not optional)

Implement `@media (prefers-reduced-motion: reduce)` so that motion is **replaced, not
just shortened**: ambient loops stop entirely; parallax/tilt/scroll-capture/cursor effects
disable; entrances become opacity-only or instant; state changes still communicate via
color/opacity. **No content may be gated behind pointer motion or an animation completing.**
In JS, check `matchMedia('(prefers-reduced-motion: reduce)')` before starting rAF loops.

## Accessibility floor

- Interactive elements are real `<button>`/`<a>` with visible `:focus-visible` rings
  (2px `--accent`, 2px offset). Decorative layers get `aria-hidden="true"` and
  `pointer-events: none`.
- Text contrast ≥ 4.5:1 (that's why buttons use `#6f4dff`, and `--hot`/`--gold` carry
  dark text). Custom controls carry correct roles (`role="switch"` + `aria-checked`,
  `aria-expanded` on accordions) and full keyboard operation.

## Verification bar

Before you're done: zero console errors; no horizontal overflow at 1440px and 390px
widths; the reduced-motion path actually renders complete (test with emulation);
every number in the prompt's motion spec appears in the code.
