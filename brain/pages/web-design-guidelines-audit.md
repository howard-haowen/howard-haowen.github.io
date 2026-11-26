---
id: web-design-guidelines-audit
title: "Web Interface Guidelines audit and optimization plan (a11y, motion, images, theme-color)"
category: project
status: active
tags: [a11y, css, hugo, optimization]
created: "2026-11-26T11:16:43"
updated: "2026-11-26T15:33:59"
---

## compiled_truth

## Goal
Bring the portfolio site (https://hw.is-a.dev/) in line with the Vercel Web Interface Guidelines (skill `web-design-guidelines`): accessibility, motion, performance, typography, theme color. Follows the PaperMod upgrade in [[optimization-backlog]].

## Scope and constraints
- Only project-owned files: `layouts/_partials/{extend_head,extend_footer,index_profile}.html`, `layouts/{list,single}.html`, `assets/css/extended/portfolio-redesign.css`, `hugo.yaml`, plus a new `layouts/_markup/render-image.html`.
- `themes/PaperMod/` is never edited (theme already sets `color-scheme`).
- Non-goals: merging the duplicate per-card links (empty `.entry-link`), self-hosting fonts, new lint/format tooling.
- Pass criteria per subtask: `HUGO_ENVIRONMENT=production hugo --minify --baseURL https://hw.is-a.dev/` exits 0 with no ERROR and no WARN.

## Plan (6 independent subtasks; full detail in `plans/web-design-guidelines-plan.md`)
1. Skip link, `main` id, focus return to `.menu-toggle` on Escape, `scroll-margin-top` on headings (done via JS in `extend_footer.html`, because `baseof.html` cannot be edited).
2. CSS: move hover transforms and menu icon transition under `prefers-reduced-motion: no-preference`; `100vh` to `svh`; prevent route-card overflow; `text-wrap: balance`; `touch-action`, tap highlight, `overscroll-behavior: contain` on mobile menu; raise 0.66-0.72rem mono text.
3. Templates: `aria-hidden` on decorative SVGs, `...` to `…`, `fetchpriority=high` on hero image, route-card `strong` to `h2` (update CSS selectors), `translate="no"` on name, no empty tags `<ul>`.
4. Image render hook: alt fallback, lazy loading, dimensions for about 27 certification pages whose images are `![](...)`. Remote Credly image sizes may not be obtainable.
5. Trim Google Fonts weights and axes (keep `SOFT` and `opsz`); set `assets.theme_color` to the page background `#f5f1e6` and sync `meta[name=theme-color]` with the active theme via JS.
6. Final verification: production build, curl status checks, unquoted-attribute greps on minified output, viewport (375/820/1280), dark mode, reduced-motion, keyboard.

## Status
Implemented 2026-11-26 on branch `papermod` (tag `pre-web-design-guidelines` marks the state before). Subtasks 1-4 and 6 as planned. Subtask 5 deviation: Google Fonts left unchanged, because narrowing axis ranges did not reduce the number of `@font-face` blocks (45) and every weight 400-800 is used; only `theme-color` was changed (`hugo.yaml` now `#f5f1e6`, `extend_footer.html` syncs it from `--theme` on theme toggle). The image hook (`layouts/_markup/render-image.html`) reads Credly sizes from the URL, static sizes via `images.Config`, and falls back to "<page title> badge" alt only in `certifications`; the two logos in `content/profile.md` got explicit alt text. Verified: production build exit 0 with no WARN/ERROR, no horizontal overflow at 375/820/1280 px, skip link is the first Tab stop, Escape returns focus to the menu toggle. Not yet merged to `main` (only `main` deploys).


## timeline

- time: 2026-11-26T11:16:43
  kind: decision
  summary: "Created this page: Web Interface Guidelines audit and optimization plan (a11y, motion, images, theme-color)"
  source: "Work session 2026-11-26; plans/web-design-guidelines-plan.md"
  affects: [web-design-guidelines-audit]

- time: 2026-11-26T11:16:44
  kind: decision
  summary: "Record the audit findings and 6-subtask plan; implementation not started"
  source: plans/web-design-guidelines-plan.md
  affects: [web-design-guidelines-audit]

- time: 2026-11-26T15:33:59
  kind: decision
  summary: "Implementation complete; font change dropped as ineffective"
  source: Work session 2026-11-26
  affects: [web-design-guidelines-audit]
