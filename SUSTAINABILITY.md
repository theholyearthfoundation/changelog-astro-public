# 🌱 Sustainability: what keeps these pages running

<div class="tldr"><i class="ph ph-plant"></i> <span><strong>TL;DR:</strong> Nothing here costs money. What keeps it alive is a short list of things that quietly expire or drift: one publishing token, a ledger token still to be added, two scheduled jobs, a vendored kit, cached link previews, and a shared wallet-connect project. Each is written down below, with what to do when it needs attention. And today one person holds most of the keys, so there's a plan to change that.</span></div>

The Holy Earth Foundation's GitHub home is two public sites built from three repositories. This page
lists what each one needs, so none of it depends on anyone's memory. It's also the repository's
`SUSTAINABILITY.md`, so it reads the same on GitHub.

## The pieces

| Repository | What it is | How it goes live |
| --- | --- | --- |
| [`theholyearthfoundation.github.io`](https://github.com/theholyearthfoundation/theholyearthfoundation.github.io) (public) | The entry portal: `/`, `/donate/`, `/pledge/`, `/map/`, the 404, the `/plants-and-miracles/` short link, and the **arrival kit** every page loads | GitHub Pages, straight from `main` |
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

### 🗝️ The ledger's reading token (`LEDGER_TOKEN`)

The [ledger](https://theholyearthfoundation.github.io/changelog-astro-public/ledger) also shows the
foundation's private work, masked: which repository, which day and what kind of change, never the
words. To read the private repositories it needs a token that can; the publishing token can't. If the
reading token is ever missing, the ledger reads this repository with the workflow's own token, and the
other private rows carry over, still masked, from the last committed snapshot. The page keeps its
shape; it just stops adding new private rows from elsewhere.

**Set since 2026-10-26:**
- **The token:** the fine-grained `ledger-read-thef`, resource owner `theholyearthfoundation`.
- **Reach:** all of the foundation's repositories, with **read-only "Contents"**.
- **No expiration.** The workflow prefers it over `PUBLIC_REPO_TOKEN`.

*To replace it:* make a new read-only token the same way. Set it on `changelog-astro` → Settings →
Secrets → Actions as `LEDGER_TOKEN`, piped from the clipboard
(`pbpaste | gh secret set LEDGER_TOKEN -R theholyearthfoundation/changelog-astro`), and run the
workflow once by hand.

### 🗓️ The daily ledger rebuild

So the ledger stays fresh on days nothing else changes, the publishing workflow also runs **every
day at 10:29 UTC** (`cron: "29 10 * * *"`). It uses a few minutes of the organization's Actions
allowance a day. After the build, a leak check confirms that no private commit's words reached the
site before anything is published. The workflow builds on **Node 24** (since 2026-10-26): Astro 7.4
needs Node 22.23.4 or newer, and a bare `22` resolved to 22.23.3.

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
windows, the donate and pledge pages and the map's cards all come from the **arrival kit** (1.4.0):
`arrival.js`, `arrival.css`, `give/give.js` and `pledge/pledge.js`, copied into the portal's
`arrival/` folder, plus this foundation's own `org.js` (facts and links) and `org.css` (the Canopy
and Morning mist look). The ledger's generator (`astro/src/ledger/ledger.mjs`) is vendored the same
way. The canonical copy lives in [`drasticstatic/my-template`](https://github.com/drasticstatic/my-template)
under `changelog-template/arrival/`. Every org keeps its own copy on purpose, so one org's change
can't break another's.

*What to do:* to upgrade, copy the kit files unchanged from the template, read its porting notes,
and push the portal **before** this AstroLog, which loads the kit from the portal. The pledge pins
its signature checker (`@noble/curves@1.9.7`, `@noble/hashes@1.8.0`, from jsDelivr) and the wallet
window is pinned too (`@reown/appkit-cdn@1.8.24`); upgrade those in the kit, not here.

### 🖼️ Cached link previews

Every page has its own share image, built with the site: Literata on the canopy's deep green, with
the twin vine of sky and gold (Kenney's teachings show their own medallions, and the Plants &
Miracles hub shows the flyer's cover). The porch shows the illustrated card the portal first shipped
with (`astro/src/og/porch-illustrated.jpg`, served as is), so the two front doors look different. WhatsApp, iMessage, Telegram, Slack and LinkedIn keep their
own copy of a link's preview for days or weeks, so after a share image changes, old previews linger.
Cards never carry counts, so a cached one doesn't go stale on its own.

*What to do:* after changing a card, refresh the important links. The organization's
[`SOCIAL-PREVIEWS.md`](https://github.com/theholyearthfoundation/.github/blob/main/SOCIAL-PREVIEWS.md)
says how for each app, and which pages have their own share image.

### 👛 Wallet giving and the pledge

[`/donate/`](https://theholyearthfoundation.github.io/donate/) has the kit's wallet module
(`arrival/give/give.js`): one Connect button (Reown AppKit) for browser wallets, phone wallets by QR
code, and email or social sign-in. [`/pledge/`](https://theholyearthfoundation.github.io/pledge/)
uses the same connection to sign a pledge of support: a message, not a transaction, so it's free,
sends nothing and records nothing, and the page shows no counts. No wallet address is published
anywhere: `give.addresses` stays `null` until the foundation publishes a real treasury address, never
a placeholder.

*What to do:* make sure the Reown project below lists `https://theholyearthfoundation.github.io`;
until it does, both pages calmly say wallet connect isn't switched on for this site yet.

### 🔌 Wallet connect (Reown project)

Every org's wallet features (giving with crypto, and the pledge of support) connect through **one
shared Reown project today: `00b6a1cd528ff580f1fd845ed08b0187`**, made for the O-R-G dApp and held in
Christopher W's Reown account. Its allowlist lists all five org origins plus
`https://drasticstatic.github.io`. If that project is deleted, suspended or runs past its free-plan
limit, wallet connect stops on every site at once (each page then says so calmly; nothing breaks).

*What to do:* the foundation gets its own project with a **one-line `org.js` change**: create a free
project at dashboard.reown.com under an account the foundation controls, add
`https://theholyearthfoundation.github.io` (and the sites moving into the organization) to its
allowlist, and set `give.reownProjectId` in the portal's `arrival/org.js` to the new ID (the pledge
uses it too). Do it before the foundation publishes a treasury address.

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

## 🤝 Stewardship and succession

**Today, one person holds most of the keys.** drasticstatic (Christopher W) owns or administers the
`theholyearthfoundation` GitHub organization, and holds the personal access tokens that publish these
pages (`PUBLIC_REPO_TOKEN`, and the `LEDGER_TOKEN` still to come) and the Reown account above. That's
a single point of failure: if he stepped away, the tokens would expire on schedule and the sites
would quietly stop updating, with nobody else able to renew them. The plan, so these pages outlive
any one person, Christopher included:

1. **At least two owners for the GitHub organization**, from the foundation's own people, each
   with two-factor sign-in. Owners can do everything; everyone else gets the narrowest
   role that works.
2. **Org-owned automation, not personal tokens.** Publishing moves to a GitHub App installed on the
   organization (short-lived tokens, scoped to the repositories it needs), or failing that a
   dedicated bot account whose credentials the owners share. No workflow should depend on one
   person's account.
3. **A token register**: every token, key and secret the sites use, with what it does, which secret
   holds it, who owns it, its scopes and its expiry date, kept where every owner can read it (never
   the values themselves).
4. **A renewal runbook**: for each item in the register, how to renew it, step by step, and a
   reminder a month before each expiry date.
5. **Documented hand-over steps**: how to add an owner, transfer a repository, move a custom domain,
   transfer the Reown project (or create the foundation's own), and remove someone who's leaving.
6. **Exports and backups**: the repositories (with their issues and history) cloned or exported
   somewhere the foundation controls, on a schedule; the same for anything that lives outside GitHub
   (iamoneself.com on Wix, forms, community spaces).

The work is also moving closer to its people: I Am One Self's and David A's sites are being moved
into this organization, and Kenney will start working on them himself, with Codex. Progress on each
step is on the [roadmap](https://theholyearthfoundation.github.io/changelog-astro-public/roadmap).

## Who looks after it

Christopher W, with Alfred (Claude Code), until the steps above are done. Questions go through
[Reach out](https://www.iamoneself.com/about-the-speaker).

*Last reviewed 2026-10-22.*
