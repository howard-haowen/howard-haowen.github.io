---
slug: background
title: Project background
role: project background
updated: "2026-11-25T07:29:19"
---

# Project background

## Why

Haowen Jiang's personal portfolio site (https://hw.is-a.dev/), a digital portfolio for profile, work, publications, talks and certifications (per `README.md`). The site is also meant to be discoverable by AI agents (`llms.txt`, `llms-full.txt`, JSON-LD Person, explicit AI-crawler allowances in `robots.txt`), added in the 2026-09-28 `feat(seo)` commits.

## Goals

- Present an AI engineer and linguist profile: hero title, subtitle and route cards (Profile, Work, Speaking, Publications) on the home page.
- Stay fast, responsive and easy to maintain; all content is Markdown.
- Be machine-readable: `llms.txt`, `llms-full.txt`, `index.json` search index, JSON-LD.

## Non-goals

> Low confidence: not stated anywhere in the repo, to be confirmed by the owner.

- Probably not a dynamic or multi-user site: there is no backend, no comments, and no JS/TS source.

## Target user

> Low confidence, inferred from content only.

- Humans: collaborators, employers, event organizers and academics reading the profile, talks and certifications.
- AI agents and crawlers via the discovery files.

See [[optimization-backlog]] for the current maintenance work.
