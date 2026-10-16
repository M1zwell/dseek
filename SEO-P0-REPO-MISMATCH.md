# dseek.ai SEO P0 — this repository is not the deployed app

Checked 2026-10-16 against live `https://dseek.ai` and `M1zwell/dseek` @ `25f9aa2`.

## Finding

`M1zwell/dseek` (this checkout) is a stub. The only tracked file before this note was `README.md` (`# dseek`). There is no Vite app, no `index.html`, no `public/`, no `netlify.toml`, and no route for `/seek`.

The site that answers `dseek.ai` is a different Netlify-hosted Vite SPA (Cloudflare in front). Its shell loads `/cesium/Widgets/widgets.css` and `/assets/index-DkW2UfFw.js`. A catch-all rewrite serves that HTML shell for `/robots.txt`, `/sitemap.xml`, and `/og.png`.

This workspace is not a monorepo, so there is no app folder here to patch. The GitHub credential for this run can see only `M1zwell/dseek`. Public siblings do not contain the live app:

- `M1zwell/dseek20250506` — empty StackBlitz repo, last push 2025-05-05.
- `M1zwell/yok-zhang` — ichina.co garden. Its README states the dseek mark in that repo is not deployed to dseek.ai. It has `public/marks/dseek.svg` only.

The live bundle names these chunks, which identify the real app once that repo is opened: `LensSwitcher`, `VenueCesiumViewer`, `YouthFootballAcademyLanding`, `SSOCallback`, `JubitCallback`, `AuthModal`, `TapePage`, `useParkingVacancyWatch`. Static head copy to search for: `Dseek — Data Intelligence Platform`.

No SEO files were added here. A `robots.txt` in this stub would not change what `dseek.ai` serves.

## Live checks (2026-10-16)

| URL | Status | Content-Type | What came back |
|-----|--------|--------------|----------------|
| `https://dseek.ai/robots.txt` | 200 | `text/html` | SPA shell, not robots rules |
| `https://dseek.ai/sitemap.xml` | 200 | `text/html` | SPA shell, not XML |
| `https://dseek.ai/og.png` | 200 | `text/html` | SPA shell, not an image |
| `https://dseek.ai/` | 200 | `text/html` | title `Dseek — Data Intelligence Platform`; description present; no `og:*`, no canonical, no `twitter:card` |
| `https://dseek.ai/seek` | 200 | `text/html` | same title and description as `/` |
| `https://dseek.ai/hk` | 301 | `text/plain` | `Location: /data/life/isometric/` then HTML 200 |

`/hk` is live as a redirect into the isometric lab, not as a public-search lander. The product URL for the one-step Hong Kong search head is `/seek`.

## Where to apply the P0 fix

Open the repository that builds this Vite app and deploy it to the Netlify site behind `dseek.ai`. In that app:

1. Serve real `robots.txt` (`text/plain`) and `sitemap.xml` (`application/xml`) ahead of the SPA fallback. Allow public pages. List at least `https://dseek.ai/` and `https://dseek.ai/seek`. Include `https://dseek.ai/hk` only if that URL should stay in the index; today it 301s to `/data/life/isometric/`.
2. Give `/seek` its own title, description, `og:title`, `og:description`, `og:url`, `og:image`, `twitter:card`, and canonical `https://dseek.ai/seek`. Suggested title: `Seek Hong Kong public info in one step — dseek`. Suggested description: one box for Hong Kong company and public-info clues; free and simple; a match is a clue, open the official page. Optional Chinese title: `一步查香港公开资料 — dseek`.
3. Add the same OG and Twitter tags on `/`. Ship a branded still at a stable HTTPS path, about 300 KB or smaller (1200×630).

Brand: dseek only. Do not introduce Oyin or Sixie. Add a personal / Stonelite line only where a legal footer already exists.

## Verification after the real app is patched

```bash
curl -sI https://dseek.ai/robots.txt | grep -i content-type
curl -s https://dseek.ai/robots.txt
curl -sI https://dseek.ai/sitemap.xml | grep -i content-type
curl -s https://dseek.ai/sitemap.xml
curl -s https://dseek.ai/seek | grep -E '<title>|description|og:|twitter:|canonical'
curl -sI https://dseek.ai/og.png | grep -iE 'content-type|content-length'
```

Expect `text/plain` and `Allow` rules, `application/xml` (or `text/xml`) with `<loc>https://dseek.ai/</loc>` and `<loc>https://dseek.ai/seek</loc>`, a `/seek` title that is not `Dseek — Data Intelligence Platform`, and an image content type with a body under about 300 KB.
