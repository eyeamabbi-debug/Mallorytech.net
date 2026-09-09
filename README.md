# mallorytech.net

Business website for Mallory Tech Enterprises LLC, served by GitHub Pages.

Plain static HTML — no build step, nothing to install. Edit, commit, push;
GitHub Pages redeploys in about a minute.

## Files

| File | Purpose |
|---|---|
| `index.html` | The live homepage — single page, all CSS inline |
| `index-v2.html` | Byte-identical copy of `index.html`, kept as the named v2 design |
| `local-lighthouse.html` | Google Business Profile product page |
| `images/` | Photos used by the homepage |
| `CNAME` | Claims the custom domain. **Do not delete** — GitHub rewrites it when you change the domain in Settings, and removing it drops the custom domain. |
| `.nojekyll` | Skips Jekyll; this is plain HTML |

Fonts load from Google Fonts. Nothing else is external.

## Hosting status

GitHub Pages is **already configured and building** — this was set up in May 2026:

- Source: `main` branch, root folder
- Custom domain: `mallorytech.net`
- Enforce HTTPS: on

**Nothing needs changing in the repo settings.** The site is unreachable purely
because of DNS, below.

## DNS — the actual blocker (fix at GoDaddy)

DNS is managed at GoDaddy (`ns09`/`ns10.domaincontrol.com`). As of 2026-09-09 it
holds leftovers from a second, abandoned deploy attempt that break the first one.

**Delete these two records:**

| Type | Name | Value | Why it breaks things |
|---|---|---|---|
| A | `@` | `162.159.140.166` | A Cloudflare address mixed in with the GitHub ones. Roughly a quarter of visitors get routed to a host that isn't serving the site, and it makes GitHub's domain check fail — which is why no HTTPS certificate has been issued. |
| CNAME | `www` | `sites.ludicrous.cloud` | Points at a service whose domain no longer resolves at all. Returns Cloudflare Error 1001. |

**Then the records should be exactly:**

| Type | Name | Value |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `eyeamabbi-debug.github.io` |

All four A records are required. Only three were present, which alone would have
kept it from working.

**After the DNS change:**

1. Wait for propagation — usually minutes, up to an hour
2. GitHub repo → Settings → Pages → re-save the custom domain to retrigger the check
3. Once the check passes, GitHub issues the certificate automatically

Verify from PowerShell:

```powershell
Resolve-DnsName mallorytech.net -Type A | Select-Object IPAddress
Resolve-DnsName www.mallorytech.net    | Select-Object NameHost
```

Expect exactly the four `185.199.10x.153` addresses and nothing else.

## Contact form

The form composes an email in the visitor's mail client, addressed to
`mallory.tech.ent@gmail.com`, with all fields filled in. The visitor still has to
press send.

**This replaced a broken setup.** The form previously posted to
`https://formspree.io/f/mallory.tech.ent@gmail.com`, which is not a valid Formspree
endpoint — those use a short generated ID, never an email address. Worse, the page
showed a "We received your details" success message on submit regardless of whether
the POST succeeded, so every enquiry was silently lost while the visitor believed
they had made contact.

**To move to a real form backend later:** create a form at <https://formspree.io>,
then in `index.html` restore `action="https://formspree.io/f/YOUR_FORM_ID"` and
`method="POST"` on the `<form>` tag and drop the `e.preventDefault()` handler.
Test by submitting the live form once and confirming the email arrives.

## Not in this repo

The June 2026 homepage redesign ("GET FOUND. GET LEADS. GET AUTOMATED.", with the
logo integrated) and the logo concept sheets remain working files in
`C:\All the Claudes\Claude World\projects\mallory-tech-website\`. The v2 design was
deliberately kept live. To switch, copy that `index.html` plus
`images/mallory-tech-logo.png` here and push.
