---
slug: stack
title: Tech stack
role: tech-stack choices
updated: "2026-11-25T18:06:08"
---

# Tech stack

| Domain | Choice | Rationale / note |
|---|---|---|
| Static site generator | Hugo 0.166.0 extended | Same version locally and in CI (`HUGO_VERSION` in `deploy2pages.yml`); `hugo.yaml` uses the `locale` key (Hugo 0.158+) |
| Theme | PaperMod, git submodule at `themes/PaperMod`, pinned `1181196` (v8.0-152) | Upgraded 2026-11-25; see [[optimization-backlog]] |
| Content | Markdown page bundles | Simple and portable |
| Styling | Plain CSS in `assets/css/` (no SCSS/Sass files) | Dark mode via `html[data-theme="dark"]` (set by the theme); custom properties use the `--portfolio-*` prefix |
| Fonts | Google Fonts: Fraunces, IBM Plex Mono, IBM Plex Sans | Linked from `extend_head.html` |
| Search | PaperMod fastsearch over `index.json` | Requires JSON in `outputs.home` |
| AI discoverability | `static/llms.txt` (hand-written), generated `llms-full.txt`, JSON-LD Person, `robots.txt` allowing AI crawlers | Added 2026-09-28 |
| CI/CD | GitHub Actions `deploy2pages.yml` to GitHub Pages; Hugo `.deb` is sha256-verified, no Sass or Node steps | Push to `main` deploys; `workflow_dispatch` deploys from the chosen ref |
| Domain | `hw.is-a.dev` via `static/CNAME` | Keep the CNAME file |

There is no package.json and no JS/TS source. No lint, formatter or unit-test tooling is configured. Expected build result: exit 0 with no WARN or ERROR lines.
