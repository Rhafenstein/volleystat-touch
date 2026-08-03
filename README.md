# VolleyStat Touch

A free, offline-first volleyball hitting statistics tracker for coaches. Two taps
records an attack. No account, no server, no ongoing cost — everything is stored
in the browser on the coach's own device.

Runs as a Progressive Web App from GitHub Pages, and can be added to the home
screen on iPhone or iPad.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | Landing page. Start / continue button, donation link, install hint. |
| `app.html` | The tracker itself — all screens, styles and logic. |
| `manifest.webmanifest` | PWA metadata. `start_url` points at `app.html`. |
| `service-worker.js` | Offline cache. Bump `CACHE_NAME` on every deploy. |
| `icon-180.png`, `icon-512.png` | Home screen icons. |

## Before deploying

Set your donation link in **both** `index.html` and `app.html`:

```js
const DONATE_URL = "https://www.paypal.com/paypalme/yourname";
```

Leave it as an empty string to hide the Support links entirely.

## Screens

- **Track** — select attacker, record outcome. Deliberately the simplest screen.
- **Team** — team totals and setting options, for the current set or whole match.
- **Player** — one player's heat, radar profile and full metrics.
- **Settings** — roster, sets, CSV export, theme, data controls.

Two themes ship in one build: **Classic** (dark blue) and **Tactical**
(monospace terminal). They are a CSS layer only — identical data and maths.

## Statistics

Raw attack events are the source of truth. Everything else is derived, so undo,
delete and edit all recalculate cleanly.

```text
Attempts       = Kills + Continues + Errors + Blocked
Kill rate      = Kills ÷ Attempts
Continue rate  = Continues ÷ Attempts
Error rate     = (Errors + Blocked) ÷ Attempts
Kill efficiency= Kills ÷ (Kills + Errors + Blocked)
Net efficiency = (Kills − Errors − Blocked) ÷ Attempts
Kill:error     = Kills ÷ (Errors + Blocked)
```

Heat is calculated chronologically per player, clamped and rounded at each step:

```text
NewHeat = round(clamp((PreviousHeat × 0.72) + OutcomeValue, 0, 100))

Kill: +35    Continue: 0    Error or blocked: −45
```

Stages: 0–24 Cold · 25–49 Building · 50–69 Hot · 70–84 Very Hot · 85–100 On Fire.

**Best option to score** ranks by kill rate. **Most stable** ranks by fewest
errors and blocks. Both need at least two attacks from a player.

## Data

Matches live in `localStorage` under `volleystat-touch-v1`. They do not sync
between devices, and clearing browser data deletes them. Use **Export CSV** in
Settings to keep a copy.
