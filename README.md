# mallorytech.net

Business website for Mallory Tech Enterprises LLC, served by GitHub Pages at
**https://mallorytech.net**.

Plain static HTML — no build step, nothing to install. Edit, commit, push to `main`;
GitHub Pages redeploys in about a minute.

## Files

| File | Purpose |
|---|---|
| `index.html` | The homepage — single page, all CSS inline |
| `404.html` | Page-not-found page (GitHub Pages serves it automatically) |
| `images/og-image.jpg` | 1200×630 preview image shown when the link is shared |
| `sitemap.xml`, `robots.txt` | For search engines. Update `lastmod` in the sitemap after real content changes |
| `CNAME` | Claims the custom domain. **Do not delete** — removing it drops the custom domain |
| `.nojekyll` | Skips Jekyll; this is plain HTML |

Fonts load from Google Fonts. Nothing else is external.

## Hosting

- Source: `main` branch, root folder
- Custom domain: `mallorytech.net`, HTTPS enforced (verified 2026-10-09; `http://` redirects to `https://`)
- DNS at GoDaddy: four A records for `@` (`185.199.108.153`, `.109.153`, `.110.153`,
  `.111.153`) and `www` CNAME → `eyeamabbi-debug.github.io`

If HTTPS ever breaks: GitHub → Settings → Pages → clear the custom domain, save,
re-enter `mallorytech.net`, save, wait for the green check, tick **Enforce HTTPS**.
That's what fixed it in September 2026.

## Before pushing changes

The site is built to WCAG 2.2 AA. After any edit, run the accessibility sweep from
the Executive Assistant project and expect 0 violations:

```bash
cd ".claude/skills/ada-audit"
node scripts/a11y-sweep.mjs "C:\All the Claudes\mallorytech-site" --out ./results-mallorytech
```

Don't put client names or client-identifying numbers on this site.

## Contact form

GitHub Pages has no server, so the free-evaluation form opens the visitor's email
app with a message to `mallory.tech.ent@gmail.com`, fields filled in. The visitor
still has to press send, and the on-page message tells them so.

**To move to a real form backend later:** create a form at <https://formspree.io>,
set `action="https://formspree.io/f/YOUR_FORM_ID"` and `method="POST"` on the
`<form>`, and replace the mailto submit handler. Test once on the live site.

## History

The previous homepage (sage and cream design, seven services) and the
`local-lighthouse.html` product page were removed on 2026-10-09. Both are in git
history if ever needed.
