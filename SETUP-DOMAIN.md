# Custom domain setup — ragulmurugan.dev

**Order matters.** Add the DNS records FIRST, then set the
custom domain on GitHub. Setting the custom domain before DNS
resolves makes GitHub 301-redirect the live site to a domain that
does not exist, which takes the site offline.

Right now the site correctly serves from
`https://ragul29.github.io/ragul-portfolio/` and the domain is
not yet configured. Work through the steps below in order.

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

## 2. Point GitHub Pages at the domain

Only after the A records resolve, set the custom domain. This
creates the `CNAME` file in the repo and updates the Pages
setting:

```bash
echo ragulmurugan.dev > CNAME
git add CNAME
git commit -m "Add CNAME for custom domain"
git push
```

GitHub's API can do it too:

```bash
echo '{"cname":"ragulmurugan.dev"}' > /tmp/pages.json
gh api -X PUT repos/Ragul29/ragul-portfolio/pages --input /tmp/pages.json
```

## 3. Wait for the certificate

After the A records resolve, GitHub issues a TLS certificate
automatically. It can take up to ~24 hours, but is usually a few
minutes. Check:

```bash
gh api repos/Ragul29/ragul-portfolio/pages --jq '{cname: .cname, https: .https_enforced, status: .status}'
```

`"https": true` and `"status": "built"` means you are done.

## 4. Enable HTTPS enforcement

Once the certificate exists:

```bash
echo '{"cname":"ragulmurugan.dev","https_enforced":true}' > /tmp/pages.json
gh api -X PUT repos/Ragul29/ragul-portfolio/pages --input /tmp/pages.json
```

Until the certificate exists this call fails with
`"The certificate does not exist yet"` — that is expected, not a
problem with the setup.

## 5. Switch the canonical URL

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
