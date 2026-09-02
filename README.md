# solarforallnola

Static copy of solarforallnola.com, migrated off Leadpages Sites.
Content and layout are identical to the original. Only the platform plumbing changed.

Verified in a headless browser: 16/16 pages render, 50/50 images load, no broken
internal links, zero calls to any Leadpages server.

## Deploy to GitHub Pages

    git init && git add -A
    git commit -m "Initial migration from Leadpages"
    git branch -M main
    git remote add origin git@github.com:<you>/solarforallnola.git
    git push -u origin main

Settings → Pages → Source: `main` / root.
Lands at `https://<you>.github.io/solarforallnola/`

All internal links are relative, so it works from a subpath or a root domain
without changes. It will also run as-is on WPX — just upload the folder.

## Cutover checklist

1. Wire the form (below) — nothing else matters until this is done
2. Add a `CNAME` file containing `solarforallnola.com`
3. Point DNS at GitHub Pages
4. **Delete `robots.txt`** and remove the `noindex` meta from all 16 pages
5. Cancel Leadpages

Until step 4, this staging copy is blocked from search engines so it can't
compete with the live site for the same keywords.

## The form is NOT wired

Every page's form has `action="FORM_ENDPOINT_TODO"`. The original posted to
`api.leadpages.io`. Submissions currently go nowhere — by design, so leads
can't silently vanish into a dead system.

Fields: first name, last name, email, street, city, state, postal code,
phone, interests (lease/purchase), plus a honeypot and reCAPTCHA.

Replace that string with a Salesforce Web-to-Lead endpoint or your own handler.

## What changed from the original

Removed — all phoned home to Leadpages on every pageview:
- page-speed analytics beacon
- "Powered by Leadpages" badge injector
- useleadbot tracking pixel
- js.center.io tracker
- Google AdSense platform tags
- 144 Leadpages meta tags

Localized — nothing now depends on a Leadpages server:
- 49 images. The page source served these as 16-pixel thumbnails; full-resolution
  originals were recovered from the CDN, then resized to 2000px max at JPEG q85
  (192 MB → 17 MB)
- Open Sans and Font Awesome 6.4.2, previously hot-linked from Leadpages

Rewired:
- 33 "GET FREE QUOTE" / "LEARN MORE" buttons were Leadpages popup triggers that
  died with the embed script. They now point to `/contact/`
- Proprietary lazy-loader replaced with native `loading="lazy"`
- 16 background images referencing a literal `undefined` URL — this was already
  broken on the live site — set to `none`

## Content to revise once partners concur

- `posigen/` — dedicated partner page
- Partnership claims naming City of New Orleans, GNOF, and PosiGen, on every page
- $400 referral fee to the GNOF Green Workforce Development Fund (home page)
- Lease financing option, which was PosiGen's product
- Footer reads "© 2024"
