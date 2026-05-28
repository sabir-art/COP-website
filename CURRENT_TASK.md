# Current Task

## Task: Full audit of scripts, embeds and custom code on test-website---sabir

### Request

Abdellah asked for a complete audit, before any change, of what is currently used and what is not on the site `test-website---sabir` (site ID `6a08929c8a27708945c53a0d`):

- scripts, classes, embeds, custom code
- remove any script not used anywhere on the site
- verify that the relevant embeds are present on Home, Seeblick and Contact, and that the scripts inside them are correctly implemented
- analyze why the site may be heavy and what can be optimized

Context: previously the scripts were registered through the Webflow MCP Bridge app via Claude. They are hosted on Amazon S3 and cannot be edited there. Abdellah is now migrating everything to Webflow Embed elements. He has manually moved some scripts but is not sure he covered everything. The workflow going forward is: he creates the embed in the right section, the agent provides the JS, he pastes it.

### Confirmed through Webflow MCP

Site:
- Site ID: `6a08929c8a27708945c53a0d` (Test website - SABIR, time zone Europe/Paris)
- Last published: 2026-05-27. Locales en-CH (primary, disabled) and de-CH (secondary, disabled).

Pages:
- Home `6a0892a08a27708945c53a25` (140 nodes)
- Seeblick Birrwil `6a110292a4b44b5af561fa98` (289 nodes)
- Contact `6a09d8751e5baf5ab32394aa` (73 nodes, mostly placeholder text)
- 404 `6a09dc0aa00841d1a03a1e8e`
- Password (401) `6a09dc0636e3334e899059dd`

Components used by the three main pages:
- Global Header `7214b4c4-a3c9-af66-6bba-a6e063092594` — embed `4a9e9a90-2e99-fd97-a7d5-49cee5771f42` carries 4 stylesheets + 4 scripts (menu, toggle, glass, uifx)
- Footer Animated `a125598e-ebde-50d8-fd45-3d389fb4ee6c` — embed `38c1eca2-4d51-7694-a158-48e7d98f3038` carries the footer marquee / reveal style + script

Scripts API state:
- Registered scripts (App library): 60
- Applied at site level: 15 (all in footer)
- Applied at page level: 0 on every page (every `get_page_scripts` call returns 404 "Custom code block not found")

Mapping of the 15 applied site scripts vs embeds already present on the site:

| Site script | Function | Duplicated in embed | Embed location |
|---|---|---|---|
| heroslider4 | `.cp-hero` slider + header-lock observer | `671fcc83-1902-78a4-c05e-c7e57d0c13ca` | Home hero |
| projhovercss2 | `.proj_card` hover CSS | `ccc74b04-0058-f968-3db4-c04815bafc73` | Home projects carousel |
| projcarousel4 | `.proj_track` drag carousel | `ccc74b04-0058-f968-3db4-c04815bafc73` | Home projects carousel |
| enhancecss2 | `.cp-proc_card` + `.cp-imp_card` + `.cp-problem` styles | `322331d4`, `facc9108`, `2671a84d` | Home problem / process / impact |
| enhancejs3 | Same selectors + `.cp-metrics_num` counter | `322331d4`, `facc9108`, `2671a84d`, `70d46781` | Home problem / process / impact / metrics |
| contactfx4 | `.cp-contact` reveal animation | `427d0b81-a1d6-2ff7-8a0e-a246a45aa435` | Home contact CTA |
| glasscss2 | header / hero caption / svc caption glass styles | `4a9e9a90` | Global Header component |
| glassjs6 | WebGL displacement filter on header / hero caption / svc caption | `4a9e9a90` | Global Header component |
| togglecss | `#cp-theme-toggle` / `#cp-lang-toggle` / `#cp-menu-open` styles | `4a9e9a90` | Global Header component |
| togglejs2 | Theme + language toggle logic | `4a9e9a90` | Global Header component |
| menucss2 | `#cp-menu` mobile menu styles | `4a9e9a90` | Global Header component |
| menujs3 | Mobile menu open/close + body lock | `4a9e9a90` | Global Header component |
| uifxcss2 | `.cp-header_link`, `.cp-header.on-dark` styles | `4a9e9a90` | Global Header component |
| uifxjs | Luminance detection -> `.cp-header.on-dark` toggle | `4a9e9a90` | Global Header component |
| footerfx2 | Footer marquee + reveal animation | `38c1eca2` | Footer Animated component |

Conclusion: each of the 15 applied site scripts is fully covered by an embed already present in a component or page. Removing the 15 site applications does not remove any functionality; it only stops the duplicate fetch + duplicate execution.

Seeblick-specific embeds (no equivalent site script):
- `26b81f22` Phone parallax (loads GSAP + ScrollTrigger from cdnjs)
- `d50a2e24` 360° panorama (loads Pannellum from jsDelivr; uses 5 panorama JPGs)
- `0427ac04` Frame slider for `.pp-frame-main`
- Small SVG icon embeds (purely decorative, no JS)

Contact page state:
- 73 nodes, no html-embed nodes, no custom scripts.
- Most text reads "This is some text inside of a div block." — the page is in template state.

### Orphan scripts (registered but never applied)

45 of 60 registered scripts are not applied at site level and not at page level (page-level returns 404 everywhere). Examples: photocardanim, embedtest, embedtestanimations, mcptestinline, iconcss, panodata/panocss/panoinit, phonefx, ppbtnarrows, visualframes/2/3/4, every older heroslider/glass*/menu*/toggle*/enhance*/contactfx*/footerfx/projhover*/projcarousel* version.

The Webflow Data API does not expose a "delete registered script" action. Removing those from the App library requires the Webflow Apps UI (MCP Bridge app), not the API.

### Performance findings (live audit)

- 15 site scripts ≈ 30 KB unminified total (no longer needed; will be removed once approved)
- Real performance bottleneck = images. PNG is being used for photographic content.
  - 5 panorama JPGs: 1.4–1.9 MB each (~8.4 MB total), eager-loaded by Pannellum
  - Seeblick hero / project renders: 2.5–3.5 MB **each PNG**
  - 6 Seeblick frame PNGs ≈ 14 MB
  - Brand board, mockups, plans: 2–3 MB each
- Seeblick adds GSAP + ScrollTrigger from cdnjs (~110 KB minified) and Pannellum from jsDelivr (~150 KB) on top of the Webflow base.
- Five duplicates of "Logo cloudonpoint icon.png" in the asset library.

### Work in progress

- Audit finished, results documented.
- Awaiting Abdellah's approval to call `clear_site_scripts` on site ID `6a08929c8a27708945c53a0d` (removes the 15 redundant site-level applications; the embeds keep doing the same work).
- Awaiting Abdellah's approval to also re-publish after the change.

### Remaining work

- After Abdellah's go-ahead: remove the 15 site-level script applications.
- Document orphan registered scripts that need to be removed manually from the Webflow Apps UI.
- Optional next steps suggested to Abdellah:
  - Decide what to do with the Contact page (currently template-state).
  - Compress/convert oversized PNG photographs to JPG / WebP / AVIF.
  - Lazy-load the 360° panorama JPGs (Pannellum supports this).
  - Deduplicate the repeated logo assets.

### Notes

- All Webflow values were confirmed live via MCP, not assumed.
- Original S3-hosted script source has been archived under `/opt/cursor/artifacts/wf_audit_scripts/` so the user can re-paste any of them into an embed if a regression appears after removal.
