# Relay Offer Page

Landing page / VSL funnel for [Relaysocial.app](https://relaysocial.app) — "Get 5 UGC ads for free."

**Live:** https://justintaylorsilver.github.io/bustem-offer-clone/
**Hosting:** GitHub Pages, serving the `main` branch root. Every push to `main` auto-deploys in ~30–60s. Pages caches HTML for 10 minutes (`max-age=600`), so hard-refresh after deploying.

## Origin / architecture

This started as a static mirror of a ClickFunnels page (offer.bustem.com/offer-1, owned by us) and was then rebranded for Relay. That history explains the structure:

- `index.html` — the entire page. ClickFunnels-generated markup (deeply nested `data-page-element` divs with generated `id-6Z-*` classes) plus our custom additions, which are all marked:
  - `<style id="relay-marquee-css">` — scrolling brand-logo marquee (pure CSS animation)
  - `<style id="relay-hero-css">` — Relay topbar logo, page-wide Manrope typeface override, hero gradient text, `$5 per ad` highlight
  - `.relay-topbar` — logo header injected right after `<body>`
  - `.relay-logo-marquee` — replaces the original static logo grid (appears twice: mobile + desktop variants)
- `assets/` — ClickFunnels CSS/JS bundles, downloaded and served locally. The JS still points its form/analytics endpoints at ClickFunnels; the page doesn't depend on it for anything critical.
- `images/` — all page images, served locally. `images/brands/` holds the 35 marquee logos.
- `raw.html` — untouched original ClickFunnels HTML (gitignored, local reference only).

## Third-party integrations (live services, not in this repo)

- **Booking calendar:** iClosed inline widget. The embed is a single div:
  `<div class="iclosed-widget" data-url="https://app.iclosed.io/e/relay/5-free-ads" ...>` + `https://app.iclosed.io/assets/widget.js`.
  To point at a different event, change `data-url` (and the `title` attribute). Bookings land in the iClosed account that owns that event.
- **Video:** Wistia embed (`fast.wistia.com`). Currently still the original Bustem VSL — swap the Wistia ID in the `wistia_async_*` class when the Relay VSL is ready.
- **Fonts:** Manrope via Google Fonts.
- **Tracking:** legacy `t.bustem.com` pixel and ClickFunnels analytics calls are still in the `<head>` — replace or strip when wiring up Relay analytics.

## Working on it

No build step. Edit `index.html`, test with any static server:

```sh
python3 -m http.server 8734
```

Push to `main` to deploy.

### Gotchas

- Two copies of some sections exist (mobile vs desktop show-only wrappers) — copy changes may need to be made twice. Search before assuming one occurrence.
- Marquee logo `<img>`s are eager-loaded on purpose; `srcset` was stripped from legacy images (duplicate-URL srcsets break Chrome's candidate selection).
- `.relay-logo-marquee` uses `width: 0; min-width: 100%; contain: inline-size;` to stop the animated track's intrinsic width from blowing out the ClickFunnels flex parents. Don't "simplify" that rule.
- The page-wide Manrope override excludes `<i>`/`.fa*` elements so Font Awesome icons keep rendering.
