# Purple Basil Medspa — Detailed Revenue Leak Audit

Static publish folder for the client report (`index.html` only). Deploy on **Vercel** as a static site—no build step.

## Vercel (Git)

1. Push this directory as its own Git repository (see `git remote` in this folder).
2. [Vercel](https://vercel.com/new) → **Import** that repository.
3. Leave defaults: **Framework Preset** = Other (or auto-detected static). **Build Command** and **Install Command** empty. **Output Directory** = `.` (repository root).
4. Deploy. The report is served at `/` from `index.html`.

If the repo root is the monorepo instead of this folder, set **Root Directory** to `clients/purple-basil-medspa/publish` and keep build/install empty.

## Vercel (CLI)

```bash
cd clients/purple-basil-medspa/publish
npx vercel          # preview
npx vercel --prod   # production
```

## Update the live site

Copy the latest report from the parent folder, then commit and push (Vercel redeploys on push):

```bash
cp ../detailed-revenue-leak-audit.html index.html
git add index.html && git commit -m "Update report" && git push
```
