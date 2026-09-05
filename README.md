# NWSL Analytics TRMNL Plugin

A TRMNL e-ink display plugin showing live NWSL standings, recent form, and xG performance powered by the free, no-auth [American Soccer Analysis](https://www.americansocceranalysis.com/) API.

## What it shows
- Full league standings (rank, W/D/L, GD, PTS, last-5 form)
- Recent match results
- xG Watch: the teams most over- and under-performing their expected goals
- Goals Added leaderboard

## How it works
A small Cloudflare Worker (`worker.js`) polls American Soccer Analysis, computes standings/form/xG splits, and returns in JSON. TRMNL polls that worker on a schedule and renders it through the Liquid markup files using TRMNL's Framework v3.3 components.

```
ASA API → Cloudflare Worker (worker.js) → TRMNL polling → Liquid markup → e-ink display
```

## Setup

1. **Deploy the worker**
   Create a Cloudflare Worker, paste in `worker.js`, and deploy it. 

2. **Create the TRMNL plugin**
   In TRMNL, add a new Private Plugin and set:
   - Strategy: `Polling`
   - Polling URL: your worker's URL
   - Polling Verb: `GET`
   - Refresh interval: `60` minutes

3. **Paste in the markup**
   Copy each `markup-*-framework.liquid` file into its matching layout tab:
   - `markup-full-framework.liquid` → Full
   - `markup-half-horizontal-framework.liquid` → Half Horizontal
   - `markup-half-vertical-framework.liquid` → Half Vertical
   - `markup-quadrant-framework.liquid` → Quadrant

See `settings.yml` for the full reference configuration.

## Files

| File | Purpose |
|---|---|
| `worker.js` | Cloudflare Worker |
| `markup-full-framework.liquid` | Full-size (800×480) layout |
| `markup-half-horizontal-framework.liquid` | Half Horizontal (800×240) layout |
| `markup-half-vertical-framework.liquid` | Half Vertical (400×480) layout |
| `markup-quadrant-framework.liquid` | Quadrant (400×240) layout |
| `settings.yml` | Reference plugin configuration (polling + custom fields) |

## Data source

All data comes from [American Soccer Analysis](https://www.americansocceranalysis.com/), a free, public, no-auth API..
