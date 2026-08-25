# nur-legal

**TL;DR**

- The privacy policy and terms of service for Nūr, published as a GitHub Pages
  site so the app and both store listings can link to stable URLs.
- Same pattern as `AGMTechnology/holdit-legal`: Jekyll, `minima` theme, one
  markdown file per document with a fixed `permalink`.
- ⚠️ **Read the claim ledger before changing a sentence here.** It lives in the
  **private** docs repo — `nur-docs/legal-claims.md` — because it names
  unshipped tickets and internal review state. Six of its rows are still
  PENDING or OWED.

## The URLs

Live since 2026-08-25, served from `main`, `/`:

| Document | URL |
|---|---|
| Home | `https://agmtechnology.github.io/nur-legal/` |
| Privacy policy | `https://agmtechnology.github.io/nur-legal/privacy-policy` |
| Terms of service | `https://agmtechnology.github.io/nur-legal/terms-of-service` |

Those are the strings the app links to (`src/features/legal/links.ts`) and the
ones that go into App Store Connect and the Play Console.

⚠️ **The permalinks are load-bearing.** They are declared in each document's
front matter, not derived from the filename, so renaming a file does not break a
link that is already live in a shipped app — but changing a `permalink` does.
An app on someone's phone cannot be updated to follow a moved URL.

### ⚠️ When Nūr gets its own domain — read this before touching anything

Ahmed's decision, 2026-08-25: **ship v1 on these `github.io` URLs and move to a
real domain later.** That is safe — but only one route to it is safe, so it is
written down here before somebody takes the other one.

**Do this.** Add the domain to **this** Pages site (Settings → Pages → Custom
domain, which writes a `CNAME` file into this repo). GitHub then serves the site
at the new domain **and 301-redirects the old
`agmtechnology.github.io/nur-legal/…` URLs to it.** Every app already on a phone
keeps working, and both store listings keep resolving. The strings in
`links.ts` can then be updated at leisure, for new installs only.

**Never do this.** Publish the documents as fresh pages on the new website and
repoint `links.ts` at them. That strands the URLs already in shipped apps, and
**no release can fix a phone that already has them.**

Three things must therefore stay true **forever**, not merely until launch:

- the repository stays **public** — a private repo's Pages site needs a paid
  plan, and losing it takes the URLs down;
- the owner and name stay `AGMTechnology/nur-legal` — the path is in the URL;
- the site is never deleted and recreated, which drops the redirect.

## Publishing

```bash
gh repo create AGMTechnology/nur-legal --public --source=. --push
gh api -X POST repos/AGMTechnology/nur-legal/pages -f 'source[branch]=main' -f 'source[path]=/'
```

The repo must be **public** — a private repo's Pages site needs a paid plan, and
a legal document behind a login is not a published legal document.

## Why this is its own repository

Three repos over one working tree, following what `app/` already does:

| Directory | Repository | Visibility |
|---|---|---|
| the root | `AGMTechnology/nur-docs` | private |
| `app/` | `AGMTechnology/nur` | private |
| `legal/` | `AGMTechnology/nur-legal` | **public** |

`nur-docs` ignores both `app/` and `legal/`, so this directory never appears in
its history and there is only ever one copy of a published sentence.

⚠️ The consequence is the same one `app/` has: a change here is committed
**here**, and a session that only commits at the root leaves it untracked.

### ⚠️ This repository is PUBLIC. Everything committed here is readable by anyone.

That is the point — a legal document behind a login is not published. But it
means the ordinary engineering habits do not apply:

- **No internal state.** No ticket numbers, no "not built yet", no review
  status, no TODOs. The claim ledger was briefly committed here and moved out
  for exactly this reason: it recorded that no scholar reviewer is named, under
  terms that promise scholar-reviewed content.
- **No `git` history to hide behind.** A file removed at HEAD is still in the
  history. If something private lands here, rewriting history is the fix and it
  has to happen before anyone clones.
- **Prose only.** If a sentence needs a caveat that cannot be published, the
  sentence is wrong — fix the sentence, do not annotate it.
