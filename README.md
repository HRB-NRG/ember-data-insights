## Data Loading

The app automatically figures out where to get its data, checking in this order:

1. **Local file** — Looks for `all_countries.csv` in the root dir (fastest, best for dev).
2. **Auto-download** — Grabs Ember's public dataset and caches it for 24h.
3. **Manual upload** — Fallback option via the sidebar (accepts CSV or ZIP).

So, you can just clone and run it — it'll download what it needs on the first run. If you want to skip the download step entirely, just drop your own `all_countries.csv` in the folder.

## Customization

- **Branding:** Tweak `BRAND_NAME` and `BRAND_TAGLINE` at the top of `app.py`. This updates the page title, social previews, and footer.
- **Logo:** Drop a `logo.png` next to `app.py` and it'll automatically show up in the header. (Leave it out if you don't want one — the UI handles it gracefully.)
- **Theme/Colors:** Modify `.streamlit/config.toml`. Be sure to update the `COLOR_*` variables in `app.py` if you want the charts to match your new theme.
- **Fonts:** It uses Inter via Google Fonts. No local font files needed.

## Expected CSV Schema

If you're bringing your own data, format it like this:

| Column | Example |
| --- | --- |
| Country | Austria |
| ISO3 Code | AUT |
| Datetime (UTC) | 2015-01-01 00:00:00 |
| Datetime (Local) | 2015-01-01 01:00:00 |
| Price (EUR/MWhe) | 22.34 |

The app groups by **Datetime (Local)** because intraday peaks and troughs are driven by local timezones.

## Data & License

The default dataset is pulled from Ember (European Wholesale Electricity Price Data), published under CC BY 4.0.

The required attribution is already built into the sidebar and footer, so you're license-compliant right out of the box. Please keep that attribution intact if you fork or redistribute the app.
