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
│   │   ├── logo/                # KS14NM badge variants
│   │   └── posts/
│   └── js/
│       └── telemetry.js         # placeholder, wire up later
├── categories/                  # Build Log, Pass Report, Telemetry, etc.
└── index.md                     # homepage, reserve a #telemetry div
```

## Visual theme (must match Instagram)

**Palette**
- Background: deep charcoal `#1c1f22`
- Paper surface: warm off-white `#f4efe6`
- Highlight: muted teal `#4fb3ac`
- Accent: amber `#e0952b`
- Secondary text: soft gray `#9aa0a6`

**Type**
- Headings: technical sans (e.g. Space Grotesk, IBM Plex Sans)
- Data/frequencies/telemetry: monospace (e.g. JetBrains Mono, IBM Plex Mono)
- Annotations: handwritten-style font (e.g. Caveat, Architects Daughter) — sparingly, for callouts only

**Layout rules**
- Fixed grid, consistent margins across all posts
- Every post: corner metadata block (date, satellite, experiment ID / build rev)
- Footer: KS14NM badge + 433 MHz / TinyGS tag where relevant
- Diagrams: hand-drawn look, faint grid/notebook background, not clip-art

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