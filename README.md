# Howard Jiang's Personal Website

This repository contains the source code and content for Howard Jiang's personal website, built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Purpose

The website serves as a digital portfolio, showcasing Howard's education, experience, projects, publications, presentations, certifications, and more. It is designed to be fast, responsive, and easy to maintain.

## Tools & Technologies

- **[Hugo](https://gohugo.io/):** A fast and flexible static site generator written in Go.
- **[PaperMod Theme](https://github.com/adityatelange/hugo-PaperMod):** A clean, feature-rich Hugo theme focused on speed and usability.
- **Markdown:** All content is written in Markdown for simplicity and portability.
- **GitHub Actions:** Used for continuous deployment and workflow automation (see [README.md](README.md) for workflow details).
- **VS Code:** Recommended for editing and managing the project.

## Repository Structure

- `content/` — Main website content (Markdown files for pages and sections)
- `archetypes/` — Archetype templates for new content
- `layouts/` — Custom layout overrides
- `static/` — Static assets (images, files)
- `themes/PaperMod/` — PaperMod theme (as a Git submodule)
- `public/` — Generated site output (ignored in version control)
- `resources/` — Hugo-generated resources (ignored in version control)
- `.github/` — GitHub workflows and issue templates
- `.vscode/` — VS Code settings

## Getting Started

1. **Install Hugo:**  
   [Installation Guide](https://gohugo.io/getting-started/installing/)

1. **Clone the repository:**  
   ```sh
   git clone --recurse-submodules https://github.com/howard-haowen/howard-haowen.github.io.git
   ```

1. **Start the development server**:
   ```sh
   hugo server
   ```

## GitHub Actions

The diagram below illustrates the workflow defined in `.github/workflows/deploy2pages.yml`.

```mermaid
flowchart TD
  %% Triggers
  subgraph "Workflow Triggers"
    direction TB
    T1["Push to main<br><i>on.push.branches: [main]</i>"]:::trigger
    T2["Manual Dispatch<br><i>on.workflow_dispatch</i>"]:::trigger
  end

  %% Permissions & Concurrency
  subgraph "Workflow Settings"
    direction TB
    P1["Permissions:<br>contents: read<br>pages: write<br>id-token: write"]:::perm
    C1["Concurrency:<br>group: pages<br>cancel-in-progress: false"]:::concur
    D1["Defaults:<br>shell: bash"]:::default
  end

  %% Build Job
  subgraph "Build Job"
    direction TB
    B0["Job: build<br>runs-on: ubuntu-latest"]:::job
    Benv["Env:<br>HUGO_VERSION=0.146.0"]:::env
    B1["Install Hugo CLI"]:::step
    B2["Install Dart Sass"]:::step
    B3["Checkout Repo<br>submodules: recursive"]:::step
    B4["Setup Pages<br>id: pages"]:::step
    B5["Install Node.js dependencies<br>npm ci if lockfile exists"]:::step
    B6["Build with Hugo<br>env:<br>HUGO_CACHEDIR, HUGO_ENVIRONMENT=production<br>args: --minify --baseURL"]:::step
    B7["Upload artifact<br>path: ./public"]:::step
  end

  %% Deploy Job
  subgraph "Deploy Job"
    direction TB
    D0["Job: deploy<br>runs-on: ubuntu-latest"]:::job
    Denv["Environment:<br>github-pages"]:::env
    D1["Deploy to GitHub Pages<br>id: deployment"]:::step
    Durl["Outputs:<br>page_url"]:::output
  end

  %% Artifact Flow
  subgraph "Artifacts"
    direction LR
    A1["public/ (Hugo Output)"]:::artifact
    A2["GitHub Pages Artifact"]:::artifact
  end

  %% Relationships
  T1 --> B0
  T2 --> B0
  B0 --> D0
  P1 -.-> B0
  P1 -.-> D0
  C1 -.-> B0
  D1 -.-> B0
  B0 -->|uses| Benv
  D0 -->|uses| Denv
  B0 -->|outputs| A1
  A1 -->|upload| B7
  B7 -->|artifact| A2
  A2 -->|deploy| D1
  D1 -->|outputs| Durl

  %% Build Steps
  B0 --> B1
  B1 --> B2
  B2 --> B3
  B3 --> B4
  B4 --> B5
  B5 --> B6
  B6 --> B7

  %% Deploy Steps
  D0 --> D1
  D1 --> Durl

  %% Styling
  classDef trigger fill:#e0f7fa,stroke:#00838f,stroke-width:2px,font-size:14px;
  classDef perm fill:#fff3e0,stroke:#ef6c00,stroke-width:2px,font-size:13px;
  classDef concur fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px,font-size:13px;
  classDef default fill:#f1f8e9,stroke:#33691e,stroke-width:2px,font-size:13px;
  classDef job fill:#e8eaf6,stroke:#1a237e,stroke-width:3px,font-size:16px;
  classDef step fill:#fffde7,stroke:#fbc02d,stroke-width:2px,font-size:12px;
  classDef env fill:#e0f2f1,stroke:#00695c,stroke-width:2px,font-size:12px;
  classDef artifact fill:#fce4ec,stroke:#ad1457,stroke-width:2px,font-size:12px;
  classDef output fill:#ede7f6,stroke:#4527a0,stroke-width:2px,font-size:12px;

  %% Tooltips and Clicks
  click T1 "https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#push" "Triggered on push to main"
  click T2 "https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#workflow_dispatch" "Manual trigger"
  click P1 "https://docs.github.com/en/actions/using-jobs/assigning-permissions-to-jobs" "GitHub Actions Permissions"
  click C1 "https://docs.github.com/en/actions/using-jobs/using-concurrency" "Concurrency Control"
  click D1 "https://docs.github.com/en/actions/using-jobs/workflow-syntax-for-github-actions#defaultsrun" "Default Shell"
  click B1 "https://github.com/gohugoio/hugo/releases" "Hugo Releases"
  click B2 "https://snapcraft.io/dart-sass" "Dart Sass Snap"
  click B3 "https://github.com/actions/checkout" "actions/checkout"
  click B4 "https://github.com/actions/configure-pages" "actions/configure-pages"
  click B7 "https://github.com/actions/upload-pages-artifact" "upload-pages-artifact"
  click D1 "https://github.com/actions/deploy-pages" "actions/deploy-pages"
  click A2 "https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts" "GitHub Actions Artifacts"
```