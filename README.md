# SortHat 🎓

A landing page + order flow for my career writing startup — live at **[sorthat.in](https://www.sorthat.in)**

Built it to practice proper UI without relying on a framework. No React, no build step, just HTML/CSS/JS deployed straight to Vercel.

---

## Live Site

**[sorthat.in](https://www.sorthat.in)**

---

## Screenshots

![Hero](public/screenshots/hero.png)

![Services](public/screenshots/services.png)

![Pricing](public/screenshots/pricing.png)

![Process & Reviews](public/screenshots/process.png)

---

## Stack

- Vanilla HTML, CSS, JS — no framework, no build step
- Google Fonts (DM Serif Display + Syne + Space Mono)
- Formspree for form submissions (endpoint stored in env)
- Vercel for hosting + auto-deploy from GitHub

---

## What's in the codebase

Everything lives in `src/index.html` — it's a single-file SPA. The JS handles:

- Page switching between the landing page, two order forms, and a success screen
- Async form submission to Formspree with loading/error states
- `IntersectionObserver` for scroll-triggered entrance animations
- A CSS marquee for the review strip (duplicated nodes for seamless looping)
- Sticky nav that picks up `backdrop-filter: blur` after scroll

CSS handles most of the heavy lifting — all animations (`fadeUp`, `scan`, `barGrow`, `marquee`, `pulse`) are pure `@keyframes` using only `transform` and `opacity` so they stay on the GPU.

---

## Decisions worth noting

**No framework** — The site is a content-heavy landing page with minimal real interactivity. React would've added 100KB+ of JS for three view toggles. Wasn't worth it, especially for mobile users on Indian data speeds.

**No backend** — Form submissions go to Formspree. Gets the job done without spinning up a server. The endpoint is an env variable, not hardcoded.

**CSS variables for everything** — All colors, fonts, transitions are in `:root`. Makes theming changes a one-liner.

**Featured plan is Silver (₹499), not Platinum** — Intentional. Middle anchoring converts better than pushing the most expensive option.

---

## Running locally

No setup needed.

```bash
git clone https://github.com/YOUR_USERNAME/sorthat-website.git
cd sorthat-website

# copy env template
cp .env.example .env
# add your Formspree endpoint to .env

# just open the file
open src/index.html
# or serve it
npx serve src/
```

Forms won't submit without a valid `FORMSPREE_ENDPOINT` but everything else works fine offline.

---

## Deploying

Vercel + GitHub. Push to `main`, it deploys. Takes ~15 seconds since there's no build step.

Add `FORMSPREE_ENDPOINT` under Project Settings → Environment Variables and you're done.

See [`docs/deployment.md`](./docs/deployment.md) for the full setup.

---

## Env variables

```
FORMSPREE_ENDPOINT=https://formspree.io/f/YOUR_FORM_ID
```

See `.env.example`.

---

## Docs

- [`docs/architecture.md`](./docs/architecture.md) — why I built it this way
- [`docs/design-system.md`](./docs/design-system.md) — colors, fonts, component patterns
- [`docs/deployment.md`](./docs/deployment.md) — Vercel setup

---

*Business contact: [hello@sorthat.in](mailto:hello@sorthat.in)*
