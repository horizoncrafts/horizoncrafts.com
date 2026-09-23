# horizoncrafts.com

Static site. Plain HTML + one CSS file + four font files. No build step, no JavaScript, no analytics, no third-party requests.

## Layout

- `index.html` — the whole page (headline, how I work, offers, evidence, writing, contact)
- `essays/*.html` — one file per essay; copy the structure of an existing one
- `style.css` — the only stylesheet; landing page in Space Grotesk + Inter, essays in a serif; light/dark follows the OS
- `fonts/` — four self-hosted woff2 subsets (latin + latin-ext for Polish). Never replace with a Google Fonts link: GDPR, and the footer promises no third-party requests
- `img/` — (create when needed) the portrait goes here as `portrait.jpg`; see the comment in `index.html`
- `CNAME` — custom domain for GitHub Pages

## Preview locally

```bash
ruby -run -e httpd . -p 8123
```

then open http://localhost:8123. (`python3 -m http.server 8123` works too, once the Xcode licence has been accepted.)

## Deploy (GitHub Pages, keeps DNS at OVH)

1. Create repo `horizoncrafts/horizoncrafts.com` (public), push this folder as `main`.
2. Repo → Settings → Pages → Source: *Deploy from a branch*, `main` / `/ (root)`.
   Custom domain: `horizoncrafts.com` (the `CNAME` file already says so). Tick *Enforce HTTPS* once the certificate is issued (can take up to an hour after DNS is right).
3. At OVH, in the DNS zone for `horizoncrafts.com`:
   - remove the existing `A` records pointing at `216.239.3x.21` (the old Google Sites redirect);
   - add four `A` records for the apex: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`;
   - change `www` from `CNAME ghs.googlehosted.com` to `CNAME horizoncrafts.github.io`;
   - **leave every mail record alone** — the `MX` records, the `v=spf1` TXT, the `google-site-verification` TXT, `google._domainkey` (DKIM) and `_dmarc`. Mail authentication is verified working; deleting any of these breaks it silently. The full list lives in the private brief, not in this repo.
4. Unpublish the old Google Sites page so it doesn't linger on any other alias.

Verify: `dig +short horizoncrafts.com A` shows the four GitHub addresses; `curl -I https://horizoncrafts.com` returns 200.
