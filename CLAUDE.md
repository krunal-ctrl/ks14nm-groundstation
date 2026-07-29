# CLAUDE.md — KS14NM Ground Station Blog

Guide file for Claude Code sessions on this repo. Read fully before editing.

## Project

Personal ground-station blog. Hosted at `krunal-ctrl.github.io`, separate repo (not the root profile repo). Daily-ish updates. Visual-heavy. Notebook/mission-control aesthetic. Paired with Instagram (@ks14nm.groundstation).

Live telemetry widget on homepage: planned, not now. Build content pipeline first, leave a slot for it.

## Stack

- **Jekyll**. Native GitHub Pages support, zero build config, markdown-first.
- Repo: `krunal-ctrl.github.io` (separate repo, not org root — confirm repo name before push, since only one `username.github.io` repo can exist per account).
- Deploy: GitHub Pages, `main` branch, `/` root.
- No JS framework. Vanilla JS only, for later telemetry widget.

## Folder structure

```
/
├── _config.yml
├── _layouts/
│   ├── default.html
│   ├── post.html
│   └── home.html
├── _includes/
│   ├── header.html
│   ├── footer.html
│   └── metadata-card.html      # corner metadata: date, sat, experiment ID
├── _posts/
│   └── YYYY-MM-DD-title.md
├── _sass/
│   └── theme.scss
├── assets/
│   ├── css/main.scss
│   ├── img/
│   │   ├── diagrams/           # hand-drawn sketches, exported from Canva
│   │   ├── logo/                # KS14NM badge variants (legacy, header/footer now use inline brand SVG)
│   │   └── posts/
│   └── js/
│       └── telemetry.js         # placeholder, wire up later
├── categories/                  # Build Log, Pass Report, Telemetry, etc.
└── index.md                     # homepage, reserve a #telemetry div
```

## Visual theme (unified with krunal-ctrl.github.io personal brand)

As of 2026-07-29, this site shares the layout/color system of the personal
portfolio (`krunal-ctrl.github.io`) instead of a standalone dark/notebook
look, so the two properties read as one brand — header/footer show both
brand marks together (personal wordmark + KS14NM badge), rather than one
replacing the other. Typography deliberately diverges from the portfolio
(see below): a public, SEO-driven blog prioritizes open-licensed,
accessibility-tested fonts over matching the portfolio's paid display font.
Instagram carousels can still use the notebook/hand-drawn aesthetic for
diagrams specifically — that's a content style, not the site chrome.

**Palette** (light theme, matches portfolio)
- Background: `hsl(210deg 25% 98%)` (`--bg`), soft panels `hsl(220deg 35% 95%)` (`--bg-soft`)
- Surface (cards): `#ffffff` (`--surface`)
- Ink (text): `hsl(222deg 22% 8%)` primary, `hsl(222deg 14% 38%)` soft, `hsl(222deg 12% 58%)` faint
- Border: `hsl(220deg 26% 90%)`
- Primary/brand accent: violet `hsl(245deg 100% 60–67%)` (`--violet` / `--primary`)
- Secondary accents: pink `hsl(333deg 90% 48%)`, teal `hsl(170deg 80% 42%)` — teal is reserved for ground-station-specific details (frequency tags, TinyGS mentions)

**Type**
- Headings & body: **IBM Plex Sans** (Google Fonts, open-source/OFL) — chosen over the portfolio's Apercu deliberately: this is a public, SEO-driven blog, so open licensing and accessibility/legibility across a wide audience outweigh pixel-matching the portfolio's typeface
- Data/frequencies/telemetry: **IBM Plex Mono** (same type family as body text, so headings/body/data read as one cohesive system instead of three unrelated fonts)
- Annotations: handwritten-style font (Caveat) — sparingly, for callouts/diagram captions only

**Layout rules**
- Rounded-card UI (`--radius` / `--radius-lg`), soft shadows (`--shadow-sm` / `--shadow-md`), pill buttons/tags — matches portfolio components (`.card`, `.btn`, `.tag`)
- Fixed grid, consistent margins across all posts (`--maxw: 1080px`)
- Every post: corner metadata block (date, satellite, experiment ID / build rev)
- Header/footer use the shared brand mark (hand-drawn "K" wordmark) and link back to the portfolio; footer keeps a KS14NM / 433 MHz / TinyGS tag as the sub-brand identifier
- Diagrams: hand-drawn look, faint grid/notebook background, not clip-art — this applies to diagram *images* specifically, not overall site chrome

## Post types (categories)

Map directly to Instagram carousel categories:
- Build Log
- Satellite Pass Report
- Telemetry Capture
- Antenna Experiment
- TinyGS Tips
- Ground Station Setup
- Failure / Debugging Analysis
- Orbit & RF Explainer

Each gets a `_layouts` variant or a `category` front-matter field with matching card style.

## Post front matter template

```yaml
---
layout: post
title: ""
category: "Build Log"
date: YYYY-MM-DD
satellite: ""
norad_id: ""
frequency: ""
experiment_id: ""
excerpt: ""
cover_image: "/assets/img/posts/xxx.png"
---
```

## Content pipeline

Instagram carousel → long-form post here. Each carousel gets one matching post:
hook → problem → hardware overview → diagram/signal path → build/config → result/telemetry → key learning → close.
Blog post = same order, expanded, with full schematics/config/code.

## Initial build order

1. Scaffold Jekyll site + `_config.yml` + folder structure above
2. Build `default` layout with theme CSS (charcoal/paper/teal/amber)
3. Build `post` layout with metadata-card include
4. Write first 3 posts: "What is TinyGS", "Ground Station Setup", "First Pass Report"
5. Homepage: post grid + `#telemetry` placeholder div (empty, commented "future: live stats")
6. Verify GitHub Pages build (Jekyll, no plugins outside `github-pages` gem whitelist)

## Image generation prompts

Always give image-gen prompts. For every diagram/cover/illustration needed.
Match theme: charcoal bg, hand-drawn notebook line art, teal/amber accents.
Give prompt even if not asked. One prompt per image slot.

## SEO requirements

Every post must be SEO-optimized:
- Title: primary keyword near front, under 60 chars
- Meta description: front matter `excerpt`, under 160 chars, keyword included
- URL slug: short, keyword-based, hyphenated
- H1 once, H2/H3 structured, keyword in first 100 words
- Alt text on every image, descriptive, keyword-aware
- Internal links to related posts/categories
- Add `jekyll-seo-tag` plugin (allowlisted) to `_config.yml`
- Add sitemap.xml via `jekyll-sitemap` plugin (allowlisted)
- Target long-tail terms: "how to setup tinyGS", "NOAA satellite ground station DIY", etc.
- Add front-matter `keywords` field per post

## Constraints

- No JS frameworks. No build step beyond Jekyll's own.
- Only plugins in the official `github-pages` gem allowlist (GitHub Pages runs Jekyll safe mode).
- All diagrams as image assets — don't try to redraw hand-sketch style in CSS/SVG unless explicitly asked.
- Keep theme tokens (colors/fonts) in one `_sass/theme.scss` file. No inline hex codes in templates.