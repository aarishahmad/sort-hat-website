# Architecture & Technical Decisions

## Overview

SortHat is a single-page application (SPA) built without any JavaScript framework. The entire product — HTML structure, CSS design system, and JS logic — lives in a single `index.html` file deployed to Vercel's edge network.

---

## Why No Framework?

The core question at the start of the project was: **does this site need React/Vue/Next?**

The answer was no, for three reasons:

1. **Interaction complexity is low.** The site has three "views" (landing page, order form, success state) with no shared state beyond the selected plan name and price. A full component tree would be over-engineering.
2. **Performance on mobile matters.** The target audience is Indian job seekers, many on mid-range Android devices on 4G. A vanilla HTML file loads in milliseconds; a React bundle with hydration adds a noticeable delay.
3. **No build pipeline means no CI complexity.** The repo deploys straight to Vercel with zero configuration — no webpack, no Babel, no package.json.

The trade-off: if the product grows to include a dashboard, order tracking, or user accounts, a framework migration would be necessary.

---

## SPA Routing Without a Router

Page switching is handled by toggling `display` properties and CSS classes on three full-page `<div>` containers:

- `#main-site` — the landing page
- `#order-section` / `#scholar-section` — two distinct order forms
- `#success-section` — post-submission confirmation

A shared `showMain()` function resets all views; `showFormPage(type)` activates the correct form. This pattern avoids URL changes intentionally — users who hit back should return to the pricing section, not get confused by a blank form URL.

---

## State Management

All shared state fits in a single object:

```js
let selectedPlan = { name: '', price: '', type: '' };
```

This is set when the user clicks a plan CTA and read when the form is submitted. No state library needed.

---

## Form Handling

Forms submit as JSON to Formspree via `fetch()`. The endpoint URL is stored as an environment variable (`FORMSPREE_ENDPOINT`) and must never be hardcoded in the source file. The JS reads it from `window.ENV_FORMSPREE_ENDPOINT`, which is injected by the hosting environment at deploy time.

The submission flow:
1. Validate required fields (name, age, phone, email) on the client
2. Disable submit button and show "Submitting..." text
3. `await fetch(endpoint, { method: 'POST', body: JSON.stringify(payload) })`
4. On success → `showSuccess()`
5. On error → `showToast('Submission failed. Please try again.')`
6. `finally` → re-enable button regardless of outcome

---

## Animation Strategy

All animations use CSS `@keyframes` and are triggered by one of two mechanisms:

- **On page load:** `animation` property with `animation-fill-mode: forwards` and staggered `animation-delay` values on hero elements
- **On scroll:** `IntersectionObserver` adds a `.visible` class to elements when they enter the viewport; CSS transitions on `opacity` and `transform` do the actual animation

All animated properties are `transform` and `opacity` — these are GPU-composited and do not trigger layout or paint recalculation, keeping animations at 60fps even on low-end devices.

The `IntersectionObserver` approach replaces the older `scroll` event listener pattern, which fires too frequently and causes jank.

---

## Responsive Strategy

Mobile-first breakpoints:

| Breakpoint | Changes |
|---|---|
| `≤ 768px` | Nav collapses to hamburger; 3-col service grid → 1-col; 2-col why-grid → 1-col; 4-col pricing → 1-col; process steps → 2-col |
| `≤ 480px` | Hero headline size reduced; hero actions stack vertically; process steps → 1-col |

The hamburger menu is a JS `classList.toggle('open')` — no CSS checkbox hack — keeping the code readable.

---

## Font Loading

Three Google Fonts are loaded via `preconnect` + a single stylesheet request:

```html
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800
      &family=DM+Serif+Display:ital@0;1
      &family=Space+Mono:wght@400;700
      &display=swap" rel="stylesheet"/>
```

`display=swap` ensures body text is visible in a fallback font while custom fonts load, avoiding invisible text during network delays (FOIT).

---

## Deployment Pipeline

```
Local edit → git push main → Vercel webhook → Build (none needed) → Deploy to CDN edge
```

Total deploy time from push to live: ~10–15 seconds. No build step means no build failures.
