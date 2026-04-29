# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based academic portfolio website using the **al-folio** theme. The site belongs to Stefan Stepanovic and is deployed at https://stepstefan.github.io.

## Build & Development Commands

```bash
# Install dependencies
bundle install

# Build the site
bundle exec jekyll build

# Serve locally with live reload
bundle exec jekyll serve

# Production build
JEKYLL_ENV=production bundle exec jekyll build

# Using Docker (recommended for consistent environment)
docker compose up

# Format code with Prettier
npx prettier . --check
npx prettier . --write
```

**Prerequisites:**
- Ruby 3.3.5+
- ImageMagick (for responsive images)
- Python 3.13+ with nbconvert (for Jupyter notebook support)

## Architecture

### Core Structure
- **`_config.yml`** - Site-wide settings, plugins, and feature toggles
- **`_layouts/`** - Liquid templates (about, cv, post, page, bib, distill)
- **`_includes/`** - Reusable components (header, footer, social, scripts)
- **`_sass/`** - Styles: `_themes.scss` for colors, `_variables.scss` for dimensions, `_base.scss` for custom styles
- **`_data/`** - Structured data files (cv.yml, repositories.yml)

### Content Collections
- **`_pages/`** - Static pages (about, cv, publications)
- **`_posts/`** - Blog posts
- **`_bibliography/`** - BibTeX files for publications (rendered via Jekyll Scholar)

### CV/Resume
Two data sources supported (auto-selected in `_layouts/cv.liquid`):
- JSON Resume format: `assets/json/resume.json`
- YAML format: `_data/cv.yml`

## Key Patterns

- **Templating**: Liquid templates with extensive use of `{% include %}` and `{% case %}`
- **Styling**: Bootstrap grid system (`container`, `row`, `col-*`) with custom SCSS
- **Publications**: BibTeX via Jekyll Scholar with custom rendering in `_layouts/bib.liquid`
- **Front Matter**: Controls layout, sidebar TOC (`toc.sidebar: left|right`), and page features

## CI/CD

- **Deploy**: GitHub Actions builds and deploys to GitHub Pages on push to main
- **Prettier**: Enforces code formatting on PRs (uses `@shopify/prettier-plugin-liquid`)
- **Broken Links**: Lychee link checker validates URLs

## Before making any change

Read these files first — they are the ground truth for the current state of the site:

- `_config.yml` — site settings, enabled/disabled features, nav structure
- `_sass/_themes.scss` — all CSS custom properties (colors, dark/light mode)
- `_sass/_variables.scss` — raw color and font-path variables
- `_sass/_custom.scss` — design system overrides (green theme, component tweaks) — **this is your primary edit target for visual changes**
- `_pages/about.md` — bio text, profile config, social toggle
- `assets/json/resume.json` — ALL CV data (work, education, volunteer, talks, skills, languages)
- `_data/socials.yml` — social links and location
- `_layouts/about.liquid` — about page structure
- `_layouts/cv.liquid` — CV page structure

## Design system

The design intent for this site lives at: https://claude.site/p/79eec983-bd07-4a9a-bdbe-ec66d77b9de4

When in doubt about colors, typography, spacing or component patterns, refer to that project. The UI kit prototype at `ui_kits/website/index.html` in that project is the reference implementation.

## Visual rules

**Colors**
- Light mode accent: `#3a6b35` (forest green) — replaces al-folio's default red `#b71c1c`
- Dark mode accent: `#6aaa61` (light green) — replaces al-folio's default cyan `#2698ba`
- Never introduce new colors outside the token system in `_sass/_custom.scss`

**Typography**
- Headings: Roboto Slab (already loaded via Google Fonts in the theme)
- Body: Roboto
- Code: Roboto Mono
- Page titles in nav and authored front matter are lowercase (`about`, `cv`, `blog`). Do NOT lowercase the about-page `<h1 class="post-title">`, which renders the user's full name ("Stefan Stepanovic") — never apply `text-transform: lowercase` globally to `.post-title`.
- No emoji anywhere — not in headings, nav, or body copy

**Layout**
- Max width: `1200px` (set in `_config.yml` as `max_width`)
- Profile photo: left-aligned, rectangular (not circular), `z-depth-1` shadow
- Navbar: fixed top. Social icons in navbar are ENABLED (`enable_navbar_social: true`) — do not hide `.navbar-brand.social`
- No gradients, no colored left-border accent cards

**Components**
- Active nav link: `3px solid` brand color bottom border + `font-weight: bolder`
- Cards: white bg light / `#212529` dark, `1px solid divider` border, `border-radius: 6px`
- Blockquotes: `border-left: 5px solid brand-color`
- Badges: brand bg, white text, `border-radius: 3px`

## CV data

All CV content lives in `assets/json/resume.json` — never hardcode CV content in templates.
The JSON follows the JSONResume schema. Sections rendered: `basics`, `work`, `education`, `volunteer`, `talks`, `skills`, `languages`.

Work entries use a nested `projects` array (non-standard extension Stefan added) — preserve this structure.

## What NOT to touch

- Do not modify files in `_sass/font-awesome/` or `_sass/tabler-icons/`
- Do not modify `Gemfile` or `Gemfile.lock` unless explicitly asked
- Do not push directly to `main` — always open a PR
- Do not change `_config.yml` `exclude:` list without checking what it deliberately hides

## Workflow

1. Read the relevant source files before making changes
2. Make changes in the smallest scope possible
3. For visual changes: edit `_sass/_custom.scss` only
4. For content changes: edit `_pages/about.md` or `assets/json/resume.json`
5. For structural changes: edit the relevant `_layouts/*.liquid` or `_includes/*.liquid`
6. Open a PR — never push to `main`
7. Describe what changed and why in the PR description

