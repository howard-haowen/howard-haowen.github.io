---
id: use-uv-for-python
title: "Always run Python through uv, never the system Python"
category: decision
status: active
tags: [python, uv, tooling]
created: "2026-11-25T08:08:00"
updated: "2026-11-25T08:08:01"
---

## compiled_truth

## Decision
Always run Python code through `uv` (for example `uv run script.py`, `uv run --with <pkg> python ...`, `uvx <tool>`). Never invoke the system Python (`python`, `python3`, `/usr/bin/python3`) directly, and never `pip install` into it.

## Rationale
- Keeps runs reproducible and isolated from the machine's system interpreter and global packages.
- Missing packages are handled per run with `uv run --with`, so nothing is installed globally.

## Scope
Applies to every agent and contributor working in this repository, including one-off scripts, inline snippets (`uv run python -c ...`), and checks such as parsing YAML or JSON.


## timeline

- time: 2026-11-25T08:08:00
  kind: decision
  summary: "Created this page: Always run Python through uv, never the system Python"
  source: User instruction on 2026-11-25
  affects: [use-uv-for-python]

- time: 2026-11-25T08:08:01
  kind: decision
  summary: "Record the rule: always use uv to run Python, never the system Python"
  source: User instruction on 2026-11-25
  affects: [use-uv-for-python]
