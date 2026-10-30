# CLAUDE.md

Guidance for Claude Code working in this repository.

## What this is

A static, single-file web app: `index.html` contains all HTML, CSS, JavaScript, map geometry and city data inline. There is **no build step, no package.json and no dependencies** to install. Keep it that way unless the user asks otherwise. The only external resource is the Inter Tight font from Google Fonts (with system-font fallbacks).

## Files

- `index.html` – the whole app
- `.github/workflows/deploy-pages.yml` – deploys the repo root to GitHub Pages on every push to `main`
- `.nojekyll` – stops GitHub Pages from running Jekyll
- `README.md` – human-facing overview

## First-time deploy (do this when the user asks to deploy)

Before running anything that creates or publishes, confirm with the user:
1. **Repo name** (suggest `timezone-planner`)
2. **Owner** – their personal account or an organization (e.g. a company org; Pages may need an org admin to allow it)
3. **Visibility** – public or private. Note: Pages sites are publicly viewable unless the account is on a plan that supports private Pages.

Then:

```bash
gh auth status                     # make sure the GitHub CLI is signed in; if not: gh auth login
git init -b main
git add .
git commit -m "Initial commit: time zone planner"
gh repo create <owner>/<repo> --<public|private> --source=. --remote=origin --push

# Turn on Pages with GitHub Actions as the source
gh api -X POST repos/<owner>/<repo>/pages -f build_type=workflow
#   If that returns 409 (Pages already exists), switch it instead:
#   gh api -X PUT repos/<owner>/<repo>/pages -f build_type=workflow

# Watch the deploy and get the URL
gh run watch --repo <owner>/<repo>
gh api repos/<owner>/<repo>/pages --jq .html_url
```

If `gh` isn't installed, tell the user (https://cli.github.com) or fall back to: create the repo on github.com, `git remote add origin …`, `git push -u origin main`, then set Settings → Pages → Source to **GitHub Actions**.

The live URL is usually `https://<owner>.github.io/<repo>/`.

## Updating later

Edit `index.html`, commit, push to `main`. The workflow redeploys automatically.

## Where things live inside index.html (search for these)

- `const VZ =` – Canada's permanent-time rules from Nov 1, 2026 (BC UTC−7, Alberta/NWT UTC−6, Manitoba UTC−5, Golden BC UTC−7). `real()` swaps to fixed offsets after each cutover.
- `const CHANGE_AT` – moment the "Today / After Nov 1" switch disappears.
- `const R =` – US states and Canadian provinces: name, country, time zone, split-zone note, "New" note.
- `const CITIES =` – hand-curated North American cities (handles split states like El Paso, Pensacola, Kenora).
- `const META =` / `const WCITIES =` – generated world country and city data (large single-line JSON; edit with care).
- `const WORLD_MAP =` / `const NA_MAP =` – projected SVG map paths (large; don't hand-edit).
- `const CHANGES =` – "Recent changes to watch" panel text.
- `DEFAULT_PINS` – starting list of pinned clocks.
- `let view = "na"` – page always opens on the North America view.
- `let entryMode="their"` – planner defaults to "Their time".
- CSS variables at the top of `<style>` – brand colors (`--green:#00F500`, black, white) and light/dark themes.

## Conventions

- Brand: black, white and green `#00F500`; font Inter Tight.
- User settings persist in `localStorage` (keys start with `tzp`). Always wrap storage access in try/catch.
- Test changes by serving locally (`python3 -m http.server`) and checking both North America and Global views, light and dark mode, and a phone-width window.
