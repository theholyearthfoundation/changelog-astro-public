# 🌎 The Holy Earth Foundation — Changelog & Library

> A running, public-safe record of what's shipped across The Holy Earth Foundation's sites — plus a
> growing library of the deeper technical write-ups behind it.

[![License: MIT](https://img.shields.io/badge/license-MIT-lightgrey?style=flat)](LICENSE)
[![Live Site](https://img.shields.io/badge/%F0%9F%8C%90%20Live-changelog--astro--public-C99A3A)](https://theholyearthfoundation.github.io/changelog-astro-public/)
[![Sync](https://github.com/theholyearthfoundation/changelog-astro/actions/workflows/sync-public.yml/badge.svg)](https://github.com/theholyearthfoundation/changelog-astro/actions/workflows/sync-public.yml)
[![Built with Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code%20CLI-blueviolet)](https://code.claude.com/docs/en/overview)

---

**🌐 [Browse the live changelog & library →](https://theholyearthfoundation.github.io/changelog-astro-public/)**

---

## 👋 What this is

Private source for the foundation's public changelog — a plain-language, dated record of what's
shipped across [iamoneself.com](https://www.iamoneself.com) and David Amaringo's site, plus a
`/library` of deeper technical write-ups for anyone curious how it's all built. Written for someone
outside the foundation who cares about the work, not for the team who already knows the context.

This README is also rendered as the live site's own homepage.

## 🗂️ What's here

- `changelog/` — one markdown file per entry, `YYYY-MM-DD-short-slug.md`.
- `library/` — deeper, standalone technical docs that don't belong to one changelog entry.
- `astro/` — the Astro site itself.
- `.github/workflows/sync-public.yml` — builds and syncs the compiled site (no source) to
  [`theholyearthfoundation/changelog-astro-public`](https://github.com/theholyearthfoundation/changelog-astro-public)
  on every push to `main`.

Built on the fleet-wide pattern documented at `my-template/changelog-template/README.md` — read
that first, it's the actual spec (frontmatter schema, the confirmed-done-goes-straight-to-changelog
convention, the org-vs-personal push-target rule).

## ✍️ Adding an entry

New file in `changelog/`, filename `YYYY-MM-DD-short-slug.md`:

```yaml
---
title: "Plain-language title"
date: "YYYY-MM-DD"
priority: "P1-high" # or P2-medium (default), P3-low
repo: "iamoneself" # which project this entry belongs to
---
```

If something can't be described publicly, it just doesn't get an entry — the private technical
record (each site's own `PENDING-TASKS.md`/`AGENT-SYNC` docs) stays exactly where it already is.

## 💻 Local development

```sh
cd astro
npm install
npm run dev
```

---

*Maintained by [theholyearthfoundation](https://github.com/theholyearthfoundation) · w/ Anthropic's Claude Code CLI*

Co-Authored-By: Alfred · ClaudeCodeCLI · Anthropic [Sonnet-5]
