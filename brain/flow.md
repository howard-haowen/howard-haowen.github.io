---
slug: flow
title: Key flows
role: key flows
updated: "2026-11-25T07:29:20"
---

# Key flows

## Authoring to production

```mermaid
sequenceDiagram
  participant Dev as Author
  participant Local as hugo server
  participant Git as GitHub (main)
  participant CI as deploy2pages.yml
  participant Pages as GitHub Pages
  Dev->>Local: edit content/, layouts/, assets/
  Local-->>Dev: preview at localhost:1313
  Dev->>Git: commit and push to main
  Git->>CI: trigger (also workflow_dispatch)
  CI->>CI: install Hugo 0.166.0, checkout with submodules
  CI->>CI: HUGO_ENVIRONMENT=production hugo --minify --baseURL https://hw.is-a.dev/
  CI->>Pages: upload ./public artifact and deploy
  Pages-->>Dev: live at hw.is-a.dev
```

## Build-time rendering

- `hugo.yaml` and `content/` feed the PaperMod templates; project overrides in `layouts/` win over theme files at the same path.
- Home outputs: HTML, RSS, `index.json` (search) and `llms-full.txt`.
- `llms-full.txt` is generated from hard-coded pages and sections in `layouts/index.llmsfull.txt`; a new section must be added there and in `static/llms.txt`.

## Known defect

Talk descriptions start with `Venue: ` and the llmsfull template adds another prefix, so `llms-full.txt` shows `Venue: Venue: ...`.
