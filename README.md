# Goaltimate Stats

A mobile-first web app for tracking live stats during goaltimate games, built to be used on a phone held in landscape on the sideline.

## What it does

- **Game setup** – name the game, enter players for each team, and choose to track **My Team** only or **Both Teams**.
- **Saved rosters** – save a team's player list and load it again for later games.
- **Live tracking** – tap players to record who holds the disc. Log **Goals** and **Turnovers** (throwaway, drop, stall), with a game clock and **Undo**.
- **Edit teams mid-game** – add or rename players without ending the game.
- **Resume** – an unfinished game is saved automatically and can be picked up again from the home screen.
- **Stats & export** – at the end of a game, view the per-player stats and download the full event log as `goaltimate-stats.csv`.
- **Installable PWA** – it can be added to the home screen, runs fullscreen in landscape, and works offline once loaded.

## Built with

- Plain HTML, CSS and JavaScript in a single file (`index.html`), with no framework, dependencies or build step
- A Web App Manifest (`manifest.json`) and a service worker (`sw.js`) for installability and offline caching
- Netlify for static hosting (`netlify.toml`)

## Running locally

No install is needed. Serve the folder with any static file server, for example:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000. Opening `index.html` straight from disk mostly works, but the service worker only registers when the page is served over `http://localhost` or HTTPS.

To test the phone layout, use your browser's device emulation in landscape. Note that the service worker caches `index.html`, so hard-refresh (or clear site data) after making changes.

## Deployment

The app is deployed as a static site on **Netlify** (https://skips-stats.netlify.app). `netlify.toml` publishes the repository root as-is, with no build command, and sets two headers:

- `sw.js` is served with `no-cache` so browsers always get the latest service worker
- every page is served with `X-Frame-Options: DENY`

When you change `index.html`, bump `CACHE` in `sw.js` (for example `goaltimate-v2` → `goaltimate-v3`). That makes installed copies drop their old cached version.

## Where data is stored

There is no backend or database, and nothing is sent to a server. All data stays on the device in the browser's `localStorage`:

| Key | Contents |
| --- | --- |
| `gt_save` | The in-progress game (state + clock), used for **Resume Game**; cleared when a new game starts |
| `gt_rosters` | Saved team rosters |

Clearing the browser's site data, or using a different browser or device, starts you with no data. To keep game stats permanently, use **Export CSV**.
