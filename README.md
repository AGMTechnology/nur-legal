# nur-legal

**TL;DR**

- The privacy policy and terms of service for Nūr, published as a GitHub Pages
  site so the app and both store listings can link to stable URLs.
- Same pattern as `AGMTechnology/holdit-legal`: Jekyll, `minima` theme, one
  markdown file per document with a fixed `permalink`.
- **Read `CLAIMS.md` before publishing.** Six claims in these documents are not
  true yet. They describe v1 tickets that have not shipped.

## The URLs

Once Pages is enabled on `main`, `/` :

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

### ⚠️ Not yet true — the interim state

`AGMTechnology/nur-legal` **does not exist yet**. Creating a public repository
under Ahmed's org and publishing legal text about his app is his call, not an
agent's, so it has not been done.

Until it is, this directory is **tracked inside `nur-docs`** so the work is not
sitting untracked on a disk. Cutting over is three commands and must happen in
this order, or the table above becomes a lie:

```bash
gh repo create AGMTechnology/nur-legal --public --source=legal --push
git rm -r --cached legal          # in nur-docs
echo "legal/" >> .gitignore       # in nur-docs
```

Until that cutover, the URLs in `src/features/legal/links.ts` **404**. The app
must not ship while they do — that is a row in `CLAIMS.md`.
