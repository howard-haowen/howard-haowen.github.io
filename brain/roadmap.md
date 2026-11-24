---
slug: roadmap
title: Roadmap
role: milestones
updated: "2026-11-25T07:29:20"
---

# Roadmap

> Low confidence: the repo contains no owner-stated milestones or dates. This page only reflects the optimization backlog from the 2026-11-24 review, in a suggested order with no schedule.

```mermaid
graph LR
  done["Done: Hugo 0.166.0 in CI, locale key, ignore tmp/ and .bob/, AGENTS.md, brain"] --> s1["1. Delete identical share_icons.html"]
  s1 --> s2["2. Upgrade PaperMod in a worktree, verify WARNs gone"]
  s2 --> s3["3. Shrink override scope"]
  s3 --> s4["4. Trim CI: Dart Sass, Node step, Hugo checksum"]
  s4 --> s5["5. Fix llms-full.txt 'Venue: Venue:'"]
  s5 --> s6["6. Review static/pdf (21M)"]
```

Details and reasoning live in [[optimization-backlog]]. Baseline tag: `pre-optimization-baseline`.
