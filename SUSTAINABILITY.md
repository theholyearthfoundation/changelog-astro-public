# 🌱 Sustainability: what keeps these pages running

<div class="tldr"><i class="ph ph-plant"></i> <span><strong>TL;DR:</strong> Nothing here costs money. What keeps it alive is a short list of things that quietly expire or drift: one publishing token, a ledger token still to be added, two scheduled jobs, a vendored kit, cached link previews, and (later) a wallet allowlist. Each is written down below, with what to do when it needs attention.</span></div>

The Holy Earth Foundation's GitHub home is two public sites built from three repositories. This page
lists what each one needs, so none of it depends on anyone's memory. It's also the repository's
`SUSTAINABILITY.md`, so it reads the same on GitHub.

## The pieces

| Repository | What it is | How it goes live |
| --- | --- | --- |
| [`theholyearthfoundation.github.io`](https://github.com/theholyearthfoundation/theholyearthfoundation.github.io) (public) | The entry portal: `/`, `/donate/`, `/map/`, the 404, the `/plants-and-miracles/` short link, and the **arrival kit** every page loads | GitHub Pages, straight from `main` |
| `changelog-astro` (private) | The source of this AstroLog: entries, write-ups, Kenney's Teachings, the Astro site | A workflow builds it and publishes **only the finished pages** |
| [`changelog-astro-public`](https://github.com/theholyearthfoundation/changelog-astro-public) (public) | The built AstroLog you're reading | GitHub Pages, from what the workflow pushes |

## What can quietly stop, and what to do

### 🔑 The publishing token (`PUBLIC_REPO_TOKEN`)

The AstroLog is published by a workflow in the private source repository, which pushes the finished
pages into `changelog-astro-public` with a **classic personal access token** stored as the secret
`PUBLIC_REPO_TOKEN` (set on 2026-10-05). Classic tokens expire. When it does, **the site stops
updating without any visible sign on the site itself**: new entries, the daily ledger and every fix
simply stop arriving, and the pages keep showing the last good build.

*What to do:* look up its expiry date (GitHub → Settings → Developer settings → Personal access
tokens) and put a reminder a week before it. To renew, make a new classic token with the `repo` and
`workflow` scopes, paste it into `changelog-astro` → Settings → Secrets → Actions →
`PUBLIC_REPO_TOKEN`, then run the workflow by hand once (Actions → Sync Public Changelog → Run
workflow) and check that the [changelog](https://theholyearthfoundation.github.io/changelog-astro-public/changelog) shows the newest entry.

### 🗝️ The ledger's reading token (`LEDGER_TOKEN`): not set yet

The [ledger](https://theholyearthfoundation.github.io/changelog-astro-public/ledger) also shows the
foundation's private work, masked: which repository, which day and what kind of change, never the
words. To read the private repositories it needs a token that can. The publishing token can't read
them, so for now the ledger reads this repository with the workflow's own token, and the other
private rows carry over, still masked, from the last committed snapshot. The page keeps its shape;
it just stops adding new private rows from elsewhere.

*What to do (Christopher's to-do):* make a **fine-grained** token, resource owner
`theholyearthfoundation`, with **read-only "Contents"** on the private repositories the ledger lists
(`astro/src/data/ledger.config.mjs` → `private`), and add it to `changelog-astro` → Settings → Secrets →
Actions as `LEDGER_TOKEN`. The workflow prefers it over `PUBLIC_REPO_TOKEN` as soon as it exists.
It expires like any token: renew it the same way.

### 🗓️ The daily ledger rebuild

So the ledger stays fresh on days nothing else changes, the publishing workflow also runs **every
day at 10:29 UTC** (`cron: "29 10 * * *"`). It uses a few minutes of the organization's Actions
allowance a day. After the build, a leak check confirms that no private commit's words reached the
site before anything is published.

*What to do:* if the ledger's newest day looks old, open the workflow's run history. A failed
scheduled run usually means the token above, or GitHub's API having a bad moment (the build then
falls back to the committed snapshot and still publishes).

### 🪞 The hourly README mirror

The portal's `README.md` is a copy of the organization profile (`theholyearthfoundation/.github`),
refreshed **every hour** by a workflow in the portal repository. GitHub switches off scheduled
workflows in a public repository after 60 days without activity.

*What to do:* edit the profile, never the portal's README. If the copy stops following, re-enable
the "Mirror org profile README" workflow in the portal's Actions tab.

### 🧰 The arrival kit is vendored

The navbar, the light and motion controls, search, the welcome guide, the contact and community
windows, the donate page and the map's cards all come from the **arrival kit**: `arrival.js` and
`arrival.css`, copied into the portal's `arrival/` folder, plus this foundation's own `org.js`
(facts and links) and `org.css` (the Canopy and Morning mist look). The canonical copy lives in
[`drasticstatic/my-template`](https://github.com/drasticstatic/my-template) under
`changelog-template/arrival/`. Every org keeps its own copy on purpose, so one org's change can't
break another's.

*What to do:* to upgrade, copy the two kit files unchanged from the template, read its porting
notes, and push the portal **before** this AstroLog, which loads the kit from the portal.

### 🖼️ Cached link previews

Every page has its own share image, built with the site: Literata on the canopy's deep green, with
the twin vine of sky and gold (Kenney's teachings show their own medallions, and the Plants &
Miracles hub shows the flyer's cover). WhatsApp, iMessage, Telegram, Slack and LinkedIn keep their
own copy of a link's preview for days or weeks, so after a share image changes, old previews linger.
Cards never carry counts, so a cached one doesn't go stale on its own.

*What to do:* after changing a card, refresh the important links. The organization's
[`SOCIAL-PREVIEWS.md`](https://github.com/theholyearthfoundation/.github/blob/main/SOCIAL-PREVIEWS.md)
says how for each app, and which pages have their own share image.

### 👛 Wallet giving, later

The "give with crypto" slot on [`/donate/`](https://theholyearthfoundation.github.io/donate/) is empty
on purpose, and no wallet address is published anywhere. The wallet-connect module that will fill
it (Reown AppKit, with Web3Auth for email sign-in) only loads on domains its projects allow.

*What to do:* when it's switched on, add `https://theholyearthfoundation.github.io` to the Reown
project's domain allowlist and to Web3Auth's, with a real client ID (never a demo one).

### 🔗 Links that leave GitHub

The navbar, the contact and community windows and the
[map](https://theholyearthfoundation.github.io/changelog-astro-public/map) point at iamoneself.com,
retreats.iamoneself.com and David A's site (its public preview on `drasticstatic.github.io`).

*What to do:* if one of those addresses changes (David's site moving to a domain of its own, say),
update the portal's `arrival/org.js` (one place for the navbar and windows) and this AstroLog's map
(`astro/src/data/map.ts`) and page cards (`astro/src/og/pages.ts`).

### 📦 Dependencies

Astro, Satori and sharp are pinned by `package-lock.json`, and the workflow installs exactly those
(`npm ci`). The share-card fonts (Literata, OFL) are committed, so a build needs no font download.
PapyrusStd stays on the sites, with its credit, and off the cards. Upgrades are deliberate: update,
build locally, check the pages and cards, then push.

## Who looks after it

Christopher W, with Alfred (Claude Code). Questions go through
[Reach out](https://www.iamoneself.com/about-the-speaker).

*Last reviewed 2026-10-13.*
