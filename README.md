# FortexAI — Prototype Portal

A live, click-through prototype of **FortexAI**, a multi-layer defense framework
against prompt-injection attacks. This repo hosts the portal as a static site on
**GitHub Pages** — no backend, no build step, no API keys required.

## What's in here

```
fortexai-portal/
├── index.html                     ← the entire app (open this to run it)
├── .nojekyll                       ← tells GitHub Pages to skip Jekyll processing
├── .github/workflows/deploy.yml    ← auto-deploys to Pages on every push to main
└── source-reference/               ← readable source of each screen, for the dev
    ├── FortexAI.dc.html            ← root shell (layout, routing, mock-data generators)
    └── components/
        ├── AuthPage.dc.html
        ├── LandingPage.dc.html
        ├── Sidebar.dc.html
        ├── Topbar.dc.html
        ├── MonitoringView.dc.html
        ├── ApiGenView.dc.html
        └── PlaceholderView.dc.html
```

**`index.html` is the only file the site needs.** It's a self-contained bundle —
React, all fonts, and every screen (Auth, Landing, Sidebar/Topbar nav, Monitoring,
API key management, etc.) are packed inside it, so it renders identically with
zero network calls. This is what you deploy.

`source-reference/` is **not used by the live site** — it's the human-readable
source of each screen (unpacked from the bundle) so you or a teammate can read
the logic and mock-data generation without digging through the minified bundle.
If you want to make edits, the easiest path is still editing the project in
[Claude Design](https://claude.ai/design) and re-exporting a fresh
`index.html`, rather than hand-editing these reference files.

## Deploy to GitHub Pages

### 1. Create the repo and push

```bash
cd fortexai-portal
git init
git add .
git commit -m "Initial commit: FortexAI portal prototype"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

### 2. Turn on Pages

Go to your repo on GitHub → **Settings → Pages**, and under **Build and
deployment → Source**, choose **GitHub Actions**. The included workflow
(`.github/workflows/deploy.yml`) will run automatically on this push and
publish the site.

(If you'd rather not use Actions: under **Source**, pick **Deploy from a
branch**, branch `main`, folder `/ (root)`. Either method works — Actions is
just the more modern option and is already wired up for you.)

### 3. Get the link

After the first deploy finishes (check the **Actions** tab for progress, ~30–60
seconds), your portal will be live at:

```
https://<your-username>.github.io/<your-repo>/
```

That's the link to send your professor or open on the classroom screen.

## Local preview (optional, before pushing)

```bash
cd fortexai-portal
python3 -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

## Notes

- Requires a reasonably modern browser (Chrome, Edge, Firefox 113+, Safari
  16.4+) — it uses the standard `DecompressionStream` API to unpack the bundled
  assets client-side. Fine for any laptop you'd present from.
- All data in the dashboards (senders, prompts, detection stats) is
  deterministically generated mock data for demo purposes — nothing here calls
  a real backend or the Anthropic API.
- If GitHub's file-size warnings ever come up: `index.html` is under 1 MB, well
  within limits.
