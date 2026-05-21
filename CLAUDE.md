# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Aria** is a static, single-file music streaming platform UI (`index.html`) built with plain HTML, CSS, and JavaScript — no build toolchain, no package manager, no framework. Open `index.html` directly in a browser to run it.

## Architecture

Everything lives in `index.html`:
- **`<style>`** — all CSS using custom properties defined in `:root`
- **`<body>`** — five sections in order: floating nav pill → hero → artists constellation → releases carousel → charts grid → footer → sticky player
- **`<script>`** — player state machine (`playTrack`, `togglePlay`, `prevTrack`, `nextTrack`, `seekTrack`, `setVolume`, `toggleLike`) plus carousel scroll logic. No external JS dependencies.

Track data is a plain array at the top of the `<script>` block. To add tracks, append to that array and add a corresponding `.art-N` CSS class.

## Design System

All design decisions are governed by `Design.md` (Mastercard-inspired). Key rules for any edits:

**Colors — use CSS variables, never raw hex:**
| Variable | Value | Role |
|---|---|---|
| `--canvas-cream` | `#F3F0EE` | Page background — never use pure white |
| `--ink-black` | `#141413` | Primary text, CTAs, footer |
| `--light-signal-orange` | `#F37338` | Orbital arcs, eyebrow dots only |
| `--signal-orange` | `#CF4500` | Reserved for consent/legal actions only |
| `--white` | `#FFFFFF` | Nav pill, satellite CTAs, modal surfaces |
| `--slate-gray` | `#696969` | Secondary/muted text |
| `--dust-taupe` | `#D1CDC7` | Dividers, disabled states |

**Border-radius — only three zones, nothing in between:**
- Small: `≤ 6px` (micro chips)
- Medium: `20px` (buttons) / `40px` (hero frame, large containers)
- Full pill: `999px` / `50%` (nav, carousel cards, circular portraits)

**Typography:**
- Font: `Sofia Sans` (Google Fonts substitute for proprietary MarkForMC)
- Headlines: `font-weight: 500`, `letter-spacing: -1.28px` to `-0.48px` (−2%)
- Body: `font-weight: 450` (not 400 — preserves the soft editorial tone)
- Eyebrow labels only: `font-weight: 700`, `letter-spacing: 0.56px`, `text-transform: uppercase`

**Shadows — atmospheric only, never hard-edged:**
- Nav pill: `rgba(0,0,0,0.04) 0px 4px 24px`
- Cards: `rgba(0,0,0,0.08) 0px 24px 48px`

**Key motifs to preserve:**
- Circular artist portraits must use `border-radius: 50%` with a radial-gradient `::after` that fades the edge into `--canvas-cream`
- Each portrait gets a **satellite CTA**: a `52px` white circle button docked `bottom: 8px; right: -10px`
- Orbital arcs between portraits are inline SVG `<path>` elements in `--light-signal-orange` at `stroke-width: 1.5`, `opacity: 0.55`
- Ghost watermark: `font-size: 128px`, color `#E8E2DA`, `position: absolute` behind content, `pointer-events: none`
- Artist cards use staggered `margin-top` values per `:nth-child` to create the asymmetric constellation layout — do not put them on a uniform grid
