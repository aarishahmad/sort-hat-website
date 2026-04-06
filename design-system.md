# Design System

## Design Philosophy

SortHat's visual identity is built around one idea: **precision over warmth**. The product helps people put their best professional foot forward, so the design needed to feel sharp, credible, and confident — not soft or generic. The result is a dark editorial aesthetic with a single high-contrast accent color.

---

## Color Palette

All colors are defined as CSS custom properties in `:root` for consistency and easy theming.

| Variable | Value | Usage |
|---|---|---|
| `--bg` | `#0a0a0a` | Page background (near-black, not pure black) |
| `--bg2` | `#111111` | Alternating section backgrounds |
| `--bg3` | `#161616` | Input fields, card insets |
| `--card` | `#141414` | Card backgrounds |
| `--accent` | `#c8f135` | Primary accent — CTAs, highlights, icons |
| `--accent2` | `#f1c135` | Secondary accent — Global Scholar plan, warnings |
| `--white` | `#f5f0e8` | Body text (warm white, not pure #ffffff) |
| `--muted` | `#6b6b6b` | Secondary text, labels, placeholders |
| `--border` | `rgba(255,255,255,0.08)` | Borders and dividers |

**Why `#0a0a0a` not `#000000`?** Pure black makes borders and shadows invisible and can cause eye strain at high contrast. Near-black gives the same dark feel with better depth.

**Why warm white (`#f5f0e8`)?** Slightly warmer text on a cool dark background reads more comfortably for extended sessions and adds a subtle premium/editorial quality.

**Why acid green (`#c8f135`)?** Distinctive without being aggressive. It reads as modern and tech-forward, differentiates from generic blue-accent SaaS sites, and has enough contrast on dark backgrounds to pass WCAG AA for large text.

---

## Typography

Three font families, each with a distinct role:

| Font | Weight(s) | Role |
|---|---|---|
| **DM Serif Display** | Regular, Italic | Headlines, large numbers, logo — editorial serif for emotional weight |
| **Syne** | 400, 600, 700, 800 | Body text, paragraphs, descriptions — modern geometric sans |
| **Space Mono** | 400, 700 | Labels, tags, code-like UI, nav links, pricing notes — monospace for technical credibility |

**Type scale (via `clamp()`):**

- Hero headline: `clamp(2.8rem, 7vw, 6rem)` — fluid scaling between mobile and desktop
- Section titles: `clamp(2rem, 4.5vw, 3.5rem)`
- Body: `1rem` / `0.85rem` for secondary
- Labels/mono UI: `0.62rem`–`0.78rem`

---

## Spacing & Layout

- **Horizontal padding:** `5%` on all sections (scales with viewport)
- **Section padding:** `6rem 5%` (desktop) → `4rem 4%` (mobile)
- **Grid gaps:** `1.5px` for service grid (creates a "border" effect between cards), `1rem` for pricing
- **Border radius:** `2px` throughout — intentionally near-square for a precise, architectural feel

---

## Component Reference

### Buttons

Two variants:

```css
/* Primary — filled accent */
.btn-primary { background: var(--accent); color: #000; }

/* Outline — transparent with border */
.btn-outline { border: 1px solid var(--border); color: var(--white); }
```

Both share: `font-family: var(--font-mono)`, `border-radius: 2px`, `letter-spacing: 0.05em`, hover `translateY(-3px)` lift.

### Cards

Service cards and plan cards use `background: var(--card)` with `border: 1px solid var(--border)`. Hover states deepen the background and add a subtle `accent`-tinted gradient via `::before` pseudo-element with `opacity` transition.

### Section Labels

Small all-caps monospace labels above section headings, with a `::before` line accent:

```css
.section-label::before { content:''; display:block; width:28px; height:1px; background:var(--accent); }
```

### Tags / Badges

Small pill elements with `border: 1px solid rgba(200,241,53,0.25)` and `color: var(--accent)`. Used for service categories, plan tiers, and review plan indicators.

---

## Motion Principles

1. **Entrance animations** use `opacity: 0 → 1` + `translateY(30px → 0)` with a custom ease (`cubic-bezier(0.16, 1, 0.3, 1)`) — snappy in, smooth settle
2. **Staggered delays** on grouped elements (`.reveal-delay-1/2/3`) create a cascade effect without JS
3. **Hover states** use `transform: translateY(-3px)` for lift — subtle, not bouncy
4. **Marquee** (review strip) runs at `40s linear infinite` — slow enough to read, fast enough to feel alive
5. **Never animate layout properties** (`width`, `height`, `margin`) — only `transform` and `opacity`

---

## Background Textures

The hero section uses two layered backgrounds:

1. **Grid overlay** — CSS `background-image` with two linear gradients at 90° to create a subtle dot/grid pattern
2. **Glow blob** — A large `radial-gradient` positioned top-right, `rgba(200,241,53,0.06)` — barely visible but adds depth
