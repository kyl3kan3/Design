# Attention by Design: Motion & Animation Patterns That Earn a Second Look

*Research synthesis — July 2026. Compiled from six parallel research passes (attention
psychology, motion craft, 2025–26 trends, award-site patterns, the modern animation stack,
and accessibility/performance/ethics). The working demos built from this research live in
[`../concepts/`](../concepts/index.html).*

---

## Executive summary

Motion catches attention because it exploits a pre-conscious pathway: peripheral vision is evolutionarily tuned to detect movement, and motion onset yanks foveal gaze involuntarily within the ~200-250ms pre-attentive sweep — before a single word is read. This makes motion the strongest salience tool available, stronger than any static contrast in color, size, or position. But salience is relative, not absolute: one moving element in a calm field pops (the Von Restorff isolation effect yields 30-50% better recall for the distinctive item); five moving elements cancel each other and read as chaos. The winning formula across attention psychology, design-system specs, and award-winning sites is the same: a calm, well-structured base layer, plus ONE choreographed moment of motion at the point of relevance, executed with physical plausibility (ease-out entrances, spring settle, origin-aware scaling) and then allowed to rest.

What separates delightful from annoying is discipline on four axes. Duration: the usable band is 100-500ms (100ms for feedback, 200-300ms for modals, exits 15-30% faster than entrances); at 500ms motion crosses from informative to obstructive. Frequency: an animation seen 50 times a day must shrink toward zero (IBM Carbon's "productive" motion, 70-240ms), while expressive brand moments (springs, overshoot, 400-700ms) are rationed to first-runs, heroes, and success states. Physics: motion that obeys intuitive physics — deceleration into place, slight overshoot ≤15%, scaling from the trigger point rather than from center or zero — registers pre-cognitively as "quality," while symmetric, linear, or center-origin motion reads as template-grade. Purpose: legitimate motion serves the user's current goal, fires once, and stops; perpetual unprompted motion (infinite pulsing badges, autoplay-next) is formally classified in CHI research as an attention-capture dark pattern, and WCAG 2.2.2 makes uncontrollable motion >5s a Level-A compliance failure, not merely a taste error.

The 2025-26 landscape makes all of this cheaply buildable in vanilla web tech: scroll-driven CSS animations (animation-timeline: scroll()/view()), the View Transitions API (Baseline Oct 2025), @starting-style + allow-discrete for pure-CSS entry/exit, @property for animatable gradients and counters, and linear() easing for real spring curves — all with no libraries. The backfire modes are equally well documented: janky main-thread animation (a single animated box-shadow can inflate INP past the 200ms "good" threshold), vestibular triggers (parallax and large zooms cause hours-long nausea for the ~35% of adults 40+ with vestibular dysfunction — always substitute crossfades under prefers-reduced-motion), and everything-animated maximalism that dilutes the very contrast attention depends on. One hero moment done exceptionally well beats ten competing animations.

---

## Core principles

1. Motion is pre-attentive and involuntary — ration it to one dominant moving element per view. Peripheral vision detects motion in the ~200-250ms parallel sweep before conscious attention; every additional animated element dilutes every other (Von Restorff: the isolated distinctive item gets 30-50% better recall). Design the page calm, then spend the motion budget on the single thing that matters.

2. Respect the duration band: 100-500ms, with 500ms as the hard annoyance ceiling. Micro-feedback (toggles, button states) 70-150ms; dropdowns 150-250ms; modals/sheets 200-300ms; full-page transitions 400-700ms only. Exits run 15-30% faster than entrances (Material: 225ms in / 195ms out; NN/g: 300ms in / 200-250ms out) because departing content carries no information.

3. Ease-out in, ease-in out, never linear for spatial motion. Entrances decelerate: cubic-bezier(0, 0, 0.38, 0.9) (Carbon entrance) or cubic-bezier(0.05, 0.7, 0.1, 1) (M3 emphasized-decelerate). Exits accelerate: cubic-bezier(0.3, 0, 0.8, 0.15). Reserve linear for opacity/color fades only. The fast start reads as responsiveness; the braking end lets the eye lock onto the final position.

4. Duration must scale with size and distance — ship a token ramp, not one constant. Carbon's verified scale: 70/110/150/240/400/700ms mapped to micro-feedback → toggles → dropdowns → expansion → large panels → full-page. Rule of thumb: ~100ms per 10% of viewport traveled; 2-5 moving objects 300-400ms, 6-10 objects 500-700ms.

5. Stagger group entrances 30-80ms apart (50ms default, or 30-70% of per-item duration), cap the whole sequence under 1 second, and never stagger exits — exit everything together and faster. Sequential onset creates a guided reading path; simultaneous onset of 12 items reads as a blob.

6. Scale from the origin of causality, never from zero, never from center-by-default. Entrances start at scale 0.9-0.95 (0.93 typical); popovers/menus set transform-origin to the trigger edge; buttons compress to scale(0.97) on :active over 100-150ms. Center-origin and scale(0) starts are the single most common tell of un-crafted motion.

7. Use spring curves for expressive moments — now native via CSS linear() easing (all engines since Dec 2023). Generate 25-75 points from a damped spring sim (damping ratio 0.7-1.0 for premium, not cartoonish, feel); keep overshoot ≤10-15% of travel. Example snappy transition: transform 0.6s linear(0, 0.36, 0.66, 1.1, 1.03, 0.98, 1). Playful bezier shortcut: cubic-bezier(0.34, 1.56, 0.64, 1).

8. Split motion into two budgets — productive vs expressive. Productive (task-flow feedback): 70-240ms, gentle curves, near-invisible, and shrinking toward zero as frequency rises. Expressive (brand moments): 150-700ms with springs and overshoot, reserved for heroes, first-runs, success states, and primary CTAs. If every hover bounces, nothing stands out and the product feels slow.

9. Animate only compositor properties: transform, opacity, filter, clip-path. Never animate width/height/top/left/box-shadow (crossfade a pseudo-element's opacity for shadows). Frame budget is 16.7ms at 60fps (~10ms usable); main-thread animation inflates INP past the 200ms 'good' threshold. Apply will-change just before animating and remove it after; pause loops offscreen via IntersectionObserver.

10. Animate the change to defeat change blindness — one change at a time. Any instant swap that coincides with a saccade (20-40ms of functional blindness), blink, or reload goes unseen; lab subjects needed 20-40 flicker cycles to spot an unanimated change. A 200-300ms transition on only the changed region, with static context dimmed, lets peripheral vision catch it automatically.

11. Reduce, don't remove, under prefers-reduced-motion — and build motion as progressive enhancement. Base styles motion-free; add animation inside @media (prefers-reduced-motion: no-preference); substitute slides/zooms/parallax with opacity crossfades (always vestibular-safe). Use animation-duration: 0.01ms rather than none so animationend still fires. Risky triggers: parallax, viewport-scale translation, zoom toward viewer, scroll-direction conflicts.

12. Animate once, then rest — the ethical and legal line. Attention cues fire a finite count (animation-iteration-count: 3, not infinite) and cancel on hover/focus/acknowledgment. WCAG 2.2.2 (Level A) requires a pause/stop/hide control for any auto-motion >5s beside other content; 2.3.1 bans >3 flashes/second. The more frequent an animation, the shorter and subtler it must be; perpetual unprompted motion serving engagement metrics is a documented dark pattern.

---

## Concept candidates (scored)

The synthesis stage proposed 19 buildable demo concepts, scored for
attention impact and feasibility as a dependency-free single HTML file (1–10):

| Concept | Family | Impact | Feasibility |
|---------|--------|:------:|:-----------:|
| Masked-Line Hero Reveal | Typography / entrance choreography | 9 | 10 |
| Text Decode / Scramble Terminal | Typography / novelty motion | 8 | 10 |
| Scrollytelling Pinned Product Story | Scroll-driven narrative | 9 | 8 |
| Animated Bento Grid with Spotlight Hover | Cards & grids | 9 | 10 |
| Magnetic Buttons + Lerped Custom Cursor | Cursor-led interaction | 9 | 9 |
| Holographic 3D Tilt Cards | Hover physics / cards | 8 | 10 |
| Aurora Mesh Gradient + Film Grain Backdrop | Ambient backgrounds | 7 | 10 |
| Velocity-Reactive Marquee with Skew | Scroll-coupled ambient motion | 8 | 9 |
| Spring Physics Playground (linear() easing lab) | Micro-interaction feedback / education | 7 | 10 |
| Gallery Morph: View Transitions + FLIP Fallback | Shared-element transitions | 9 | 8 |
| Preloader Counter → Curtain → Hero Chain | First-impression sequence | 8 | 10 |
| Conic Glow Borders + Pure-CSS Count-Ups (@property) | Cards & grids / ambient emphasis | 7 | 9 |
| Zero-JS Dialog & Popover Choreography (@starting-style) | Transitions / overlays | 7 | 9 |
| Mid-Scroll Chapter Theme Swap | Scroll-driven / macro contrast | 7 | 10 |
| Flashlight Cursor Reveal | Cursor-led / curiosity | 8 | 10 |
| Neo-Brutalist Snap Interactions | Anti-design / pattern violation | 7 | 10 |
| Attention Etiquette: Once vs. Forever | Attention cues / feedback ethics | 7 | 10 |
| Change Blindness: Flicker vs. Motion | Attention psychology / interactive proof | 8 | 10 |
| Scroll-Scrubbed SVG Line Drawing | Scroll-driven / anticipation | 7 | 9 |

<details>
<summary><strong>Full concept briefs</strong> (what each demo should show, the mechanism it exploits, and the techniques to use)</summary>

### Masked-Line Hero Reveal

*Family: Typography / entrance choreography · Impact 9/10 · Feasibility 10/10*

A full-viewport hero where an oversized headline (clamp(3rem, 11vw, 10rem), system-ui, weight 800) reveals line by line: each line sits inside an overflow:hidden wrapper and slides up from translateY(110%) to 0 with opacity 0 to 1, staggered 70ms per line, followed 200ms later by a subhead fade and a CTA that scales in from 0.93. A replay button re-runs the sequence. Vanilla JS wraps each line in mask spans at load (split on manual <span> line markers to avoid measuring). Include a reduced-motion variant that crossfades only.

- **Attention mechanism:** Sequential motion onset is the strongest low-level attention trigger; the stagger creates a guided reading path across the headline, and the mask boundary makes text appear printed into the page — signaling craft within the first 500ms.
- **Key techniques:** Per-line overflow:hidden wrappers; transform: translateY(110%) → 0 over 900ms with cubic-bezier(0.16, 1, 0.3, 1) (expo.out); stagger via transition-delay: calc(var(--i) * 70ms); CTA enters at scale 0.93 → 1 over 250ms cubic-bezier(0, 0, 0.38, 0.9); optional 8px → 0 blur variant; @media (prefers-reduced-motion: reduce) swaps transforms for 300ms opacity fades.

### Text Decode / Scramble Terminal

*Family: Typography / novelty motion · Impact 8/10 · Feasibility 10/10*

A dark terminal-styled page (monospace system stack, near-black bg, green/white text, blinking block cursor) where headlines resolve out of randomized glyphs: each character cycles through a charset (A-Z, 0-9, !@#$%) at ~30ms per swap and locks into place left to right over 1.5-2s. Nav links re-scramble for 400ms on hover. A stat line types itself with a typewriter effect. Buttons trigger re-decode of the hero phrase from a rotating list.

- **Attention mechanism:** High-frequency character churn is motion the eye cannot ignore, and the progressive lock-in creates a micro-mystery ('what will it say?') with a resolution payoff — a small closable curiosity gap, the exact structure Loewenstein's information-gap theory says maximizes attention.
- **Key techniques:** Single rAF loop (never setInterval) assigning each char a random resolve-frame; per-char span rendering with fixed-width font to avoid reflow; charset swap every ~2 frames; total tween 1.5-2.5s with earlier chars locking first; hover scramble 300-500ms; aria-label carries real text; blinking cursor via steps(2) 1s keyframe; skip scramble under prefers-reduced-motion (instant text).

### Scrollytelling Pinned Product Story

*Family: Scroll-driven narrative · Impact 9/10 · Feasibility 8/10*

A 400vh page where a sticky full-viewport scene stays pinned while scroll progress drives a three-act story: an abstract 'product' (layered CSS gradients + inline SVG shapes) rotates, explodes into labeled parts, and reassembles, while three copy blocks fade in/out at keyed progress points (10-30%, 40-60%, 70-90%). A right-edge progress rail fills with scroll. Built with position:sticky inside the tall parent; progress computed from getBoundingClientRect in a rAF scroll handler, with CSS animation-timeline: view() used where supported via @supports.

- **Attention mechanism:** Direct manipulation illusion — the user's own gesture drives the spectacle, creating agency and a game-like loop; sequential reveal builds serial curiosity gaps ('what happens if I keep scrolling'), the mechanic behind +317% scroll-depth findings for interactive scroll stories.
- **Key techniques:** Sticky container in a 400vh parent; scroll progress 0-1 mapped to transform: rotate()/translate()/scale() on 4-6 layers with different multipliers (parallax depth: background 0.2x, midground 0.5x, foreground 1x); copy blocks opacity/translateY keyed to progress windows with 10% fade ramps; transform/opacity only; @supports (animation-timeline: view()) pure-CSS path with animation-range: entry 0% cover 40%; reduced-motion collapses to plain stacked sections.

### Animated Bento Grid with Spotlight Hover

*Family: Cards & grids · Impact 9/10 · Feasibility 10/10*

A dark SaaS-style bento: CSS Grid (repeat(4, 1fr)) with one 2x2 hero tile and six smaller tiles, each containing a live CSS-animated micro-demo (a mini chart drawing itself, a toggle flipping, a counter ticking). Tiles reveal on scroll with translateY(20px) + fade, staggered 70ms. On hover: a radial 'flashlight' gradient follows the cursor across each tile's 1px border ring (Stripe-style), the tile scales to 1.02, and sibling tiles dim to 0.55 opacity with 2px blur.

- **Attention mechanism:** Matches natural scan mechanics — the eye goes to the largest tile first, then adjacent blocks (bento layouts report +47% dwell); the cursor-tracked spotlight makes the whole grid feel like one continuous reactive material, and sibling dimming spotlights the user's own focus.
- **Key techniques:** IntersectionObserver (threshold 0.15) toggling .in-view, transition-delay: calc(var(--i) * 70ms), 500ms cubic-bezier(0, 0, 0.38, 0.9); spotlight via per-tile --mouse-x/--mouse-y custom props updated on pointermove feeding a ::before radial-gradient(350px at var(--mouse-x) var(--mouse-y), rgba(120,140,255,0.5), transparent 70%) masked to a 1px border ring with padding-box/border-box mask-composite; hover scale 1.02 over 250ms; .grid:hover .tile:not(:hover) { opacity: 0.55; filter: blur(2px) }; unobserve after first fire.

### Magnetic Buttons + Lerped Custom Cursor

*Family: Cursor-led interaction · Impact 9/10 · Feasibility 9/10*

A minimal dark page with a nav and three large pill buttons demonstrating cursor physics: a small dot rides the pointer exactly while a larger ring lags behind via lerp (factor 0.15/frame), inverting content beneath it with mix-blend-mode: difference. Buttons within a ~100px proximity radius pull toward the cursor at 0.4x offset (inner label at 0.2x for parallax) and spring back with elastic overshoot on leave. Cursor ring grows and shows a 'View' label over a demo card. Touch devices get the native cursor untouched.

- **Attention mechanism:** Violates the learned expectation that UI is static — buttons appear to want to be clicked (implied agency), and the elastic snap-back delivers a micro-reward that makes users hover repeatedly; the lagging ring gives the pointer perceived mass, reframing the page as an 'experience'.
- **Key techniques:** Fixed divs positioned via translate3d in one rAF loop; ring lerp factor 0.15 (dot 1.0); mix-blend-mode: difference on the ring; magnetic pull = pointer offset from button center x 0.4 (label x 0.2), applied while within expanded bounding box; release with 900ms elastic curve approximated by linear(0, 1.24 32%, 0.92 54%, 1.03 74%, 0.99 88%, 1); keep real hit target stationary (move only visual layer); pointer:coarse media query disables everything; cursor:none only on desktop.

### Holographic 3D Tilt Cards

*Family: Hover physics / cards · Impact 8/10 · Feasibility 10/10*

Three trading-card-style cards (pure CSS gradients + inline SVG art) that rotate in 3D toward the cursor: pointer position relative to card center maps to rotateY/rotateX clamped at ±12deg with scale 1.03, inside a perspective: 900px parent. A radial-gradient glare layer tracks the same offset at max-opacity 0.4, and one 'holo' card adds a shifting rainbow linear-gradient sheen driven by the tilt angle. Cards reset with a soft spring on pointerleave. Children use translateZ for internal depth parallax.

- **Attention mechanism:** Coherent tilt plus a moving specular highlight triggers the perception of a physical lit object — the brain flags 'this is a thing, not a picture' — and the toy-like responsiveness invites playful exploration of every card.
- **Key techniques:** pointermove → normalized offset (-0.5..0.5) → rotateY = x * 24deg, rotateX = -y * 24deg, clamped ±12deg; transition: transform 150ms ease-out while moving, 800ms linear(0, 1.12 40%, 0.97 65%, 1) spring on leave; glare = absolute radial-gradient(circle at (50 + x*60)% (50 + y*60)%, rgba(255,255,255,0.45), transparent 55%); holo sheen = linear-gradient rotated by tilt with mix-blend-mode: color-dodge; transform-style: preserve-3d + translateZ(30px) on badge elements; static under reduced-motion.

### Aurora Mesh Gradient + Film Grain Backdrop

*Family: Ambient backgrounds · Impact 7/10 · Feasibility 10/10*

A full-viewport 'premium SaaS' backdrop: 4 large blurred color blobs (radial-gradients on absolutely-positioned divs, filter: blur(100px)) drifting on slow 25-40s alternating keyframe paths over a near-black base, beneath a dark-glass content card (backdrop-filter: blur(18px) saturate(170%), rgba(18,18,26,0.6), 1px rgba(255,255,255,0.12) inset border with a top-edge highlight gradient). A fixed SVG feTurbulence grain layer at 6% opacity with mix-blend-mode: overlay flickers via steps() background-position jitter at 10fps. Controls let the viewer toggle grain and adjust drift speed to feel the difference.

- **Attention mechanism:** Slow ambient color drift registers in peripheral vision and makes the page feel alive without demanding focus; grain adds material quality ('printed, not rendered'), kills gradient banding, and — in the anti-AI-slop era — organic imperfection reads as human and trustworthy.
- **Key techniques:** Blob keyframes animate transform: translate/scale only (20-40s, ease-in-out, alternate), never background-position; blur(80-120px) on the blobs; grain = inline <svg><filter><feTurbulence type='fractalNoise' baseFrequency='0.72' numOctaves='2'/></filter></svg> tiled as a 300%-sized fixed layer, opacity 0.05-0.08, pointer-events:none, jittered with steps(1) keyframes at 100ms intervals; glass card per Liquid-Glass web approximation: blur(12-24px) saturate(160-180%) + inset top highlight; drift pauses under reduced-motion (static gradient stays).

### Velocity-Reactive Marquee with Skew

*Family: Scroll-coupled ambient motion · Impact 8/10 · Feasibility 9/10*

A tall editorial page with three infinite marquee bands (oversized outlined text, duplicated 2x, looping via translateX 0 to -50%). Scroll velocity modulates each band: scrolling fast multiplies playback speed up to 4x, scrolling upward reverses direction, and the text skews proportional to velocity (clamped ±8deg), all easing back to base state ~0.8s after scrolling stops. Bands pause entirely when offscreen.

- **Attention mechanism:** Constant peripheral motion keeps the page alive at idle, while the velocity coupling creates the uncanny 'the page feels my hand' sensation — responding to input INTENSITY, not just position, is the current differentiator between good and award-level, and users scroll just to play with it.
- **Key techniques:** Track scrollY delta per rAF frame as velocity; timeScale = 1 + clamp(velocity/300, -3, 3) applied by advancing a manual progress accumulator (progress += baseSpeed * timeScale * dt) driving transform: translateX(calc(progress * -1%)); skewX = clamp(velocity/40, -8, 8)deg lerped back to 0 at factor 0.08/frame; base loop 20s per 50%; content duplicated to 200% width; IntersectionObserver pauses offscreen bands; WCAG 2.2.2 pause button top-right persisting via localStorage; static under reduced-motion.

### Spring Physics Playground (linear() easing lab)

*Family: Micro-interaction feedback / education · Impact 7/10 · Feasibility 10/10*

An interactive comparison lab: a row of identical panels each animating the same element (a card sliding in, a toggle flipping, a button pressing) with different easings — linear, ease-in-out, Material emphasized-decelerate, and three pre-baked linear() springs (damping 0.6 bouncy / 0.8 snappy / 1.0 critical). Sliders for duration and a 'slow motion 0.25x' toggle let viewers feel why springs read as alive. Each panel shows its CSS one-liner in a copyable code chip.

- **Attention mechanism:** Overshoot-and-settle mimics real mass and damping — human perception reads spring curves as 'alive' versus the sterile feel of ease-in-out, which can never pass 100%; the side-by-side replay makes an invisible craft difference suddenly visible, which is itself a curiosity hook.
- **Key techniques:** Pre-baked spring strings, e.g. snappy: linear(0, 0.0087, 0.0345, 0.0764, 0.1327, 0.2016, 0.2807, 0.3678, 0.4605, 0.5566, 0.654, 0.836 44%, 0.9853, 1.0895, 1.1518, 1.1787, 1.1772 66%, 1.1128, 1.0577, 1.0181, 0.9944, 0.9857, 1) (≥25 points for convincing motion); durations 300-700ms; replay via class re-toggle with void el.offsetWidth reflow; toggle press at scale 0.97; @supports (transition-timing-function: linear(0,1)) with cubic-bezier(0.34,1.56,0.64,1) fallback; all transform/opacity.

### Gallery Morph: View Transitions + FLIP Fallback

*Family: Shared-element transitions · Impact 9/10 · Feasibility 8/10*

A photo-grid (CSS gradient 'artwork' tiles) where clicking a thumbnail morphs it into a full-screen detail view — the tile grows continuously into the hero position while a caption panel slides up — and back on close. Uses document.startViewTransition with view-transition-name on the active tile where supported; otherwise a hand-rolled FLIP: measure first/last rects, apply inverted translate/scale, release over 320ms. Background content dims and blurs behind the detail view.

- **Attention mechanism:** Object permanence — the brain tracks one persisting element through the change, so navigation feels like moving within a single physical space instead of teleporting between screens; shared-element continuity is the strongest single signal of native-app quality on the web.
- **Key techniques:** if (document.startViewTransition) path: assign view-transition-name: active-card to clicked tile + detail hero, customize ::view-transition-group(active-card) { animation-duration: 320ms; animation-timing-function: cubic-bezier(0.2, 0, 0, 1) }; FLIP path: getBoundingClientRect before/after, el.animate([{transform: translate(dx,dy) scale(sw,sh)}, {transform:'none'}], {duration: 320, easing: 'cubic-bezier(0.2, 0, 0, 1)'}); exit 240ms (faster); backdrop fade 200ms + blur(6px); Escape/click-outside close; reduced-motion = instant crossfade.

### Preloader Counter → Curtain → Hero Chain

*Family: First-impression sequence · Impact 8/10 · Feasibility 10/10*

The full agency-site opening ritual as one file: a black overlay with a huge 0→100 counter (weighted increments easing with power2.inOut over ~1.8s) and a thin progress line; at 100 the overlay wipes upward as a curtain (clip-path inset animating over 700ms), handing off directly into a staggered masked hero text reveal and a nav fade-in. sessionStorage skips the preloader on revisit; a 'replay intro' link restores it.

- **Attention mechanism:** Establishes theater and brand voice before any content and converts unavoidable wait into anticipation — visible progress toward 100 exploits the goal-gradient effect, and the seamless counter-to-hero handoff brands the site as 'crafted' in the first two seconds.
- **Key techniques:** Counter tweens a JS number 0→100 over 1.6-2s with easeInOutQuad, rendered via textContent with font-variant-numeric: tabular-nums; curtain exit: clip-path: inset(0) → inset(0 0 100% 0) over 700ms cubic-bezier(0.87, 0, 0.13, 1) (expo.inOut); hero timeline chained 100ms before curtain completes (overlap reads as confidence); total budget ≤3s; sessionStorage.setItem('seen'); reduced-motion = simple 300ms fade-out.

### Conic Glow Borders + Pure-CSS Count-Ups (@property)

*Family: Cards & grids / ambient emphasis · Impact 7/10 · Feasibility 9/10*

A stats section where four dark cards carry slowly rotating conic-gradient borders (a registered <angle> custom property animating 0→360deg over 6s) with a soft outer glow, and each stat number counts up from 0 using the pure-CSS @property <integer> + counter() trick when scrolled into view, decelerating into its final value with easeOutExpo feel. One 'featured' card gets a brighter, faster ring to demonstrate rationed emphasis.

- **Attention mechanism:** A number in motion is read twice — once as movement, once as value — doubling dwell on the claim, and the deceleration gives the final figure landing weight; the rotating ring is constant sub-threshold peripheral motion that reads as premium without demanding focus.
- **Key techniques:** @property --angle { syntax: '<angle>'; inherits: false; initial-value: 0deg } animated in keyframes, used in background: conic-gradient(from var(--angle), transparent 70%, #7c8cff, transparent) masked to a 1.5px ring; count-up: @property --n { syntax: '<integer>' } + counter-reset: n var(--n) + ::after { content: counter(n) }, transition: --n 2s cubic-bezier(0.16, 1, 0.3, 1), triggered by IntersectionObserver at 50% adding a class; tabular-nums reserves width; JS rAF fallback when @property unsupported; rings pause under reduced-motion, numbers render final value instantly.

### Zero-JS Dialog & Popover Choreography (@starting-style)

*Family: Transitions / overlays · Impact 7/10 · Feasibility 9/10*

A demo page of native <dialog> and popover-attribute elements with fully CSS-driven entry/exit: the dialog enters at opacity 0 / scale 0.93 / translateY(8px) and settles over 280ms while its ::backdrop fades; menus scale from their trigger edge with correct transform-origin per placement (bottom-center for a dropdown opening down); everything animates OUT (220ms, accelerate) before display:none lands, via transition-behavior: allow-discrete. A toolbar shows that a second tooltip appears with 0ms transition once one is already open.

- **Attention mechanism:** Elements that fade/scale into place read as physical objects arriving rather than content popping into existence; origin-aware scaling preserves causality — motion emanating from the point of interaction confirms 'your tap did this' pre-cognitively.
- **Key techniques:** dialog { opacity: 0; transform: scale(0.93) translateY(8px); transition: opacity 280ms, transform 280ms cubic-bezier(0.05, 0.7, 0.1, 1), display 280ms allow-discrete, overlay 280ms allow-discrete } dialog[open] { opacity: 1; transform: none; @starting-style { opacity: 0; transform: scale(0.93) translateY(8px) } }; exit 220ms cubic-bezier(0.3, 0, 0.8, 0.15); ::backdrop fade with same allow-discrete pattern; popover + popovertarget for zero-JS wiring; transform-origin set per placement; instant-subsequent-tooltip via a body class toggled on first open.

### Mid-Scroll Chapter Theme Swap

*Family: Scroll-driven / macro contrast · Impact 7/10 · Feasibility 10/10*

A long-form portfolio page in four sections whose entire palette lives in CSS custom properties on body (--bg, --fg, --accent). Crossing each section boundary tweens the whole page between themes — cream hero → near-black work showcase → deep-blue lab → warm contact — over 700ms, retinting nav, text, borders, and a fixed progress dot simultaneously. Section content fades up with 60ms-staggered reveals as each chapter opens.

- **Attention mechanism:** A full-viewport color change is the largest visual delta a scroll can produce; it functions as a chapter break that resets attention and reframes the following content's mood, while everything swapping in lockstep makes the page feel like one continuous material.
- **Key techniques:** Palettes as data-theme attribute values; IntersectionObserver (rootMargin -40% top/bottom) sets body dataset on enter; body { transition: background-color 700ms cubic-bezier(0.4, 0, 0.2, 1), color 700ms }; all components inherit via var(); check WCAG contrast in every theme pair; per-section reveals: opacity/translateY(16px) 500ms with transition-delay: calc(var(--i) * 60ms); fixed nav and scroll-dot read the same variables; reduced-motion keeps the color crossfade (opacity/color changes are the WCAG-blessed safe harbor) but drops the translate.

### Flashlight Cursor Reveal

*Family: Cursor-led / curiosity · Impact 8/10 · Feasibility 10/10*

A near-black page hiding a richly detailed scene (inline SVG constellation map with labeled points, gradient nebulae, fine grid lines). A ~260px soft-edged circle of light follows the cursor via a radial-gradient mask, revealing whatever it passes over; dwelling near a hidden hotspot for 600ms blooms its label with a scale-settle spring. The light radius eases larger while the pointer moves fast and tightens when still. A 'reveal all' toggle (and touch/keyboard default) lifts the mask entirely.

- **Attention mechanism:** Spotlight mechanics weaponize the information gap — the user KNOWS content is hidden just beyond the light's edge, and closing small, closable curiosity gaps is precisely the condition under which attention peaks; the pointer becomes the narrative instrument.
- **Key techniques:** Content layer with mask-image: radial-gradient(circle 260px at var(--mx) var(--my), #000 55%, transparent 100%) (plus -webkit-mask-image), custom props updated on pointermove through one rAF with lerp 0.18 for weighted light; radius modulated by pointer speed (lerped 220-320px); hotspot dwell timer 600ms → label scale 0.9→1 over 500ms linear(0, 1.08 60%, 0.99 80%, 1); dim base layer at 8% opacity so the page never looks empty; pointer:coarse and reduced-motion default to fully revealed.

### Neo-Brutalist Snap Interactions

*Family: Anti-design / pattern violation · Impact 7/10 · Feasibility 10/10*

A loud one-pager in the Gumroad dialect: white background, thick 3px black borders, clashing yellow/pink/cyan blocks, monospace headings. Interactions are deliberately snappy: cards carry box-shadow: 8px 8px 0 #000 that collapses to 0 with a 2px translate on :active (physical press); hovers swap colors instantly (transition: none) or with steps(3) jank; a marquee ticker runs at hard constant speed; stickers are randomly rotated (-3deg to 3deg) and 'peel' up on hover; a glitch headline uses clip-path slice keyframes with RGB-split text-shadow, capped at 2 bursts per hover.

- **Attention mechanism:** Pattern violation — against a feed of polished template sites, 'wrong-looking' design triggers the orienting response; abrupt no-easing state flips feel rebellious and confident, and the irony signals in-group taste to design-literate audiences.
- **Key techniques:** Hard shadows: box-shadow: 8px 8px 0 #000; :active { box-shadow: 0 0 0 #000; transform: translate(6px, 6px) } with transition: 90ms steps(2); instant hover color swaps via transition: none; marquee: translateX keyframes, linear, 18s, content duplicated 2x, with a visible pause button (WCAG 2.2.2); glitch: 3 stacked copies with clip-path: inset() keyframes at ≤3 bursts/sec staying under the flash threshold, text-shadow: 2px 0 #f0f, -2px 0 #0ff; sticker peel: rotate + translateY(-4px) 120ms ease-out.

### Attention Etiquette: Once vs. Forever

*Family: Attention cues / feedback ethics · Impact 7/10 · Feasibility 10/10*

A split-screen teaching demo. Left ('Respectful'): a notification bell that bounces exactly 3 times when a message arrives then rests, a CTA with a pulse ring at animation-iteration-count: 3 that cancels on hover/focus, a toast that enters, waits 4s, exits. Right ('Hostile'): the same components looping infinitely — shaking bell, perpetual pulse, self-replacing toasts. A reading-comprehension paragraph sits between them so viewers FEEL the peripheral-motion tax; buttons trigger fresh events on either side, and a meter counts how many times each side has demanded attention.

- **Attention mechanism:** Motion onset triggers involuntary pre-attentive orienting — an evolutionary reflex users cannot opt out of. Making the visitor experience the difference between rationed one-shot cues and perpetual capture is itself a memorable, shareable hook, and it demonstrates the exact line WCAG 2.2.2 and CHI dark-pattern research draw.
- **Key techniques:** Bell bounce: keyframe rotate ±12deg, 600ms, animation-iteration-count: 3, cubic-bezier(0.34, 1.56, 0.64, 1); pulse ring: box-shadow-free ::after scale 1→1.6 + opacity fade, 1.2s x3, cancelled via :hover/:focus-visible { animation: none }; toast: enter 300ms decelerate, exit 220ms accelerate after 4s; hostile side uses infinite iteration + a persistent WCAG-mandated pause control to stay legal while making the point; attention counter increments on each animationiteration event.

### Change Blindness: Flicker vs. Motion

*Family: Attention psychology / interactive proof · Impact 8/10 · Feasibility 10/10*

An interactive experiment in one file: a dashboard mock (cards, numbers, a status chip) is shown twice side by side. Pressing 'Flicker swap' changes one detail (a price, a badge color) behind an 80ms blank flash — the classic change-blindness paradigm — and the viewer tries to spot it, with attempts counted. Pressing 'Animated swap' changes an equivalent detail with a 250ms highlighted transition (the changed region scales 1→1.06→1 with a brief accent glow) that peripheral vision catches instantly. A results panel tallies detection speed for each mode and explains the saccadic-blindness mechanism.

- **Attention mechanism:** Turns the research's most counterintuitive finding into a felt experience: instant swaps hidden by a flicker took lab subjects 20-40 cycles to find, while a smooth transition preserves motion continuity so peripheral vision flags the change automatically — proving why UIs must animate the change, one change at a time.
- **Key techniques:** Flicker: overlay div flashes background for 80ms via setTimeout while a random property mutates underneath; animated mode: changed element runs a 250ms transform: scale(1.06) + outline glow keyframe with cubic-bezier(0, 0, 0.38, 0.9) then settles; changeable slots defined as data-slot elements with value pools; click-to-guess handlers score detection; timer via performance.now(); tally rendered with tabular-nums count-up; reduced-motion mode replaces scale pulse with a 300ms background-color highlight fade (safe-harbor color change).

### Scroll-Scrubbed SVG Line Drawing

*Family: Scroll-driven / anticipation · Impact 7/10 · Feasibility 9/10*

A vertical narrative where a single continuous inline-SVG line (a winding path connecting four milestone nodes, ~2000px of path length) draws itself in lockstep with scroll via stroke-dashoffset mapped to scroll progress. As the line reaches each node, the node's circle pops in with a spring scale and its caption fades up. Scrolling backward un-draws everything symmetrically. A subtle dot rides the line's current endpoint.

- **Attention mechanism:** Watching a line being drawn triggers perceptual closure — the viewer's brain participates in finishing the shape — and scrubbing it with scroll literalizes progress through the story, rewarding continued scrolling with visible completion.
- **Key techniques:** path.getTotalLength() → stroke-dasharray: L; stroke-dashoffset: L → 0 mapped from section scroll progress in a rAF scroll handler (or @supports pure-CSS animation-timeline: scroll() variant); node pop at progress thresholds: scale 0 → 1 (from 0.5, not 0, visually) over 450ms linear(0, 1.15 55%, 0.97 78%, 1); endpoint dot positioned via path.getPointAtLength(progress * L); vector-effect: non-scaling-stroke for responsive scaling; captions opacity/translateY(12px) 400ms decelerate; reduced-motion shows the completed line with fade-in captions.


</details>

---

## Attention psychology & visual salience

### Key insights

- Attention runs in two stages: a pre-attentive parallel sweep that fires in 200-250ms across the ENTIRE visual field with no conscious effort, then serial focal attention that inspects one item at a time. Design for the first stage - whatever wins the pre-attentive contrast computation is what the user 'sees' before they read a single word.
- Only a handful of features are truly pre-attentive and can be found in constant time regardless of clutter: color (hue), motion, orientation, size/length, and to a weaker degree shape and enclosure. Text, icon meaning, and anything requiring reading are NOT pre-attentive - they require serial fixation. Never encode primary salience in something that must be read.
- Motion is the single strongest attention magnet because peripheral vision (rod-dominant, magnocellular pathway) is evolutionarily tuned to detect motion faster than the fovea and it involuntarily yanks foveal gaze toward it. This is why a single moving element beats any static contrast - but it is also why gratuitous motion is the most destructive distractor.
- Salience is relative, not absolute - it is feature CONTRAST against the local background, not intrinsic brightness or size. One red item in a field of gray pops; the same red item among other reds vanishes. The Von Restorff isolation effect (30-50% better recall for the odd item) is the same mechanism at the memory level. Every extra emphasized element dilutes every other - if everything is bold, nothing is.
- The F-pattern is a FAILURE STATE, not a target. It appears when a wall of text gives the eye no meaningful cues, so users default to scanning the top and left edge and abandon the right side. Good structure (front-loaded content, descriptive headings, bolded keywords, bullets) converts F-scanning into the far more efficient layer-cake pattern where users read headings fully.
- Fixations last ~200-300ms and saccades (jumps between them) take only 20-40ms, during which you are functionally blind. Attention shifts to the saccade target BEFORE the eye arrives (presaccadic attention). This blindness window is what magicians and interfaces exploit - and what causes change blindness: any change that coincides with a saccade, blink, flicker, or page reload goes undetected.
- Animation is the antidote to change blindness. A flicker/instant swap forces the user to compare two static states (lab subjects needed 20-40 flicker cycles to spot a change); a smooth transition preserves motion continuity so peripheral vision catches the change automatically and central vision follows. Animate only the region that changed, one change at a time, and dim what stays static.
- UI animation has a tight usable band: ~100ms for feedback (toggle, checkbox), 200-300ms for substantial screen changes (modal entry), 100-400ms overall. Past 500ms motion crosses from 'informative' to 'a drag' and users feel blocked. Exit/dismiss animations should be faster than entrances because they need less attention.
- The aesthetic-usability effect (Kurosu & Kashimura 1995 ATM study) means users perceive beautiful interfaces as more usable and forgive MINOR usability flaws - but only on first impression and only for small problems. Beauty buys goodwill and masks minor friction; it cannot rescue large usability failures, and the halo erodes as frustration accumulates.
- Curiosity is a drive state triggered by a perceived information gap (Loewenstein 1994). Attention peaks when the gap is SMALL and closable - the user senses they almost know. Gaps that are too large produce low curiosity or frustration. This is why progressive disclosure, teasers, and 'X of Y complete' work, and why vague clickbait eventually breeds distrust.
- Faces and gaze are hardwired attention hijackers, and gaze direction is a directional cue the viewer involuntarily follows. Direct (mutual) gaze holds attention ON the face; AVERTED gaze redirects the viewer's attention along the model's line of sight - in ad eye-tracking, averted-gaze faces produced a near 5-fold jump in product dwell time and roughly tripled implicit recall vs no face. Point your model's eyes at the CTA, not the camera.
- Banner blindness is schema-driven, not just spatial: users learn the visual signature of ads (top banners, right rail, boxy promo units) and pre-attentively filter anything matching it - even relevant content placed there. It is distinct from ad fatigue (ignoring a specific creative seen too often). Making something LOOK like an ad to grab attention backfires; ironically, more animation on ads increased fixations but hurt reading of surrounding content.

### Numbers worth remembering

- Pre-attentive processing completes in ~200-250ms - a full parallel sweep of the visual field before conscious attention engages
- Typical fixation duration: 200-300ms (full range ~200-600ms); the fovea covers only ~2 degrees of sharp vision
- Saccade duration: 20-40ms, during which vision is functionally suppressed (saccadic blindness) - the window that causes change blindness
- Presaccadic attention shifts to the saccade target BEFORE the eye moves, pre-sharpening acuity there
- Von Restorff isolation effect: distinctive items show 30-50% better recall than surrounding uniform items
- UI animation - feedback (toggle/checkbox): ~100ms; substantial screen change (modal): 200-300ms; overall usable range 100-400ms (400ms only for large-screen movements)
- 500ms is the annoyance ceiling - animations at/above 500ms feel like a drag and block the user
- Micro-interactions best at 100-200ms; larger transitions 200-500ms
- Change blindness lab data: with a flicker interrupting the change, subjects needed 20-40 alternation cycles to locate a single altered detail
- Averted-gaze ad eye-tracking: product dwell time showed a near 5-fold increase (vertical banners) vs mutual gaze
- Averted-gaze explicit brand recognition: 1.50 items vs 1.06 (mutual gaze) vs 0.54 (no face); implicit recall 64.48% vs 47.79% vs 41.54%
- F-pattern has 3 phases: full horizontal top sweep, shorter second horizontal sweep, then a vertical scan down the left edge
- Truly pre-attentive features (constant-time pop-out): color/hue, motion, orientation, size/length, enclosure; weaker for shape; text/meaning are NOT pre-attentive
- Aesthetic-usability effect first documented by Kurosu & Kashimura, 1995 (ATM interface study); Loewenstein information-gap theory of curiosity, 1994; F-pattern from NN/G eye-tracking, 2006 (confirmed still valid on desktop and mobile)

<details>
<summary><strong>Sources</strong> (19)</summary>

- [Stimulus Saliency Modulates Pre-Attentive Processing Speed in Human Visual Cortex (PLOS ONE / PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC3025013/)
- [Pre-Attentive Attributes: What They Are and Why They Matter - The Comm Spot](https://thecommspot.com/comm-subjects/visual-communication/data-visualization/principles-of-data-visualization/pre-attentive-attributes-what-they-are-and-why-they-matter/)
- [How Peripheral Vision Guides User Focus - Collect Dots](https://collectdots.com/2024/10/18/how-peripheral-vision-guides-user-focus/)
- [The Surprising Potential of Peripheral Vision in Driving User Attention - Progress](https://www.progress.com/blogs/the-surprising-potential-of-peripheral-vision-in-driving-user-attention)
- [Von Restorff Effect: UX Law of Distinctiveness - UXtweak](https://blog.uxtweak.com/von-restorff-effect/)
- [The Von Restorff Effect - Laws of UX / cursorup](https://www.cursorup.com/blog/von-restorff-effect)
- [F-Shaped Pattern of Reading on the Web: Misunderstood, But Still Relevant - NN/G](https://www.nngroup.com/articles/f-shaped-pattern-reading-web-content/)
- [The Layer-Cake Pattern of Scanning Content on the Web - NN/G](https://www.nngroup.com/articles/layer-cake-pattern-scanning/)
- [5 Principles of Visual Design in UX - NN/G](https://www.nngroup.com/articles/principles-visual-design/)
- [The Aesthetic-Usability Effect - NN/G](https://www.nngroup.com/articles/aesthetic-usability-effect/)
- [Change Blindness in UX: Definition - NN/G](https://www.nngroup.com/articles/change-blindness-definition/)
- [Change Blindness Causes People to Ignore What Designers Expect Them to See - NN/G](https://www.nngroup.com/articles/change-blindness/)
- [Executing UX Animations: Duration and Motion Characteristics - NN/G](https://www.nngroup.com/articles/animation-duration/)
- [Duration & Easing - Material Design](https://m3.material.io/styles/motion/easing-and-duration)
- [Banner Blindness Revisited: Users Dodge Ads on Mobile and Desktop - NN/G](https://www.nngroup.com/articles/banner-blindness-old-and-new-findings/)
- [The influence of banner advertisements on attention and memory: human faces with averted gaze (PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC3941030/)
- [Curiosity, Information Gaps, and the Utility of Knowledge - Golman & Loewenstein (CMU)](https://www.cmu.edu/dietrich/sds/docs/golman/golman_loewenstein_curiosity.pdf)
- [Visual Attentional Orienting by Eye Gaze: A Meta-Analytic Review of the Gaze-Cueing Effect](https://www.researchgate.net/publication/358701907_Visual_Attentional_Orienting_by_Eye_Gaze_A_Meta-Analytic_Review_of_the_Gaze-Cueing_Effect)
- [Types of Eye Movements (fixations, saccades) - Tobii](https://www.tobii.com/resource-center/learn-articles/types-of-eye-movements)

</details>

---

## Motion design foundations & craft

### Key insights

- Easing matters more than duration for perceived quality: use ease-out (decelerate) for anything entering or responding to a tap so it starts at peak velocity and settles — the fast start reads as responsiveness; use ease-in (accelerate) only for exits; use ease-in-out for elements moving while staying on screen; never use linear for spatial motion (reserve it for opacity/color fades). Built-in CSS keywords are too flat — premium UIs use custom cubic-beziers (e.g. Material's standard cubic-bezier(0.2, 0, 0, 1)) that brake harder at the end.
- Enter/exit asymmetry is a codified rule, not a taste call: exits should be ~15-30% faster than entrances (Material spec: 225ms enter with decelerate vs 195ms exit with accelerate; NN/g: 300ms in, 200-250ms out) because users must visually track arriving content but departing content just needs to get out of the way. Exits can also skip stagger and sometimes skip animation entirely (Emil Kowalski: subsequent tooltips in a toolbar should appear with transition-duration: 0ms).
- Duration must scale with size and distance traveled, not be one global constant: micro-feedback (checkbox, toggle, button state) ~70-150ms; dropdowns/selects ~150-250ms; modals and bottom sheets 200-300ms (up to 400ms with emphasized easing); full-page/large-canvas transitions 400-700ms. Anything over ~400-500ms on a repeated interaction reads as lag. Carbon makes this dynamic ('the larger the change in distance or size, the longer the animation') and even ships a duration calculator.
- Frequency dictates restraint — 'motion as feedback' and 'motion as branding' need separate budgets: IBM Carbon formalizes this as productive motion (fast, subtle, ease-heavy, for task flows: 70-240ms) vs expressive motion (slower, more overshoot and character, reserved for 'occasional, important moments' like page opens and primary CTAs: 150-700ms). Salesforce isolates 'Personality & Branding' as its own motion category. An animation a user sees 50 times a day should shrink toward zero; a first-run or success moment can afford 500ms+ of personality.
- Disney's principles translate to specific UI moves: squash-and-stretch = button press states (scale to ~0.97 on :active); anticipation = preparatory hover/press cues before the main transition; staging = one primary movement at a time so attention isn't split; follow-through/overlapping action = spring overshoot on settle plus elements of a group arriving at slightly offset times (stagger); slow-in/slow-out = easing curves themselves; arcs = elements moving diagonally should follow a curved path (Material's arc motion) rather than a straight line.
- Never scale from 0 and never scale from center-by-default: entrances should start at scale 0.85-0.95 (0.93 is a good default — real objects like a deflating balloon never vanish to a point), and popovers/menus/tooltips must set transform-origin to the trigger point (e.g. transform-origin: bottom center for a dropdown opening downward; Radix/Base UI expose this as var(--radix-dropdown-menu-content-transform-origin)). Center-origin scaling is the single most common tell of un-crafted motion.
- Stagger interval sweet spot is 30-70ms between items (usable range 20-100ms; a common recipe is 50ms intervals, or ~30-70% of each item's own duration, e.g. 100ms gaps on 300ms items) — and cap the total choreography so the last item lands within ~1s. Stagger entrances only; staggered exits feel like the UI is dawdling.
- Springs beat duration-based curves for interactive/gestural motion because they are interruptible and velocity-preserving — a redirected drag inherits momentum instead of restarting. Reference parameters: SwiftUI .spring() defaults to response 0.55 / dampingFraction 0.825 (Apple guidance: damping 0.7-1.0 for subtle elasticity without cartoonish bounce); Framer Motion defaults stiffness 100 / damping 10 / mass 1; stiffness 170 / damping 15 is a classic snappy preset.
- CSS can now do real springs natively via linear() easing (supported in all major browsers since Dec 2023): generate 25-75 points from stiffness/damping/mass with tools like Jake Archibald & Adam Argyle's linear-easing-generator or Easing Wizard, values >1 create overshoot/bounce, performance cost is negligible (~1.3kB gzipped for three 75-point springs), and ship it as a CSS custom property with a cubic-bezier fallback behind @supports (animation-timing-function: linear(0, 1)).
- Continuity is built with FLIP (Paul Lewis: First, Last, Invert, Play — measure both layouts, invert the delta as a transform, then release it as a transition) so you only ever animate compositor-friendly transform and opacity at 60fps; the modern successor is the View Transitions API, where a shared view-transition-name morphs an element (thumbnail → hero) between states and you tune duration/easing on ::view-transition-group(). Shared-element ('container transform') continuity preserves object permanence so navigation feels like moving one object, not swapping screens. Always honor prefers-reduced-motion / Reduce Motion by minimizing or removing non-essential animation.

### Patterns

#### Ease-out in, ease-in out (entrance/exit asymmetry)

Entering elements decelerate into place (start fast, settle softly) over a slightly longer duration; exiting elements accelerate away over a shorter one. Every major design system encodes this: Material (225ms decelerate in / 195ms accelerate out), Carbon (separate entrance and exit bezier tokens), NN/g (300ms in / 200-250ms out).

- **Why it catches attention:** A fast start reads as instant responsiveness while the deceleration gives the eye time to lock onto the new element's final position; departing content carries no information so slower exits register as the UI wasting the user's time. Symmetric motion feels mechanical because nothing in the physical world starts and stops identically.
- **Implementation:** Entrances: cubic-bezier(0, 0, 0.38, 0.9) (Carbon productive entrance) or cubic-bezier(0.05, 0.7, 0.1, 1) (M3 emphasized-decelerate), 200-300ms. Exits: cubic-bezier(0.2, 0, 1, 0.9) (Carbon) or cubic-bezier(0.3, 0, 0.8, 0.15) (M3 emphasized-accelerate), 150-250ms. Pair opacity fade with a small translate/scale so motion has direction. Avoid the CSS keyword ease-in for anything except exits.
- **Seen at:** Material Design dialogs and sheets; IBM Carbon modals and side panels; iOS sheet presentation; toast libraries like Sonner

#### Motion token scale (duration hierarchy tied to size/distance)

Instead of one transition duration, systems ship a ramp of duration tokens mapped to the scale of change: Carbon's 70/110/150/240/400/700ms, Material 3's 50→1000ms across short/medium/long/extra-long, with explicit component assignments (short = selection controls, medium = bottom sheets, long+ = full-screen).

- **Why it catches attention:** Perceived consistency requires objective inconsistency: a checkbox and a full-page transition animated at the same speed feel wrong because larger perceived distance implies longer travel time in the physical world. A tuned ramp makes the whole product feel like one physical material.
- **Implementation:** Define 4-6 duration custom properties (e.g. --duration-fast: 100ms … --duration-slow: 500ms) and 3 easing tokens (standard/enter/exit). Scale duration with distance (~100ms per 10% viewport traveled) or element count (2-5 objects 300-400ms, 6-10 objects 500-700ms). Use a non-linear ramp — steps get bigger as durations grow — for better perceived evenness (Carbon's approach).
- **Seen at:** IBM Carbon @carbon/motion package; Material 3 md.sys.motion.duration tokens; Sprout Social Seeds; Norton Design System

#### Productive vs expressive dual-mode motion (feedback vs branding)

Two distinct motion vocabularies in one system: productive motion is fast, subtle, and near-invisible for task completion (button states, dropdowns, table sorting); expressive motion is slower, larger, with more curve personality, reserved for milestone moments (page opens, primary CTA, success states). IBM colors them differently in specs (blue vs magenta); Salesforce splits 'Personality & Branding' into its own category; Material frames it as informative/focused/expressive.

- **Why it catches attention:** Brand personality registers precisely because it is rationed — if every hover bounces, nothing stands out and the product feels slow. Restricting expressive motion to rare moments keeps daily-use interactions frictionless while still landing memorable brand beats.
- **Implementation:** Maintain two easing sets: productive ≈ gentle curves like cubic-bezier(0.2, 0, 0.38, 0.9) at 70-240ms; expressive ≈ stronger curves like cubic-bezier(0.4, 0.14, 0.3, 1) with springs/overshoot at 150-700ms. Budget rule: the more frequently an interaction occurs, the shorter/subtler its animation — high-frequency actions may deserve no animation at all.
- **Seen at:** IBM Carbon / IBM Design Language; Salesforce Lightning; Material Design; Stripe's restrained dashboard vs expressive marketing pages

#### Spring physics and CSS linear() bounce

Motion driven by simulated mass-spring-damper physics instead of fixed bezier+duration. Springs settle naturally, can overshoot (follow-through), and — critically — preserve velocity when interrupted mid-flight. Now expressible in pure CSS via the linear() easing function, which approximates any curve including overshoot past 1.

- **Why it catches attention:** Real objects have momentum; spring motion triggers the same physical intuition, so elements feel like tangible things rather than crossfading pixels. The slight overshoot-and-settle is Disney's follow-through principle, and interruptibility means gestures never feel like they 'reset'.
- **Implementation:** Native: SwiftUI .spring(response: 0.55, dampingFraction: 0.825) (defaults), damping 0.7-1.0 for subtlety. JS: Framer Motion type:'spring' (stiffness 100/damping 10/mass 1 defaults; 170/15 for snappy). CSS: generate linear(0, 0.013 0.6%, … 1) with 25-75 points from linear-easing-generator or Easing Wizard; values >1 give bounce; ship as custom property with cubic-bezier fallback inside @supports (animation-timing-function: linear(0,1)). Reserve springs for movement/scale, not opacity, and skip them for data-critical UI where instant feedback wins.
- **Seen at:** All of iOS (sheets, app open/close); Framer Motion/Motion One sites; Family and Arc browser apps famous for spring-heavy feel; Josh Comeau's blog interactions

#### Staggered choreography

Members of a group (list items, cards, menu rows) animate identically but with tiny sequential delays — 20-100ms apart, typically 50ms — creating a wave that implies structure and order. Disney's 'overlapping action': things don't move all at once.

- **Why it catches attention:** The visual system tracks one moving object at a time; simultaneous movement of 12 items reads as a single blob or as chaos, while a wave guides the eye along the content's reading order and makes loading feel deliberately orchestrated rather than dumped.
- **Implementation:** CSS: transition-delay: calc(var(--index) * 50ms) with a custom property per item, or SCSS loops. JS: stagger(0.05) helpers in Motion/GSAP/Framer. Keep interval at 30-70% of per-item duration (100ms gap on 300ms items); ensure total sequence completes within ~1s (shrink interval as count grows); stagger entrances only — exit everything together and faster.
- **Seen at:** Linear's issue-list load-in; Vercel dashboard; Flutter staggered menu pattern; iOS home-screen icon cascade

#### Origin-aware transforms (scale from the trigger, squash on press)

Every scaling element grows from where it logically comes from: dropdowns from the button edge (transform-origin: bottom/top + side), context menus from the cursor, dialogs from the triggering card. Companion micro-pattern: buttons compress slightly on press (scale 0.97) — Disney's squash-and-stretch as tactile feedback. Entrances start at scale 0.9-0.95, never scale(0).

- **Why it catches attention:** It preserves causality — motion that emanates from the point of interaction confirms 'your tap did this' at a pre-cognitive level. Center-origin or from-zero scaling breaks the physical metaphor (real objects don't materialize from a point) and instantly reads as template-grade animation.
- **Implementation:** Set transform-origin per placement (bottom center for a dropdown opening downward); Radix/Base UI expose var(--radix-dropdown-menu-content-transform-origin) computed per collision side. Press feedback: .btn:active { scale: 0.97; transition: 100-150ms ease-out }. Entrance: opacity 0→1 + scale 0.93→1 over ~200ms ease-out. A 2px filter: blur() during morphs masks discontinuities.
- **Seen at:** macOS/iOS context menus; Radix UI and Base UI primitives; Vaul drawer; Emil Kowalski's animations.dev examples

#### Anticipation and follow-through (Disney micro-physics)

Anticipation: a small preparatory cue before the main action (hover pre-state, a button dipping before a panel launches, pull-to-refresh tension). Follow-through/overlapping action: elements slightly overshoot their destination and settle back, with secondary parts (icon inside a button, header vs body) arriving on marginally different schedules.

- **Why it catches attention:** Anticipation lets users predict outcomes before committing (reducing errors and perceived latency), while follow-through exploits the expectation that moving masses can't stop instantly — both make interfaces obey intuitive physics, which the brain rewards as 'quality'.
- **Implementation:** Anticipation: 50-100ms hover/press pre-transitions; slight counter-movement (translate -2px before +x). Follow-through: springs with dampingFraction ~0.7-0.8, or CSS linear() with points exceeding 1 (e.g. peaks at 1.05-1.15); offset child animation-delays 30-60ms from the parent. Keep overshoot ≤10-15% of travel for premium (vs cartoonish) feel.
- **Seen at:** Pull-to-refresh in iOS Mail; Telegram send-button morph; Duolingo celebration moments; Apple's rubber-band overscroll

#### FLIP and shared-element continuity (container transform / View Transitions)

One element visually persists across a layout or route change: a thumbnail morphs into the hero, a card expands into a detail page. Classically implemented with Paul Lewis's FLIP (First, Last, Invert, Play — measure both states, apply the inverted delta as a transform, then transition it away), natively now via the View Transitions API with matching view-transition-name across states.

- **Why it catches attention:** Object permanence: the brain tracks the persisting element through the change, so navigation feels like moving within one physical space instead of teleporting between screens — the strongest single signal of a 'native-quality' web app.
- **Implementation:** FLIP: getBoundingClientRect() before/after, transform: translate/scale by the inverted delta, then release with a 250-400ms ease-out transition — only transform+opacity animate, keeping everything compositor-only at 60fps. View Transitions: assign view-transition-name to both states; default is a crossfade — override on ::view-transition-group(name) { animation-duration: 300ms; animation-timing-function: cubic-bezier(0.2, 0, 0, 1) } so it cascades to old/new; use document.startViewTransition() in SPAs.
- **Seen at:** Android/Material container transform; Airbnb listing photo → detail; Chrome's view-transitions demos; react-flip-toolkit and Framer Motion layoutId

#### Performance and accessibility guardrails

The non-negotiables under all of the above: animate only compositor properties (transform, opacity, and clip-path for reveals), never layout properties (width/height/top/left); honor prefers-reduced-motion / Reduce Motion by removing or drastically reducing non-essential movement; review animations at slow speed to catch choreography misalignments.

- **Why it catches attention:** Nothing destroys 'premium' faster than dropped frames — a 60fps mediocre curve beats a janky perfect one; and motion-sensitive users experience unguarded animation as literal discomfort, which Apple now audits in App Store accessibility criteria.
- **Implementation:** transform/opacity only (FLIP any layout change); will-change sparingly; clip-path for tab-highlight and reveal transitions instead of moving backgrounds; @media (prefers-reduced-motion: reduce) { * { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important } } or swap movement for pure opacity fades; test at 0.1x speed (Chrome DevTools animation inspector) to verify simultaneous property sync.
- **Seen at:** Apple HIG Reduce Motion requirements; web.dev/RAIL guidance; Linear and Stripe engineering practice


### Numbers worth remembering

- Perception thresholds (NN/g): <100ms feels instantaneous; 100-500ms is the usable animation band; ~1s is the limit of uninterrupted flow of thought; at 500ms animations 'start to feel like a real drag'
- General duration guidance: NN/g 100-400ms for most UI (400ms only for big cross-screen movement); Val Head: small elements 200-350ms, large/bouncy motion 400-500ms; Emil Kowalski: keep UI animation under 300ms (a 180ms select feels better than 400ms); Apple HIG-aligned practice: 0.2-0.5s for most transitions
- Enter vs exit asymmetry (Material Design spec): enter 225ms with deceleration curve, exit 195ms with acceleration curve; NN/g example: popup 300ms in, 200-250ms out; transitions exceeding 400ms feel too slow
- Material Design 3 duration tokens: short1-4 = 50/100/150/200ms (selection controls, small utility), medium1-4 = 250/300/350/400ms (area transitions like bottom sheets), long1-4 = 450/500/550/600ms and extra-long1-4 = 700/800/900/1000ms (large, full-screen transitions)
- Material Design 3 easing tokens: standard cubic-bezier(0.2, 0, 0, 1); standard-decelerate cubic-bezier(0, 0, 0, 1); standard-accelerate cubic-bezier(0.3, 0, 1, 1); emphasized-decelerate cubic-bezier(0.05, 0.7, 0.1, 1); emphasized-accelerate cubic-bezier(0.3, 0, 0.8, 0.15); legacy M2 standard cubic-bezier(0.4, 0, 0.2, 1), decelerate cubic-bezier(0, 0, 0.2, 1), accelerate cubic-bezier(0.4, 0, 1, 1)
- IBM Carbon duration tokens (verified from @carbon/motion v11.47.0 source): fast-01 70ms (button states, micro-feedback), fast-02 110ms (toggles, fades), moderate-01 150ms, moderate-02 240ms (dropdowns, expansion, system communication), slow-01 400ms (large expansion, page-level), slow-02 700ms (background dim, full-page transitions); base static tokens: productive 100ms, expressive 150ms
- IBM Carbon easing curves — productive: standard cubic-bezier(0.2, 0, 0.38, 0.9), entrance cubic-bezier(0, 0, 0.38, 0.9), exit cubic-bezier(0.2, 0, 1, 0.9); expressive: standard cubic-bezier(0.4, 0.14, 0.3, 1), entrance cubic-bezier(0, 0, 0.3, 1), exit cubic-bezier(0.4, 0.14, 1, 1)
- Stagger: 20-100ms between items with 50ms as the common default; delay ≈ 30-70% of per-item duration (e.g. 100ms gap on 300ms items); keep the full staggered sequence under ~1s; don't stagger exits
- Spring parameters: SwiftUI .spring() defaults response 0.55 / dampingFraction 0.825 / blendDuration 0; damping ratio 0.7-1.0 for subtle non-cartoonish elasticity; interpolatingSpring snappy preset stiffness 170 / damping 15 / mass 1; Framer Motion defaults stiffness 100 / damping 10 / mass 1
- CSS linear() springs: 11 points = robotic, 25 points = acceptable, 50-75+ points = convincing; supported all major browsers since Dec 2023 (~88% coverage as of Oct 2025); ~1.3kB gzipped for three 75-point springs; no measurable framerate cost at 100+ points
- Micro-interaction transforms: button :active scale 0.97; entrance scale start 0.9-0.95 (0.93 typical), never scale(0); dialogs/menus enter from scale 0.85-0.9; 2px filter:blur() can mask rough state transitions
- Dynamic-duration formulas (designsystems.com): small components 100-200ms, page transitions 500-700ms; alternatively ~100ms per 10% of viewport traveled; complexity-based: 2-5 moving objects 300-400ms, 6-10 objects 500-700ms

<details>
<summary><strong>Sources</strong> (20)</summary>

- [Material Design 3 — Easing and duration tokens & specs](https://m3.material.io/styles/motion/easing-and-duration/tokens-specs)
- [MDUI Design Tokens (full M3 motion token values)](https://www.mdui.org/en/docs/2/styles/design-tokens)
- [IBM Carbon Design System — Motion](https://carbondesignsystem.com/elements/motion/overview/)
- [IBM Design Language — Motion UI basics (easing curves, productive vs expressive)](https://design-language-website.netlify.app/design/language/motion-ui/basics/)
- [@carbon/motion package (duration/easing source of truth, v11.47.0)](https://github.com/carbon-design-system/carbon/tree/main/packages/motion)
- [Apple Human Interface Guidelines — Motion](https://developer.apple.com/design/human-interface-guidelines/motion)
- [NN/g — Executing UX Animations: Duration and Motion Characteristics](https://www.nngroup.com/articles/animation-duration/)
- [Val Head — How fast should your UI animations be?](https://valhead.com/2016/05/05/how-fast-should-your-ui-animations-be/)
- [Material Design (M1) — Duration & easing (225ms enter / 195ms exit)](https://m1.material.io/motion/duration-easing.html)
- [Josh W. Comeau — Springs and Bounces in Native CSS (linear())](https://www.joshwcomeau.com/animation/linear-timing-function/)
- [Chrome Developers — Create complex animation curves with linear()](https://developer.chrome.com/docs/css-ui/css-linear-easing-function)
- [Emil Kowalski — 7 Practical Animation Tips](https://emilkowal.ski/ui/7-practical-animation-tips)
- [Emil Kowalski — Good vs Great Animations](https://emilkowal.ski/ui/good-vs-great-animations)
- [Paul Lewis — FLIP Your Animations](https://aerotwist.com/blog/flip-your-animations/)
- [CSS-Tricks — Animating Layouts with the FLIP Technique](https://css-tricks.com/animating-layouts-with-the-flip-technique/)
- [MDN — View Transition API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API)
- [IxDF — How to Apply Disney's 12 Principles of Animation to UI Design](https://ixdf.org/literature/article/ui-animation-how-to-apply-disney-s-12-principles-of-animation-to-ui-design)
- [Aninix — Mastering UI Animation: The Art of Stagger Techniques](https://www.aninix.com/wiki/how-to-create-a-good-stagger-in-the-ui-animation)
- [designsystems.com — 5 steps for including motion design in your system](https://www.designsystems.com/5-steps-for-including-motion-design-in-your-system/)
- [GetStream — SwiftUI Spring Animations guide (default parameters)](https://github.com/GetStream/swiftui-spring-animations)

</details>

---

## 2025-2026 trends in attention-catching motion design

### Key insights

- Restraint is THE 2026 meta-trend: every credible source describes 'purposeful motion' — one hero moment done exceptionally well beats ten competing animations. The version that ships respects prefers-reduced-motion, loads fast on a mid-range phone, and fires when the user has stopped to read, not mid-scroll.
- Scroll-driven animation went native: CSS animation-timeline: scroll()/view() (shipped Chrome 115, now cross-browser) replaces JS scroll listeners and runs off the main thread — scroll-linked effects are now cheap enough for production marketing sites, not just Awwwards showpieces. GSAP ScrollTrigger + Lenis remains the stack for complex pinned scenes.
- The View Transitions API hit Baseline Newly Available in October 2025 (Firefox 144 joined Chrome/Edge/Safari, ~85%+ coverage), triggering a page-transition renaissance: shared-element morphs between pages on plain multi-page sites with ~20 lines of CSS replacing hundreds of lines of Framer Motion/Barba.js; React support is in canary.
- 3D/WebGL crossed from portfolio flex to mainstream: 3D elements appear on 28% of Awwwards-featured sites (up from 12% in 2023) and average 3D hero load time dropped from 4.2s to 1.8s thanks to Draco/KTX2 compression and lazy hydration — cursor-reactive 3D heroes are now viable for e-commerce and SaaS.
- Micro-interaction timing has converged on hard numbers: 200-500ms total, 150-200ms for simple state changes, 300-400ms for complex ones. The differentiating 2025-26 move is one signature interaction (Duolingo's owl, Linear's sub-100ms snappiness) repeated until it IS the brand.
- Apple's Liquid Glass (WWDC June 2025) reset glassmorphism from static backdrop blur to glass as an optical object — edge refraction, motion-responsive specular highlights, adaptive contrast. The dominant web expression is dark-glass panels floating over animated aurora/mesh gradient backdrops — the current SaaS default look (Stripe/Linear lineage).
- The anti-AI-slop backlash is now a design driver: 78% of consumers say AI-generated imagery isn't authentic, so grain, glitch, collage, hand-drawn marks and deliberately imperfect motion read as 'a human made this' and work as attention+trust signals. Bonus: film-grain noise overlays also fix gradient banding — one technique, two payoffs.
- Bento grids won because they match attention mechanics — eye goes to the largest tile first, then adjacent blocks (claims of +47% dwell, +38% CTR). The 2025-26 evolution is the ANIMATED bento: 60-100ms staggered scroll-in reveals and per-tile hover micro-demos with live product footage (Apple, Vercel, Linear, Notion).
- Scrollytelling has measurable ROI: interactive scroll-driven data viz recorded +62% dwell time and +317% scroll depth in a 2025 study — which is why product brands (Scout Motors, Cartier, Igloo Inc) now use pinned 3D scroll sequences as their primary conversion narrative, not just editorial garnish.
- Kinetic typography became practical because of variable fonts: animating wght/wdth axes of one font file via CSS font-variation-settings (or cursor-proximity mapping per character) delivers fluid type motion with tiny payloads and no reflow flash — the cheapest high-impact motion trick of 2025-26, but display type only, never body copy.

### Patterns

#### Kinetic and oversized typography

Massive display type as the hero visual itself — often filling the viewport — that moves: slides, warps, scales, or reveals word-by-word on load, scroll, or hover. 2026 framing is 'type as the interface' with deliberate restraint (heroes and section breaks only, never body copy).

- **Why it catches attention:** Scale violates expected typographic hierarchy so the eye locks on immediately; motion on text exploits the reading reflex — we can't not read moving words. High contrast + few elements gives the brain a single dominant target.
- **Implementation:** SplitType/GSAP SplitText for per-character staggered reveals (20-40ms/char); clip-path or overflow-hidden line masks for 'rising text' entrances; scroll-mapped transforms via CSS animation-timeline: view() or ScrollTrigger scrub; viewport-relative sizing (clamp() with vw units, e.g. clamp(3rem, 12vw, 12rem)); keep motion 300-700ms with ease-out.
- **Seen at:** Van Holtz Co (massive interactive animated homepage type), Eyesprint (warping kinetic hero lettering), Safari Riot, Monolith, Apres Creative Studio, Bloomers agency (retro 60s oversized purple type), Glossier and Samsung (bold kinetic hero lettering), Jitter and Sofi (fluid well-timed product text motion)

#### Scrollytelling / scroll-driven animation

The page as a timeline: pinned sections, scrubbing 3D sequences, image sequences, and charts that animate in sync with scroll position, turning a landing page into a narrative the user drives at their own pace.

- **Why it catches attention:** Direct manipulation illusion — the user's own gesture causes the spectacle, creating agency and a game-like feedback loop; sequential reveal builds curiosity gaps ('what happens if I keep scrolling'), which is why scroll depth lifts +317% on interactive scroll stories.
- **Implementation:** Native CSS: animation-timeline: scroll()/view() with animation-range for enter/exit choreography (compositor-thread, jank-free). Complex scenes: GSAP ScrollTrigger with scrub + pin, Lenis for smooth scroll normalization; pre-rendered image sequences or Three.js camera paths scrubbed by scroll progress; Shorthand/Webflow interactions for no-code versions. Always gate behind prefers-reduced-motion.
- **Seen at:** Igloo Inc (Awwwards Site of the Year — 3D igloo world scrubbed by scroll), Scout Motors (scroll test-drive), Cartier 'The Fabulous Journey' (3D brand storytelling), Esimple (cinematic typewriter-to-rooms scroll narrative), Apple product pages (AirPods-style pinned image-sequence scrubbing, the template everyone copies), Wamos Air, Lusion.co

#### Bento grids (animated)

Modular, asymmetric card mosaics — one dominant hero tile plus varied smaller tiles — used to present many features as parallel propositions. The 2025-26 version is animated: staggered scroll-in reveals, per-tile hover states that play a micro-demo, and live UI footage looping inside cells.

- **Why it catches attention:** Matches natural scan behavior (largest element first, then adjacent), lets users self-select the tile matching their need, and each cell is a self-contained dopamine hit — sources report +47% dwell time and +38% CTR versus linear feature sections.
- **Implementation:** CSS Grid with spanning cells (grid-template-columns: repeat(4, 1fr) + col/row spans); IntersectionObserver or animation-timeline: view() for staggered entrance (translateY 16-24px + fade, 60-100ms stagger); hover scale 1.02-1.03 with subtle border-glow; <video muted loop> or Lottie inside tiles playing on hover/in-view only.
- **Seen at:** Apple (originated on iPhone/iPad feature pages — big hero card + spec widgets), Vercel (deployment/observability/AI features as parallel tiles), Linear (speed/design/integrations tiles with live UI footage), Notion (AI agents/databases/connectors), Google Analytics dashboards; by 2026 the default pattern for new B2B SaaS homepages

#### 3D and WebGL elements

Interactive, cursor- and scroll-reactive 3D — product configurators, navigable worlds, shader-driven hero backgrounds — now mainstream beyond agency portfolios into e-commerce and SaaS, at 28% of Awwwards-featured sites.

- **Why it catches attention:** Depth and parallax trigger real spatial perception rather than 'looking at a page'; objects that respond to your cursor create a touchable, toy-like quality that sustains exploration far longer than static imagery.
- **Implementation:** Three.js / react-three-fiber + drei; Draco/KTX2 compression and lazy hydration to hit the new 1.8s hero-load norm; scroll-scrubbed camera paths; cursor parallax via lerped pointer coordinates fed to mesh rotation (dampened ~0.05-0.1 lerp factor); custom GLSL shaders for liquid/refraction effects; Spline for no-code embeds; static poster + reduced-motion fallback mandatory.
- **Seen at:** Bruno Simon portfolio (drivable-car 3D world, Awwwards Site of the Month Jan 2026), Igloo Inc, Lusion.co (agency setting the shader benchmark), Lando Norris site (cursor-reactive 3D imagery), Cartier Fabulous Journey, Times Two, DHNN, Panasonic virtual home interior, ExxonMobil CCS experience, Nike and IKEA product/AR previews

#### Glassmorphism evolution: Liquid Glass and dark-glass

Post-WWDC-2025 glass: no longer flat frosted blur but an optical material with edge refraction, specular highlights that shift with motion, and adaptive tint/contrast. On the web the dominant expression is dark-glass — translucent charcoal panels with 1px light borders floating over animated gradient backdrops.

- **Why it catches attention:** Layered translucency creates instant depth hierarchy (the glass card reads as 'closer'); light glints that move with scroll/cursor mimic real material physics, which the eye tracks involuntarily; dark glass over a glowing aurora background maximizes contrast between chrome and content.
- **Implementation:** backdrop-filter: blur(12-24px) saturate(160-180%); rgba(15-25,15-25,20-30,0.5-0.7) fills for dark glass; 1px inset border rgba(255,255,255,0.08-0.15) + subtle top-edge highlight gradient to fake refraction; animated specular sweep via background-position keyframes on a linear-gradient overlay; Apple's real lensing needs shaders — web versions approximate with layered gradients; always test text contrast over busy backdrops.
- **Seen at:** Apple Liquid Glass across iOS 26/macOS Tahoe (the reference implementation: real-time lensing, motion response, adaptive contrast); Fluid Glass site (frosted UI mirroring physical product); Vision Pro marketing; ubiquitous in 2025-26 SaaS dashboards and fintech (Stripe-style panels); Microsoft Fluent Acrylic as the ecosystem parallel

#### Grain, noise textures, and aurora/mesh gradient backgrounds

Slow-drifting multi-color mesh gradients ('aurora' backgrounds) overlaid with film grain — the premium backdrop of 2025-26. Noise reduces banding and, in the anti-AI-slop era, texture reads as human and tactile.

- **Why it catches attention:** Slow ambient color drift (10-30s loops) registers in peripheral vision and makes a page feel alive without demanding focus; grain adds perceived material quality ('printed, not rendered'), and organic imperfection now signals authenticity against the flood of hyper-smooth AI output.
- **Implementation:** Mesh/aurora: 3-5 large radial-gradients or blurred (filter: blur(80-120px)) colored blobs animated with slow transform keyframes (20-40s, alternate); or a tiny WebGL/canvas simplex-noise shader. Grain: SVG feTurbulence (baseFrequency ~0.65-0.9, fractalNoise) as a fixed overlay at 4-15% opacity with mix-blend-mode: overlay, or a tiled 128px noise PNG; keep grain in a separate layer so it doesn't blur with the gradient. Generators expose 0-60% grain sliders for the 'Stripe/Linear-style' finish.
- **Seen at:** Stripe (the canonical animated WebGL mesh-gradient hero, endlessly cloned), Linear (dark aurora glows behind product shots), Vercel and Raycast (grain + glow dark themes), Gladeye and Perplexity ('dreamy eerie softness' — grainy low-contrast gradients), countless 2025 SaaS launches via tools like auroragradient.com and Grainient

#### Brutalism / anti-design with motion

Neo-brutalism (raw HTML energy, hard borders, unstyled-looking type, harsh shadows, clashing color) energized by deliberate motion: jump-cut hovers, marquee tickers, cursor chaos, glitch effects. Two flavors: playful neobrutalism (thick black outlines, candy colors) and cold fashion-brutalism (monospace, whitespace, silence).

- **Why it catches attention:** Pattern violation — against a feed of polished template sites, 'wrong-looking' design triggers orienting response; abrupt, snappy motion (no easing, instant state flips) feels rebellious and confident; irony signals in-group taste to design-literate audiences.
- **Implementation:** System/monospace font stacks; box-shadow: 6px 6px 0 #000 hard offsets that collapse to 0 on :active for a physical 'press'; hover states that swap colors instantly (transition: none) or use steps() easing for intentional jank; infinite CSS marquee tickers; oversized underlines and visible borders; glitch via clip-path slice keyframes + RGB-split text-shadow; deliberately mixed type sizes and rotated stickers.
- **Seen at:** Gumroad (poster child: vibrant pink, sharp black outlines, digital-zine layout), Balenciaga (weaponized cold brutalism — monospace, hard edges, silence between elements), Diesel, Mailchimp (softened anti-design accents), Figma and Poolsuite (playful retro-brutalist moments), Vercel/Evil Rabbit 'technical mono' code-brutalism as the 2026 dev-tool dialect

#### Micro-interactions as brand signature

Small, repeated feedback animations (button presses, toggles, success states, loaders) designed so distinctively they become brand assets — the motion equivalent of a logo. 2025-26 shifts from 'add delight everywhere' to one ownable signature repeated consistently.

- **Why it catches attention:** Immediate causal feedback satisfies the action-reaction loop (perceived responsiveness); surprise-and-delight moments trigger small reward responses users remember and share; consistency turns a 300ms animation into recognition — you know a Duolingo celebration without seeing the logo.
- **Implementation:** Duration discipline: 150-200ms simple, 300-400ms complex, never past ~500ms in-flow; ease-out on user-triggered responses; spring/overshoot curves (cubic-bezier(0.34,1.56,0.64,1)) for playful brands, crisp ease for serious ones; Lottie/Rive for character or complex vector moments; transform+opacity only (compositor-safe); define motion tokens (durations, easings) in the design system so the signature stays consistent.
- **Seen at:** Duolingo (celebrating owl — the canonical motion mascot), Stripe (animated gradient + precise hover physics as brand feel), Linear (instant, sub-100ms interactions AS the brand promise of speed), Facebook emoji reactions, Netflix hover-preview cards, Rocket Air (play-button cursor triggering video transitions), Webflow-featured signature hovers

#### AI-era aesthetics and the anti-slop backlash

Twin movements: (1) aesthetics ABOUT the AI era — technical mono/code-brutalism, terminal UIs, surveillance-cam dystopia, Y3K chrome hyperfuturism; (2) aesthetics AGAINST AI sameness — collage, scrapbook/kidcore, hand-drawn marks, grain, glitch, interface nostalgia (Frutiger Aero, PC-98 pixel art) that prove human authorship.

- **Why it catches attention:** Differentiation against a flood of near-identical AI-generated layouts — 78% of consumers consider AI imagery inauthentic, so visible human fingerprints (tape, scribbles, dithering, imperfect motion) function as trust and novelty signals simultaneously; nostalgia motifs hit emotional memory in Gen-Z/millennial audiences.
- **Implementation:** Technical mono: monospaced type (Berkeley Mono, Geist Mono), ASCII-art dividers, blinking-cursor animations, terminal-style typed-text effects. Collage/scrapbook: layered PNGs with hard drop shadows, slight random rotations (transform: rotate(-2deg to 2deg)), sticker hovers that 'peel'. Nostalgia: dithered gradients, pixel fonts with image-rendering: pixelated, OS-chrome window components as cards. Glitch: chromatic aberration text-shadow + clip-path slicing keyframes.
- **Seen at:** Vercel (Evil Rabbit's technical-mono house style), Factory AI, Unit Software, Paul Macgregor and Tina Mai portfolios (code brutalism); ngr.ev (surveillance aesthetic); FVCKRENDER, Mugler (Y3K hyperfuturism); Toilet Paper Barcelona, Jackie Hu (collage/kidcore); Milosz Chudy (desktop-core interface nostalgia); UXTools.co (lo-fi pixel); Gladeye, Perplexity (dreamy grain softness)

#### Cursor-led experiences

The cursor as a designed character: morphing shapes, magnetic buttons, trails, flashlight/spotlight reveals, and hover-driven media scrubbing — making pointer position itself the primary interaction channel on desktop.

- **Why it catches attention:** The cursor is where attention already is; augmenting it creates continuous feedback with zero learning curve. Spotlight/reveal mechanics add curiosity ('what's hidden here'), and magnetic elements produce an almost tactile pull that makes hovering feel physical.
- **Implementation:** Custom cursor: hide native (cursor: none) + fixed div following pointer with lerp smoothing (0.1-0.2 factor) for weighted lag; morph size/blend-mode on data-attribute hover targets (mix-blend-mode: difference for invert effect); magnetic buttons via translating element toward cursor within a proximity radius (max 8-16px offset); flashlight via radial-gradient mask or clip-path following pointer; always keep native cursor on touch/keyboard and honor reduced-motion.
- **Seen at:** Moooi (flashlight cursor illuminating hidden detail), Rocket Air (draggable play-button cursor triggering video transitions), Lando Norris (cursor-reactive 3D), Lusion and Active Theory-style agency sites (fluid WebGL cursor trails), Netflix (cursor-reactive preview cards), countless Awwwards SOTD winners using mix-blend-mode invert dots + magnetic nav

#### Dopamine color and maximalism

Bold saturated palettes, neon gradients, clashing high-contrast pairings, dense layered compositions, and playful motion everywhere — the emotional-energy counterswing to a decade of muted minimal SaaS. Pinterest logged +215% 'eclectic maximalism' search growth for 2025.

- **Why it catches attention:** High saturation and hue contrast are pre-attentive stimuli — processed before conscious focus; 'dopamine design' explicitly optimizes for mood lift and shareability; density gives the eye endless secondary discoveries, extending session time.
- **Implementation:** Neon/electric palettes (high-chroma OKLCH colors hold saturation across displays); duotone or triadic clash pairings with black anchors; layered overlapping elements with mix-blend-mode: multiply/screen; animated gradient text (background-clip: text + moving gradient); marquee tickers, sticker clusters, oversized emoji/illustration; discipline: anchor chaos with one strong grid and generous type hierarchy so it energizes rather than exhausts.
- **Seen at:** Spotify (Wrapped and campaign microsites — the maximalist benchmark), Liquid Death (irreverent maximalism), Lush, Starface, Headspace (bright dopamine palettes), Gumroad (dopamine neobrutalism crossover), Planetono (all-in vibrant yellow), lifestyle/beauty/youth brands broadly per Figma's 2026 report

#### Interactive hero sections

Above-the-fold areas rebuilt as playable moments: cursor-reactive 3D or particle fields, draggable objects, generative shaders, mini-games, or type that responds to input — the hero as proof-of-craft rather than a static banner + CTA.

- **Why it catches attention:** First-impression novelty within the critical first 3-5 seconds; discovering the page responds to YOU converts a passive visitor into a participant, and interaction investment increases scroll-through and recall; it also demos the brand's technical/creative competence implicitly.
- **Implementation:** Pointer-driven WebGL shaders (fluid simulation, particle displacement responding to velocity); draggable physics elements via Matter.js or react-three/rapier; variable-font heroes where cursor proximity maps to wght/wdth per character; typed-text or generative headline sequences on load (staggered 40-80ms); keep hero payload <1.8s with lazy-loaded heavy assets and a designed static fallback for mobile/reduced-motion.
- **Seen at:** Bruno Simon (drive-a-car hero), Lusion (fluid shader heroes), Igloo Inc, Vercel and Linear (restrained animated-gradient + product-motion heroes), Van Holtz Co (interactive type hero), Eyesprint (warping type field), Nike product heroes with scroll/cursor-linked 3D, Silo and Jitter (Figma-cited motion heroes)

#### Page transition renaissance (View Transitions API)

Native browser-animated navigation: shared elements morph between pages (thumbnail grows into article header), content cross-fades, and app-like continuity arrives on ordinary multi-page sites — Baseline Newly Available since Firefox 144 (Oct 2025).

- **Why it catches attention:** Eliminates the white flash of navigation, preserving attentional continuity; shared-element morphs exploit object permanence — the brain tracks 'the same thing moving' instead of re-parsing a new page, which makes sites feel dramatically faster and more premium than they are.
- **Implementation:** Same-document: document.startViewTransition(updateDOM). Cross-document MPA: @view-transition { navigation: auto; } in CSS. Shared elements: matching view-transition-name on both pages (Chrome 137+ auto-names via match-element); style ::view-transition-old/new pseudo-elements with custom keyframes; view-transition-class for grouped list animations; Chrome 140+ nested groups enable clipped transitions; scoped element.startViewTransition() coming (Chrome 147); Astro/Nuxt/SvelteKit ship built-in support, React in canary. Keep transitions 250-400ms.
- **Seen at:** Astro sites (first framework to mainstream it), Chrome team demo galleries, Airbnb-style photo-to-detail morphs now replicated across e-commerce galleries and blogs; developers report replacing ~90% of dashboard animation JS with ~20 lines of CSS; RedBull-style editorial and portfolio sites adopting MPA transitions through 2025-26

#### Variable fonts in motion

Animating the axes of a single variable font file — weight, width, slant, optical size, custom axes — continuously in response to load, scroll, hover, cursor proximity, audio, or time, making type itself the motion medium.

- **Why it catches attention:** Text that physically flexes is uncanny in the best way (letters behave like matter, not print); per-character proximity effects create a ripple that tracks the user's own movement; unlike swapping fonts, interpolation is perfectly smooth — no jank, no reflow flash — so the effect reads as organic breathing.
- **Implementation:** CSS: @keyframes animating font-variation-settings ('wght' 100 to 900) or transition on hover — note font-variation-settings animates but forces layout, so isolate to display type; scroll mapping via animation-timeline: scroll() driving wght; cursor proximity: JS distance-per-character mapped to axis values (the classic 'bulge' effect, throttled with rAF); Google Fonts knowledge docs cover interactive axes; pair with font-display: swap and subset files (variable fonts stay ~50-150KB); avoid animating body text.
- **Seen at:** Google Fonts interactive specimen playgrounds (the reference demos), AVA SRG, Jitter and Sofi (weight-shifting product type), kinetic hero treatments cited across Envato/Figma 2026 trend reports, type foundry sites (Dinamo, OH no Type-style specimen toys) where axis-play IS the product demo, award-site heroes where headline weight breathes on scroll


### Numbers worth remembering

- Micro-interactions: 200-500ms overall sweet spot; 150-200ms for simple actions (hover/toggle); 300-400ms for complex transitions; 600ms-1s reserved for deliberately 'gentle' moments; anything over ~500ms starts to feel slow
- Ease-out for elements entering/responding to user input; ease-in-out for on-screen moves; avoid linear except for opacity/color; custom cubic-bezier spring-like curves (e.g. cubic-bezier(0.34, 1.56, 0.64, 1)) for playful brand moments
- Stagger intervals for bento/card reveals: 60-100ms per item feels choreographed without dragging
- CSS scroll-driven animations: shipped Chrome 115 (Aug 2023), now in all major engines; animation-timeline: scroll() / view() runs compositor-side, off main thread
- View Transitions API: Baseline Newly Available Oct 2025 (Firefox 144); Chrome 111+/137+, Edge, Safari 18/18.4+; ~85%+ browser coverage; Chrome 137+ adds view-transition-name: match-element auto-naming; Chrome 140+ nested transition groups; scoped element.startViewTransition() experimental in Chrome 140, targeting 147
- 3D adoption: 28% of Awwwards-featured sites use 3D elements, up from 12% in 2023; average 3D hero load time fell 4.2s to 1.8s
- Bento grid claims: +47% dwell time, +38% click-through vs conventional feature layouts
- Scrollytelling/interactive data viz: +62% average dwell time, +317% scroll depth (2025 study)
- Maximalism demand signals: Pinterest search spikes 'eclectic maximalism' +215%, 'vintage maximalism' +260% (2025 report)
- Grain/noise overlays: 0-60% film-grain opacity range on mesh gradients (typical premium look sits ~5-15%); SVG feTurbulence baseFrequency ~0.6-0.9 for fine grain
- AI authenticity backlash: 78% of global consumers say an AI-generated image cannot be considered authentic (Getty VisualGPS)
- Accessibility: 67% of practitioners rate accessibility overlay widgets poorly — motion trends must ship with prefers-reduced-motion fallbacks built in, not bolted on

<details>
<summary><strong>Sources</strong> (22)</summary>

- [Figma — Top Web Design Trends for 2026](https://www.figma.com/resource-library/web-design-trends/)
- [Envato Elements — Web design trends for 2026: kinetic type, broken grids and the return of visual personality](https://elements.envato.com/learn/web-design-trends)
- [Ioana Teleanu — Aesthetics in the AI era: Visual + web design trends for 2026 (Design Bootcamp)](https://medium.com/design-bootcamp/aesthetics-in-the-ai-era-visual-web-design-trends-for-2026-5a0f75a10e98)
- [Chrome for Developers — What's new in view transitions (2025 update)](https://developer.chrome.com/blog/view-transitions-in-2025)
- [web.dev — Same-document view transitions are now Baseline Newly available](https://web.dev/blog/same-document-view-transitions-are-now-baseline-newly-available)
- [Everyday UX — Glassmorphism in 2025: How Apple's Liquid Glass is reshaping interface design](https://www.everydayux.net/glassmorphism-apple-liquid-glass-interface-design/)
- [Setproduct — Glassmorphism vs neumorphism vs liquid glass (2026)](https://www.setproduct.com/blog/liquid-glass-vs-glassmorphism)
- [7K Ecosystem — 15 Web Design Trends Dominating 2025 (With Real Examples & Data)](https://7kc.me/blog/web-design-trends-2025)
- [Awwwards — Best Scroll Websites collection](https://www.awwwards.com/websites/scrolling/)
- [Shorthand — Scrollytelling examples: 12 scroll-driven stories](https://shorthand.com/the-craft/scrollytelling-examples/index.html)
- [Really Good Designs — 21 Scrollytelling Website Examples](https://reallygooddesigns.com/scrollytelling-website-examples/)
- [Downgraf — 20 Neobrutalism Web Design Examples That Break All the Rules](https://www.downgraf.com/inspiration/20-neobrutalism-web-design-examples-that-break-all-the-rules/)
- [LS Graphics — Brutalism on the Web: Examples That Work](https://www.ls.graphics/ideas/brutalism-on-the-web-examples-that-work)
- [Pravin Kumar — Bento Grids Are Quietly Winning B2B SaaS Homepages in 2026](https://www.pravinkumar.co/blog/bento-grids-b2b-saas-homepage-design-trend-2026)
- [HubSpot — How to use custom animated cursors to upgrade website UX](https://blog.hubspot.com/website/animated-cursor)
- [Justinmind — Best web micro-interaction examples and guidelines for 2025](https://www.justinmind.com/web-design/micro-interactions)
- [Google Fonts Knowledge — Interactive animations with variable fonts](https://fonts.google.com/knowledge/using_variable_fonts_on_the_web/interactive_animations_with_variable_fonts)
- [CSS-Tricks — Grainy Gradients](https://css-tricks.com/grainy-gradients/)
- [Creative Bloq — Texture, warmth and tactile rebellion: the big graphic design trends for 2026](https://www.creativebloq.com/design/graphic-design/texture-warmth-and-tactile-rebellion-the-big-graphic-design-trends-for-2026)
- [Getty Images VisualGPS — 2026: Human Craft or AI Potential?](https://www.gettyimages.com/visualgps/creative-trends/culture/2026-human-craft-or-ai-potential)
- [HTMLBurger — 14 Examples of Websites with Oversized Experimental Typography](https://htmlburger.com/blog/oversized-experimental-typography-examples/)
- [Wix Blog — The 11 Biggest Web Design Trends of 2026](https://www.wix.com/blog/web-design-trends)

</details>

---

## Signature patterns from award-winning sites

### Key insights

- Nearly every Awwwards-tier site is built on a smooth-scroll foundation first (Lenis is the current industry standard, replacing Locomotive Scroll) — it normalizes wheel/trackpad/touch input into one lerped rAF loop so every scroll-linked effect (parallax, pins, velocity skew) stays frame-synced with the DOM and WebGL; the signature effects layer on top of this base.
- There are really only two core 'engines' behind most signature patterns: cursor physics (lerp/linear interpolation toward a target at ~0.1-0.2 per frame) and scroll choreography (GSAP ScrollTrigger with scrub/pin). Magnetic buttons, custom cursors, trails, tilt cards are all cursor-lerp variants; pinned sections, horizontal galleries, image reveals, theme swaps, counters are all ScrollTrigger variants.
- The universal attention mechanism is the STAGGER: sequential onset (chars/words/lines/cards firing 30-80ms apart) exploits the eye's sensitivity to motion onset and creates a reading path. GSAP's stagger 'from' option (start/center/end/edges/random) is the single highest-leverage parameter for making a reveal feel designed rather than default.
- Velocity-reactivity is the current differentiator between good and award-level: marquees that speed up/reverse with scroll velocity, skew that increases with scroll speed (clamped ±5-10deg), cursor scale that responds to movement speed. Responding to input INTENSITY, not just position, makes the page feel physically alive.
- Masked/clipped reveals (overflow:hidden line wrappers, clip-path inset animations) read as 'premium' because motion happens inside a boundary — content appears to be printed/uncovered rather than moved. GSAP SplitText's mask:'lines' feature (free since GSAP 3.13) automates this and is now the default hero-text treatment on award sites.
- Strict performance discipline is what separates winners from janky imitations: animate ONLY transform and opacity (compositor-only properties), use will-change sparingly, drive everything from a single gsap.ticker/rAF loop (never setInterval), and gate entrance animations with IntersectionObserver + unobserve to fire once.
- The first-second impression stack is: preloader/counter intro → custom cursor appears → hero text mask-reveals in staggered lines → marquee or grain establishes texture. This sequence brands the site as 'crafted' before any content is read; agencies treat the preloader as unskippable brand theater and use it to hide asset loading (fonts, WebGL, image sequences).
- Apple's scroll-driven product pages are pre-rendered video split into image frames scrubbed on a canvas (the AirPods Pro page used 65 PNGs / 15.2MB; WebP conversion cuts that ~90% to 1.7MB) inside a tall sticky container — position:sticky + frame index mapped to scroll progress, with copy blocks fading in/out at keyframed progress points.
- Accessibility/restraint is judged: winners respect prefers-reduced-motion (swap movement for opacity fades), keep hover choreography under ~300ms so grids feel responsive, and follow the Vercel/Linear school of restraint — one grid texture, one glow accent, deep blacks and Inter-style geometric sans, rather than stacking every effect.
- Section-level color theme swaps mid-scroll (light hero → dark work section) are implemented by tweening CSS custom properties on <body> via ScrollTrigger onEnter/onLeaveBack (or scrubbed fromTo), which retints every element inheriting the variables at once — a cheap trick that reads as a dramatic 'chapter change'.

### Patterns

#### Staggered split-text hero reveal (mask/clip lines)

Hero headline is split into lines/words/chars, each wrapped in an overflow-hidden container; pieces slide up from below their mask with a per-piece stagger, so the headline appears to be printed into place line by line.

- **Why it catches attention:** Sequential motion onset is the strongest low-level attention trigger the eye has; the stagger creates a guided reading path across the headline, and the mask boundary makes text seem to emerge from the page itself — signaling craft within the first 500ms of the visit.
- **Implementation:** GSAP SplitText (free since 3.13) with type:'chars,lines' and mask:'lines' (auto-creates overflow:hidden wrappers); tween yPercent:100→0, opacity 0→1, duration 0.8-1.2s, ease expo.out/power4.out, stagger 0.05-0.08 per line; variants: blur(8px)→0, clip-path inset reveal, rotate-in from 5deg; re-split on resize; respect prefers-reduced-motion with fade-only fallback. CSS-only approximation: nested span with translateY inside overflow:hidden parent + animation-delay stagger.
- **Seen at:** Ubiquitous on Awwwards SOTD winners; Apple marketing pages, Obys Agency, Dennis Snellenberg's portfolio (SOTD), Studio Freight/Darkroom Engineering sites, Locomotive's own site

#### Infinite marquee / velocity-reactive ticker

A horizontal band of repeating text or logos loops endlessly; in the award-level version its speed and direction respond to scroll velocity — scrolling fast accelerates it, scrolling up reverses it, and it eases back to base speed at rest.

- **Why it catches attention:** Constant peripheral motion keeps the page feeling alive even when idle; the velocity coupling creates an uncanny 'the page feels my hand' sensation that rewards interaction and makes users scroll just to play with it.
- **Implementation:** Duplicate content 2x (or clone until 2x viewport width), animate xPercent 0→-50 with repeat:-1, ease:'none', base loop 15-30s; hook ScrollTrigger onUpdate to read self.getVelocity(), map it to timeScale (including negative for direction flip), then gsap.to the timeScale back to 1 over ~0.5-1s; add skewX proportional to velocity for extra energy; CSS-only fallback: @keyframes translateX(-50%) loop; pause when offscreen via ScrollTrigger/IntersectionObserver.
- **Seen at:** Studio Freight (now Darkroom Engineering), Lusion, countless agency/fashion SOTD winners; the pattern is a staple of Webflow cloneables and GSAP community showcases

#### Magnetic buttons

Buttons and nav links physically pull toward the cursor when it enters a proximity radius, translating (and slightly rotating) toward the pointer, then springing back with elastic overshoot on exit; text inside often moves at a different rate than the pill for parallax.

- **Why it catches attention:** Violates the learned expectation that UI is static — the element appears to 'want' to be clicked, creating implied agency; the elastic snap-back delivers a tiny dopamine reward that makes users hover repeatedly.
- **Implementation:** On pointermove within the element's bounding box (or an expanded ~100px radius), compute offset from element center, apply translate at 0.3-0.5x the offset (inner label at ~0.2x for parallax); lerp position each frame at factor ~0.15 for weight; on pointerleave, gsap.to back to 0 with ease:'elastic.out(1, 0.3)', duration ~1s; keep the actual hit target stationary for accessibility — move only the visual layer.
- **Seen at:** Cuberto (canonical example), Dennis Snellenberg portfolio, Aristide Benoist, most Awwwards agency sites of 2020-2026

#### Custom cursor + cursor trail / follower

Native cursor is replaced or accompanied by a lerped follower element (dot + lagging ring, or a blob) that smoothly trails the pointer, morphs over interactive elements (scales up, shows 'View'/'Drag' labels, inverts via blend mode), and may leave image or particle trails.

- **Why it catches attention:** Instantly reframes the site as an 'experience' rather than a document — the lag/inertia gives the pointer perceived mass, and context morphing (cursor becomes a 'Play' badge over video) previews affordances before click, keeping attention on the pointer's dance.
- **Implementation:** Fixed-position div, translate3d updated in a rAF loop that lerps toward pointer coords at 0.1-0.2/frame; mix-blend-mode:difference for the inversion look; scale/label changes driven by data attributes (data-cursor='view') on hover targets; image trails: spawn absolutely-positioned imgs at pointer positions, animate scale/opacity out, cap pool size; hide via cursor:none only when the replacement is fully functional; disable entirely on touch devices and honor prefers-reduced-motion.
- **Seen at:** Cuberto's noisy blob cursor (Paper.js + simplex noise), Pangram Pangram foundry, Locomotive, most SOTD portfolio sites; Codrops has the canonical tutorials

#### Hover choreography on card grids (spotlight borders, image zoom, content slide)

Cards in a grid respond to hover with layered, sequenced motion: a radial 'flashlight' gradient follows the cursor across the border/surface (Stripe-style), the internal image scales 1.05-1.1x while the frame stays fixed, hidden CTA/text slides up, and sibling cards may dim or blur.

- **Why it catches attention:** Multi-part choreography (border glow + zoom + content slide firing 50-100ms apart) reads as depth and quality; the cursor-tracked spotlight creates a lighting metaphor that makes the whole grid feel like one continuous reactive material, and sibling de-emphasis spotlights the user's focus.
- **Implementation:** Spotlight: on pointermove set --mouse-x/--mouse-y CSS vars per card; ::before with radial-gradient(400px at var(--mouse-x) var(--mouse-y), glowColor, transparent) — for border-only glow, put gradient on a parent layer masked to a 1px ring (padding trick or mask-composite). Image zoom: img inside overflow:hidden wrapper, transform:scale(1.06), 400-600ms custom cubic-bezier. Content slide: translateY(100%)→0 on a gradient-scrimmed panel. Sibling dimming: grid:hover .card:not(:hover){opacity:.5; filter:blur(2px)}. All transform/opacity only.
- **Seen at:** Stripe's flashlight border cards (the pattern's namesake), Linear's feature grid, Vercel dashboard marketing, Cruip/Hyperplexed tutorials replicating them

#### Sticky/pinned scroll sections + horizontal scroll galleries

A section pins to the viewport while scroll progress drives an internal animation — most famously a horizontal gallery: vertical scrolling translates a row of panels sideways until the sequence completes, then the page releases and continues vertically.

- **Why it catches attention:** Breaking scroll direction is a pattern interrupt — the user's model of the page is violated in a controlled way, forcing re-engagement; pinning converts scroll distance into narrative time, giving the designer film-like control over pacing (scrollytelling).
- **Implementation:** GSAP ScrollTrigger: pin:true, scrub:1 (1s catch-up smoothing), end:'+=' + container scrollWidth; tween x to -(scrollWidth - viewport); nest child parallax with containerAnimation so items animate relative to the horizontal tween; add snap:1/(panels-1) for panel snapping; CSS-native alternative: position:sticky inside a tall (300-500vh) parent with scroll-driven animation-timeline:view(). Keep pinned content transform-only; provide keyboard/reduced-motion fallback to plain vertical layout.
- **Seen at:** Apple product pages (pinned feature explainers), virtually every Awwwards portfolio 'selected work' gallery, Locomotive Scroll showcases, Web Bae/GSAP demo canon

#### Parallax depth layers

Foreground, midground and background elements translate at different rates during scroll (or in response to pointer position), producing an illusion of depth — from subtle hero-image drift to full multi-plane illustrated scenes.

- **Why it catches attention:** Motion parallax is a hardwired depth cue — the brain reads differential speed as 3D space, making a flat page feel dimensional and immersive; even 10-20% speed differences register subconsciously as 'expensive'.
- **Implementation:** ScrollTrigger with scrub: tween yPercent at different magnitudes per layer (background -10, midground -25, foreground -50); data-speed attributes for generic systems; image parallax inside a clipped container (image 120% tall, translated through the crop); pointer parallax: layers translate by (pointer offset x depth factor), lerped; sync WebGL planes to DOM via Lenis' rAF; avoid parallax on text for readability, clamp on mobile.
- **Seen at:** Firewatch game site (the classic), Apple AirPods/iPhone pages, ChungiYoo and countless SOTD portfolios, Vercel/Linear use micro-parallax on hero assets

#### Image/video reveals on scroll (clip-path + scale)

Media enters the viewport by un-masking: a clip-path inset (or overflow crop) animates open — bottom-up, curtain-split, or circle — while the image inside simultaneously scales down from ~1.2x to 1x, creating a 'develop before your eyes' effect.

- **Why it catches attention:** The counter-motion (mask expanding while image contracts) creates internal tension that reads as cinematic; content appearing through a boundary implies the page has physical layers, and each reveal is a small payoff that keeps users scrolling for the next one.
- **Implementation:** Wrapper: clip-path: inset(100% 0 0 0) → inset(0), 1-1.4s, power4.out/expo.out, triggered by ScrollTrigger or IntersectionObserver at ~20% visibility; inner img: scale(1.2-1.3) → 1 over the same duration (the parallax-crop combo); alternates: circle() clip expansion, curtain divs sliding away, greyscale→color filter, scrubbed clip tied to scroll progress; for video, autoplay muted on reveal; transform/clip-path both compositor-friendly.
- **Seen at:** Obys Agency, most photography/architecture SOTD winners, Codrops 'image reveal' demo series, Apple editorial pages

#### 3D tilt cards with glare

Cards rotate in 3D toward/away from the cursor (rotateX/rotateY mapped from pointer position relative to card center) with a moving radial-gradient highlight simulating light glancing off a glossy surface; extreme versions add holographic foil gradients.

- **Why it catches attention:** Coherent tilt + moving specular highlight triggers the perception of a physical object obeying real lighting — the brain flags 'this is a thing, not a picture'; the effect invites playful exploration of every card.
- **Implementation:** Parent perspective: 800-1000px; on pointermove compute normalized offset from center, map to rotateY (horizontal) and rotateX (inverted vertical) clamped ±10-20deg, plus scale(1.02-1.05); glare = absolutely-positioned radial-gradient (or linear-gradient sheen) whose position/opacity tracks the same offset, max opacity 0.3-0.5; lerp rotations or use transition 100-200ms to avoid jitter; reset with elastic ease on leave; vanilla-tilt.js packages the whole thing (data-tilt-max, glare:true, data-tilt-max-glare); transform-style:preserve-3d + translateZ on children for internal parallax.
- **Seen at:** vanilla-tilt.js showcase, Linear's early feature cards, Pokémon-card CSS demos that went viral (holographic effect), Framer template marketplace cards

#### Preloader / intro counter sequence

Before the page shows, a branded loading sequence plays: typically a 0→100 percentage counter (real or simulated), logo animation, or word cycle, ending with a choreographed exit (curtain wipe, mask lift) that hands off directly into the hero text reveal.

- **Why it catches attention:** It establishes theater and brand voice before any content, converts unavoidable wait into anticipation (counters exploit the goal-gradient effect — visible progress toward 100 is inherently compelling), and masks font/WebGL/image loading so the hero lands fully assembled.
- **Implementation:** Fixed overlay + GSAP timeline; counter tweens an object value 0→100 with power2-4.inOut, rendered via textContent snapping (real progress from asset promises or simulated with weighted increments); exit: overlay yPercent:-100 or clip-path wipe (0.6-1s, expo.inOut) with the hero reveal timeline chained via position parameter; total budget 1.5-3s; sessionStorage flag to skip on repeat visits; document resources percentage can come from performance API/asset counting.
- **Seen at:** Active Theory, Lusion, Dennis Snellenberg's '0-100' counter (heavily cloned), most agency SOTD entries; Codrops 'Page Preloading Effect' is the canonical tutorial

#### Number counters / stat reveals

Key metrics ('$4B processed', '99.99% uptime') animate from 0 to their final value when scrolled into view, usually with fast-start/slow-settle easing, paired with a staggered fade-up of the stat cards.

- **Why it catches attention:** A number in motion is read twice — once as movement, once as value — doubling dwell time on the claim; the deceleration into the final figure gives it landing weight, making the metric feel earned and bigger than static text would.
- **Implementation:** IntersectionObserver at ~50% visibility → requestAnimationFrame loop (or gsap.to on an object with snap:{textContent:1}), duration ~2s, easeOutExpo; format with toLocaleString for thousands separators; unobserve after firing to prevent replays; reserve width with tabular-nums (font-variant-numeric) to prevent layout shift; suffix animation (+, %, K/M) fades in at the end; reduced-motion: render final value immediately.
- **Seen at:** Stripe, SaaS landing pages universally, agency 'by the numbers' sections; CounterUp.js and GSAP snap plugins are the common implementations

#### Mid-scroll section theme swaps (color transitions)

The entire page's background/text palette crossfades as the user crosses section boundaries — e.g., white hero into near-black work showcase — with every inherited element retinting simultaneously.

- **Why it catches attention:** A full-viewport color change is the largest possible visual delta a scroll can produce; it functions as a chapter break, resetting attention and re-framing the following content's mood (dark = premium work, light = approachable contact).
- **Implementation:** Define palette as CSS custom properties on body; ScrollTrigger per section with onEnter/onLeaveBack calling gsap.to('body', {'--bg': ..., '--fg': ..., duration:0.6-1}) for discrete swaps, or fromTo with scrub for a continuous blend tied to scroll position; ensure transition on color/background-color for non-GSAP fallback; check contrast in both themes; nav/logo/cursor read the same variables so everything swaps in lockstep.
- **Seen at:** Dennis Snellenberg portfolio, Locomotive, many SOTM winners; standard GSAP community recipe (dozens of forum threads/CodePens)

#### Noise/grain overlays

A faint animated film-grain texture sits fixed over the entire page (or over gradients/images), flickering at a stepped low frame rate, adding analog texture to otherwise flat digital surfaces.

- **Why it catches attention:** Grain kills the 'sterile vector' look — it adds micro-contrast the eye associates with film and print, makes gradients band-free, and its subtle flicker keeps the page alive at the threshold of perception without distracting from content.
- **Implementation:** Fixed full-viewport div, pointer-events:none, z-index above content; texture from a tiled 128-256px transparent noise PNG or inline SVG feTurbulence (fractalNoise, baseFrequency 0.65-0.9); opacity 0.03-0.08; mix-blend-mode: overlay or soft-light; animate with steps() keyframes jittering background-position at 8-12fps for film flicker (or keep static for cheapest render); oversize the layer (e.g., 300%) and translate it so no repaints occur.
- **Seen at:** Zajno, A24-adjacent film/culture sites, dark cinematic portfolio SOTDs, Resend and other dev-tool sites using grain over glow gradients

#### SVG line-drawing animations

Logos, illustrations, underlines, and diagram connectors draw themselves stroke-by-stroke, often synced to scroll progress or used as the preloader's centerpiece.

- **Why it catches attention:** Watching a line being drawn triggers anticipation of the completed shape (perceptual closure) — the viewer's brain participates in finishing the image; as a scroll-scrubbed element it literalizes progress through a narrative.
- **Implementation:** Set stroke-dasharray and stroke-dashoffset to the path's getTotalLength(), then animate dashoffset to 0 (CSS keyframes, transition, or scrubbed via ScrollTrigger); GSAP DrawSVG plugin wraps this with percentage syntax (drawSVG:'0% 100%') and handles multi-segment reveals; for handwriting on irregular strokes, animate a masking stroke over the artwork; text must be converted to outlined stroke paths; stagger multiple paths for diagram build-ups.
- **Seen at:** Logo intros on agency preloaders, CSS-Tricks' canonical polygon demo, product diagram build-ins on dev-tool sites, signature/handwriting reveals on personal portfolios

#### Text scramble / decode effects

Text resolves out of randomized glyphs — characters cycle through a charset (A-Z, 0-9, symbols) and lock into place one by one, like a cipher decrypting or terminal booting; used on hero words, nav hovers, and stat labels.

- **Why it catches attention:** Constant character churn is high-frequency motion the eye cannot ignore, and the progressive lock-in creates a micro-mystery (what will it say?) with a resolution payoff; it instantly codes the brand as technical/hacker/futuristic.
- **Implementation:** GSAP ScrambleTextPlugin (now free): scrambleText:{text, chars:'upperCase' or custom set, revealDelay:1, speed:0.2-1}, duration 1.5-3s; DIY: rAF/gsap.ticker loop assigning each character a random resolve-frame, rendering random glyphs from a charset until locked (never setInterval); combine with SplitText for per-character control; on nav links, run a short 300-500ms scramble on hover; keep an accessible aria-label with the real text and skip under prefers-reduced-motion.
- **Seen at:** Active Theory, terminal-aesthetic dev tools and web3/security sites, Vercel-adjacent hacker-styled launches; the pattern traces to the famous 'text scramble' CodePen and GSAP's plugin demos

#### Smooth-scroll foundation + velocity skew (the invisible pattern)

Not a visible effect itself but the substrate: Lenis (or GSAP ScrollSmoother) lerps scroll position in a rAF loop so all scroll-linked motion is butter-smooth; a common visible byproduct is velocity skew — content shearing 5-10deg while scrolling fast and settling when stopped.

- **Why it catches attention:** Smoothness itself reads as expense — judges and users consistently rate lerped scroll as 'premium'; velocity skew adds a physics fiction (content has inertia) that makes fast scrolling feel kinetic rather than jarring.
- **Implementation:** new Lenis() + lenis.raf loop wired into gsap.ticker, ScrollTrigger.update on lenis 'scroll' event; Lenis works via scrollTo (not transforms) so position:sticky, IntersectionObserver, and native APIs keep working — its key advantage over Locomotive Scroll; skew: onUpdate read velocity, map to skewY clamped ±5-10deg with a proxy 'clamp and decay' pattern, tween back to 0 with power3 when velocity drops.
- **Seen at:** Darkroom Engineering's Lenis powers a large share of recent SOTD/SOTY sites; velocity-skew galleries popularized by Locomotive and Studio Freight showcases


### Numbers worth remembering

- Cursor-follow lerp factor: 0.15 per frame is the tested sweet spot (0.1 = heavier lag/fluidity, 0.2+ = snappier); magnetic buttons translate toward cursor at 0.3-0.5x the offset within a ~100px activation radius, returning with elastic.out(1, 0.3) ease on leave
- Hero text reveal: yPercent:100 → 0 with opacity 0 → 1, duration 0.8-1.2s, ease expo.out or power4.out, stagger 0.05-0.08s per line (0.02-0.03s per char); mask:'lines' on SplitText for clip effect
- Micro-interactions (hover states, button feedback): 150-300ms; entrance/reveal animations: 600-1200ms; preloader sequences: 1.5-3s max before it reads as artificial delay
- 3D tilt cards: perspective 800-1000px (lower = more extreme), rotation clamp ±10deg subtle / ±20deg dramatic, glare via radial-gradient overlay at max-opacity 0.3-0.5, scale 1.02-1.05 on hover
- Velocity skew: skewX/skewY proportional to scroll velocity, clamped to ±5-10deg, lerped back to 0 when scrolling stops; GSAP ScrollTrigger getVelocity() divided by ~300-500 as multiplier
- Infinite marquee: duplicate content 2x, animate xPercent 0 → -50 with repeat:-1 linear ease; base duration 15-30s per loop; scroll-velocity multiplier changes timeScale (and sign for direction reversal), tweened back to 1 over ~0.5-1s
- ScrambleText decode: tween duration 1.5-3s, revealDelay ~1s (scrambled chars fully visible before resolving), speed 0.2-1 for character refresh rate, charset 'ABCDEF...0123456789!@#$%' or 'upperCase'
- Apple-style canvas image sequence: 60-150 frames, scroll container 300-500vh tall with sticky canvas; AirPods Pro reference: 65 PNGs = 15.2MB, WebP = 1.7MB (~90% smaller); preload frames before enabling scrub
- Number counters: duration ~2s with easeOutExpo (fast start, slow settle — reads as 'big number'), triggered by IntersectionObserver at ~50% visibility, unobserve after firing; use requestAnimationFrame, never setInterval
- Grain/noise overlay: fixed full-viewport layer, opacity 0.03-0.08, mix-blend-mode: overlay or soft-light, tiled 128-256px noise PNG or SVG feTurbulence (baseFrequency ~0.65-0.9), optionally stepped 8-12fps position jitter for film-grain flicker; pointer-events:none
- Vercel-style blueprint grid: line/dot grid via repeating linear-gradient or radial-gradient at 16px or 24px background-size, 5-10% opacity; accent glows in blue/purple/magenta on near-black
- SplitText/perf budget: animate only opacity + transform; spotlight card border: radial-gradient ~200-400px radius following cursor via CSS vars (--mouse-x/--mouse-y) updated on pointermove, masked to a 1px border ring

<details>
<summary><strong>Sources</strong> (27)</summary>

- [Awwwards - Best GSAP Animation Websites](https://www.awwwards.com/websites/gsap/)
- [5 features to make your website look like an AWWWARDS winner (Bogdan Bendziukov, Medium)](https://medium.com/@bogdanfromkyiv/5-features-to-make-your-website-look-like-an-awwwards-winner-e34ddd2af352)
- [7 Must-Know GSAP Animation Tips for Creative Developers (Codrops)](https://tympanus.net/codrops/2025/09/03/7-must-know-gsap-animation-tips-for-creative-developers/)
- [GSAP SplitText Docs](https://gsap.com/docs/v3/Plugins/SplitText/)
- [GSAP Text Animation: A Practical SplitText Guide](https://lab.good-fella.com/blog/gsap-text-animation-splittext-guide)
- [Building a Magnetic Cursor Effect That Actually Feels Good (100 Days of Craft)](https://www.100daysofcraft.com/blog/motion-interactions/building-a-magnetic-cursor-effect)
- [How to Make a Custom Cursor with Lerp that Follows Pointer](https://medium.com/javascript-in-plain-english/how-to-make-a-custom-cursor-with-lerp-that-follows-pointer-6aa6f92fe48a)
- [Custom Cursor Effects (Codrops)](https://tympanus.net/codrops/2019/01/31/custom-cursor-effects/)
- [How to build horizontal marquee effects with GSAP (Envato Tuts+)](https://webdesign.tutsplus.com/how-to-build-horizontal-marquee-effects-with-gsap--cms-108794t)
- [Horizontal Scrolling Section with Parallax and Velocity Skew Effects (Web Bae)](https://www.webbae.net/posts/horizontal-scrolling-section-with-parallax-and-velocity-skew-effects)
- [How to Create a Spotlight Card Hover Effect with Tailwind CSS (Cruip)](https://cruip.com/how-to-create-a-spotlight-card-hover-effect-with-tailwind-css/)
- [33 CSS Card Hover Effects (CodeFronts)](https://codefronts.com/motion/css-card-hover-effects/)
- [Let's Make One of Those Fancy Scrolling Animations Used on Apple Product Pages (CSS-Tricks)](https://css-tricks.com/lets-make-one-of-those-fancy-scrolling-animations-used-on-apple-product-pages/)
- [Creating scroll animations similar to Apple's AirPods Pro page](https://ankittrehan2000.medium.com/creating-scroll-animations-similar-to-apples-airpods-pro-page-bc5c1c0814df)
- [Vercel aesthetic: complete guide to Blueprint Grid design (Setproduct)](https://www.setproduct.com/blog/complete-guide-to-blueprint-grid-design)
- [How To create the Stripe Website Gradient Effect (Bram.us)](https://www.bram.us/2021/10/13/how-to-create-the-stripe-website-gradient-effect/)
- [Vanilla-tilt.js](https://micku7zu.github.io/vanilla-tilt.js/)
- [Using CSS Perspective To Create a 3D Card Tilt Animation (Frontend.fyi)](https://www.frontend.fyi/tutorials/css-3d-perspective-animations)
- [45 Examples of Amazing Website Preloaders (HTMLBurger)](https://htmlburger.com/blog/website-preloaders/)
- [Page Preloading Effect (Codrops)](https://tympanus.net/codrops/2014/08/05/page-preloading-effect/)
- [GSAP ScrambleText Plugin Docs](https://gsap.com/docs/v3/Plugins/ScrambleTextPlugin/)
- [How SVG Line Animation Works (CSS-Tricks)](https://css-tricks.com/svg-line-animation-works/)
- [Lenis – Smooth Scroll (darkroom.engineering)](https://www.lenis.dev/)
- [Building Smooth Scroll in 2025 with Lenis (Edoardo Lunardi)](https://www.edoardolunardi.dev/blog/building-smooth-scroll-in-2025-with-lenis)
- [Animated JavaScript Counter-Up with the Intersection Observer API (getButterfly)](https://getbutterfly.com/animated-javascript-counter-up-with-the-intersection-observer-api/)
- [GSAP ScrollTrigger Docs](https://gsap.com/docs/v3/Plugins/ScrollTrigger/)
- [GSAP Community: Change background color while scrolling with ScrollTrigger](https://gsap.com/community/forums/topic/28409-change-the-background-color-while-scrolling-in-scrolltrigger/)

</details>

---

## Modern animation tech: native CSS/JS capabilities & libraries

### Key insights

- Scroll-driven animations (animation-timeline: scroll()/view()) are production-ready in Chromium (Chrome/Edge 115+, July 2023) and Safari (26.0, Sept 2025; moved to the compositor thread in 26.4), but Firefox still has them behind the layout.css.scroll-driven-animations.enabled flag as of Firefox 152 (mid-2026). They are a named Interop 2026 focus area, so Firefox is coming — until then ship them inside @supports (animation-timeline: view()) with an IntersectionObserver fallback (or no fallback: scroll effects degrade gracefully to static).
- Same-document View Transitions hit Baseline Newly Available on Oct 14, 2025 (Chrome/Edge 111+, Safari 18+, Firefox 144+) — safe to use everywhere with document.startViewTransition guarded by a feature check. Cross-document view transitions (@view-transition at-rule, zero JS) work in Chrome/Edge 126+ and Safari 18.2+ but NOT Firefox in early 2026; the fallback is just an instant navigation, so it's free progressive enhancement. 2025 additions: view-transition-name: match-element (no manual naming), nested transition groups (Chrome 140+), and ViewTransition.waitUntil()/document.activeViewTransition (Chrome 142).
- The pure-CSS entry/exit animation stack is now Baseline (Aug 2024, completed by Firefox 129): @starting-style defines the 'from' state for elements entering the DOM or leaving display:none, and transition-behavior: allow-discrete lets display/overlay participate in transitions so exit animations complete before the element disappears. This makes dialogs, popovers, and toasts fully animatable in CSS with zero JS — a task that previously required class-toggling + transitionend listeners or a library like AnimatePresence.
- @property (registered custom properties) became Baseline in July 2024 (Chrome 85 since 2020, Safari 16.4, Firefox 128) and unlocks animation of things CSS could never tween: gradient color stops and angles, numeric counters (via <integer> + counter-reset trick), conic-gradient progress rings. Because the browser knows the type, it interpolates the variable instead of snapping — animated gradients are now a pure-CSS one-liner.
- linear() easing shipped in all engines by Dec 2023 (~88-90% support by late 2025) and lets you encode spring/bounce curves as piecewise-linear points in pure CSS — use a generator (Kevin Grajeda's css-springs tool, Josh Comeau's guide) to bake a damped spring into a linear() string. Critical caveat: it's a pre-baked curve tied to a fixed duration, NOT interruptible physics — if the user reverses mid-animation it won't retarget with preserved velocity; for that you still need Motion's or GSAP's JS spring solvers.
- GSAP went 100% free (including commercial use) in April 2025 after Webflow's Oct 2024 acquisition — ALL formerly-paid Club plugins are now free: ScrollTrigger, ScrollSmoother, SplitText, MorphSVG, DrawSVG, Inertia, Flip. This removes the last licensing reason to avoid it; GSAP+ScrollTrigger remains the cross-browser answer for pinning, scrubbing with smoothing/lag, complex timeline orchestration, and anything Firefox users must see (since native scroll-driven CSS doesn't reach them yet).
- Framer Motion became the independent 'Motion' library (motion.dev) with vanilla JS, React, and Vue APIs. Its hybrid engine prefers hardware-accelerated WAAPI (off-main-thread transform/opacity) and falls back to a JS driver for springs and non-compositable values — the 'animate' mini export is ~2.5kb vs GSAP core ~23kb min+gzip. WAAPI itself (element.animate, composite: 'add', getAnimations, commitStyles) is long-Baseline and accepts ScrollTimeline/ViewTimeline objects where supported.
- Popover API (Baseline 2024) plus CSS Anchor Positioning (Chrome 125+, Safari core in 18.2/complete in 26, Firefox 147 — newly cross-browser in 2026, ~91% coverage) plus @starting-style = fully animated, light-dismissing, top-layer tooltips/menus/popovers with literally zero JavaScript, replacing Floating UI/Popper for simple cases. Safari 18.2-18.3 lacks @position-try auto-flipping, so keep a sane default placement.
- Performance rules unchanged but sharper in the INP era: only transform, opacity, filter, and (in Chromium) clip-path animate on the compositor; anything else (width, height, top, box-shadow, font-variation-settings) runs on the main thread and directly inflates INP (good < 200ms) by competing with input handling. Apply will-change ~200ms before an animation and remove it after — every promoted layer costs GPU memory. content-visibility: auto skips rendering offscreen subtrees, cutting rendering work on long animated pages. Frame budget: 16.7ms at 60Hz, ~8ms at 120Hz.
- Watch for animation-trigger (Chrome 145, 2026): scroll-TRIGGERED (fire a time-based animation once when crossing a viewport threshold) as opposed to scroll-DRIVEN (scrub progress with scroll). Until it lands cross-browser, the IntersectionObserver toggle-a-class pattern remains the universal 'reveal on scroll' workhorse, and Chromium's scrollsnapchange/scrollsnapchanging events (Chrome 129+) are the modern way to react to carousel snaps without observer hacks.

### Patterns

#### Pure-CSS scroll scrubbing (scroll-driven animations)

Bind animation progress to scroll position (animation-timeline: scroll()) or to an element's journey through the viewport (view()), with animation-range: entry/exit/cover to control the active window. Covers progress bars, parallax layers, image reveals, sticky-section scrubs, and horizontal-scroll galleries with zero JavaScript.

- **Why it catches attention:** Direct manipulation: motion is locked 1:1 to the user's scroll velocity, which feels tactile and responsive in a way time-based triggers don't; it also runs off the main thread so it never stutters while JS is busy.
- **Implementation:** animation-timeline: view(); animation-range: entry 0% cover 40%; animation-fill-mode: both; wrap in @supports (animation-timeline: view()) {}; JS equivalent via new ViewTimeline({subject, axis:'block'}) passed to element.animate(); Firefox fallback: IntersectionObserver adding a class, or just let it degrade to static.
- **Seen at:** Chrome's scroll-driven-animations.style demo gallery; Apple-style product pages recreated in pure CSS; progress-read indicators on blogs (bram.us, Josh Comeau's scroll-driven guide)

#### Page morphs with View Transitions (same-document and cross-document)

Snapshot old and new DOM states and auto-animate between them; shared elements tagged with view-transition-name morph position/size across the change. Works for SPA state changes (document.startViewTransition) and, in Chromium+Safari, plain MPA navigations via the @view-transition at-rule with no JS at all.

- **Why it catches attention:** Shared-element continuity — the thumbnail becoming the hero image — gives web apps the native-app feel; users perceive spatial continuity instead of a page 'blink'.
- **Implementation:** document.startViewTransition(() => updateDOM()); view-transition-name: hero-img (or match-element to auto-name); customize via ::view-transition-old(name)/::view-transition-new(name) keyframes; cross-doc: @view-transition { navigation: auto; } in both pages; performance tip (bram.us): animate the ::view-transition-group with transform instead of default width/height interpolation.
- **Seen at:** Airbnb-style gallery-to-detail morphs; Astro's built-in view transitions; Chrome's live demos (view-transitions.chrome.dev); React canary <ViewTransition> integration

#### Entry/exit choreography with @starting-style + allow-discrete

Animate elements IN from display:none (dialogs, popovers, toasts, dropdowns) and OUT again, entirely in CSS. @starting-style supplies the first-frame styles; transition-behavior: allow-discrete defers the display:none swap until the transition ends.

- **Why it catches attention:** Elements that fade/scale into place read as physical objects arriving, not content popping into existence; exit animations especially were previously impossible without JS cleanup timing.
- **Implementation:** dialog { opacity: 0; transition: opacity .3s, display .3s allow-discrete, overlay .3s allow-discrete; } dialog[open] { opacity: 1; @starting-style { opacity: 0; } }; include 'overlay' in the transition to keep top-layer elements painted during exit; combine with popover attribute for light-dismiss.
- **Seen at:** Chrome dev blog 'Four new CSS features for entry/exit animations' demos; Adam Argyle's nerdy.dev stage-effects demos; native-feeling <dialog> modals across modern design systems

#### Animated gradients and counters via @property

Register a custom property with a real type (<angle>, <color>, <percentage>, <integer>) so the browser interpolates it, then use the variable inside gradients, conic progress rings, or CSS counters to animate previously un-animatable values.

- **Why it catches attention:** Slow-rotating conic borders, shimmering gradient text, and odometer-style number count-ups produce constant subtle motion that reads as 'premium' — previously a canvas/JS trick, now declarative.
- **Implementation:** @property --angle { syntax: '<angle>'; inherits: false; initial-value: 0deg; } then animate --angle in @keyframes and use background: conic-gradient(from var(--angle), ...); counter trick: @property --n { syntax: '<integer>' } + counter-reset: n var(--n) + content: counter(n); note: gradient repaints are main-thread paint work, keep areas small.
- **Seen at:** Glowing animated card borders (Vercel/Linear-style conic borders); Temani Afif's animated-gradient demos; pure-CSS stat count-ups on landing pages

#### CSS springs and bounces with linear()

Approximate spring physics, bounce, and elastic easings as a series of linear() stops generated from a real physics simulation, applied to plain CSS transitions/animations.

- **Why it catches attention:** Overshoot-and-settle motion mimics real-world mass and damping; human perception reads spring curves as 'alive' vs. the sterile feel of ease-in-out, which cannot overshoot past 100%.
- **Implementation:** transition: transform 0.6s linear(0, 0.36, 0.66, 1.1, 1.03, 0.98, 1) — generate strings with Kevin Grajeda's css-springs generator or easingwizard.com; pair with transform-only properties for compositor execution; remember duration is fixed, so match the generator's suggested duration; for interruptible/retargetable springs use Motion's spring() or GSAP.
- **Seen at:** Josh Comeau's 'Springs and Bounces in Native CSS'; PQINA's spring-easing article; iOS-like toggle switches and menu pops in modern design systems

#### Zero-JS anchored overlays (popover + anchor positioning)

Tooltips, dropdown menus, and hover cards positioned relative to a trigger with anchor()/position-area, toggled via the popover attribute (top layer, light dismiss, Esc handling), animated with @starting-style.

- **Why it catches attention:** Overlays that spring from their trigger's exact location maintain spatial context; the top layer guarantees they're never clipped by overflow:hidden ancestors — a chronic JS-library pain point.
- **Implementation:** button { anchor-name: --btn; } [popover] { position-anchor: --btn; position-area: block-end; position-try-fallbacks: flip-block; margin: 8px 0 0; } plus @starting-style opacity/translate for entry; popovertarget attribute wires the trigger with no JS; feature-detect with @supports (anchor-name: --a).
- **Seen at:** Chrome's anchor-positioning-api blog demos; OddBird's anchor polyfill for older browsers; native HTML select/menu prototypes (Open UI)

#### IntersectionObserver reveal-on-scroll (the cross-browser workhorse)

Observe elements entering the viewport and toggle a class that runs a CSS transition/animation — still the only fully cross-browser way (Firefox included) to do scroll-triggered reveals in early 2026.

- **Why it catches attention:** Staggered fade-up reveals reward scrolling and create progressive disclosure rhythm; because the animation itself is CSS transform/opacity, it stays compositor-cheap.
- **Implementation:** new IntersectionObserver(entries => entries.forEach(e => e.target.classList.toggle('in-view', e.isIntersecting)), { threshold: 0.15, rootMargin: '0px 0px -10% 0px' }); unobserve after first fire for animate-once; stagger with transition-delay: calc(var(--i) * 60ms); respect prefers-reduced-motion; soon replaceable by animation-trigger (Chrome 145).
- **Seen at:** AOS.js-style landing pages; virtually every marketing site's fade-up sections; lazy media loading combined with reveal

#### FLIP layout animation (First-Last-Invert-Play)

Measure an element's position before and after a layout change, apply an inverting transform, then animate the transform back to zero — turning expensive layout animation into cheap compositor animation. GSAP's Flip plugin and Motion's layout prop industrialize it.

- **Why it catches attention:** Items smoothly gliding to new positions in a reordered grid or expanding card preserve object permanence — the eye tracks the element instead of losing it to a re-render.
- **Implementation:** const first = el.getBoundingClientRect(); mutate DOM; const last = el.getBoundingClientRect(); el.animate([{ transform: `translate(${first.x-last.x}px, ${first.y-last.y}px) scale(${first.width/last.width})` }, { transform: 'none' }], { duration: 300, easing: 'ease' }); or GSAP Flip.getState()/Flip.from(); or Motion <motion.div layout layoutId="card">; View Transitions API is the native successor for whole-view cases.
- **Seen at:** Paul Lewis's original Aerotwist article; GSAP Flip demos (grid-to-detail); Framer/Motion shared-layout card expansions; sortable lists and filtered galleries

#### GSAP ScrollTrigger orchestration (pin, scrub, smooth)

JS-driven scroll choreography beyond what native CSS can express: pinning sections while a timeline scrubs, scrub smoothing/lag (scrub: 1), snapping to labels, batching staggered reveals, and cross-browser consistency including Firefox.

- **Why it catches attention:** Pinned 'scrollytelling' sequences turn scrolling into a narrative controller — award-site territory — and scrub smoothing adds inertia that pure CSS scroll-driven animations cannot replicate.
- **Implementation:** gsap.timeline({ scrollTrigger: { trigger: '.section', start: 'top top', end: '+=200%', pin: true, scrub: 1, snap: 'labels' } }); ScrollTrigger.batch() for staggered entries; SplitText (now free) for per-char/line text reveals; combine with matchMedia() for responsive/reduced-motion variants; all plugins free since Apr 2025.
- **Seen at:** Most Awwwards/FWA-winning marketing sites; Webflow's GSAP-powered Interactions (default since summer 2025); Codrops free-GSAP-plugin demos

#### Interactive vector animation: Rive state machines vs Lottie

Designer-authored vector animation shipped as runtime files. Lottie = After Effects/Bodymovin JSON with a massive ecosystem; Rive = purpose-built editor + binary format with state machines, inputs, and data binding for logic-driven, input-responsive animation rendered via its own WebGL renderer.

- **Why it catches attention:** Characters and icons that respond to cursor position, form state, or live data (Rive's Apr 2025 data binding) blur the line between animation and UI; Duolingo-style reactive mascots create emotional engagement no keyframe loop can.
- **Implementation:** Lottie: <dotlottie-player> or lottie-web (prefer canvas renderer over default SVG for many concurrent animations — SVG renderer is CPU/main-thread); use .lottie format for 40-70% smaller files. Rive: @rive-app/canvas runtime, wire state machine inputs (booleans/numbers/triggers) to app events; files typically 10-15x smaller than Lottie JSON. Rule of thumb: Lottie for decorative add-on animation, Rive when interactivity is the product.
- **Seen at:** Duolingo's Rive-built characters; Figma and Notion Lottie micro-animations; the animated Rive marketing sites; LottieFiles marketplace

#### Kinetic typography via variable font axes

Animate font-variation-settings (wght, wdth, slnt, optical size, custom axes) with CSS transitions/keyframes for breathing headlines, hover weight-shifts, and per-letter wave effects.

- **Why it catches attention:** Type that changes weight or width mid-word is still rare enough to feel novel, and it animates the actual glyph outlines rather than faux-scaling — organic in a way transforms can't fake.
- **Implementation:** h1 { font-variation-settings: 'wght' 300; transition: font-variation-settings .4s; } h1:hover { font-variation-settings: 'wght' 800; }; per-letter waves via Splitting.js spans + animation-delay: calc(var(--i)*80ms); CAUTION: this re-rasterizes glyphs and triggers layout every frame (main-thread), so restrict to headlines/short strings and honor prefers-reduced-motion; text-wrap: balance/pretty improves the static typography but is not animatable.
- **Seen at:** abcdinamo.com typeface specimens; CSS-IRL variable font demos; v-fonts.com specimen playgrounds; award-site hero headlines

#### Compositor-only performance discipline (the INP contract)

A production constraint, not an effect: keep continuous animation on transform/opacity/filter via CSS or WAAPI so it runs on the compositor thread, isolate offscreen work with content-visibility, and keep JS animation ticks out of input-handling paths.

- **Why it catches attention:** Users perceive smoothness itself — a 60/120fps site 'feels expensive'; conversely a single main-thread-animated box-shadow can make every tap feel laggy by inflating INP past the 200ms 'good' threshold.
- **Implementation:** Animate only transform/opacity (+ filter, clip-path in Chromium); replace top/left tweens with translate; shadow 'animation' via pseudo-element opacity crossfade; will-change applied ~200ms pre-animation and removed after (each layer costs GPU memory); content-visibility: auto + contain-intrinsic-size on long pages; verify with DevTools Performance panel layer view and the Long Animation Frames (LoAF) API; scroll-driven CSS animations run threaded in Chrome and Safari 26.4+, unlike JS scroll listeners.
- **Seen at:** web.dev animations-guide recommendations; Motion Magazine's 'Web Animation Performance Tier List'; every Core Web Vitals remediation of scroll-jank


### Numbers worth remembering

- Scroll-driven animations: Chrome/Edge 115+ (Jul 2023), Safari 26.0 (Sep 2025), Safari 26.4 = threaded/compositor scroll-driven animations, Firefox behind flag through Firefox 152 (Jun 2026), Interop 2026 focus area
- Same-document View Transitions: Chrome 111+, Safari 18+, Firefox 144+ — Baseline Newly Available Oct 14, 2025; cross-document: Chrome/Edge 126+, Safari 18.2+, Firefox unsupported early 2026
- View transitions 2025 features: nested groups Chrome 140+, ViewTransition.waitUntil() + document.activeViewTransition Chrome 142, view-transition-name: match-element
- @property: Baseline Newly Available Jul 9, 2024 (Chrome 85 / Safari 16.4 / Firefox 128); Widely Available projected Jan 2027
- linear() easing: all engines since Dec 2023 (Chrome 113, Firefox 112, Safari 17.2); ~88% global support Oct 2025
- @starting-style + transition-behavior: allow-discrete: Baseline Aug 2024 (Firefox 129 completed the set)
- Anchor positioning: Chrome 125+, Safari 18.2 partial / 26 complete (@position-try needs 18.4+), Firefox 147 — Baseline 2026, ~91% traffic coverage; Popover API Baseline 2024
- Scroll snap events scrollsnapchange/scrollsnapchanging: Chrome 129+ only (early 2026); animation-trigger targeted at Chrome 145 (2026)
- GSAP free (all plugins incl. ScrollTrigger/SplitText/MorphSVG) since Apr 2025; Webflow acquired GreenSock Oct 15, 2024; GSAP core ~23kb min+gzip vs Motion 'animate' mini ~2.5kb
- Rive binary files typically 10-15x smaller than equivalent Lottie JSON; dotLottie (.lottie) zip format cuts Lottie sizes 40-70%; Rive data binding shipped Apr 2025
- Performance budgets: INP 'good' < 200ms; 16.7ms/frame at 60fps, ~8.3ms at 120Hz; apply will-change ~200ms before animation starts and remove after; compositor-safe properties = transform, opacity, filter, clip-path (Chromium)

<details>
<summary><strong>Sources</strong> (39)</summary>

- [CSS scroll-driven animations - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll-driven_animations)
- [Animate elements on scroll with scroll-driven animations - Chrome for Developers](https://developer.chrome.com/docs/css-ui/scroll-driven-animations)
- [CSS scroll-triggered animations are coming (animation-trigger, Chrome 145) - Chrome Blog](https://developer.chrome.com/blog/scroll-triggered-animations)
- [What's new in view transitions (2025 update) - Chrome Blog](https://developer.chrome.com/blog/view-transitions-in-2025)
- [Same-document view transitions are now Baseline Newly available - web.dev](https://web.dev/blog/same-document-view-transitions-are-now-baseline-newly-available)
- [View Transitions API (single-document) - Can I Use](https://caniuse.com/view-transitions)
- [@property: next-gen CSS variables now with universal browser support - web.dev](https://web.dev/blog/at-property-baseline)
- [@property - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@property)
- [Springs and Bounces in Native CSS (linear()) - Josh W. Comeau](https://www.joshwcomeau.com/animation/linear-timing-function/)
- [Create complex animation curves with the linear() easing function - Chrome for Developers](https://developer.chrome.com/docs/css-ui/css-linear-easing-function)
- [Now in Baseline: animating entry effects (@starting-style, allow-discrete) - web.dev](https://web.dev/blog/baseline-entry-animations)
- [Four new CSS features for smooth entry and exit animations - Chrome Blog](https://developer.chrome.com/blog/entry-exit-animations)
- [@starting-style - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@starting-style)
- [transition-behavior - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition-behavior)
- [WebKit Features in Safari 26.0 (scroll-driven animations ship)](https://webkit.org/blog/17333/webkit-features-in-safari-26-0/)
- [WebKit Features for Safari 26.4 (threaded scroll-driven animations)](https://webkit.org/blog/17862/webkit-features-for-safari-26-4/)
- [Launching Interop 2026 - Mozilla Hacks](https://hacks.mozilla.org/2026/02/launching-interop-2026/)
- [Announcing Interop 2026 - WebKit](https://webkit.org/blog/17818/announcing-interop-2026/)
- [Introducing the CSS anchor positioning API - Chrome Blog](https://developer.chrome.com/blog/anchor-positioning-api)
- [Anchor Positioning Updates for Fall 2025 - OddBird](https://www.oddbird.net/2025/10/13/anchor-position-area-update/)
- [Scroll Snap Events (scrollsnapchange/scrollsnapchanging) - Chrome Blog](https://developer.chrome.com/blog/scroll-snap-events)
- [Using scroll snap events - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll_snap/Using_scroll_snap_events)
- [ScrollTimeline - Web Animations API - MDN](https://developer.mozilla.org/en-US/docs/Web/API/ScrollTimeline)
- [ViewTimeline - Web Animations API - MDN](https://developer.mozilla.org/en-US/docs/Web/API/ViewTimeline)
- [GSAP Pricing (100% free including all plugins)](https://gsap.com/pricing/)
- [Webflow makes GSAP 100% free - Webflow Updates](https://webflow.com/updates/gsap-becomes-free)
- [From SplitText to MorphSVG: 5 demos using free GSAP plugins - Codrops](https://tympanus.net/codrops/2025/05/14/from-splittext-to-morphsvg-5-creative-demos-using-free-gsap-plugins/)
- [Framer Motion is now independent, introducing Motion - Motion Magazine](https://motion.dev/magazine/framer-motion-is-now-independent-introducing-motion)
- [Motion: JavaScript & React animation library](https://motion.dev/)
- [Rive as a Lottie alternative - Rive Blog](https://rive.app/blog/rive-as-a-lottie-alternative)
- [LottieFiles or Rive: which fits your needs - LottieFiles Blog](https://lottiefiles.com/blog/lottie-animations/lottiefiles-or-rive)
- [How to create high-performance CSS animations - web.dev](https://web.dev/articles/animations-guide)
- [Why are some animations slow? - web.dev](https://web.dev/articles/animations-overview)
- [will-change - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/will-change)
- [The Web Animation Performance Tier List - Motion Magazine](https://motion.dev/magazine/web-animation-performance-tier-list)
- [More performant ::view-transition-group animations - Bram.us](https://www.bram.us/2025/02/07/view-transitions-applied-more-performant-view-transition-group-animations/)
- [Animating Layouts with the FLIP Technique - CSS-Tricks](https://css-tricks.com/animating-layouts-with-the-flip-technique/)
- [FLIP Your Animations - Aerotwist (Paul Lewis)](https://aerotwist.com/blog/flip-your-animations/)
- [Variable Font Animation with CSS and Splitting JS - CSS IRL](https://css-irl.info/variable-font-animation-with-css-and-splitting-js/)

</details>

---

## Motion accessibility, performance & ethics

### Key insights

- prefers-reduced-motion means REDUCE, not eliminate: keep functional feedback (loading states, focus indication, state changes) and substitute risky motion with safe equivalents — replace slides/zooms/parallax with opacity crossfades, dissolves, and color changes. A global '* { animation: none !important }' nuke destroys essential feedback and can break JS that waits on animationend/transitionend; the standard workaround is 'animation-duration: 0.01ms' so events still fire imperceptibly.
- Architect motion as progressive enhancement: write base styles with NO motion, then add animation inside '@media (prefers-reduced-motion: no-preference)'. This is safer than overriding inside 'reduce' because unaudited new animations default to off for sensitive users. Users set the flag at OS level (Windows 11: Accessibility > Visual Effects > Animation Effects; macOS: Accessibility > Motion > Reduce motion; iOS: Accessibility > Motion; Android 9+: Remove animations), and it is detectable in JS via matchMedia('(prefers-reduced-motion: reduce)') with a change listener, and even in HTML via media attributes on <source> to serve a static image instead of an animated one.
- The legal floor for attention-grabbing motion is WCAG Level A, not AAA: SC 2.2.2 requires a pause/stop/hide mechanism for any moving/blinking/scrolling content that starts automatically, lasts more than 5 seconds, and sits alongside other content; SC 2.3.1 bans more than 3 flashes per second. An infinite attention loop (pulsing badge, auto-advancing carousel) with no persistent pause control is a compliance failure, not just an annoyance. Note: pausing only while hovered/focused does NOT count as a pause mechanism under 2.2.2.
- WCAG 2.3.3 (AAA) extends coverage to interaction-triggered motion: any motion animation triggered by interaction (parallax on scroll, zoom-on-click transitions, scroll-linked decorations) must be disableable unless essential. Honoring prefers-reduced-motion is the W3C's named sufficient technique. 'Motion animation' excludes pure opacity/color/blur changes — so crossfades are always a safe harbor.
- Vestibular trigger taxonomy (Val Head / A List Apart): RISKY = large-distance translation, parallax (foreground/background at different speeds — the single most-cited trigger), rapid scaling/zooming toward the viewer, spinning/vortex effects, scrolljacking, and any motion decoupled from the user's scroll direction or speed. SAFE = opacity fades, color changes, blurs, and small-scale movement. Symptoms are not trivial: nausea, migraines, and dizziness that can last hours from a single parallax exposure; ~35% of adults 40+ have experienced vestibular dysfunction.
- Performance: only transform, opacity, and (in modern browsers) filter and clip-path can animate on the compositor thread without re-running layout/paint. Animating width/height/top/left/margin triggers layout every frame; box-shadow/drop-shadow can fall back to single-threaded CPU rasterization and consume an entire core at 60fps. Move things with transform: translate(), never by animating position properties.
- Animation competes with input handling for the main thread and inflates INP (a Core Web Vital): long animation frames delay the next paint after a user interaction. Budget is 16.67ms/frame at 60fps (~10ms usable after ~6ms browser overhead); use the Long Animation Frames API (Chrome 123+) to find frames that block interactions, and never run JS-driven animation during interaction-heavy moments.
- Constant animation has an energy bill: WebKit explicitly tells developers to minimize continually animating content and warns that offscreen/invisible spinners still trigger paint and drain battery. Prefer declarative CSS animations (browsers can optimize them away when content is not visible, and throttle background tabs) over rAF loops; pause looping animation when it leaves the viewport (IntersectionObserver) and when document.visibilityState is hidden. iOS Low Power Mode disables video autoplay entirely — design for that fallback.
- will-change / layer promotion is a scalpel, not a default: every promoted layer costs GPU memory and CPU-GPU bandwidth; blanket promotion ('layer explosion') can blow GPU memory budgets and crash pages on mobile. Apply it just before an animation and remove it after, or reserve it for continuously animating elements.
- Autoplay etiquette: the only acceptable autoplay is muted (browsers enforce this — Chrome allows muted autoplay only; sound requires prior user interaction or high Media Engagement Index; iOS Safari requires muted + playsinline). Even muted autoplay should be short (5s is the accessibility ceiling before 2.2.2 controls are mandatory), pausable, and suppressed entirely under prefers-reduced-motion.
- Attention ethics: motion onset captures attention involuntarily — peripheral vision is evolutionarily tuned to movement, which is exactly why it must be rationed. CHI research classifies autoplay-next and infinite scroll as 'attention capture damaging patterns' (a forced-action dark pattern subcategory) with measured harms (overconsumption, disrupted sleep, goal misalignment). The line between legitimate salience and a dark pattern: legitimate motion serves the user's current goal, fires once at the moment of relevance, then RESTS; dark-pattern motion is perpetual, unprompted, and serves engagement metrics against user intent. NN/g: the more frequent an animation, the shorter and subtler it must be; slower, position-stable entrances for non-user-initiated content minimize attention theft.

### Patterns

#### Reduce, don't remove (crossfade substitution)

Under prefers-reduced-motion: reduce, substitute vestibular-risky motion with a calmer equivalent that preserves the communication: page slide -> crossfade, zoom-in modal -> fade, parallax -> static layers, spring bounce -> simple opacity. Keep essential feedback animations (spinners as subtle pulses, progress indication).

- **Why it catches attention:** Preserves the salience and meaning of the transition (something changed, here's the new content) through luminance/opacity change — which attracts attention via contrast change rather than retinal motion, avoiding the optic-flow signals that conflict with the inner ear and trigger vestibular symptoms.
- **Implementation:** Base styles motion-free; wrap animations in @media (prefers-reduced-motion: no-preference); provide a parallel @media (prefers-reduced-motion: reduce) block swapping transform keyframes for opacity keyframes; use a CSS custom property duration token (--motion-duration) that collapses to 0.01ms under reduce so animationend/transitionend still fire; JS: matchMedia('(prefers-reduced-motion: reduce)') + change listener to cancel Web Animations API animations mid-flight; HTML: <picture><source srcset='animated.gif' media='(prefers-reduced-motion: no-preference)'><img src='static.png'></picture>.
- **Seen at:** web.dev's own site, GitHub (disables its animated contribution effects), Apple.com product pages (serve static hero images under reduce), Slack and Discord in-app reduced motion toggles mirroring the OS flag

#### Pause/Stop/Hide control (WCAG 2.2.2)

Any auto-starting motion that runs longer than 5 seconds next to other content — carousels, tickers, background video, animated hero loops — must ship with a persistent, obvious control to pause, stop, or hide it. Hover-to-pause alone does not qualify.

- **Why it catches attention:** Continuous peripheral motion involuntarily and repeatedly recaptures gaze (motion is pre-attentively processed), which is exactly why people with ADHD and cognitive disabilities cannot read adjacent text while it runs — the control returns attentional agency to the user.
- **Implementation:** Add a visible pause/play button that toggles animation-play-state: paused / video.pause(); persist choice in localStorage; for carousels, stop auto-advance permanently after any user interaction; auto-pause when off-viewport via IntersectionObserver; treat prefers-reduced-motion: reduce as an implicit 'never auto-start'.
- **Seen at:** BBC and gov.uk homepage carousels with pause buttons, LinkedIn feed video pause control, Microsoft.com hero video pause toggle in the corner

#### Flash safety envelope (WCAG 2.3.1)

Nothing may flash more than 3 times per second unless below the general-flash and red-flash thresholds or smaller than 25% of a 10-degree visual field. Applies to strobe effects, rapid white/black cuts, glitch effects, and saturated-red pulsing.

- **Why it catches attention:** Flashing is the most aggressive salience mechanism available — high-frequency luminance oscillation drives strong synchronized responses in visual cortex — which is precisely what makes it seizure-inducing for photosensitive epilepsy and why it is effectively banned rather than rationed.
- **Implementation:** Keep any blink/pulse at <=3Hz; avoid opposing luminance swings >=10% of max relative luminance over large areas; never flash saturated red; keep unavoidable flashes under ~341x256px at typical viewing distance; validate video/animation with the free PEAT (Photosensitive Epilepsy Analysis Tool); prefer glow/scale micro-pulses under 3Hz over hard on/off blinking.
- **Seen at:** Video platforms' photosensitivity warnings (Netflix, game intros); the 1997 Pokemon broadcast incident is the canonical cautionary case; UK Ofcom/ITC broadcast flash limits use the same thresholds

#### Vestibular-safe motion vocabulary

Categorize every animation by trigger risk before shipping: risky (large translations, parallax, zoom/scale toward viewer, spin/vortex, scrolljacking, motion opposing scroll direction) vs safe (opacity, color, blur, small-distance movement). Reserve risky motion for opt-in moments and always gate it behind no-preference.

- **Why it catches attention:** Large-field coherent motion generates optic flow that the brain reads as self-motion; when the inner ear disagrees, the mismatch produces dizziness, nausea, and migraines — the bigger, faster, and more multi-directional the movement, the stronger both the attention capture and the physiological harm.
- **Implementation:** Audit checklist per Val Head: how large is the movement, how fast, does it conflict with scroll, does it fill the viewport? Replace parallax with position: static backgrounds under reduce; cap scale animations (e.g., 0.95->1.0 rather than 0->1); keep movement distances small (a few rem, not viewport-scale); never animate multiple axes/directions simultaneously in ambient UI.
- **Seen at:** Apple's iOS 7 zoom backlash (led to the OS Reduce Motion switch — crossfade app transitions replaced zooms), A List Apart case studies, Stripe.com gating its animated gradients behind reduced-motion checks

#### Compositor-only animation

Restrict all continuous or attention-grabbing animation to transform and opacity (plus filter/clip-path where support allows) so it runs on the compositor thread, skipping layout and paint entirely and staying smooth even when the main thread is busy.

- **Why it catches attention:** Smoothness itself is perceptual quality — dropped frames read as broken and draw the wrong kind of attention; compositor animations hold 60fps+ independent of main-thread jank, keeping deliberate motion crisp.
- **Implementation:** Move with transform: translate/scale/rotate, never top/left/width/height/margin; fade with opacity, never display or visibility keyframes alone; avoid animating box-shadow (crossfade two pre-rendered shadow layers' opacity instead); check property costs on csstriggers-style references; verify with Chrome DevTools Rendering > Paint Flashing and the Performance panel; use will-change sparingly — apply before animating, remove after, because each promoted layer costs GPU memory and layer explosion can crash mobile pages.
- **Seen at:** web.dev animations guide, Motion (motion.dev) performance tier list, GSAP and Framer Motion default to transform/opacity for this reason

#### Main-thread hygiene for INP (Long Animation Frames)

Keep animation work from blocking input: JS-driven animation, scroll handlers, and style recalculation that exceed the frame budget delay the paint after user interactions and directly degrade INP, a Core Web Vital that affects search ranking.

- **Why it catches attention:** A page that stutters when tapped feels untrustworthy; conversely, input that responds within 200ms sustains the perception of direct manipulation.
- **Implementation:** Target <=16.67ms per frame (~10ms of your own work); prefer CSS animations/transitions and compositor-driven Web Animations over rAF loops; observe PerformanceObserver({type:'long-animation-frame'}) (Chrome 123+) to find frames with high blockingDuration; break long JS into chunks with scheduler.yield()/setTimeout; never batch DOM reads and writes interleaved (layout thrashing) — read all, then write all, or use FastDOM-style scheduling.
- **Seen at:** Chrome's LoAF documentation, DebugBear and SpeedCurve INP case studies, nitropack LoAF-to-INP optimization guides

#### Power-conscious animation (animate only what's watched)

Continuous animation is a battery tax: run motion only while it is visible, in the foreground, and still relevant. Stop loops when offscreen, when the tab is hidden, and after they have delivered their message.

- **Why it catches attention:** N/A as attention-getter — this is the cost side: invisible spinners and perpetual background loops keep the CPU/GPU awake with zero communicative payoff, and on mobile the drain is user-perceptible (warm phone, dying battery), which actively damages brand perception.
- **Implementation:** Prefer declarative CSS animations — WebKit optimizes them away when content is not visible and suspends them in background tabs, unlike rAF loops; pause loops leaving the viewport with IntersectionObserver; stop updates on visibilitychange; remove 'display:none but still animating' spinners; consolidate timers to minimize CPU wake-ups; remember GPU compositing is fast but not free — sustained GPU effects also drain mobile batteries.
- **Seen at:** WebKit 'How Web Content Can Affect Power Usage' guidance, Safari Battery/Energy Impact panel in Web Inspector, Chrome's background-tab rAF suspension

#### Muted autoplay etiquette

If media must autoplay: muted, short, inline, pausable, and skipped entirely for reduced-motion users. Browsers already enforce the mute; accessibility and ethics require the rest.

- **Why it catches attention:** Autoplaying video is among the strongest attention magnets on a page — full-motion imagery in peripheral vision is nearly impossible to ignore — which is why unrequested autoplay reads as hostile and interferes with screen readers, cognitive accessibility, and metered connections.
- **Implementation:** autoplay muted playsinline attributes together for cross-browser mobile support; handle the play() promise rejection gracefully (iOS Low Power Mode blocks autoplay outright); keep loops under ~5s or provide 2.2.2 pause controls; gate autoplay on (prefers-reduced-motion: no-preference) via matchMedia before calling play(); lazy-load and use poster images; never autoplay with sound — Chrome requires prior user interaction or high Media Engagement Index anyway.
- **Seen at:** Chrome autoplay policy (developer.chrome.com/blog/autoplay), MDN Autoplay guide, Netflix homepage previews (with a global 'disable autoplay' setting after user backlash)

#### Animate once, then rest (legitimate salience vs attention-capture dark patterns)

The ethical boundary: motion is legitimate when it fires once, at the moment of relevance, in service of the user's current goal — then stops. It becomes a dark pattern when it is perpetual, unprompted, and engineered to farm engagement (autoplay-next, infinite scroll, endlessly pulsing badges). CHI research formally classifies these as 'attention capture damaging patterns' with measured harms: overconsumption, disrupted sleep, and choices misaligned with users' own goals.

- **Why it catches attention:** Motion onset triggers involuntary, pre-attentive orienting — an evolutionary threat-detection reflex users cannot opt out of. That involuntariness is what makes rationing it an ethical duty: repeated exploitation of the reflex is coercion of attention, not persuasion.
- **Implementation:** Play attention animations a finite number of times (animation-iteration-count: 3, not infinite); stop the moment the user acknowledges (hover/focus/click cancels the loop); scale intensity to genuine urgency — reserve strong motion for destructive/time-critical events; NN/g rules: the more frequent the animation, the subtler and shorter it must be; non-user-initiated content should enter slowly with little positional change; never move UI the user is about to click; require a user gesture before starting the next content unit (no autoplay-next by default); audit each motion with 'whose goal does this serve?'
- **Seen at:** Netflix autoplay-next backlash and the UChicago experimental study on it; YouTube's autoplay toggle; Time Well Spent / calm-technology critiques; TikTok/Instagram infinite scroll as the canonical ACDP; contrast: Slack's one-shot bounce on new-message vs. an endlessly shaking notification bell


### Numbers worth remembering

- Frame budget: 16.67ms per frame at 60fps; browser overhead consumes ~6ms, leaving ~10ms for your JS + rendering work per frame
- INP 'good' threshold: ≤200ms; Long Animation Frames API (Chrome 123+) reports frames whose blockingDuration delays input response
- WCAG 2.2.2 (Level A): pause/stop/hide control required for auto-starting moving/blinking/scrolling content lasting >5 seconds presented in parallel with other content; auto-updating content needs the control regardless of duration (no 5s exception)
- WCAG 2.3.1 (Level A): max 3 flashes in any 1-second period; general flash = pair of opposing luminance changes ≥10% of max relative luminance with darker state <0.80 relative luminance; red flash = any opposing transition involving saturated red; exemption if flash area <25% of any 10-degree visual field (0.006 steradians ≈ a 341x256px area at typical viewing distance); test with the free PEAT tool
- WCAG 2.3.3 (Level AAA): interaction-triggered motion animation must be disableable unless essential; opacity/color/blur changes are excluded from the definition of motion animation
- NN/g duration guidance: 100–500ms total range; ~100ms for small feedback (toggles, checkboxes); 200–300ms for modals/substantial changes; >500ms feels sluggish; exit animations 50–100ms shorter than entrances (e.g., 300ms in / 200–250ms out); ease-out for entrances; linear motion reads as unnatural
- prefers-reduced-motion browser support: Chrome 74+, Firefox 63+, Safari 10.1+, Edge 79+ — Baseline 'widely available' since January 2020
- Reduced-motion event-preservation hack: animation-duration: 0.01ms (not animation: none) so animationend still fires
- Vestibular prevalence: ~35% of adults 40+ have experienced vestibular dysfunction; up to 40% of people experience vertigo at least once; 4% of US adults report chronic balance problems, +1.1% chronic dizziness
- Autoplay video: must be muted + playsinline for mobile; sweet spot 5–12s with controls; iOS Low Power Mode blocks autoplay completely; Chrome allows unmuted autoplay only after user interaction with the domain or high Media Engagement Index
- Compositor-safe properties: transform, opacity (universally), filter and clip-path (modern browsers); everything else risks layout/paint per frame

<details>
<summary><strong>Sources</strong> (20)</summary>

- [prefers-reduced-motion: Sometimes less movement is more — web.dev](https://web.dev/articles/prefers-reduced-motion)
- [prefers-reduced-motion — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion)
- [Designing With Reduced Motion For Motion Sensitivities — Smashing Magazine](https://www.smashingmagazine.com/2020/09/design-reduced-motion-sensitivities/)
- [Designing Safer Web Animation For Motion Sensitivity (Val Head) — A List Apart](https://alistapart.com/article/designing-safer-web-animation-for-motion-sensitivity/)
- [Accessibility for Vestibular Disorders — A List Apart](https://alistapart.com/article/accessibility-for-vestibular/)
- [Understanding SC 2.2.2: Pause, Stop, Hide — W3C WAI](https://www.w3.org/WAI/WCAG21/Understanding/pause-stop-hide.html)
- [Understanding SC 2.3.1: Three Flashes or Below Threshold — W3C WAI](https://www.w3.org/WAI/WCAG22/Understanding/three-flashes-or-below-threshold.html)
- [Understanding SC 2.3.3: Animation from Interactions — W3C WAI](https://www.w3.org/WAI/WCAG21/Understanding/animation-from-interactions)
- [Web accessibility for seizures and physical reactions — MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Guides/Seizure_disorders)
- [How to create high-performance CSS animations — web.dev](https://web.dev/articles/animations-guide)
- [Stick to Compositor-Only Properties and Manage Layer Count — web.dev](https://web.dev/articles/stick-to-compositor-only-properties-and-manage-layer-count)
- [How Web Content Can Affect Power Usage — WebKit Blog](https://webkit.org/blog/8970/how-web-content-can-affect-power-usage/)
- [Long Animation Frames API — Chrome for Developers](https://developer.chrome.com/docs/web-platform/long-animation-frames)
- [Long animation frame timing — MDN](https://developer.mozilla.org/en-US/docs/Web/API/Performance_API/Long_animation_frame_timing)
- [Executing UX Animations: Duration and Motion Characteristics — Nielsen Norman Group](https://www.nngroup.com/articles/animation-duration/)
- [Animation for Attention and Comprehension — Nielsen Norman Group](https://www.nngroup.com/articles/animation-usability/)
- [Defining and Identifying Attention Capture Deceptive Designs in Digital Interfaces — CHI 2023](https://dl.acm.org/doi/full/10.1145/3544548.3580729)
- [An Experimental Study Of Netflix Use and the Effects of Autoplay on Watching Behaviors](https://arxiv.org/html/2412.16040v1)
- [Autoplay policy in Chrome — Chrome for Developers](https://developer.chrome.com/blog/autoplay)
- [Autoplay guide for media and Web Audio APIs — MDN](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay)

</details>

---
