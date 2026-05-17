# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
bundle exec jekyll serve       # start local dev server at http://localhost:4000
bundle exec jekyll build       # build site to _site/
bundle install                 # install/update Ruby gems
```

## Architecture

This is a Jekyll 4.0 personal blog using the **minima** theme, deployed to GitHub Pages via the `matevojts.github.io` domain.

**Key directories:**
- `_posts/` — blog posts as Markdown files, filename format: `YYYY-MM-DD-slug.md`
- `_layouts/` — overrides minima's default layout (`default.html` wraps head/header/content/footer)
- `_includes/` — partial templates (head, header, footer, google-analytics)
- `_site/` — generated output, never edit directly, not committed to git
- `assets/images/` — static images

**Post frontmatter convention:**
```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD
excerpt_separator: <!--more-->
---
```

Posts are primarily written in Hungarian. The `<!--more-->` tag marks where the excerpt ends on the homepage listing.

**Theme overrides:** The site overrides minima's `_layouts/default.html` to inject custom includes. Google Analytics (`G-PVCGJCFHQB`) loads only when `JEKYLL_ENV=production`.

**Deployment:** Pushing to `master` triggers GitHub Pages to build and serve the site automatically.