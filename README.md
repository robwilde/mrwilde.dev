# mrwilde.dev

Personal tech blog built with [Hugo](https://gohugo.io/).

## Tech Stack

- **Static Site Generator**: [Hugo](https://gohugo.io/) (extended version, min v0.116.0)
- **Theme**: [terminal-hugo-theme](https://github.com/techbarrack/terminal-hugo-theme) - Tailwind CSS with monospace fonts
- **CSS Processing**: PostCSS with Autoprefixer

## Deployment

The site is hosted on a self-managed server via [Dokploy](https://dokploy.com/) and served by nginx.

**Current workflow:**
1. Write/edit content in `content/`
2. Build locally with `hugo`
3. Commit both source files AND the `public/` folder
4. Push to `main` branch
5. Trigger deploy in Dokploy UI

**Important:** The `public/` folder must be committed because Dokploy serves the pre-built output directly without running a build step.

**Live site**: [https://mrwilde.dev](https://mrwilde.dev)

### Future Improvements

- Configure Dokploy to run `hugo` build step (eliminate need to commit `public/`)
- Or switch to GitHub Pages with custom domain

## Local Development

```bash
# Install dependencies (for PostCSS/Autoprefixer)
npm install

# Run development server with live reload
hugo server --buildDrafts --disableFastRender -w

# Build for production
hugo
```

## Project Structure

```
.
├── content/           # Markdown content (blog posts, pages)
│   ├── blog/          # Blog posts
│   ├── authors/       # Author profiles
│   └── about.md       # About page
├── layouts/           # Custom layout overrides
│   └── partials/      # Partial templates (head, SEO, etc.)
├── static/            # Static assets (images, favicons)
├── themes/terminal/   # Theme (git submodule)
├── public/            # Generated site output
└── hugo.toml          # Hugo configuration
```

## Creating New Posts

```bash
hugo new blog/my-new-post.md
```

For posts with images, create a page bundle:
```
content/blog/my-post/
├── index.md
└── my-image.png
```