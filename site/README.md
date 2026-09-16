# Conference Explorer — static site

This folder is the deployable copy of the Explorer UI. It is **generated** — do not
hand-edit `index.html` or `events.json`.

## Rebuild after any registry change

```bash
python "_system/scripts/explorer_ui_data.py"
python "_system/scripts/build_site.py"
```

The first regenerates `events.json` from `wiki/events/**` and the candidate lists;
the second wraps `_system/scripts/explorer-ui.html` into a standalone HTML document
and stages both files here.

## Deploy

One-time login (opens a browser; only Christina can do this):

```bash
npx vercel login
```

Then, from anywhere:

```bash
npx vercel --cwd "_system/site" --prod
```

Vercel prints the live URL. Re-run the same command after a rebuild to publish updates.

## How this differs from the claude.ai artifact

| | Artifact (claude.ai) | This site (Vercel) |
|---|---|---|
| Audience | Signed-in TFO colleagues only | Anyone with the URL |
| Planner (Tracking / Attending) | Shared database, syncs back into the vault | Browser-local only, never syncs |
| Notes | Land in the event's `## My notes` | Stay in that browser |
| Data freshness | Republished after each sweep | Whatever was last deployed |

The artifact remains the system of record for planning. This site is the read-only
shop window: search, filter, and live registration links.

## Before sharing the URL

The deployment is public to anyone holding the link — `noindex` keeps it out of
search engines, but it is not access control. The page exposes the full registry
including `my_status` values (tracking / registered). Vercel's password protection
(Deployment Protection) is a paid feature; without it, treat the URL as semi-public.
