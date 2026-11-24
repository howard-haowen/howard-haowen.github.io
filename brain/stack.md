---
slug: stack
title: Tech stack
role: tech-stack choices
updated: "2026-11-25T07:29:19"
---

# Tech stack

| Domain | Choice | Rationale / note |
|---|---|---|
| Static site generator | Hugo 0.166.0 extended | Same version locally and in CI (`HUGO_VERSION` in `deploy2pages.yml`); `hugo.yaml` uses the `locale` key (Hugo 0.158+) |
| Theme | PaperMod, git submodule at `themes/PaperMod`, pinned `5a46517` (v8.0-32) | Upstream is v8.0-152 (120 commits ahead); see [[optimization-backlog]] |
| Content | Markdown page bundles | Simple and portable |
| Styling | Plain CSS in `assets/css/` (no SCSS/Sass files) | Dark mode via the `body.dark` class; custom properties use the `--portfolio-*` prefix |
| Fonts | Google Fonts: Fraunces, IBM Plex Mono, IBM Plex Sans | Linked from `extend_head.html` |
| Search | PaperMod fastsearch over `index.json` | Requires JSON in `outputs.home` |
| AI discoverability | `static/llms.txt` (hand-written), generated `llms-full.txt`, JSON-LD Person, `robots.txt` allowing AI crawlers | Added 2026-09-28 |
| CI/CD | GitHub Actions `deploy2pages.yml` to GitHub Pages | Push to `main` deploys; `workflow_dispatch` deploys from the chosen ref |
| Domain | `hw.is-a.dev` via `static/CNAME` | Keep the CNAME file |

There is no package.json and no JS/TS source. No lint, formatter or unit-test tooling is configured. Expected build result: exit 0 with exactly two WARN lines from PaperMod templates (`.Language.LanguageDirection`, `.Language.LanguageCode`).
