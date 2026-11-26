---
id: optimization-backlog
title: "Project optimization backlog (PaperMod dependency, CI, assets)"
category: project
status: active
tags: [optimization, papermod, ci, hugo]
created: "2026-11-25T07:26:34"
updated: "2026-11-26T11:16:44"
---

## compiled_truth

## Goal
Reduce maintenance cost of the Hugo + PaperMod site and keep CI and docs consistent. Baseline tag: `pre-optimization-baseline` (e9bb69f).

## Dependency: themes/PaperMod
- Upgraded 2026-11-25 to `1181196` (v8.0-152) on branch `papermod`, via `.worktrees/papermod-revisions` (branch `papermod-upgrade`). Build: exit 0, no WARN.
- Upstream moved dark mode from `body.dark` to `html[data-theme]`. Project CSS now uses `[data-theme="dark"]`, and dark custom properties go in `:root[data-theme="dark"]` (specificity must beat the theme's `:root[data-theme="dark"]`).
- 6 files still replace theme files: `layouts/list.html`, `layouts/single.html`, `layouts/robots.txt`, `layouts/_partials/{index_profile,post_nav_links,extend_head}.html`. Removed: `share_icons.html` (identical), `header.html` (mobile menu toggle now built in JS from `extend_footer.html`), `post-single.css` (share-button rules moved to extended CSS).
- Verified with before/after screenshots (home, talk, certifications; light/dark; desktop and mobile menu). Known cosmetic differences: breadcrumb separators are now chevrons; `.header-nav` replaces `.nav`.

## Items
1. Done: delete `share_icons.html`, update AGENTS.md.
2. Done: theme upgrade (see above).
3. Done: override scope shrunk from 9 to 6 files.
4. Done: removed Dart Sass and Node.js steps from CI (not yet exercised in a real CI run).
5. Done: Hugo `.deb` sha256 verified against the release checksums file.
6. Done: README no longer hard-codes the Hugo version.
7. Closed: `static/pdf` (11 files, 21M) is all referenced by `content/publications.md`; owner decided not to compress the PDFs.
8. Done: `llms-full.txt` no longer repeats `Venue:` (talk descriptions already start with it; template now prints them as-is).
9. Open: work is on branch `papermod`; only `main` deploys, so merge to `main` after a CI run.

## Status
Items 1-8 done. Remaining: item 9 (merge `papermod` to `main`, which deploys).


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

- time: 2026-11-25T07:32:37
  kind: note
  summary: "Full review text recorded in papermod-dependency-review; item 9 first bullet is done (AGENTS.md committed, .bob/ ignored)"
  source: Project review on 2026-11-24
  affects: [papermod-dependency-review]

- time: 2026-11-25T18:06:08
  kind: decision
  summary: "Theme upgraded to v8.0-152, overrides cut to 6 files, CI cleaned; items 7-8 still open"
  source: Work session 2026-11-25
  affects: [optimization-backlog]

- time: 2026-11-25T18:27:10
  kind: evidence
  summary: "CI run 45594103701 on throwaway branch ci-verify (build-only): Hugo .deb sha256 OK, no Sass/Node steps, build exit 0 with no WARN, 197 pages, deploy skipped. Branch deleted."
  source: CI run 2026-11-25
  affects: [optimization-backlog]

- time: 2026-11-25T19:06:36
  kind: decision
  summary: "Fix llms-full Venue duplication; PDF compression declined"
  source: Work session 2026-11-25
  affects: [optimization-backlog]

- time: 2026-11-26T11:16:44
  kind: note
  summary: "Follow-up work tracked in web-design-guidelines-audit (UI audit against Web Interface Guidelines)"
  source: Work session 2026-11-26
  affects: [optimization-backlog, web-design-guidelines-audit]
