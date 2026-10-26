# Purple Basil Medspa — Detailed Revenue Leak Audit

Static publish folder for the client report. Deploy on **Vercel** as a static site—no build step.

| File | URL path |
|------|----------|
| `index.html` | `/` — full Detailed Revenue Leak Audit |
| `broken-pages.html` | `/broken-pages.html` — 3 actionable broken URLs (11 orphan 404s documented separately) |
| `citedby-recommended-actions.html` | `/citedby-recommended-actions.html` — ThriveStack AI visibility recommended actions |

## Canonical publish (read this first)

**Production URL:** **https://report-test-viewer.vercel.app/**

| | |
|---|---|
| **GitHub repo** | [Limitless-Enterprise/report-test-viewer](https://github.com/Limitless-Enterprise/report-test-viewer) (this directory when checked out as that repo) |
| **Monorepo path** | `clients/purple-basil-medspa/publish/` in `revenue-leak-audit` |
| **Vercel** | Team **test11-6763**, project **report-test-viewer** |
| **GitHub environment** | **Production – report-test-viewer** only (stale generic **Production** was removed; do not recreate it) |

### DO

1. **Sync HTML from the monorepo client folder** into this publish directory before commit:
   - `index.html` — regenerate with `embed_detailed_html.py` (see below); do **not** raw-copy `detailed-revenue-leak-audit.html`
   - `broken-pages.html`
   - `citedby-recommended-actions.html`
2. **Commit and push to `main` only** in `report-test-viewer`. Vercel’s Git integration deploys production automatically.
3. **Push with GitHub account `dave4272-lle`** (`gh auth switch` if needed). `Dave4272-Office` gets **403** on this repo.
4. Confirm one deployment in [Vercel → report-test-viewer → Deployments](https://vercel.com/test11-6763/report-test-viewer) or GitHub **Deployments** (environment **Production – report-test-viewer**).

### DON'T

- **Do not run `vercel --prod`** (or `npx vercel --prod --scope test11-6763`) for routine releases. Git push + CLI prod both firing caused **double deploys** and duplicate GitHub deployment records for the same commit.
- **Do not** `cp ../detailed-revenue-leak-audit.html index.html` — that copies client-only `broken-pages-action-list.html` links and breaks the live 404 details page.
- **Do not** treat **https://publish-inky-iota.vercel.app/** as production — that URL is not on team **test11-6763** (stray CLI preview project).
- **Do not** add a second Vercel Git import for the same repo (duplicate deploy hooks).

### Release commands (monorepo → publish repo)

From **revenue-leak-audit** root, regenerate `index.html`, copy sibling pages, then push **this repo** (`report-test-viewer`):

```bash
# 1) Regenerate publish/index.html (uses broken-pages.html, not the client action-list path):
python3 scripts/generate-report/embed_detailed_html.py \
  --root . --client purple-basil-medspa

# 2) Copy other live pages into publish/ (paths relative to monorepo):
cp clients/purple-basil-medspa/broken-pages-action-list.html \
   clients/purple-basil-medspa/publish/broken-pages.html
cp clients/purple-basil-medspa/citedby-recommended-actions.html \
   clients/purple-basil-medspa/publish/

# 3) Commit in the publish git repo only:
cd clients/purple-basil-medspa/publish
git add index.html broken-pages.html citedby-recommended-actions.html README.md
git commit -m "Update live report pages"
git push origin main
```

Use `broken-pages.html` naming in publish; the monorepo source file may still be named `broken-pages-action-list.html`.

## First-time setup (Vercel + Git)

1. Push this directory as its own Git repository (`git remote` → `Limitless-Enterprise/report-test-viewer`).
2. [Vercel](https://vercel.com/new) → **Import** that repository under team **test11-6763**.
3. Leave defaults: **Framework Preset** = Other (or auto-detected static). **Build Command** and **Install Command** empty. **Output Directory** = `.` (repository root).
4. Connect **Production Branch** = `main`. After that, only pushes are needed.

If the repo root is the monorepo instead of this folder, set **Root Directory** to `clients/purple-basil-medspa/publish` and keep build/install empty.

## Vercel CLI (emergency only)

Local linking uses `.vercel/` (gitignored). The linked project must be **report-test-viewer** on **test11-6763**.

```bash
cd clients/purple-basil-medspa/publish
npx vercel --scope test11-6763          # preview URL only — OK for smoke tests
# NOT for routine releases:
# npx vercel --prod --scope test11-6763
```

Use CLI prod deploy **only** if Git integration is broken and you need an immediate fix; then restore Git-only workflow.

## GitHub environments (reference)

Vercel now reports deployments to **Production – `{project-name}`** → **Production – report-test-viewer**. That is the only environment operators should see for production.

The old GitHub environment named **Production** (legacy Vercel label, last activity ~Sept 2025) was removed via API—it did not host a separate site. Do not recreate it.
