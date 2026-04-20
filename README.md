# DTM — marketing site (janus-site)

Static single-page site for **DTM (Dynamic Trajectory Memory)** — the public-facing company and thesis — and **Janus DTM OS**, the governance-first AI operating system. **Arbiter** is described on-page as the intent-clarification ritual (human and Arbiter align before execution).

## URLs

| What | URL |
|------|-----|
| **Live site (GitHub Pages)** | https://janusadmin.github.io/janus-site/ |
| **Source repository** | https://github.com/Janusadmin/janus-site |

The deployed site follows this repo’s `index.html` (DTM positioning). If you add a custom domain, update this table and any hosting settings (for example a `CNAME` file for GitHub Pages).

## What’s in this repo

- **`index.html`** — One long landing page: hero, problem, three-layer system overview, principles, Arbiter ritual (six steps), evidence / Greenfield narrative, founder, contact, footer. Includes a small inline script for the mobile nav toggle.
- **`style.css`** — Layout and styling (Inter from Google Fonts, responsive rules from ~768px down).

There is no build step, framework, or package manager: open the files in a browser or deploy the folder as static assets.

## Local preview

From this directory, serve static files so asset paths behave like production:

```powershell
# Python 3
python -m http.server 8080
```

Then open http://localhost:8080/

## Deploying

This repo is set up for **GitHub Pages** (the live URL above matches the default `https://<org>.github.io/<repo>/` pattern). Pushes to the branch GitHub Pages is configured to use will update the site.
