# Deployment Guide

## Platform: Vercel

SortHat is deployed on [Vercel](https://vercel.com), which provides:
- Edge CDN distribution globally
- Automatic HTTPS/SSL
- Git-based CI/CD (push to deploy)
- Environment variable management

---

## Initial Setup

### 1. Connect the repo

1. Go to [vercel.com/new](https://vercel.com/new)
2. Import this GitHub repository
3. Vercel auto-detects it as a static site (no framework)
4. Set the **Root Directory** to `src/` (where `index.html` lives)
5. Leave Build Command and Output Directory blank

### 2. Configure environment variables

In **Project Settings → Environment Variables**, add:

| Name | Value |
|---|---|
| `FORMSPREE_ENDPOINT` | `https://formspree.io/f/YOUR_FORM_ID` |

Set it for **Production**, **Preview**, and **Development** environments.

### 3. Deploy

Click **Deploy**. Done. Every subsequent push to `main` auto-deploys.

---

## Custom Domain

1. Go to **Project Settings → Domains**
2. Add your domain (e.g. `sorthat.in`)
3. Update your DNS records as instructed by Vercel
4. SSL certificate is provisioned automatically

---

## Environment Variables in Production

Since this is a purely static HTML file (no server-side rendering), `process.env` variables are not available at runtime. The Formspree endpoint is handled differently:

The JS reads from `window.ENV_FORMSPREE_ENDPOINT`. For production, this value should be injected into the HTML at deploy time (e.g., via a Vercel Edge Function or a simple build script that replaces a placeholder).

For the current setup, the endpoint can also be set directly in the Vercel environment and a simple build step added if needed. See `docs/architecture.md` for the full form submission flow.

---

## Preview Deployments

Every pull request gets an automatic preview URL from Vercel (e.g. `sorthat-git-feature-xyz.vercel.app`). This is useful for testing changes before merging to `main`.

---

## Rollback

If a bad deploy goes out:
1. Vercel dashboard → **Deployments**
2. Find the last good deployment
3. Click **Promote to Production**

Rollback is instant — Vercel just switches which deployment the domain points to.
