# Deploying to Streamlit Cloud + embedding in WordPress

## How the app finds its data

The app resolves its data source in this order:

1. **Local file** — `all_countries.csv` next to `app.py` (fastest, for local dev)
2. **Auto-download** — Ember's public zip, fetched automatically from
   `files.ember-energy.org`, unzipped in memory, cached for 24 hours
3. **Manual upload** — sidebar uploader, accepts CSV or zip (final fallback)

For Streamlit Cloud, **path 2 is what runs** — you do not need to commit the
data file to GitHub.

---

## Step 1 — Push to GitHub

1. Create a free GitHub account if you don't have one.
2. Create a new **public** repository (e.g. `bess-dashboard`).
3. From a terminal, in this project folder:

   ```bash
   git init
   git add app.py requirements.txt README.md DEPLOY.md .gitignore .streamlit/
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/<your-username>/bess-dashboard.git
   git push -u origin main
   ```

   The `.gitignore` excludes `all_countries.csv` and any `*.zip`, so the data
   file stays off GitHub — that's intended; the app downloads it at runtime.

   Note: no font files or logos ship with this project, so there's nothing
   extra to commit. If you add your own `logo.png`, remember to `git add` it.

---

## Step 2 — Deploy on Streamlit Cloud

1. Go to <https://share.streamlit.io>.
2. Sign in with your GitHub account.
3. Click **New app**, point it at:
   - Repository: `<your-username>/bess-dashboard`
   - Branch: `main`
   - Main file path: `app.py`
4. Click **Deploy**.

The first run takes a few minutes — Streamlit Cloud installs the packages, then
the app downloads the Ember zip on first visit (one-time, cached 24h after).

You'll get a URL like `https://<your-app-name>.streamlit.app`.

---

## Step 3 — Embed in WordPress

1. In WordPress, edit the page where you want the dashboard.
2. Add a **Custom HTML** block (in the block editor: `/html`).
3. Paste this, replacing the URL with yours:

   ```html
   <iframe
     src="https://your-app-name.streamlit.app/?embed=true"
     width="100%"
     height="950"
     frameborder="0"
     style="border: none; border-radius: 8px;"
     allow="fullscreen">
   </iframe>
   ```

   Notes:
   - `?embed=true` hides Streamlit's top/bottom chrome for a cleaner look.
   - `height="950"` is a starting value — adjust to fit the tallest view
     (the market ranking, usually). Streamlit apps don't auto-size in iframes.

4. **Preview** the page. If the iframe shows a "refused to connect" error, a
   security header is blocking it — see troubleshooting below.

---

## Troubleshooting

### "Refused to connect" in the iframe

Streamlit Cloud sometimes serves apps with strict X-Frame-Options. If you see
this, add this to `.streamlit/config.toml`, then push and redeploy:

```toml
[server]
enableXsrfProtection = false
enableCORS = false
```

### App is slow on first load

That's the one-time Ember download. The 24-hour cache means subsequent visitors
hit the cached copy. Streamlit Cloud also spins apps down after inactivity, so
the first visit after a quiet period will be slow. Their paid tier (or
Render/Railway with an always-on plan) avoids the cold start.

### Ember changes the download URL

The URL is hard-coded in `app.py` as `EMBER_ZIP_URL`. If it breaks, search the
file for that constant and update it. The app then falls back to the manual
uploader, so visitors still get a usable error path.

---

## Attribution

The sidebar's "About the data" expander and the page footer both credit Ember
under CC BY 4.0. Keep these if you redistribute.
