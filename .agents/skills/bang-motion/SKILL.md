---
name: bang-motion
description: Build cinematic browser-based motion graphics (product promo videos, openers, animated intros, bumpers, kinetic typography, visual explainers) using HTML5, CSS3, GSAP (+ Three.js when required) that move like genuine cinema instead of presentation slides. Use this skill whenever the user asks for a "promo video", "opener", "motion graphic", "animated intro", "bumper", "kinetic typography", "cinematic text animation", or "explainer video", or whenever web animations feel too static like PowerPoint slides.
license: MIT
metadata:
  version: "1.19.0"
  author: "Bang Motion (Translated to English)"
---

# Bang Motion — Cinematic Web Motion Graphics (Not Slides)

This skill provides guidelines and recipes for creating motion graphics that move like real video rather than slide decks.

---

## Prerequisites — Browser Only

The primary deliverable (`index.html`) requires only a modern browser and internet access for CDN dependencies (GSAP, Google Fonts).
- **Never create a deliverable that requires `npm install` just to view.**
- The output file must be fully self-contained and run cleanly via direct file access (`file:///path/to/index.html`) or any standard web server.

### Player Behavior
- **Zero On-Screen UI by Default:** The motion graphic should start playing automatically on load and loop smoothly (`onComplete: () => tl.play(0)`).
- **Hidden Controls:**
  - `Space`: Pause / Resume
  - `R`: Restart from time 0
- **Scrubber / Review Mode:** URL query `?debug=1` enables an interactive scrub bar and timecode.
- **Clean Mode:** URL query `?clean=1` pauses playback on frame 0 for frame export or screenshot capture.

---

## The Structural Laws (Anti-PPT Rules)

Models naturally tend to fall into presentation-slide patterns. Before delivering any motion graphic, check your CODE against these rules:

1. **No Section-Toggling:**
   Do NOT build 3+ `<section>` or `.scene` blocks that simply fade in and out using `autoAlpha` or `opacity`. Scenes must transition because the **3D World / Camera moves** (push-through, pan, dolly, whip, tilt, dimensional rotations), not because slides are cross-fading.
2. **Visual Continuity (The Red Thread):**
   There must be an ongoing visual anchor — a continuous 3D subject, a living canvas, or an evolving document/product ecosystem.
3. **Maximum Two Levels of Text:**
   Never stack kicker + title + body paragraph + badge lists in a single scene. That immediately looks like a slide deck. Keep it to at most **one large punchy sentence + one contextual label/tag**.
4. **Sentence Case & No Auto Periods:**
   Use sentence case (weights 500–600). Titles and short claims default to **NO trailing periods**. Full stops are reserved only for intentional staccato pacing. Uppercase-bold is reserved for at most one high-impact scene.
5. **Alive Backgrounds (No Flat Solids):**
   Backgrounds must never be static or a flat single color. Use at least 2–3 composite layers:
   - Deep gradient base
   - Subtle geometric blueprint grid / texture / grain
   - Ambient moving light sweeps or drifting glow orbs
6. **Strictly Deterministic Timelines:**
   Never read system clocks (`Date.now()`) or unseeded `Math.random()` in render pathways. Every visual property must be a pure, scrubbable function of the GSAP timeline time `tl.time()`.

---

## Style Brief Before Code

Before scaffolding code, establish this brief:

1. **Theme & Product:** Three core adjectives characterizing the feeling (e.g., *Precision, Swift, Liberating*).
2. **Concept & Structural Fingerprint:**
   - Define the storytelling vehicle (e.g., *Surgical Identity Relay · 4 acts · 3D Floating Document · Opener: scan beam · Climax: verified stamp & logo*).
3. **Brand Palette:**
   - Primary brand color (from logo/identity)
   - Accent color (action/success/highlight)
   - High-contrast background (e.g. Deep Charcoal `#070a12`)
4. **Typography:**
   - Characterful Display font (e.g., *Plus Jakarta Sans, Cabinet Grotesk, Syne, Clash Display*)
   - Technical / Monospace font for data tags (*JetBrains Mono*)
5. **Background & Motion Language:**
   - Ambient lighting orbs, directional motion blur (`filter: blur(...)`) on text entries/exits, and smooth spring physics (`ease: "back.out(2)"`).
6. **Special Climax Moment:**
   - One standout sequence where visual transformation peaks (e.g., in-place live whiteout replacement).

---

## Canonical Scene Rundown (Opener / Intro: 15–30 Seconds)

Structure the timeline into clear narrative acts:

| Time | Act | Message | Visual Response |
|---|---|---|---|
| **0.0s – 4.0s** | **The Hook** | Core problem / bold premise | 3D subject floats in perspective; ambient lighting and HUD activate. |
| **4.0s – 8.0s** | **The Action** | The intelligent mechanism | Camera dollies in; scan beam slices through; target bounding boxes lock in. |
| **8.0s – 12.0s** | **The Climax** | Transformation / Result | Extreme close-up; live in-place modification; verified stamp hits canvas. |
| **12.0s – 16.0s** | **The Outro** | Brand logo & Call to Action | Subject transitions out; 3D logo enters with rim light; live URL pill glows. |

---

## Architecture Template

Every Bang Motion project should be scaffolded with:
- Stage container: `1920x1080` (16:9), auto-scaled responsively using CSS `transform: scale(...)`.
- Perspective viewport: `perspective: 1200px`.
- 3D World Rig: `#world` with `transform-style: preserve-3d`.
- GSAP Timeline: `const tl = gsap.timeline({ repeat: -1 })`.
- Debug UI: Activated via `?debug=1` with range slider and play/pause controls.
