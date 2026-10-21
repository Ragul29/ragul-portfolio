# Custom domain setup — ragulmurugan.dev

GitHub Pages is already configured to serve this site from
`ragulmurugan.dev` (the `CNAME` file is committed and the Pages
`cname` setting is set). The only remaining step is DNS, which has
to be done at your domain registrar.

## 1. Add the DNS records

In the DNS zone for `ragulmurugan.dev` (wherever you bought the
domain — Namecheap, GoDaddy, Cloudflare, etc.), add:

| Type | Name | Value |
| --- | --- | --- |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `ragul29.github.io` |

The four A records are GitHub's Pages IPs. All four should be
present — GitHub rotates across them.

If your provider has an "ALIAS"/"ANAME" or "apex" record type,
an ALIAS to `ragul29.github.io` also works.

## 2. Wait for the certificate

After the A records resolve, GitHub issues a TLS certificate
automatically. It can take up to ~24 hours, but is usually a few
minutes. Check:

```bash
gh api repos/Ragul29/ragul-portfolio/pages --jq '{cname: .cname, https: .https_enforced, status: .status}'
```

`"https": true` and `"status": "built"` means you are done.

## 3. Enable HTTPS enforcement

Once the certificate exists:

```bash
echo '{"cname":"ragulmurugan.dev","https_enforced":true}' > /tmp/pages.json
gh api -X PUT repos/Ragul29/ragul-portfolio/pages --input /tmp/pages.json
```

Until the certificate exists this call fails with
`"The certificate does not exist yet"` — that is expected, not a
problem with the setup.

## 4. Switch the canonical URL

The page currently declares its canonical as the GitHub Pages
address, which is correct and safe while the domain is being set
up. Once `ragulmurugan.dev` is serving over HTTPS, update these
four places in `index.html`:

- `<link rel="canonical">`
- `<meta property="og:url">`
- `<meta property="og:image">` and `<meta name="twitter:image">`
- `"url"` inside the JSON-LD `Person` block

Then also update `robots.txt` and `sitemap.xml` to use the new
domain.

> Keep the `github.io` URL live as-is. A canonical plus a
> consistent set of URLs is what matters; a redirect from
> `github.io` to the custom domain is optional but nice.

## Where the site lives

- Repo: https://github.com/Ragul29/ragul-portfolio
- Pages: https://ragul29.github.io/ragul-portfolio/
- Vercel (preview): https://ragul-dev-xi.vercel.app
