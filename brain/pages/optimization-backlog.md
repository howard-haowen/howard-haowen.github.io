---
id: optimization-backlog
title: "Project optimization backlog (PaperMod dependency, CI, assets)"
category: project
status: active
tags: [optimization, papermod, ci, hugo]
created: "2026-11-25T07:26:34"
updated: "2026-11-25T07:26:34"
---

## compiled_truth

## Goal
Reduce maintenance cost of the Hugo + PaperMod site and keep CI and docs consistent. Baseline tag: `pre-optimization-baseline` (e9bb69f).

## Dependency: themes/PaperMod
- Submodule pinned at `5a46517` (v8.0-32); upstream `origin/master` is v8.0-152, 120 commits behind.
- Upstream moved `layouts/_default/*` to `layouts/`, `layouts/partials/*` to `layouts/_partials/*`, `shortcodes/` to `_shortcodes/`; upstream `min_version` is 0.158.0 (local and CI Hugo is 0.166.0, so OK).
- Upstream `a5d59c7` fixes the Hugo 0.158 deprecations, which should clear the two expected WARN lines (`.Language.LanguageDirection`, `.Language.LanguageCode`). Not yet verified.
- 9 project files replace theme files whole. `layouts/partials/share_icons.html` is byte-identical to the pinned theme file. The other 8 differ. `assets/css/common/post-single.css` diverges most from upstream (+68/-263).
- Project overrides still sit at the old paths; assumed (not verified) that Hugo's compatibility keeps project files taking precedence after an upgrade.

## Recommended items
1. Delete `layouts/partials/share_icons.html` (identical to theme, blocks future updates) and drop it from the AGENTS.md override list.
2. Upgrade PaperMod in `.worktrees/papermod-revisions` first; diff each override against upstream, run `hugo --minify`, confirm the WARN lines disappear.
3. Shrink override scope: keep `extend_head.html` (theme hook); evaluate whether `header.html`, `post_nav_links.html`, `single.html`, `list.html`, and `post-single.css` can move to `assets/css/extended/` or other hooks.
4. CI: the `Install Dart Sass` step looks unused (no .scss/.sass files); `Install Node.js dependencies` is a no-op without `package-lock.json`. Verify with a CI run before removing.
5. CI: Hugo `.deb` is downloaded with `wget` and installed without checksum verification; use an action or verify a checksum.
6. Hugo version is written in three places (`deploy2pages.yml`, README mermaid diagram, AGENTS.md); consider not hard-coding it in README.
7. `static/` is 22M, of which `static/pdf` (11 files) is 21M; check for unreferenced or compressible PDFs.
8. Fix the known defect: `llms-full.txt` shows `Venue: Venue: ...` (fix in `layouts/index.llmsfull.txt`).
9. Housekeeping: work is on branch `papermod`; only `main` deploys, so confirm the branch strategy before the theme upgrade.

## Status
Not started. Already done: Hugo bump to 0.166.0 (ff45a81), `locale` key (b1a960c), ignore `tmp/` and `.bob/`, add AGENTS.md.


## timeline

- time: 2026-11-25T07:26:34
  kind: decision
  summary: "Created this page: Project optimization backlog (PaperMod dependency, CI, assets)"
  source: Project review on 2026-11-24
  affects: [optimization-backlog]

- time: 2026-11-25T07:26:34
  kind: decision
  summary: Record recommended optimizations from the project review
  source: Project review on 2026-11-24
  affects: [optimization-backlog]
