# European Power Market Volatility — BESS Revenue Dashboard

Interactive Streamlit dashboard for analysing daily peak-to-trough (intraday)
electricity price swings across European countries, and estimating the gross
arbitrage revenue a battery (BESS) could have earned in each market.

The headline use case: **see where to deploy battery storage next**, ranked by
gross revenue opportunity per MW.

## What it shows

Six views, all driven by the same sidebar filters (date range + countries):

1. **Volatility trend** — Smoothed line chart of daily swings per country, with
   an adjustable rolling-average window (1–30 days) and macro-event annotations.
2. **Calendar heatmap** — Week-by-weekday grid of swings for any one country,
   making weekly patterns and outlier days visible at a glance.
3. **Market ranking** — Horizontal bar chart of countries ranked by mean,
   median, max, or 95th-percentile swing, plus a box plot of the distribution.
4. **BESS revenue** — Models a 1–4 hour battery doing daily price arbitrage and
   ranks markets by gross €/MW/year, with an optional minimum-spread dispatch
   rule and a monthly revenue trend.
5. **Hydrogen producer** — A virtual electrolyser that runs only in hours where
   the day-ahead price is at or below an adjustable switch-on price. Shows the
   capacity factor, the electricity cost per kg of H₂, and a market ranking, so
   you can see where cheap power makes green hydrogen most attractive.
6. **Data** — Filtered table of daily peak / trough / swing / mean prices and
   per-duration spreads, with a CSV download button.

The default date range is the most recent 12 months in the dataset.

## Setup

```bash
pip install -r requirements.txt
```

## Run

```bash
streamlit run app.py
```

The app resolves its data source automatically, in this order:

1. **Local file** — `all_countries.csv` next to `app.py` (fastest, for dev).
2. **Auto-download** — Ember's public price dataset, fetched and cached for 24h.
3. **Manual upload** — sidebar uploader (CSV or zip), as a final fallback.

So on a fresh machine you can just run it — it will download the data on first
use. Dropping a local `all_countries.csv` in the folder skips the download.

## Make it yours

- **Name / tagline:** edit `BRAND_NAME` and `BRAND_TAGLINE` near the top of
  `app.py`. They flow through the page title, social-share preview, and footer.
- **Logo (optional):** drop a `logo.png` next to `app.py` and it appears in the
  header automatically. No logo ships by default — the header looks fine
  without one.
- **Colours / theme:** edit `.streamlit/config.toml` (and the matching `COLOR_*`
  constants in `app.py` if you want the charts to follow).
- **Fonts:** the app uses Inter (loaded from Google Fonts, open-licensed). No
  font files are bundled.

## Expected CSV schema

| Column              | Example                |
|---------------------|------------------------|
| `Country`           | `Austria`              |
| `ISO3 Code`         | `AUT`                  |
| `Datetime (UTC)`    | `2015-01-01 00:00:00`  |
| `Datetime (Local)`  | `2015-01-01 01:00:00`  |
| `Price (EUR/MWhe)`  | `22.34`                |

The app uses `Datetime (Local)` for grouping, since intraday peaks and troughs
are local-clock phenomena.

## Data & attribution

Data comes from [Ember](https://ember-energy.org/) — European Wholesale
Electricity Price Data — published under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Attribution is built
into the sidebar's "About the data" expander and the page footer, so you're
licence-compliant out of the box. Keep that attribution if you redistribute.
