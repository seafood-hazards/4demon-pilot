# CLAUDE.md

Static site for the **4Demon Pilot Database**, built with Quarto (R) and deployed to GitHub Pages.
Presents a pilot SQLite schema + geospatial/data analysis of the 4Demon Belgian marine sediment monitoring dataset.

## Stack
- Quarto website project (`_quarto.yml`), R 4.6.0 via `renv` (`renv.lock`).
- Pages are `.qmd` — R chunks for tables/data prep, `{ojs}` (Observable JS) chunks for interactive client-side widgets (Leaflet maps, Plot charts, `Inputs.table`).
- Client-side SQLite: `pilot_4demon.sqlite` is queried in-browser via `stratum-sqlite` + `sql.js` (wasm). `_db-setup.qmd` opens the shared `db` connection; page `.qmd` files `{{< include _db-setup.qmd >}}` it before running `db.query(...)`.
- `download_resources.R` (pre-render script) fetches the sqlite DB and sql.js/stratum-sqlite libs into `libs/sqljs/` if not already present — needed for local Quarto renders.

## Layout
- `index.qmd`, `db-schema.qmd`, `data-preparation.qmd`, `distance-to-coast.qmd`, `location-names.qmd`, `distance-interactive-map.qmd`, `data-export.qmd`, `pilot-db-viewer.qmd`, `sediment-map.qmd` — nav pages, structure/order defined in `_quarto.yml`.
- `header.html` — injected `<head>` JS/CSS: image-zoom modal, stratum-sqlite init (resolves DB/lib paths relative to page depth).
- `image/`, `libs/` (generated, gitignored) — not needed for editing content.
- DB schema: 6 tables — `project`, `station`, `parameter`, `method` (reference), `sample`, `sediment` (fact table). See `db-schema.qmd` for full column definitions.

## Conventions
- New nav page → add `.qmd` file + register it in `_quarto.yml` under `website.navbar.left`.
- Chunk options: `echo: false`, `warning: false`, `message: false` are global defaults (`_quarto.yml`); R tables use `df-print: kable`.
- OJS SQL queries interpolate filter values directly into template strings (e.g. `sediment-map.qmd`) — this is a client-side, read-only, single-user wasm DB (no server, no auth boundary), so this is accepted here; don't "fix" it into parameterized queries without discussing, since stratum-sqlite/sql.js query API is string-based.
- Keep page prose consistent with existing pages' tone: short methods explanation + caveats callout (`::: {.callout-note}`) where data is estimated/algorithmic.

## Workflow
- Gitflow branching: `main` (published/deployed) ← `develop` ← `feature/*`. Merge feature branches into `develop` directly (no PR); no need to open a pull request for routine feature work.
- CI/CD: `.github/workflows/publish.yml` — push to `main` triggers `quarto render` + deploy to GitHub Pages. `develop` pushes do **not** deploy.
- Releases: GitHub release assets referenced directly in pages, e.g. `data-export.qmd` and `download_resources.R` link to `.../releases/download/vX.Y.Z/...`. Bump `CHANGELOG.md` (Keep a Changelog format) on notable changes.
- Local render: `renv::restore()` then Quarto "Render Website" in RStudio.

## Don't
- Don't read/edit `pilot_4demon.sqlite` directly (binary; source of truth is upstream 4Demon export, prepared per `data-preparation.qmd`).
- Don't hand-edit `.quarto/`, `_site/`, `libs/` — all generated/gitignored.
