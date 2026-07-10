# Attention Lab

Design research and working concepts exploring **patterns that catch attention through motion
and animation** — what makes an interface earn a second look, and what makes that attention
feel respected rather than hijacked.

Two deliverables live here:

1. **Research** — [`research/attention-motion-design-research.md`](research/attention-motion-design-research.md):
   a synthesis of attention psychology, motion-design craft rules, 2025–26 trends, signature
   patterns from award-winning sites, the modern CSS/JS animation stack, and the accessibility
   and performance constraints that keep motion honest.
2. **Concepts** — [`concepts/`](concepts/): fifteen interactive demos, one pattern family each.
   Every demo is a **single self-contained HTML file** — vanilla CSS/JS, system fonts, inline
   SVG, zero dependencies, zero network requests. Open any file in a browser, or start at
   [`concepts/index.html`](concepts/index.html).

## The fifteen concepts

| # | Concept | Pattern family | What it demonstrates |
|---|---------|----------------|----------------------|
| 01 | [Kinetic Typography](concepts/01-kinetic-typography.html) | Type in motion | Staggered mask reveals, cursor-repelled letters, cycling word masks |
| 02 | [Scroll-Driven Story](concepts/02-scroll-story.html) | Scrollytelling | Native scroll-timeline progress bar, pinned parallax scene, scroll-filled text, roll-up counters |
| 03 | [Micro-Interactions](concepts/03-micro-interactions.html) | Tactile feedback | Magnetic button, particle-burst like, squash-and-stretch toggle, morphing submit, error shake, star cascade |
| 04 | [Bento Choreography](concepts/04-bento-grid.html) | Hover systems | Cursor-tracked spotlight borders, per-tile ambient motion that intensifies on hover |
| 05 | [Velocity Marquee](concepts/05-marquee.html) | Ambient motion | Infinite tickers that accelerate, skew, and reverse with scroll velocity |
| 06 | [Cursor & Spotlight](concepts/06-cursor-spotlight.html) | Cursor-led | Custom cursor with lag, mass, and context states; a mask-image flashlight uncovering a generative canvas night scene |
| 07 | [Dimensional Cards](concepts/07-tilt-cards.html) | Depth & parallax | Pointer-driven 3D tilt, layered translateZ parallax, gliding specular glare |
| 08 | [Aurora Ambient Hero](concepts/08-aurora-hero.html) | Living backgrounds | Canvas-rendered aurora curtains (layered sine fields, GPU blur) over a starfield, stepped film grain, `@property` conic-ring CTA |
| 09 | [Shared-Element Morph](concepts/09-shared-element.html) | Transitions | The FLIP technique — a card physically travels into its detail view and back |
| 10 | [Attention Cues](concepts/10-attention-cues.html) | Notification design | The salience ladder: unread counts, bell wiggle, spring toasts with pause-on-hover, a feed where events leave a record, one confetti |
| 11 | [Horizontal Gallery](concepts/11-horizontal-gallery.html) | Scroll capture | A pinned section converting vertical scroll into sideways travel with internal parallax |
| 12 | [Redaction Reveal](concepts/12-text-decode.html) | Type in motion | A declassified file un-redacting itself character by character, capped by a stamped DECLASSIFIED |
| 13 | [Change Blindness Lab](concepts/13-change-blindness.html) | Perception science | Interactive experiment: spotting changes under a flicker cut vs. under animation |
| 14 | [Spring & Easing Lab](concepts/14-easing-lab.html) | Motion craft | Five easing curves raced on one duration; a damped-spring designer emitting native CSS `linear()` |
| 15 | [Intro Sequence](concepts/15-intro-sequence.html) | Entrance choreography | Preloader counter → staggered curtain wipe → masked hero arrival |

Each demo has an **About** panel (top-right) documenting the pattern, the perceptual mechanism
behind it, the exact timing/easing values used, and its reduced-motion behavior.

## Shared craft rules

All twelve demos follow the same motion system (see the research doc for sources):

- **Entrances decelerate, exits accelerate** — and exits run faster than entrances.
- **Micro-feedback in 100–200ms**, standard transitions 250–400ms, hero reveals 600–1000ms.
  Nothing a user waits on exceeds ~1s.
- **Staggers of 30–80ms** between siblings; total choreography stays under ~1s.
- **Compositor-friendly properties only** on the hot path: `transform`, `opacity`, `filter`,
  `clip-path`/`mask`. rAF loops are passive and coalesced.
- **`prefers-reduced-motion` honored everywhere** — movement is replaced (not just shortened)
  with opacity/color changes, ambient loops stop, parallax/tilt/scroll-capture disable
  entirely, and no content is gated behind pointer motion.
- **Attention is a budget**: each page has one loud element; everything else whispers.

## Viewing locally

No build step. Either open `concepts/index.html` directly, or serve the folder
(some browsers restrict a few features on `file://`):

```bash
cd concepts && python3 -m http.server 8000
# → http://localhost:8000
```
