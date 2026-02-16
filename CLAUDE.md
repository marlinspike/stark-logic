# stark-logic

Personal website for Reuben Cleetus at `reuben.cleet.us`. Built with Hugo, hosted on GitHub Pages.

## Architecture

- **Hugo static site** with a custom theme (no external theme dependency)
- Dark theme, minimal design, mobile-first responsive
- GitHub Pages serves from the `docs/` folder on `main` branch

## Key paths

- `hugo.toml` — Site config (baseURL, menus, params)
- `content/` — Markdown pages (edit these to change site content)
- `layouts/` — HTML templates (baseof, single, list, index, partials)
- `static/css/style.css` — All styles
- `static/CNAME` — Custom domain file for GitHub Pages
- `docs/` — Built output (do not edit directly)

## Commands

- `hugo server` — Local dev server with live reload
- `hugo` — Build site into `docs/`

## Workflow

1. Edit content in `content/*.md` or layouts in `layouts/`
2. Preview with `hugo server`
3. Run `hugo` to rebuild `docs/` before committing
4. Commit and push to deploy via GitHub Pages

## Content structure

- `content/_index.md` — Homepage intro text
- `content/about.md` — About page
- `content/contact.md` — Contact info
- `content/blog/_index.md` — Blog listing
- `content/blog/*.md` — Individual blog posts

## Blog post format

```markdown
---
title: "Post Title"
date: 2026-02-16
summary: "Short description for the blog list."
draft: true
---

Post body in Markdown.
```

## Claude Code commands

- `/new_post <key ideas>` — Researches and writes a blog post as a draft (800-3000 words), then runs the humanizer skill to clean up AI patterns. Posts are elaborations on LinkedIn ideas.
- `/elaborate_post <filename>` — Picks up a LinkedIn post from `LIPosts/`, researches the topics deeply, and expands it into a full blog post draft. Supports partial filename matching; prompts if multiple match.
- `/publish_post <slug>` — Publishes a draft post. Supports partial slug matching; prompts if multiple match. One post at a time.

## Skills

- `humanizer` — Installed at `.claude/skills/humanizer`. Detects and removes AI writing patterns. Used automatically by `/new_post`.

## Design

- Background: `#0a0a0a`, accent: `#3b82f6` (electric blue)
- Fonts: Inter (body), JetBrains Mono (code) via Google Fonts
- CSS variables defined in `:root` in `style.css`
