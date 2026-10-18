# RimRunnerWashington

Unofficial Washington HS boys basketball ratings. Not affiliated with the WIAA or MaxPreps.
Live site: https://rimrunnerwashington.pages.dev/ (backup copy: GitHub Pages).

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project (accounts, picks, follows).

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 07:30 and 15:00 UTC
(11:30 pm / 7:00 am Pacific standard time) and on manual dispatch. Before the Washington boys season opens
(`SEASON_START` in index.html = first game on the MaxPreps schedules) the script makes no requests.
WIAA schools' games before Mon Nov 23, 2026 (earliest legal contest: first practice Nov 16 + 7 practice days) are ignored.
From then on it reads boys basketball finals for the last 3 days from the MaxPreps Washington scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and
bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals. It commits only when data
changed, then deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
