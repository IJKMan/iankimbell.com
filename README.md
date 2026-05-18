# iankimbell.com

Personal site for Ian Kimbell — independent practice in storytelling,
presentation coaching, AI, and executive-ready messaging.

## Status

Early scaffold. The current working draft is `previews/v1.html` — a single
long-scroll consulting-funnel page. Copy is placeholder pending Ian's input.

## What's here

| Path | Purpose |
|------|---------|
| `previews/v1.html` | Current working draft |
| `assets/` | Drop the headshot here as `ian.jpg` |

## Stack

Plain HTML and CSS in one file. No build step, no framework.
Fonts loaded from Google Fonts (Fraunces + Inter Tight).

## Hosting

Cloudflare Pages, auto-deploying from this repo's `main` branch.
Every push to `main` triggers a redeploy.

Cloudflare project settings:

- Framework preset: **None**
- Build command: *(empty)*
- Build output directory: `/`
- Production branch: `main`

## Local preview

Open the file directly in a browser:

```
open previews/v1.html
```

## Path to ship

1. Ian's real copy swaps into the placeholder blocks
   (visually marked with a teal left bar in the draft)
2. Headshot lands in `assets/ian.jpg`
3. Promote `previews/v1.html` to `index.html` at repo root
4. Connect this repo in Cloudflare Pages (one-time setup)
5. Point `iankimbell.com` DNS at the Pages project
