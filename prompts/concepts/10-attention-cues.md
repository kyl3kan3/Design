# 10 · Attention Cues — generation prompt

> **Setup:** paste [`_design-system.md`](../_design-system.md) first, then this prompt.
> Default target is a single self-contained HTML file (vanilla CSS/JS); to target another
> stack (React + Tailwind + Framer Motion, Vue, Svelte), say so — keep every value identical.

## Build this

A mock team-inbox where you fire the events yourself and watch the interface choose its attention weapon: toast springs, badge pops, bell wiggle, unread counts, a one-shot confetti celebration — and a feed where every event leaves a persistent record.

**Structure.** A notification design study: the "salience ladder" (dot → count → wiggle → toast → takeover, each rung labeled with when to use it), a live feed where events land with a spring and leave a record, spring toasts with pause-on-hover lifetime bars, and exactly one confetti moment.

## Why it works — design intent the implementation must serve

Each cue sits at a deliberate rung on the salience ladder — a count whispers, a pulse hums, a wiggle taps your shoulder, a toast interrupts. The toast is the transient and the feed row is the consequence: notification design fails when the knock has no letter behind it. And the rare celebration stays special because everything else stays quiet.

## Exact motion spec

- Toasts enter over 500ms on cubic-bezier(.34,1.56,.64,1) with a visible lifetime bar (hover or focus pauses it — WCAG 2.2.2).
- Exits take 250ms, half the entrance. Feed rows drop in over 450ms and their tone highlight decays after 1.8s — new stays loud only while it's new. Badge pops 45% past scale.
- The bell rings through 5 decaying swings (700ms).

## Reduced motion

Toasts crossfade in place, the bell and flame hold still, pings stop looping, and confetti is skipped entirely — every state change stays visible as a color/badge change.

## Acceptance checklist

- [ ] Toast lifetime bar pauses on hover AND on focus
- [ ] The feed keeps history — attention cues leave a record instead of vanishing
- [ ] Single self-contained file, zero external requests, works from `file://`
- [ ] Every number in the motion spec above appears in the code
- [ ] Reduced-motion path implemented as specified (replaced, not shortened)
- [ ] Zero console errors; no horizontal overflow at 1440px and 390px
- [ ] Hot-path animation is compositor-only (transform / opacity / filter / mask)

## Reference

Working reference implementation: [`concepts/10-attention-cues.html`](../../concepts/10-attention-cues.html) — the bar is
"indistinguishable in craft from the reference." Open it, interact, and compare before calling done.
