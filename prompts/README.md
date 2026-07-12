# Attention Lab — prompt library

Copy-paste generation prompts for every pattern in this repo. Each prompt regenerates its
piece **at reference quality** in any capable AI coding tool (Claude Code, claude.ai, Cursor,
etc.) — because each one was distilled *from* a working, verified implementation that lives
in this repo, not written aspirationally.

## How to use

1. Paste [`_design-system.md`](_design-system.md) — the shared tokens + craft rules — at
   the top of your prompt. Every prompt below assumes it.
2. Paste one prompt from [`concepts/`](concepts/) or [`components/`](components/).
3. Optionally add a stack directive ("as a React component with Tailwind and Framer Motion,
   same values") — the prompts default to a single dependency-free HTML file.
4. Hold the output to the prompt's **acceptance checklist**, then compare against the linked
   reference implementation. The bar is "indistinguishable in craft."

Why this beats a snippet library: a snippet gives you one frozen artifact; a prompt + spec +
reference lets you regenerate the pattern *in your own stack and brand* while keeping the
motion values that make it feel right.

## Component prompts (13)

| Component | What it builds |
|---|---|
| [Magnetic button](components/magnetic-button.md) | A primary CTA that is pulled toward the cursor inside a proximity field, then springs back — the flagship "the interface noticed you" microinteraction. |
| [Spring toggle](components/spring-toggle.md) | A switch that answers with body language: squash on press, overshoot on travel. |
| [Morphing submit](components/morphing-submit.md) | One button that is the whole submission story: label → spinner → success check, with no jump cuts and no element swaps. |
| [Heart burst](components/heart-burst.md) | A like button whose confirmation is a small celebration — felt, brief, and cheap. |
| [Star cascade](components/star-cascade.md) | A rating control where choosing 4 stars feels like lighting 4 candles in a row. |
| [Toast stack](components/toast.md) | Notifications that arrive with a spring, wait politely, and leave without drama. |
| [Bell + unread badge](components/unread-badge.md) | The two rungs of quiet notification salience: a badge that pops in, a bell that physically rings. |
| [Spotlight card](components/spotlight-card.md) | A card whose 1px border lights up under the cursor — a gradient ring that follows you, masked so only the border shows. |
| [Redaction reveal](components/redaction-line.md) | A line of classified text that un-redacts itself — blocks shimmer, then lift character by character. |
| [Stat counter](components/counter.md) | A number that rolls up to its value on first sight — momentum you can read. |
| [Curtain wipe](components/curtain-wipe.md) | A four-panel curtain that wipes away to reveal content — the theatrical reveal, kept fast. |
| [Dialog choreography](components/dialog-choreography.md) | A native <dialog> whose entrance and exit are pure CSS — @starting-style in, allow-discrete out. |
| [View Transitions swap](components/view-swap.md) | A grid whose shuffles and filters morph — items physically travel to their new slots via the View Transitions API. |

## Concept prompts (24)

Full-page patterns and applied examples, 01–20:

| # | Concept | One-liner |
|---|---------|-----------|
| 01 | [Kinetic Typography](concepts/01-kinetic-typography.md) | Oversized display type that enters with a staggered mask reveal, reacts to the cursor, and cycles key words through a clip mask. |
| 02 | [Scroll-Driven Story](concepts/02-scroll-story.md) | A narrative page where scroll position drives everything: a progress bar, staggered section reveals, a text passage that “fills” with brightness as you read, a pinned parallax scene, and counters that roll up on arrival. |
| 03 | [Micro-Interactions](concepts/03-micro-interactions.md) | Six interaction stations: a magnetic button, a particle-burst like, a squash-and-stretch toggle, a morphing submit, an error shake, and a cascading star rating. |
| 04 | [Bento Grid Choreography](concepts/04-bento-grid.md) | A feature grid in the bento style — mixed tile sizes, each with its own ambient motion that intensifies on hover, plus a cursor-tracked spotlight that lights up card borders as you sweep across. |
| 05 | [Velocity Marquee](concepts/05-marquee.md) | Three infinite ticker bands drifting at different speeds and directions. |
| 06 | [Cursor & Spotlight](concepts/06-cursor-spotlight.md) | The cursor becomes a designed object: a dot with a spring-lagged ring that morphs by context (grows on links, becomes a “VIEW” pill over work items) — and a section where it turns into a literal flashlight, uncovering a generative canvas night scene: a harbor skyline, a moon, and eleven windows still lit. |
| 07 | [Dimensional Cards](concepts/07-tilt-cards.md) | Trading-card-style tiles that rotate in 3D toward the pointer. |
| 08 | [Aurora Ambient Hero](concepts/08-aurora-hero.md) | A hero lit by a real-time aurora: canvas-drawn curtains of light undulating over a starfield and ridge line, under stepped film grain, with a shimmer through the headline and a CTA ringed by a rotating conic gradient. |
| 09 | [Shared-Element Morph](concepts/09-shared-element.md) | Click any specimen: its artwork physically travels from the grid into the detail sheet and settles there — one object moving between two layouts, rather than a cut between two screens. |
| 10 | [Attention Cues](concepts/10-attention-cues.md) | A mock team-inbox where you fire the events yourself and watch the interface choose its attention weapon: toast springs, badge pops, bell wiggle, unread counts, a one-shot confetti celebration — and a feed where every event leaves a persistent record. |
| 11 | [Horizontal Gallery](concepts/11-horizontal-gallery.md) | A section pins itself to the viewport and converts your vertical scroll into horizontal travel through four exhibition rooms, each with its own internal parallax and a segmented progress indicator. |
| 12 | [Text Decode](concepts/12-text-decode.md) | A declassified file that un-redacts itself: the headline resolves out of solid redaction bars character by character, a DECLASSIFIED stamp thunks in when it finishes, dossier rows re-redact and clear on hover, and status cells resolve when they enter view. |
| 13 | [Change Blindness Lab](concepts/13-change-blindness.md) | A self-experiment. |
| 14 | [Spring & Easing Lab](concepts/14-easing-lab.md) | Five identical moves, five different easing curves, raced side by side — then a designer that samples a real damped-spring simulation into CSS's native linear() function, with copyable output. |
| 15 | [Intro Sequence](concepts/15-intro-sequence.md) | The signature opening of award-site portfolios: a percentage counter builds anticipation, curtain panels wipe upward in a stagger, and the hero text rises through masks — three acts, one continuous gesture. |
| 16 | [Applied: Drift Landing](concepts/16-drift-landing.md) | A complete landing page assembled from the Attention Lab design system: aurora canvas hero, spotlight-border bento, roll-up counters, hover rows, and a magnetic CTA — all on the shared tokens. |
| 17 | [Particle Field](concepts/17-particle-field.md) | 262,144 particles simulated entirely on the GPU: positions and velocities live in floating-point textures, a fragment shader integrates curl-noise flow, spring-to-shape forces and your pointer every frame, and the points render with additive blending and depth-attenuated sprites. |
| 18 | [Liquid Raymarch](concepts/18-liquid-hero.md) | A signed-distance scene sphere-traced per pixel in one fragment shader: five metaballs on incommensurate orbits blend through a cubic smooth-minimum, one chases your cursor, and a click sends a merge-pulse through the blend radius. |
| 19 | [Flowmap Type](concepts/19-flowmap-type.md) | A live fluid velocity field follows your cursor: each move splats momentum into a half-float simulation texture that advects itself forward. |
| 20 | [Applied: Signal SaaS](concepts/20-signal-landing.md) | A complete SaaS landing page at template-market polish density — glass nav, glowing hero with a live product mockup, logo marquee, glass bento, counters, testimonial wall, pricing with billing toggle, FAQ — every section from Attention Lab components. |
| 21 | [Meridian Studio](concepts/21-molten-studio.md) | What this is: an editorial studio landing whose centrepiece is a real-time raymarched liquid-metal object — three metaballs sphere-traced per pixel, chrome-shaded by reflecting a procedural warm/cool studio environment, morphing on its own and bending toward your cursor. |
| 22 | [Lumen Nebula](concepts/22-lumen-nebula.md) | What this is: an editorial landing whose full-viewport hero is a real-time generative nebula — layered value-noise domain-warped three times in a fragment shader, drifting on its own and swirling around your cursor. |
| 23 | [Strata Lattice](concepts/23-strata-lattice.md) | What this is: an architecture-studio landing whose fixed hero is a real-time raymarched gyroid — a minimal surface used in real structural engineering — carved into a slowly rotating specimen. |
| 24 | [Atelier Noir](concepts/24-atelier-noir.md) | What this is: a fragrance-house landing whose hero wordmark is drawn into a live fluid field — a GPU flow simulation you stir with the cursor, smearing and chromatically splitting the letters like ink in water, then letting them heal. |

## Provenance

Every prompt is backed by a working reference implementation in this repo
([`concepts/`](../concepts/), [`design-system/`](../design-system/)), each verified
headlessly (console-clean, no overflow, reduced-motion pass) and visually reviewed.
The motion values in the prompts are the values in the code — see each concept's About
panel and [`research/`](../research/) for why those values were chosen.
