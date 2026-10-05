# Sri Sai Dental Clinic website – Security + SEO report

Scope: the marketing site (index.html) only. The dashboard, API and n8n code were not provided, so backend items are NOT audited.

## Security
Found and fixed:
- No API keys, tokens or passwords in the page (scanned). No forms, and all innerHTML uses hard-coded constants, so there is no XSS input surface.
- Added a Content-Security-Policy (meta tag plus nginx header), Referrer-Policy, and noopener noreferrer on all external links.
- nginx config adds HSTS, nosniff, X-Frame-Options, frame-ancestors, Permissions-Policy, HTTP-to-HTTPS and www-to-apex redirects, blocks dotfiles/backups/logs, and hides the server version.

Remaining:
- Medium: CSP needs 'unsafe-inline' because scripts and styles are inline. Move them to files to tighten it.
- Medium: the headers only take effect once nginx-saidentalclinicylk.conf is installed.
- Not audited (needs the dashboard/API code): authentication, password hashing, admin routes, IDOR, rate limiting, CORS, webhook signature checks (WhatsApp/n8n), database access, uploads, dependencies (`npm audit`), Git history for secrets, .env handling.

## SEO
Found and fixed:
- OG/Twitter image was a base64 data URI (ignored by social platforms). Now a real 1200x630 og-image.jpg at an absolute URL.
- Page was 1.7 MB because 20 images were inlined. Now 131 KB HTML plus 20 WebP files with descriptive names.
- Phone, WhatsApp and directions links were href="#" and address/phone/email read "Loading…" until JavaScript ran. Now real links and text in the HTML.
- Schema: fixed phone format; added email, image, logo, hasMap, specialties, WebSite and FAQPage (matches the visible FAQ).
- Added main landmark, skip link, focus outlines, aria-expanded on FAQ buttons, lightbox alt text, theme-color, apple-touch-icon, robots.txt, sitemap.xml, 404.html and a tighter meta description.

Remaining:
- Single-page site: no separate service, doctor or location URLs (biggest SEO opportunity).
- Weekend hours are missing; doctor cards have no photos.
- aggregateRating is kept (real Google numbers) but Google ignores self-hosted ratings for rich results.
- Search Console and Google Business Profile are NOT connected. Submit the sitemap there and make sure hours and phone match the profile.
- PageSpeed and Rich Results Test were not run (no internet in this environment).

## Files
index.html, images/*.webp, og-image.jpg, apple-touch-icon.png, robots.txt, sitemap.xml, 404.html, nginx-saidentalclinicylk.conf

## Manual setup
Upload the whole folder (keep images/ next to index.html), install the nginx config (set root and SSL paths), reload nginx. No environment variables are needed for this static site.

## Readiness (static site only)
- Security: good once nginx headers are live; backend unknown.
- SEO: good for a one-pager, limited by being a single page.
- Performance: good.
- Accessibility: fair to good; contrast and keyboard menu not fully tested.
