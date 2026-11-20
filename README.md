# DTM - marketing site (janus-site)

Multi-page static site for **DTM (Dynamic Trajectory Memory)** and **Janus OS**. Served from the repository root on GitHub Pages.

## URLs

| What | URL |
|------|-----|
| **Live site** | https://dtm-system.com |
| **Source repository** | https://github.com/Janexissys/janus-site |

Custom domain: `CNAME` is `dtm-system.com`.

## What’s in this repo

- **`index.html`**, **`product.html`**, **`company.html`**, **`about.html`**, **`roadmap.html`**, **`technical.html`**, **`contact.html`** - public pages with shared nav and footer
- **`style.css`** - layout and styling (Geist Mono, Instrument Serif, DM Sans; responsive rules from ~768px)
- **`blog.html`** - redirect to https://blog.dtm-system.com

There is no build step or package manager: serve the folder as static assets.

## Local preview

From this directory:

```bash
python -m http.server 8080
```

Open http://localhost:8080/

Preview at the domain root (not under a `/janus-site/` subpath) so paths match production.

## Deploying

Merging to `main` publishes to https://dtm-system.com when GitHub Pages is configured for branch `main`, folder root. **Deploy only after Jesse approves that slice’s preview and PR.**