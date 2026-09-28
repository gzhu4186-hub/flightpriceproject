# FlightPrice Project

机票价格监控与旅行规划 / Flight price monitoring &amp; trip planning.

Static landing page for the project. Plain HTML + CSS + a few lines of vanilla JS —
no build step, no framework, no external assets, no CDN.

**Live site:** https://gzhu4186-hub.github.io/flightpriceproject/

## What the project is

A price-monitoring pipeline, not a fare-search page:

```
Discovery  →  Rank  →  Verify  →  Assess  →  Store
```

The central question is not "how cheap is it" but **"is this cheap fare real?"**
Every price is tagged as cached (`indicative`) or live (`live`), low-price candidates
are re-priced against an independent real-time source, and each result gets a
credibility verdict:

| Status | Meaning |
| --- | --- |
| `DISCOVERED` | Discovery price only, not yet verified |
| `VERIFIED` | A live source reproduced the price within the agreement threshold (≤5%) |
| `HIGH_CONFIDENCE` | Two or more *independent* live sources agree |
| `INVALID_OR_STALE` | Discovery price sits well below every live quote (>15% divergence) |

Only `VERIFIED` / `HIGH_CONFIDENCE` are ever allowed to raise an alert.

## Repository layout

```
index.html                       the landing page
assets/style.css                 all styling
.github/workflows/pages.yml      GitHub Pages deployment
.nojekyll                        serve files as-is (no Jekyll)
```

## Local preview

```bash
python -m http.server 8000
# then open http://localhost:8000
```

## Deployment

Pages is configured through **Settings → Pages → Build and deployment → Source: GitHub Actions**.
Every push to `main` triggers `.github/workflows/pages.yml`, which uploads the repository
root as the site artifact and deploys it. The workflow can also be run manually from the
Actions tab (`workflow_dispatch`).

## Scope

Phase 1 covers the backend, CLI, database and verification only. Not included:
web app, dashboard, scraping of consumer OTAs, browser automation, alert channels,
AI analysis, fare prediction, or automatic booking.
