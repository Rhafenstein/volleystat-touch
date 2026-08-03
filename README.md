# VolleyStat Touch

**Live volleyball hitting stats for coaches. Two taps per attack. Works with no signal.**

[**Open the app →**](https://rhafenstein.github.io/volleystat-touch/)

Free, no account, no sign-up. Everything stays on your own device.

<p align="left">
  <img src="screenshots/track.png" width="240" alt="Live tracker">
  <img src="screenshots/team.png" width="240" alt="Team stats and setting options">
  <img src="screenshots/player.png" width="240" alt="Player profile">
</p>

## What it does

Tap a player, tap the outcome. That's the whole interaction, and it's fast enough
to keep up with a live rally.

- **Kill · Continue · Error · Blocked** — four outcomes, one tap each
- **Heat Score** — a rolling momentum figure per player, so you can see who's hot right now
- **Error-free kill streaks**
- **Setting options** — who's scoring most per attack, and who's making fewest mistakes
- **Net efficiency, kill rate, kill efficiency, kill-to-error ratio**
- **Radar profile** for each player
- **Multiple sets**, undo, and per-attack removal
- **CSV export**
- **Two themes** — Classic and Tactical

## Using it on a phone or tablet

Open the link, then add it to your home screen — on iPhone or iPad, tap Share
then **Add to Home Screen**. It opens like a normal app, full screen, and works
without an internet connection once installed.

Every time you open it you'll get a home screen offering **Continue** on the
match in progress, or **New match**.

## Your data

Matches are saved in your browser on the device you recorded them on. Nothing is
uploaded, there's no account, and nobody else can see them.

That also means matches don't sync between devices, and clearing your browser
data deletes them. Use **Export CSV** in Settings to keep a copy of anything you
care about.

## The numbers

Raw attack events are the source of truth — every statistic is recalculated from
them, so undo and delete always stay consistent.

```
Attempts        = Kills + Continues + Errors + Blocked
Kill rate       = Kills ÷ Attempts
Continue rate   = Continues ÷ Attempts
Error rate      = (Errors + Blocked) ÷ Attempts
Kill efficiency = Kills ÷ (Kills + Errors + Blocked)
Net efficiency  = (Kills − Errors − Blocked) ÷ Attempts
Kill : error    = Kills ÷ (Errors + Blocked)
```

**Heat Score** is a 0–100 momentum index, not a percentage. It runs
chronologically through each player's attacks:

```
Kill              heat = heat × 0.84 + 30
Error or blocked  heat = heat × 0.84 − 38
Continue          heat = heat × 0.95        (ball still live, no penalty)
```

Clamped to 0–100 at each step, rounded once at the end. Decay of 0.84 gives a
half-life of about four attacks. From cold, consecutive kills read
30 · 55 · 76 · 94 · 100.

| Heat | Stage |
| --- | --- |
| 0–24 | Cold |
| 25–49 | Building |
| 50–69 | Hot |
| 70–84 | Very Hot |
| 85–100 | On Fire |

**Best option to score** ranks by kill rate. **Most stable** ranks by fewest
errors and blocks. Both need at least two attacks before a player appears.

## Support

VolleyStat is free and will stay that way. If it's useful to you and your team,
you can [chip in here](https://paypal.me/volleystat) — it covers the domain and
the time.

## For developers

Plain HTML, CSS and JavaScript. No build step, no dependencies, no server.

| File | Purpose |
| --- | --- |
| `index.html` | Home screen — continue or start a new match |
| `app.html` | The tracker: all screens, styles and logic |
| `manifest.webmanifest` | PWA metadata. `start_url` is `index.html` |
| `service-worker.js` | Offline cache |
| `icon-180.png`, `icon-512.png` | Home screen icons |

Hosted on GitHub Pages from `main`. **Bump `CACHE_NAME` in `service-worker.js`
whenever you change `index.html` or `app.html`**, or devices with the app
installed will keep serving the cached version.

"New match" navigates to `app.html?new=1`, which clears entries, sets and the
match name while keeping the roster and theme, then rewrites the URL so a
refresh doesn't clear it again.

<p align="left">
  <img src="screenshots/track-tactical.png" width="240" alt="Tactical theme">
</p>
