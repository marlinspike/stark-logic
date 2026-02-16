# reuben.cleet.us

Personal website for Reuben Cleetus. Built with [Hugo](https://gohugo.io/), hosted on GitHub Pages.

## Prerequisites

- [Hugo](https://gohugo.io/installation/) (extended edition)
- [Claude Code](https://claude.com/claude-code) (for blog writing commands)

## Project structure

```
content/           Markdown pages — edit these to change site content
  _index.md        Homepage intro
  about.md         About page
  contact.md       Contact page
  blog/
    _index.md      Blog listing page
    *.md           Blog posts
layouts/           HTML templates (custom theme, no external dependency)
static/css/        Styles
static/CNAME       Custom domain for GitHub Pages
docs/              Built site output (served by GitHub Pages — do not edit)
hugo.toml          Site configuration
```

## Hugo commands

```bash
# Start local dev server (live reloads on file changes)
hugo server

# Start dev server including draft posts
hugo server --buildDrafts

# Build the site into docs/ for deployment
hugo
```

## Writing blog posts with Claude Code

This repo includes Claude Code commands for writing and publishing blog posts.

### Create a new post

```
/new_post <key ideas>
```

Example:

```
/new_post the real cost of fine-tuning vs prompting, when each makes sense, practical decision framework
```

This will:
1. Research the topic
2. Write an 800-3000 word post in `content/blog/` as a **draft**
3. Run the humanizer skill to clean up AI writing patterns
4. Show you the title, slug, and summary

The post is created with `draft: true` so it won't appear on the live site yet.

### Elaborate a LinkedIn post

Drop a LinkedIn post (plain text file) into the `LIPosts/` folder, then run:

```
/elaborate_post <filename>
```

Example:

```
/elaborate_post fine-tuning
```

This will:
1. Find the matching file in `LIPosts/` (supports partial filename matching)
2. Analyze the LinkedIn post and identify key ideas to expand on
3. Research each topic deeply for data, examples, and counterarguments
4. Write a full 800-3000 word blog post that goes well beyond the original
5. Run the humanizer skill to clean up AI writing patterns

The LinkedIn post is the seed — the blog post digs deeper into *why* the ideas matter, adds evidence, and explores implications the short format couldn't cover.

### Preview a draft

```bash
hugo server --buildDrafts
```

Open the URL Hugo prints (usually `http://localhost:1313/`) and navigate to the blog to see your draft.

### Publish a post

```
/publish_post <slug>
```

Example:

```
/publish_post fine-tuning-vs-prompting
```

This sets `draft: false`, rebuilds `docs/`, and the post is ready to deploy. Run `/publish_post` with no arguments to list all drafts.

## Editing pages directly

Pages are Markdown files in `content/`. Edit them with any text editor or ask Claude Code to make changes. The dev server live-reloads on save.

| Page    | File                  |
|---------|-----------------------|
| Home    | `content/_index.md`   |
| About   | `content/about.md`    |
| Contact | `content/contact.md`  |

## Deploying

1. Run `hugo` to rebuild `docs/`
2. Commit and push to `main`
3. GitHub Pages serves from the `docs/` folder

The `CNAME` file in `static/` gets copied to `docs/` on build, so the custom domain stays configured.

## Design

- Dark theme (`#0a0a0a` background, `#3b82f6` electric blue accent)
- Inter (body) + JetBrains Mono (code) via Google Fonts
- Responsive, mobile-first
- All styles in `static/css/style.css`, using CSS custom properties in `:root`
