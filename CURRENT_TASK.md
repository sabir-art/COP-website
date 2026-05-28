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

### Follow-up — 2026-05-28 — Header v5 (clean black overlay + staggered text reveal)

Two refinements requested after Abdellah validated v4's scrollbar behaviour:

1. When the menu is open, the header still emitted its `box-shadow` (`inset 0 1px 0 rgba(255,255,255,.6)` at the top and `0 8px 30px rgba(0,0,0,.08)` below). Against the solid dark overlay these created a subtle horizontal separation line, visible as a "white strip" at the top of the viewport. Fix: zero out the header's `box-shadow`, `border`, `backdrop-filter`, `filter` and add an explicit `outline: none` when `body.menu-lock` is active, so the header is fully invisible and the overlay reads as one continuous black surface.

2. Add a staggered reveal animation on the in-overlay text elements when the menu opens. Each block (nav links, tagline, meta rows) fades in from `opacity:0; translateY(24px); blur(6px)` to `opacity:1; translateY(0); blur(0)` using a `cubic-bezier(.22,1,.36,1)` over 0.55s, with delays cascading from ~0.18s. Respects `prefers-reduced-motion: reduce` (snap to final state, no transition).

File saved at `/opt/cursor/artifacts/wf_fixes/01_header_embed_v5.html`.

### Follow-up — 2026-05-28 — Robust scrollbar-width handling on menu open (v4)

Abdellah pushed back on the previous approach (MutationObserver + reading body.style.paddingRight) because the compensation was indirect, asynchronous, and could flicker. The new requirement: header / burger / × must keep exactly the same visual reference between menu-closed and menu-open states, on every browser and OS, regardless of scrollbar width.

New approach in header embed v4:

1. `<html> { scrollbar-gutter: stable; }` — permanent CSS reservation of the scrollbar gutter. Supported on Chrome 94+, Firefox 97+, Safari 16+ (≈2022). When the body switches to `overflow: hidden` on menu open, the gutter stays reserved, so the layout does not shift. Side effect: on pages shorter than the viewport, a small empty gutter is visible on the right; on a site like Cloudonpoint, almost every page is longer than the viewport.
2. `:root { --sbw: 0px; }` — CSS custom property that defaults to zero. Single source of truth for the scrollbar compensation.
3. `body.menu-lock { padding-right: var(--sbw) !important; }`
4. `body.menu-lock .cp-header { right: var(--sbw) !important; ... }`
5. JS `lockScroll(on)` computes `window.innerWidth - documentElement.clientWidth` synchronously and writes it to `--sbw` BEFORE adding the `menu-lock` class. On unlock, `--sbw` is cleared after a 700 ms delay so the close transition doesn't snap.

Removed:
- `watchHeaderForLock()` and its MutationObserver — no longer needed; `--sbw` drives both body and header simultaneously in a single frame.

Net effect: zero possibility of flicker, no hardcoded pixel value, no by-eye guesswork, identical visual reference between closed and open menu on every browser that supports scrollbar-gutter (modern browsers) AND on older ones via the JS fallback.

File saved at `/opt/cursor/artifacts/wf_fixes/01_header_embed_v4.html`.

### Follow-up — 2026-05-28 — Strict separation of header / hero / services embeds

Abdellah requested strict ownership: code about the header lives only in the header embed, code about a section lives only in that section's embed. Re-read all three embeds on 2026-05-28 via MCP to confirm the actual current content, then produced a clean split.

Ownership matrix the new embeds enforce:

| Embed | Owns | Does NOT touch |
|---|---|---|
| Header `4a9e9a90` (Global Header component) | header base + transitions, menu overlay, body lock + header right-padding sync, `body.menu-lock .cp-header { position:fixed }`, header on-dark detection, WebGL liquid-glass on `.cp-header` only, theme/lang/burger toggles | anything about `.cp-hero_caption` or `.svc_caption` |
| Hero `671fcc83` (Home page) | hero zoom/slide animations, hero slider rotation, glass + cursor halo + WebGL on `.cp-hero_caption` | anything about header, `.svc_caption`, or other sections |
| Services `a96d8deb` (Home page) | svc_item active/expand, svc_preview image swap, glass + cursor halo + WebGL on `.svc_caption` | anything about header, `.cp-hero_caption`, or other sections |

Files to paste, in /opt/cursor/artifacts/wf_fixes/:
- 01_header_embed_v3.html  (~16.8 KB) → into embed 4a9e9a90 inside the Global Header component
- 02_hero_embed_v2.html    (~9.3 KB)  → into embed 671fcc83 on the Home page hero
- 03_services_embed_v2.html (~7.7 KB) → into embed a96d8deb on the Home page services section

Recommended paste order: header first, then hero, then services, then a single re-publish.

The previous combined `header_embed_v2.html` is now superseded but kept as a reference.

### Follow-up — 2026-05-28 — Header overlay refactor (post-publish bugs)

Abdellah re-published the site after the cleanup and reported two regressions:

1. The pointer-tracked white highlight on `.cp-hero_caption` ("PROJECT PRESENCE" card) and `.svc_caption` ("ACTIVE CAPABILITY" card) stopped following the cursor.
2. On Seeblick (and any non-Home page), opening the burger menu after scrolling > 0 left the menu overlay visible but with the header (logo + close-X) scrolled off-screen. At scroll = 0 the menu opened correctly. On Home the issue was hidden because the page's hero embed accidentally still injected the fix.

Root causes (confirmed by reading the live HTML at `https://test-website---sabir.webflow.io/Seeblick-Birrwil`):

- The pointer-move handler that powered the white highlight lived in `glasscss2.js`. The header embed only carried the matching CSS, never the JS. When `glasscss2` was unapplied at site level, the highlight stopped following the mouse on every page.
- `.cp-header` is not `position: fixed` in the published Webflow CSS. The rule `body.menu-lock .cp-header { position: fixed !important; top: 0; left: 0; right: 0; z-index: 600; }` used to be injected at runtime by `heroslider4.js`. When `heroslider4` was unapplied at site level, the rule survived only on the Home page (because Home's hero embed still carries the same code). Every other page lost it.

Fix: a single refactored header embed that owns the entire header / menu / glass behaviour. Saved at `/opt/cursor/artifacts/wf_fixes/header_embed_v2.html` (~20 KB). Abdellah pastes it into the existing Global Header embed (`4a9e9a90-2e99-fd97-a7d5-49cee5771f42`), replacing the previous content, then re-publishes. Embeds in other pages and components are not touched.

What the new embed does:

- `body.menu-lock .cp-header { position: fixed !important; ... }` is now a permanent rule in the `<style>` block, so it applies on every page from first paint.
- A `MutationObserver` on `<body>` keeps the header right-aligned with the locked body (same logic that used to live in `heroslider4`).
- A `pointermove` / `pointerleave` handler sets the `--gx` / `--gy` custom properties on `.cp-hero_caption` and `.svc_caption`, restoring the radial white highlight that follows the cursor.
- All previous behaviour (menu open/close, theme toggle, lang toggle, on-dark luminance detection, WebGL displacement glass filter) is preserved and re-organized into clearly labelled sections.
- Idempotent boot guarded by `window.__COP_HEADER_BOOTED__`.

### Work done — 2026-05-27 cleanup

- Approved by Abdellah: remove every script applied to the site and remove the 60 orphan registered scripts.
- Action taken via MCP: `data_scripts_tool > clear_site_scripts` on site `6a08929c8a27708945c53a0d`.
- API response: `"All scripts removed from the site."`
- Verification (also via MCP, immediately after):
  - `get_site_scripts` → 404 "Custom code block not found" (= 0 applied site scripts)
  - `get_page_scripts` on Home, Seeblick, Contact → 404 "Custom code block not found" (= 0 applied page scripts on every page)
  - `get_registered_scripts` → still 60 entries

### Limitation discovered during the cleanup

The Webflow Data API does NOT expose any endpoint to delete a registered script. Confirmed in the official Webflow Developer Documentation: the `DELETE /v2/sites/{site_id}/custom_code` endpoint description states explicitly "This endpoint will not remove scripts from the site's registered scripts." Only the *applications* of a script (site-level or page-level) can be removed.

Consequence:
- The 60 entries in the MCP Bridge app library remain in the registry, but they no longer inject anything on any page.
- They consume registry space (60 of 800 per-site quota) but do not affect bandwidth, performance, or behavior.
- The only ways to actually empty the registry are:
  1. Uninstall the MCP Bridge App from the Webflow Workspace (Workspace settings → Apps & integrations → Authorized apps → remove). Uninstalling an app removes all the scripts it had registered.
  2. Wait for Webflow to ship a DELETE endpoint for registered scripts (none today).

### Remaining work

- Abdellah re-publishes the site so that the cleared site-level scripts are no longer included in the live HTML.
- Decide whether to uninstall the MCP Bridge App to also wipe the 60 registry entries (optional, cosmetic only).
- Open follow-up workstreams (separate from this task):
  - Contact page is currently in template state (placeholder text everywhere).
  - PNG photographs are 2.5–3.5 MB each on Home and Seeblick → convert to JPG / WebP.
  - 360° panorama JPGs are eager-loaded by Pannellum (~8.4 MB on first paint of Seeblick).
  - Five duplicates of `Logo cloudonpoint icon.png` in the asset library.

### Notes

- All Webflow values were confirmed live via MCP, not assumed.
- Source of every S3-hosted script archived under `/opt/cursor/artifacts/wf_audit_scripts/` (15 files) + `_BACKUP_state_before_cleanup.json` so the configuration can be reapplied if a regression appears.
- Embeds were not touched. They keep doing the work that the cleared site-level scripts were duplicating.
