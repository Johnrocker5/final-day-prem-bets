# EPL Parlay Tracker

Tracks multi-leg parlays across Premier League fixtures using [Polymarket](https://polymarket.com) live prices as the probability source.

## Files

| File | Description |
|---|---|
| `index.html` | Landing page with links to all trackers. |
| `gameweek-37.html` | **GW37** — 9 fixtures, May 17–19 2026. PWA-enabled. |
| `gameweek-38.html` | **GW38** — 10 fixtures, May 24 2026 (final day). PWA-enabled. |
| `parlay-tracker.html` | **v1** — URL-paste / per-leg-odds tracker with 2-up detection. |
| `manifest-gw37.json` | PWA manifest for GW37. |
| `manifest-gw38.json` | PWA manifest for GW38. |
| `sw-gw37.js` | Service worker for GW37 (offline shell caching). |
| `sw-gw38.js` | Service worker for GW38 (offline shell caching). |
| `icon-192.png` | 192×192 dark-themed app icon. |
| `404.html` | GitHub Pages SPA fallback. |

## Quick Start

Open any gameweek file in a browser (desktop or mobile — works on both). No build step, no dependencies — each is a self-contained HTML file that fetches live Polymarket prices directly from your device. All values in ZAR (R).

1. Wait for the status pill to show **LIVE** (fixtures loaded from Polymarket).
2. Tap **+ NEW PARLAY**.
3. Enter a name, stake, and potential payout.
4. For each fixture, pick a bet type and option. The default option auto-selects — no need to toggle back and forth.
5. Save. Expected P&L is computed from live Polymarket prices.
6. Data auto-refreshes every 15 seconds and persists in `localStorage`.

## Deployment (GitHub Pages)

```bash
git init
git add -A
git commit -m "Initial"
git remote add origin git@github.com:YOUR_USER/YOUR_REPO.git
git branch -M main
git push -u origin main
```

Then enable GitHub Pages in the repo settings:
- **Source**: Deploy from a branch → `main` → `/ (root)`
- Your site will be at `https://YOUR_USER.github.io/YOUR_REPO/`

Bookmark this URL on your phone and use **Share → Add to Home Screen** in Safari to install it as a standalone app (no browser chrome).

## Bet Types

| Type | Options | Polymarket Source |
|---|---|---|
| 1X2 | Home / Draw / Away | `{base}-{home\|draw\|away}` Yes price |
| Double Chance | 1X (Home or Draw) / X2 (Draw or Away) | `{base}-{home\|away}` No price |
| Handicap | Home/Away ±1.5, ±2.5 | `{base}-spread-{home\|away}-{N}pt5` |
| BTTS | Yes / No | `{base}-btts` |
| Total Goals | Over/Under 0.5–5.5 | `{base}-total-{N}pt5` |
| No bet | — | Skipped |

## Math

- **Combined probability** = product of all leg probabilities from Polymarket
- **Expected P&L** = (payout × combined probability) − stake
- Legs with missing data are shown as **PARTIAL** and excluded from aggregate EV

## GW37 Fixtures

9 fixtures spanning May 17–19, 2026:
Man Utd vs Nottingham Forest, Leeds vs Brighton, Everton vs Sunderland, Brentford vs Crystal Palace, Wolves vs Fulham, Newcastle vs West Ham, Arsenal vs Burnley, Chelsea vs Tottenham, Bournemouth vs Man City.

## GW38 Fixtures

10 fixtures on May 24, 2026 (final day):
Crystal Palace vs Arsenal, Liverpool vs Brentford, Brighton vs Man United, Man City vs Aston Villa, Tottenham vs Everton, Sunderland vs Chelsea, West Ham vs Leeds, Nottingham Forest vs Bournemouth, Burnley vs Wolves, Fulham vs Newcastle.

## Persistence

- v1: `localStorage` key `parlay_state_v1`
- GW37: `localStorage` key `parlay_state_gw37`
- GW38: `localStorage` key `parlay_state_gw38`
- Keys do not overlap — all versions can coexist.
