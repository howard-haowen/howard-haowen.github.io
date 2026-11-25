# AGENTS.md

This file provides guidance to agents when working with code in this repository.

Personal portfolio site (https://hw.is-a.dev/) built with Hugo. The PaperMod theme is a git submodule at `themes/PaperMod`, pinned at `1181196` (`git describe`: v8.0-152). "Code" here means Go templates in `layouts/`, CSS in `assets/css/`, `hugo.yaml`, and Markdown content. There is no package.json and no JS/TS source.

## Commands
- Dev server: `hugo server` at http://localhost:1313/. It renders to `public/` (git-ignored) unless `--renderToMemory` is passed.
- Production build, same as CI: `HUGO_ENVIRONMENT=production hugo --minify --baseURL https://hw.is-a.dev/`
- Single-page check (no test runner): with `hugo server` running, `curl -s -o /dev/null -w '%{http_code}' http://localhost:1313/talks/2025-ntue-2/` prints `200`.
- No lint, formatter, or unit tests are configured. Don't add tooling unless asked.
- Pass criteria: exit 0 with no ERROR. No WARN lines are expected.

## Versions
- Hugo 0.166.0 extended is used locally and in CI (`HUGO_VERSION` in `.github/workflows/deploy2pages.yml`). Keep them equal.
- `hugo.yaml` uses `locale`, the Hugo 0.158+ key, not `languageCode`. Don't edit `themes/PaperMod/` to clear the WARN lines.

## Gotchas
- `--minify` strips HTML attribute quotes. Grep built HTML without quotes: `grep -c 'class=paginav'`. The quoted form returns 0 even when the element exists.
- A project file at a theme path replaces the theme file whole. These six do: `layouts/list.html`, `layouts/single.html`, `layouts/robots.txt`, `layouts/_partials/index_profile.html`, `layouts/_partials/post_nav_links.html`, and `layouts/_partials/extend_head.html`. Theme updates don't reach them, so diff each against `themes/PaperMod/` after any submodule bump.
- The theme's `layouts/` and `layouts/_partials/` layout is the Hugo 0.146+ one. Put overrides at those paths, not `layouts/_default/` or `layouts/partials/`.
- `extend_head.html` is the theme's head hook. Keep its font links, llms/JSON discovery links, and `jsonld_person.html` include.
- `extend_footer.html` is the theme's footer hook. It builds the mobile menu toggle in JS, so `header.html` is no longer overridden. The toggle styles and the share-button rules live in `assets/css/extended/portfolio-redesign.css`.
- Search needs JSON in `outputs.home`, because `themes/PaperMod/assets/js/fastsearch.js` fetches `../index.json`.
- `params.env: production` in `hugo.yaml` turns on production-only theme output (OG, Twitter, schema, robots index) in every environment, including `hugo server`.
- Pushes to `main` deploy to production. Manual dispatch of `.github/workflows/deploy2pages.yml` also deploys, from whichever ref it runs on. Git-ignored paths (`public/`, `resources/_gen/`, `plans/`, `.github/prompts/`) never reach CI.
- Hero title, subtitle, and image come from `hugo.yaml` `params.profileMode`. The kicker, gloss strip, field card, and route cards are hard-coded in `layouts/_partials/index_profile.html`. `profileMode.buttons` is unused.

## Layout
- `content/talks/<YYYY>-<venue>[-n]/` and `content/certifications/<slug>/` are page bundles: `index.md` plus its cover image beside it. Talk and certification `_index.md` files set `type` and `cascade`.
- `layouts/index.llmsfull.txt` generates `llms-full.txt` from hard-coded pages and section names. `static/llms.txt` is hand-written.
- `assets/css/extended/*.css` is concatenated after theme CSS. Put new CSS there.
- Keep `static/CNAME` (custom domain).
- Don't edit generated output (`public/`, `resources/_gen/`) or `themes/PaperMod/`.

## Code style
- Partials are called by name: `partial "x.html" .`. No imports.
- No formatter. Indentation is per file: 2 spaces in `layouts/list.html`, `layouts/single.html`, and `assets/css/extended/portfolio-redesign.css`; 4 spaces in `layouts/_partials/index_profile.html`. Match the file you edit.
- Front matter: double-quote `title` and `description`, unquoted ISO `date`, inline `tags` list, boolean `draft`. `hugo new` writes `draft: true` and single-quoted YAML (`archetypes/default.md`), so fix new files by hand.
- Dark mode is `data-theme="dark"` on `<html>`, so color rules need a `[data-theme="dark"] ...` variant, and dark custom properties go in `:root[data-theme="dark"]`. New custom properties use the `--portfolio-*` prefix.
- Missing params render nothing silently. Guard optional values with `with`, `if`, or `| default`. Check `resources.Get` results for nil, as the theme's `header.html` does.
- Commits: Conventional Commits with optional scope (`feat(seo): ...`, `fix(ui): ...`, `chore: ...`).

<!-- BEGIN brain.md -->
## Project Brain

This project keeps a **Project Brain**: a persistent memory layer of its durable decisions, requirements, and constraints. Read `./BRAIN.md` for the full read/write contract.

Use it actively:
- Before any task or discussion, load the relevant brain context with the `brain` CLI's read commands.
- Whenever a decision, requirement, constraint, or durable insight surfaces — in discussion or in code — record it with the `brain` CLI before moving on; don't wait to be asked.
- All reads and writes go through the `brain` CLI — never hand-edit brain files.

The brain skills (`brain-setup`, `brain-page`, `brain-ingest`, `brain-bootstrap`) are installed in your global skills directory.
<!-- END brain.md -->
