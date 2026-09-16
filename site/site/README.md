# Conference Explorer — static site

A searchable registry of conferences, summits and hackathons, with live registration
links. This repo is the **deployable copy**; it is generated from an Obsidian vault
and `index.html` / `events.json` should never be hand-edited.

Everything is static — no build step, no dependencies, no server.

```
index.html      the page (self-contained: CSS + JS inline)
events.json     the data: 179 registry events + 68 scouted candidates
vercel.json     security headers + cache policy
```

## Deploy to Vercel from this repo

1. Push this repo to GitHub (**private** is recommended — see Privacy below).
2. In Vercel: **Add New → Project → Import** this repository.
3. Settings that matter:
   - **Framework Preset:** Other
   - **Root Directory:** the folder that directly contains `index.html`
   - **Build Command:** leave empty
   - **Output Directory:** leave empty
4. Deploy.

**Root Directory is the one that bites.** Vercel serves whatever sits in that folder at
`/`, so it must be the folder holding `index.html` — not the folder above it. If these
files were uploaded inside a subfolder (e.g. the repo shows `Event / site / index.html`),
set Root Directory to `site`. Getting this wrong is what produces `404 NOT_FOUND` on an
otherwise successful deployment: the build is green, there is simply no `index.html` at
the path being served. It can be changed any time under
**Settings → Build and Deployment**, then redeploy the latest deployment.

Every later push to the default branch redeploys automatically.

## Updating the data

The source of truth is the Obsidian vault, not this repo. After any registry change
(a sweep, a new event), rebuild and push:

```bash
python "_system/scripts/explorer_ui_data.py"
python "_system/scripts/build_site.py"
git add -A && git commit -m "Refresh registry data" && git push
```

`explorer_ui_data.py` regenerates `events.json` from `wiki/events/**` and the candidate
lists; `build_site.py` wraps the UI source into a standalone HTML document and stages
both files here.

## How this differs from the Claude artifact

| | Artifact (claude.ai) | This site |
|---|---|---|
| Audience | Signed-in colleagues only | Anyone with the URL |
| Planner (Tracking / Attending) | Shared database, syncs back to the vault | Browser-local only, never syncs |
| Notes | Land in the event's `## My notes` | Stay in that browser |
| Freshness | Republished after each sweep | Whatever was last pushed |

The artifact remains the planning tool. This site is the read-only shop window:
browse, filter, and follow registration links.

## Privacy

The page exposes the full registry, including `my_status` values (tracking /
registered). `noindex` headers keep it out of search engines, but **that is not access
control** — anyone with the URL can read it. Keep the GitHub repo private, and if the
deployment itself needs to be locked down, use Vercel's Deployment Protection
(a paid feature).
