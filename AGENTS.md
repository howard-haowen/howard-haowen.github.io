# AGENTS.md

This file provides guidance to agents when working with code in this repository.

Personal portfolio site (https://hw.is-a.dev/) built with Hugo. The PaperMod theme is a git submodule at `themes/PaperMod`, pinned at `5a46517` (`git describe`: v8.0-32). "Code" here means Go templates in `layouts/`, CSS in `assets/css/`, `hugo.yaml`, and Markdown content. There is no package.json and no JS/TS source.

## Commands
- Dev server: `hugo server` at http://localhost:1313/. It renders to `public/` (git-ignored) unless `--renderToMemory` is passed.
- Production build, same as CI: `HUGO_ENVIRONMENT=production hugo --minify --baseURL https://hw.is-a.dev/`
- Single-page check (no test runner): with `hugo server` running, `curl -s -o /dev/null -w '%{http_code}' http://localhost:1313/talks/2025-ntue-2/` prints `200`.
- No lint, formatter, or unit tests are configured. Don't add tooling unless asked.
- Pass criteria: exit 0 with no ERROR. Exactly two WARN lines are expected, both from PaperMod templates (`.Language.LanguageDirection`, `.Language.LanguageCode`).

## Versions
- Hugo 0.166.0 extended is used locally and in CI (`HUGO_VERSION` in `.github/workflows/deploy2pages.yml`). Keep them equal.
- `hugo.yaml` uses `locale`, the Hugo 0.158+ key, not `languageCode`. Don't edit `themes/PaperMod/` to clear the WARN lines.

## Gotchas
- `--minify` strips HTML attribute quotes. Grep built HTML without quotes: `grep -c 'class=paginav'`. The quoted form returns 0 even when the element exists.
- A project file at a theme path replaces the theme file whole. These nine do: `layouts/_default/list.html`, `layouts/_default/single.html`, `layouts/robots.txt`, `layouts/partials/header.html`, `layouts/partials/index_profile.html`, `layouts/partials/share_icons.html`, `layouts/partials/post_nav_links.html`, `layouts/partials/extend_head.html`, and `assets/css/common/post-single.css`. Theme updates don't reach them, so diff each against `themes/PaperMod/` after any submodule bump.
- `extend_head.html` is the theme's head hook. Keep its font links, llms/JSON discovery links, and `jsonld_person.html` include.
- Search needs JSON in `outputs.home`, because `themes/PaperMod/assets/js/fastsearch.js` fetches `../index.json`.
- `params.env: production` in `hugo.yaml` turns on production-only theme output (OG, Twitter, schema, robots index) in every environment, including `hugo server`.
- Pushes to `main` deploy to production. Manual dispatch of `.github/workflows/deploy2pages.yml` also deploys, from whichever ref it runs on. Git-ignored paths (`public/`, `resources/_gen/`, `plans/`, `.github/prompts/`) never reach CI.
- Hero title, subtitle, and image come from `hugo.yaml` `params.profileMode`. The kicker, gloss strip, field card, and route cards are hard-coded in `layouts/partials/index_profile.html`. `profileMode.buttons` is unused.
- Known defect, not yet fixed: talk descriptions start with `Venue: `, and `layouts/index.llmsfull.txt` adds another prefix, so `llms-full.txt` shows `Venue: Venue: ...`.

## Layout
- `content/talks/<YYYY>-<venue>[-n]/` and `content/certifications/<slug>/` are page bundles: `index.md` plus its cover image beside it. Talk and certification `_index.md` files set `type` and `cascade`.
- `layouts/index.llmsfull.txt` generates `llms-full.txt` from hard-coded pages and section names. `static/llms.txt` is hand-written.
- `assets/css/extended/*.css` is concatenated after theme CSS. Put new CSS there.
- Keep `static/CNAME` (custom domain).
- Don't edit generated output (`public/`, `resources/_gen/`) or `themes/PaperMod/`. `.worktrees/papermod-revisions/` is a separate git worktree.

## Code style
- Partials are called by name: `partial "x.html" .`. No imports.
- No formatter. Indentation is per file: 2 spaces in `layouts/_default/*.html` and `assets/css/extended/portfolio-redesign.css`; 4 spaces in `layouts/partials/header.html`, `layouts/partials/index_profile.html`, and `assets/css/common/post-single.css`. Match the file you edit.
- Front matter: double-quote `title` and `description`, unquoted ISO `date`, inline `tags` list, boolean `draft`. `hugo new` writes `draft: true` and single-quoted YAML (`archetypes/default.md`), so fix new files by hand.
- Dark mode is the `body.dark` class, so color rules need a `.dark ...` variant. New custom properties use the `--portfolio-*` prefix.
- Missing params render nothing silently. Guard optional values with `with`, `if`, or `| default`. Check `resources.Get` results for nil, as `layouts/partials/header.html` does.
- Commits: Conventional Commits with optional scope (`feat(seo): ...`, `fix(ui): ...`, `chore: ...`).
