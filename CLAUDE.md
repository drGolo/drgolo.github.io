# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About This Site

Personal academic website for Dr. Golo Henseke (Associate Professor in Applied Economics, UCL Institute of Education), built on the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme. Deployed to GitHub Pages from the `master`/`main` branch via GitHub Actions.

## Commands

**Local development (Docker, recommended):**
```bash
docker compose up
# Site available at http://localhost:8080 with live reload on port 35729
```

**Local development (native Ruby):**
```bash
bundle install
bundle exec jekyll serve
```

**Build only:**
```bash
bundle exec jekyll build
```

**Format Liquid/HTML/JS/SCSS with Prettier:**
```bash
npx prettier --write .
```

**Check formatting without writing:**
```bash
npx prettier --check .
```

## Architecture

The site is a standard Jekyll project with Liquid templates. The al-folio theme provides the full layout and plugin stack — most customisation happens through `_config.yml` and the `_data/` YAML files rather than template edits.

### Key content files

| Path | Purpose |
|---|---|
| `_config.yml` | Master config: site identity, feature flags, jekyll-scholar settings, third-party library versions |
| `_bibliography/papers.bib` | All publications as BibTeX; rendered by jekyll-scholar |
| `_data/socials.yml` | Social/professional links shown in navbar and about page |
| `_data/cv.yml` | CV structured data (currently contains al-folio demo content — replace with real data) |
| `_data/venues.yml` | Journal/conference abbreviation → colour/URL mappings for publication badges |
| `_data/coauthors.yml` | Co-author name → profile URL mappings for auto-linking author names |
| `_pages/about.md` | Homepage (layout: about); profile photo, bio, and feature toggles |
| `_projects/*.md` | Research project pages; `importance:` controls display order |

### Publications system (jekyll-scholar)

Publications are driven entirely by `_bibliography/papers.bib`. The scholar config in `_config.yml` highlights the author's own name using `last_name: [Henseke]` and `first_name: [Golo, G.]`.

Custom BibTeX fields understood by the theme (stripped from rendered output via `filtered_bibtex_keywords`):

- `selected={true}` — shows the entry in the "Selected Publications" section on the about page
- `abstract={}` — shown in expandable abstract button
- `preview={}` — thumbnail image filename (stored in `assets/img/publication_preview/`)
- `pdf={}` — link to PDF
- `html={}` — link to journal page
- `arxiv={}` — arXiv ID
- `google_scholar_id={}` — used for citation badge

Publication badges (Altmetric, Dimensions, Google Scholar) are enabled in `_config.yml` under `enable_publication_badges`.

### Feature flags in `_config.yml`

Many optional features are toggled in `_config.yml` under `# Optional Features`. Currently notable settings:
- Blog, announcements, pagination, newsletter: **disabled**
- Dark mode, math (MathJax), masonry layout: **enabled**
- `posts_in_search: false` — blog posts excluded from search

### Liquid templates

`_layouts/` contains page-level templates; `_includes/` contains reusable partials. Templates use Liquid syntax (`.liquid` extension). The bib entry layout is `_layouts/bib.liquid` — edit this to change how individual publications render.

### Styling

SCSS lives in `_sass/`. Main entry points: `_variables.scss` (colours, fonts), `_base.scss` (global styles), `_layout.scss` (structural). Theme variables for light/dark mode are in `_themes.scss`.

### Deployment

GitHub Actions (`.github/workflows/deploy.yml`) builds and deploys on push to `master`/`main`. The build pipeline: installs ImageMagick → builds Jekyll → runs PurgeCSS to strip unused styles → deploys `_site/` to GitHub Pages.

Pre-commit hooks (`.pre-commit-config.yaml`) check for trailing whitespace, missing end-of-file newlines, and valid YAML.
