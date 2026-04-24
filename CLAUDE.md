# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

**Metac** is a single-page, zero-dependency web app for browsing and sharing active-learning methodologies (técnicas de aprendizaje activo). It is a single HTML file (`index.html`) with all CSS and JavaScript inlined.

There is no build step, no package manager, and no test suite. To develop, open `index.html` directly in a browser or serve it with any static server (e.g. `python3 -m http.server`).

## Architecture

Everything lives in `index.html` in three logical sections:

1. **CSS** (`<style>`) — CSS custom properties drive theming; layout uses CSS Grid for the card grid and Flexbox elsewhere. No external stylesheet.

2. **HTML** — Declarative shell only. All dynamic content (cards, tabs, modal body, panel items) is injected by JavaScript at runtime.

3. **JavaScript** (`<script>`) — Plain vanilla JS, no framework. Key sections, in order:

   - **CONFIG** — `CSV` object holds Google Sheets published-CSV URLs for ES and CA languages. `COLORS` / `FIELD_MAP` map field names to badge colors.
   - **I18N** — `I18N` object with all UI strings for `es` and `ca`. Access via `i(key)`.
   - **STATE** — `S` object is the single source of truth: `lang`, `data` (per-language parsed arrays), `favorites` (Set of indices), `search`, `field` (active tab), `modal` (open index), `shared` (URL-decoded indices).
   - **CSV PARSER** — `parseCSV` / `toObjects` convert raw Google Sheets CSV into technique objects `{ name, desc, tags, fields, programs }`. Programs are parsed from the format `Label|url ; Label2|url2`.
   - **RENDER functions** — `renderTabs()`, `renderCards()`, `openModal(idx)`, `renderPanel()`. All re-read `S` and rebuild DOM.
   - **DATA FETCH** — `loadLang(lang)` fetches the CSV for the given language (cached in `S.data`). Also falls back to local CSV files during development (the two `.csv` files in the repo).
   - **SHARING** — `shareURL(indices)` encodes technique indices as `?t=0,3,5&lang=es`. The `?t=` param populates `S.shared` on load, which filters the card grid.
   - **INIT** — Reads URL params, loads data, wires all event listeners.

## Data sources

- **Live** (production): Google Sheets published as CSV, URLs in the `CSV` constant.
- **Local** (backup/offline fallback): `data/metac-ca.csv` (Catalan) and `data/metac-es.csv` (Spanish). These mirror the Google Sheets data and are loaded automatically if the published Google Sheets CSV is unavailable.

## CSV column format

| Column | Field | Notes |
|--------|-------|-------|
| 0 | `name` | Technique name |
| 1 | `desc` | Markdown-lite description: `## Heading`, numbered lists `1. item`, ALL-CAPS: label |
| 2 | `tags` | Comma-separated keywords |
| 3 | `fields` | Comma-separated ámbitos (field/domain badges) |
| 4 | `programs` | Semicolon-separated `Label|url` pairs (optional) |

## Key conventions

- **Indices are language-agnostic positional**: favorites and share URLs store row indices into the parsed array. Switching language reuses the same indices — both CSV files must stay row-aligned.
- **`normField()`** corrects known typos in the ES CSV field names; update it if new typos appear.
- **`fieldColor()`** maps field names to colors via keyword matching in `FIELD_MAP`. Add entries there for new field categories before falling back to the hash.
- **`formatDesc()`** is a lightweight renderer: `## ` → `<h4>`, numbered lines → `<ol>`, ALL-CAPS-colon prefixes → `<span class="section-label">`. It is not a Markdown parser; keep the CSV format consistent with these rules.
- **LocalStorage key** is `metec_favs_v1` (note: "metec", not "metac" — changing it would lose existing users' favorites).
