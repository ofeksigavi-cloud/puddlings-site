# puddlings.app website

Plain HTML and CSS. No framework, no build step, no JavaScript. Everything in this folder is the site; upload it as it is.

| File | What it is |
|---|---|
| `index.html`, `privacy.html`, `support.html`, `404.html` | The pages |
| `style.css` | The only stylesheet (light and dark mode) |
| `assets/pets/` | Pet sprites copied from `Characters/` (64px native, shown at 2x or 3x) |
| `assets/fonts/` | Tiny5 pixel font and its `OFL.txt` licence (keep both) |
| `assets/og.png` | Share image (1200x630). Rebuild with `swift tools/make_og_image.swift` from the repo root |
| `favicon.png`, `apple-touch-icon.png` | The frog's face |
| `CNAME`, `.nojekyll` | Only used by GitHub Pages |

Links use paths from the root (`/privacy.html`), so open the site through a server, not by double-clicking a file.

## Preview locally

```bash
cd site
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Deploy

Not done yet. Ofek decides when.

### Option A (recommended): GitHub Pages

1. Make a new public repo, for example `puddlings-site`, and put the contents of this `site/` folder at its root. (Use a separate repo so the PRD and plans stay private; Pages on a private repo needs a paid plan.)
2. Repo Settings > Pages > Deploy from a branch > `main` / root.
3. Same page > Custom domain: `puddlings.app`. Tick Enforce HTTPS when it becomes available (up to an hour after DNS works).
4. At Porkbun (Domain Management > puddlings.app > DNS), remove any default parking ALIAS/CNAME records, then add:

| Type | Host | Answer |
|---|---|---|
| A | (blank) | 185.199.108.153 |
| A | (blank) | 185.199.109.153 |
| A | (blank) | 185.199.110.153 |
| A | (blank) | 185.199.111.153 |
| AAAA | (blank) | 2606:50c0:8000::153 |
| AAAA | (blank) | 2606:50c0:8001::153 |
| AAAA | (blank) | 2606:50c0:8002::153 |
| AAAA | (blank) | 2606:50c0:8003::153 |
| CNAME | www | `<your-github-username>.github.io` |

Leave the MX and TXT records for the hello@puddlings.app email forwarding alone. These GitHub addresses are the ones GitHub documents today; check [GitHub's custom domain page](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site) before adding them.

### Option B: Cloudflare Pages

1. Cloudflare dashboard > Workers & Pages > Create > Pages. Either connect a repo with build command empty and output directory `site`, or upload the folder (`npx wrangler pages deploy site --project-name puddlings`).
2. Project > Custom domains > add `puddlings.app` and `www.puddlings.app`.
3. Cloudflare needs to be the DNS host for the bare domain. In Cloudflare, add the site, then at Porkbun change the nameservers to the two Cloudflare gives you. Cloudflare then creates the DNS records for the custom domain itself.
4. Warning: this moves DNS away from Porkbun, so Porkbun's email-forwarding records stop applying. Re-create them in Cloudflare (or use Cloudflare Email Routing) before switching, or hello@puddlings.app stops arriving.
5. Without moving nameservers you can only point `www`: Porkbun CNAME `www` to `<project>.pages.dev`.

### After it is live

- Put the final URLs into App Store Connect: privacy policy `https://puddlings.app/privacy.html`, support `https://puddlings.app/support.html`.
- Replace the "Coming soon" badge on `index.html` with the official App Store badge and link (search the file for `TODO`).
- Search all pages for `TODO` and settle each one first: `grep -rn TODO site --include=*.html`.
