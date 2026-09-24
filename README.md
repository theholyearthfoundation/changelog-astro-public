# THEF Changelog — private source

Private source for The Holy Earth Foundation's public changelog. Built on the fleet-wide pattern
documented at `my-template/changelog-template/README.md` — read that first, it's the actual spec
(frontmatter schema, the confirmed-done-goes-straight-to-changelog convention, the org-vs-personal
push-target rule).

Public output (compiled static site only, no source): `theholyearthfoundation/changelog-astro-public`,
deployed via GitHub Pages, synced from this repo by `.github/workflows/sync-public.yml` on every
push to `main`.

## Adding an entry

New file in `changelog/`, filename `YYYY-MM-DD-short-slug.md`, frontmatter:

```yaml
---
title: "Plain-language title"
date: "YYYY-MM-DD"
priority: "P1-high" # or P2-medium (default), P3-low
org: "thef"
---
```

Write it for someone outside the foundation who cares about the work, not for the team who already
knows the context. If something can't be described publicly, it just doesn't get an entry — the
private technical record (each site's own `PENDING-TASKS.md`/`AGENT-SYNC` docs) stays exactly where
it already is.

## Local development

```sh
cd astro
npm install
npm run dev
```

---

Co-Authored-By: Alfred · ClaudeCodeCLI · Anthropic [Sonnet-5]
