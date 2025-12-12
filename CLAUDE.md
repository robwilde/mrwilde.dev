# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo static site for mrwilde.dev, a personal tech blog. It uses the "terminal" theme (a Tailwind CSS-based theme with monospace fonts).

## Common Commands

```bash
# Development server with live reload
hugo server --buildDrafts --disableFastRender --gc --ignoreCache --noHTTPCache --forceSyncStatic -w

# Build for production
hugo

# Create a new blog post
hugo new blog/my-post-name.md
```

## Deployment Workflow

The site is hosted via Dokploy (self-managed server). **The `public/` folder must be committed** because Dokploy serves pre-built output without running Hugo.

```bash
# Full publish workflow
hugo                          # Build the site
git add content/ public/      # Stage source AND built output
git commit -m "Add new post"
git push                      # Push to GitHub
# Then trigger deploy in Dokploy UI
```

## Content Structure

- **Blog posts** go in `content/blog/` as either:
  - Single markdown files: `content/blog/post-name.md`
  - Page bundles with assets: `content/blog/post-name/index.md`
- **Authors** are defined in `content/authors/<name>/_index.md`
- **Tags** are auto-generated from post front matter

## Blog Post Front Matter

```yaml
---
title: "Post Title"
date: 2025-06-19
draft: false
params:
  slug: "post-url-slug"
layout: "post"
tags: ["Tag1", "Tag2"]
authors: ["Robert Wilde"]
description: Brief description for SEO and previews.
---
```

## Architecture Notes

- **Theme**: Git submodule at `themes/terminal` - avoid modifying theme files directly
- **Layout overrides**: Custom layouts go in `layouts/` to override theme defaults
- **Shortcodes**: Custom shortcodes in `layouts/_shortcodes/` (e.g., `youtube.html`)
- **Third-party JS**: Place scripts in `layouts/partials/third_party_js/`
- **Hugo extended** is required (min version 0.116.0) for Tailwind CSS processing
- Permalink format: `/blog/:year/:month/:day/:title/`