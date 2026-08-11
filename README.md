# eazy.chat

Static site for **eazy.chat**, the customer-support platform operated by
ITS - Innovative Tech Solutions, Unipessoal, Lda.

Four pages. It exists to state plainly who operates the platform and to publish the
legal disclosures that Meta App Review and the GDPR require.

| Route            | File                       | Purpose                                            |
| ---------------- | -------------------------- | -------------------------------------------------- |
| `/`              | `index.html`               | Landing page                                       |
| `/privacy`       | `privacy/index.html`       | Privacy Policy → Meta "Privacy Policy URL"          |
| `/terms`         | `terms/index.html`         | Terms of Service → Meta "Terms of Service URL"      |
| `/data-deletion` | `data-deletion/index.html` | → Meta "Data Deletion Instructions URL"            |
| —                | `404.html`                 | Not-found page                                     |

**These three URLs must never change.** They go into the Meta app dashboard.

## How it is built

Hand-written HTML and one stylesheet. No build step, no framework, no dependencies,
no JavaScript required for anything on the page.

**Hard rule: zero external network requests.** The typeface is self-hosted, icons are
inline SVG, there is no CDN, no analytics, no cookies. This is not a preference — the
Privacy Policy asserts it, and anyone can verify it in DevTools in ten seconds. Do not
add a font CDN, an analytics snippet or an embedded widget without changing
`privacy/index.html` section 3 to match.

- Typeface: **Archivo** (SIL Open Font License, `assets/fonts/`, licence included)
- Colour: neutral greys with the brand teal `#29A2A7` as the single accent
  (darkened to `#0e7c81` where it carries text, for AA contrast)
- Light and dark via `prefers-color-scheme`; `prefers-reduced-motion` respected
- The site is `noindex` (`robots.txt` + meta tag) — reachable by direct URL, kept out
  of search results

## Editing

Company details, the contact address and the effective date appear in several places.
When one changes, grep for it — it is in the footer of all five pages plus the body of
the legal pages:

```bash
grep -rn "PT515363677\|outbound@eazy.chat\|2026-08-11" --include="*.html" .
```

## Local preview

```bash
python -m http.server 8765
```

Then open <http://localhost:8765>. Clean URLs (`/privacy` rather than `/privacy.html`)
work because each page is an `index.html` inside its own directory.

## Deploying

GitHub Pages serves `main` from the repository root. Push and it redeploys in about a
minute. `CNAME` pins the custom domain; `.nojekyll` stops Jekyll from touching the
files.

DNS lives in Cloudflare:

| Type    | Name  | Value                                                                                        |
| ------- | ----- | -------------------------------------------------------------------------------------------- |
| `A`     | `@`   | `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`                     |
| `AAAA`  | `@`   | `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`      |
| `CNAME` | `www` | `jcmatoskx.github.io`                                                                          |

**Set all of these to "DNS only" (grey cloud).** Orange-cloud proxying blocks GitHub
from issuing the TLS certificate, and with Cloudflare's SSL mode on Flexible it
produces a redirect loop. This is the usual way this setup fails.

The existing `app.eazy.chat` record and the MX records are untouched by any of this.

## Facts to keep current

These are stated on the site and someone will check them:

- Operator: ITS - Innovative Tech Solutions, Unipessoal, Lda — VAT `PT515363677`
- Registered office: Largo Asilo da Gandarinha, n.º 128, 3720-362 Vila de Cucujães,
  Oliveira de Azeméis, Portugal
- Contact for everything: `outbound@eazy.chat`
- Application hosting: netcup GmbH, servers in the EU
- Email hosting: MXroute, servers in the United States (disclosed as a transfer under
  Standard Contractual Clauses in Privacy Policy section 10)
- Website hosting: GitHub, Inc., United States

**Retention periods in Privacy Policy section 11 are a stated policy, not a technical
fact:** support conversations 24 months, order-linked records 10 years, records of data
subject requests 3 years. Confirm these match what ITS actually does, and change them
here if not.
