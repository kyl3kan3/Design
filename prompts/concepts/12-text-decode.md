# 12 · Text Decode — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A declassified file that un-redacts itself: the headline resolves out of solid redaction bars character by character, a DECLASSIFIED stamp thunks in when it finishes, dossier rows re-redact and clear on hover, and status cells resolve when they enter view.

**Structure.** A classified-document scene: mono type under black redaction blocks that shimmer, then un-redact character-by-character left to right; a rotated DECLASSIFIED stamp slams in after the last line clears.

## Why it works — design intent the implementation must serve

Text that hasn't finished becoming text is an open loop — the brain is compelled to watch until every bar lifts (the Zeigarnik effect applied to rendering). Redaction adds stakes the old Matrix-style cipher never had: the bars say someone hid this from you, which is the strongest curiosity gap there is.

## Exact motion spec

- Each character gets a random settle time (staggered left-to-right, full headline ≈2s).
- Unresolved characters re-roll through block glyphs of varying density (█▓▌) every 2 frames (~33ms) so the bars shimmer like ink under light.
- The stamp lands 220ms after the last character on a 300ms emphasized curve with 2.1→1 scale — a physical thunk. Hover scrambles run a shorter 450ms so navigation never feels blocked.

## Reduced motion

All text renders instantly in its final form with the stamp already placed; hover and viewport effects are disabled.

## Acceptance checklist

- [ ] Un-redacted text is real selectable text (not canvas), present in the DOM throughout for assistive tech
- [ ] Stamp lands once, with overshoot ≤ 25%
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/12-text-decode.html`](../../concepts/12-text-decode.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
