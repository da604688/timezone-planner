# Time Zone Planner

A single-page tool for planning calls across time zones, built for 911 / public-safety teams working across North America and beyond.

## Features

- **North America view** (US states and Canadian provinces) and a **Global view**; opens on North America
- **Plan a call** from *their time* (default) or *my time*; "Where are you?" defaults to your detected city
- Daylight saving handled automatically for any date, including places that don't change clocks
- Canada's **Nov 1, 2026** changes built in (BC, Alberta, Manitoba, Northwest Territories), so times are right even on devices that haven't updated
- "Today / After Nov 1, 2026" preview switch that hides itself once the change has happened
- Pinned clocks with work-hours ribbons, all-zones list, 12/24-hour toggle, light and dark themes
- Zoomable map, search across ~3,000 cities

## Run locally

It's one self-contained file. Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploy

Pushing to `main` deploys to GitHub Pages through `.github/workflows/deploy-pages.yml`.
Pages must be set to **Source: GitHub Actions** (Settings → Pages) — see `CLAUDE.md` for the one-time setup.

## Data sources

Map shapes: US Census (us-atlas), Natural Earth (world-atlas), public Canadian and Australian boundary files. City list: the `city-timezones` package. Time zone rules: the browser's built-in time zone database.
