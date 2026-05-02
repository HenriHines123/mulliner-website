# Mulliner Website

Marketing site for Mulliner — an Australian property/capital advisory firm — plus a construction-cost insights dashboard. Deployed to Netlify at mulliner.com.au.

## Stack

- **Pure static HTML.** No build step, no framework, no `package.json`. Each page is a single self-contained file with inline CSS and inline vanilla JS.
- **One Netlify Function** (`netlify/functions/refresh.js`) — a thin proxy from the browser to the Anthropic Messages API, used by the insights page's "Live refresh" button.

## Files

- `index.html` — marketing page. Hero video, three divisions (Property, Capital, COFORMA), about, contact prompt, footer.
- `insights.html` — construction-cost dashboard. Renders ~24 building-material rows from a `SEED` constant; the live-refresh button calls the Netlify function to fetch updated AUD prices via Claude with web search.
- `netlify/functions/refresh.js` — POST proxy to `api.anthropic.com/v1/messages`. Reads `ANTHROPIC_API_KEY` from Netlify env vars. CORS open to `*`.
- `netlify.toml` — function bundler (esbuild) and security headers (X-Frame-Options, X-Content-Type-Options).
- `Video_Project__1_.mp4`, `shutterstock_*.jpg` — hero video and section imagery.

## Conventions

- **Inline everything.** CSS lives in `<style>` inside each page; JS lives in `<script>` at the bottom. Don't add external CSS/JS files unless we deliberately decide to refactor.
- **Design tokens** are CSS custom properties on `:root` at the top of each page. Reuse them rather than hardcoding colors/spacing. Note `index.html` and `insights.html` define overlapping but slightly different tokens — keep them in sync if changing brand values.
- **Brand:** display font Cormorant Garamond, body Darker Grotesque, logo Fraunces. Accent color `#032B03` (deep green).
- **Nav** is duplicated across pages (it's a static site). Update both when changing nav.

## Local dev

- Static preview: `npx http-server -p 8080 -c-1` then open http://localhost:8080. Good for HTML/CSS/JS edits.
- Full preview including the refresh function: install Netlify CLI (`npm i -g netlify-cli`) and run `netlify dev`. Requires `ANTHROPIC_API_KEY` in env or `.env`.

## Deploy

Currently deployed by manual upload to Netlify. Goal is GitHub → Netlify auto-deploy (in progress).

## Notes for future edits

- The Netlify function hardcodes the Claude model id (`claude-sonnet-4-20250514`). If touching the refresh flow, consider updating to a newer Sonnet (e.g. `claude-sonnet-4-6`) — the API contract is the same.
- `ANTHROPIC_API_KEY` lives in Netlify site settings → Environment variables. Do not commit it.
- The video file is ~30MB. Fine for git, but be mindful before adding more large binaries — consider Git LFS if the asset count grows.
