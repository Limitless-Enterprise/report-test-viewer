# Purple Basil Medspa — Detailed Revenue Leak Audit

Static publish folder for the client report. Deploy on **Vercel** as a static site—no build step.

| File | URL path |
|------|----------|
| `index.html` | `/` — full Detailed Revenue Leak Audit |
| `broken-pages.html` | `/broken-pages.html` — 3 actionable broken URLs (11 orphan 404s documented separately) |
| `citedby-recommended-actions.html` | `/citedby-recommended-actions.html` — ThriveStack AI visibility recommended actions |

## Canonical production URL

**https://report-test-viewer.vercel.app/** — Vercel project `report-test-viewer`, team **test11-6763**, GitHub repo **Limitless-Enterprise/report-test-viewer**.

## Update the live site (preferred)

Use **Git push only**. Vercel’s Git integration redeploys production on every push to `main`. Do not run `vercel --prod` for normal publishes (it creates an extra Vercel deployment and can register a second GitHub deployment for the same commit).

```bash
# Regenerate publish/index.html (uses broken-pages.html, not the client action-list path):
python3 ../../../scripts/generate-report/embed_detailed_html.py \
  --root ../../../ --client purple-basil-medspa
git add -A && git commit -m "Update report" && git push origin main
```

Do **not** `cp ../detailed-revenue-leak-audit.html index.html` — that copies client-only
`broken-pages-action-list.html` links and breaks the live 404 details page.

Watch the deploy in [Vercel → report-test-viewer → Deployments](https://vercel.com/test11-6763/report-test-viewer) or GitHub **Actions / Deployments** (environment **Production – report-test-viewer**).

## First-time setup (Vercel + Git)

1. Push this directory as its own Git repository (`git remote` points at `Limitless-Enterprise/report-test-viewer`).
2. [Vercel](https://vercel.com/new) → **Import** that repository under team **test11-6763**.
3. Leave defaults: **Framework Preset** = Other (or auto-detected static). **Build Command** and **Install Command** empty. **Output Directory** = `.` (repository root).
4. Connect **Production Branch** = `main`. After that, only pushes are needed.

If the repo root is the monorepo instead of this folder, set **Root Directory** to `clients/purple-basil-medspa/publish` and keep build/install empty.

## Vercel CLI (emergency only)

Local linking uses `.vercel/` (gitignored). The linked project should be **report-test-viewer** on **test11-6763** — not ad-hoc preview projects on other teams (e.g. stray `publish-*.vercel.app` URLs from `vercel` without `--scope`).

```bash
cd clients/purple-basil-medspa/publish
npx vercel --scope test11-6763          # preview URL only — OK for smoke tests
# Avoid for routine releases:
# npx vercel --prod --scope test11-6763
```

Use CLI prod deploy only if Git integration is broken and you need an immediate fix; then restore Git-only workflow.

## GitHub “two Production environments” (historical)

Vercel used to report deployments to a GitHub environment named **Production**. Newer integrations use **Production – `{project-name}`** (here: **Production – report-test-viewer**). Both names referred to the **same** Vercel project; the sidebar showed two environments because the old one was never removed.

**Canonical GitHub environment:** **Production – report-test-viewer** (matches current Vercel GitHub App behavior).

**Stale environment:** **Production** (last activity ~Sept 2025). Safe to delete — it does not host a separate site; it was only a GitHub deployment label.

To remove manually: repo **Settings → Environments → Production → Delete environment** (requires admin). Deleting it does not change https://report-test-viewer.vercel.app/.

Do **not** add a second Vercel Git connection for the same repo (duplicate imports create duplicate deploy hooks).
