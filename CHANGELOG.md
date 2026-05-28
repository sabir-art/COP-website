# Changelog

Use this file to document changes made to the project memory or to the Webflow website.

## Format

```md
## YYYY-MM-DD — Short title

### Changed

- [What changed]

### Verified

- [What was tested or confirmed]

### Notes

- [Anything important]
```

## 2026-05-27 — Repository memory setup

### Changed

- Added the shared GitHub memory structure to the existing Cursor branch.
- Added Markdown files for AI instructions, validated context, current task tracking, changelog, failed attempts, and open questions.

### Verified

- The files were added to `cursor/setup-dev-environment-ee62`.

### Notes

- The live website remains in Webflow.
- GitHub is not the source of truth for the website itself.

## 2026-05-27 — Audit of scripts and embeds on test-website---sabir

### Changed

- No live Webflow change yet. Documented full audit in `CURRENT_TASK.md`.
- Saved a snapshot of all 15 S3-hosted site-applied scripts under `/opt/cursor/artifacts/wf_audit_scripts/` so Abdellah can paste any code back into an embed if needed after cleanup.

### Verified through Webflow MCP

- Site ID `6a08929c8a27708945c53a0d` has 60 registered scripts, 15 applied at site level (all in footer), and 0 applied at any page level.
- Every one of the 15 applied site scripts is functionally duplicated by an embed already present in the matching page or in the Global Header / Footer Animated component (see CURRENT_TASK.md for the per-script mapping).
- Home and Seeblick pages have full coverage by embeds. The Contact page has no embeds and is mostly placeholder content ("This is some text inside of a div block.").
- The Data API does not expose a "delete registered script" action; orphan registry entries must be removed from the Webflow Apps UI of the MCP Bridge app.

### Notes

- Awaiting Abdellah's explicit go-ahead before calling `clear_site_scripts` and before re-publishing the site.
- The real performance bottleneck on the live site is photographic PNG assets (2.5–3.5 MB each on Home and Seeblick), not the scripts.

## 2026-05-28 — Seeblick 360° section: missing pp-pano-embed container restored

### Changed

The Pannellum init script on the Seeblick "Immersive view" section was looping forever because its target container `.pp-pano-embed` did not exist in the published DOM. The section had the head, the embed (script + config), the tabs and the note, but no element with the class the script queries.

Restored via MCP, directly on the Seeblick page canvas:
- Enriched the existing `pp-pano-embed` style (previously only `margin-top:16px`) with `position:relative; width:100%; aspect-ratio:16/9; overflow:hidden; background-color:#000000; border-radius:12px`. The page now reserves the 16:9 dark placeholder before Pannellum loads, eliminating any layout shift.
- Inserted a new Div Block with class `pp-pano-embed` as a sibling, positioned just before `pp-pano-tabs` inside `.pp-inner`. Display name set to "360° Pano Viewer" so it shows up clearly in the Navigator. Webflow element id: `4655f7f1-7e0a-f8ae-65d0-5f0546048bcf`.

Final section order verified via `element_tool > query_elements`:
1. `.pp-grid-6-6.pp-head-mb` (eyebrow + h2 + lead)
2. The HTML Embed (Pannellum script + window.__PANO config)
3. `.pp-pano-embed` (new — the 16:9 viewer placeholder)
4. `.pp-pano-tabs` (Living / Kitchen / Bedroom / Dressing / Bathroom)
5. `.pp-pano-note`

### Why

Reading the published HTML showed `.pp-pano-embed` was completely missing while the script kept polling for it (`setTimeout(I, 500)` indefinitely), silently failing with nothing on screen between the title and the tabs.

### Notes

- No script change required — the existing init logic already does the right thing once it finds the container.
- Abdellah needs to re-publish the site for the change to go live.
- Pannellum is loaded from `cdn.jsdelivr.net/npm/pannellum@2.5.6`. If a CSP or network rule ever blocks jsDelivr, we'll need to self-host Pannellum in the Webflow asset library.

## 2026-05-28 — Split into three section-owned embeds (header / hero / services)

### Changed

Abdellah requested strict ownership: header code in the header embed, section code in each section embed. Re-read all three embeds on 2026-05-28 via MCP to confirm exact current content, then produced three replacement embeds with no cross-section bleed.

Three replacement embeds saved under `/opt/cursor/artifacts/wf_fixes/`:
- `01_header_embed_v3.html` (~16.8 KB) → paste into the Global Header embed `4a9e9a90-2e99-fd97-a7d5-49cee5771f42`. Header-only: menu overlay, toggles, on-dark, WebGL liquid-glass on `.cp-header`, body scroll lock, `body.menu-lock .cp-header { position:fixed ... }`, header right-padding sync. No reference to `.cp-hero_caption` or `.svc_caption`.
- `02_hero_embed_v2.html` (~9.3 KB) → paste into the Home hero embed `671fcc83-1902-78a4-c05e-c7e57d0c13ca`. Hero-only: hero zoom/slide animations, hero slider rotation, glass styling + cursor-tracked halo + WebGL displacement on `.cp-hero_caption`. No reference to header or `.svc_caption`.
- `03_services_embed_v2.html` (~7.7 KB) → paste into the Home services embed `a96d8deb-ece3-ad5e-146f-ec7b7634dcb7`. Services-only: `.svc_item` active/expand, preview image swap, glass + cursor halo + WebGL on `.svc_caption`. No reference to header or `.cp-hero_caption`.

The earlier combined `header_embed_v2.html` is superseded; this three-file split replaces it.

### Why

The earlier combined embed put `.cp-hero_caption` and `.svc_caption` styling inside the header embed, which mixed concerns. Abdellah wanted strict separation so each section embed is self-contained for its own visuals and interactions.

### Notes

- Boot guards are now scoped per embed: `window.__COP_HEADER_BOOTED__`, `window.__COP_HERO_BOOTED__`, `window.__COP_SVC_BOOTED__`.
- The shared `<svg id="lgsvg">` root is created on demand by whichever embed needs it first; each filter has a unique id (`lgH`, `lgHC`, `lgSC`) so they coexist safely.
- Recommended paste order: header first, then hero, then services, then a single re-publish.

## 2026-05-28 — Refactored Global Header embed (fix for two post-publish bugs)

### Changed

- Produced a single, self-contained replacement for the Global Header embed at `/opt/cursor/artifacts/wf_fixes/header_embed_v2.html` (~20 KB). Abdellah pastes this content into the existing header embed `4a9e9a90-2e99-fd97-a7d5-49cee5771f42`, replacing what is there now, then re-publishes.
- No live Webflow change yet from this session — embed content can only be edited through the Designer.

### Why

After the 2026-05-27 cleanup + Abdellah's republish, two issues appeared:
- The cursor-tracked white highlight on `.cp-hero_caption` and `.svc_caption` stopped following the pointer.
- On Seeblick (and every non-Home page), opening the burger menu after scrolling left the menu overlay visible but the header (logo + close × control) scrolled off-screen. The Home page hid the bug because its hero embed still injects the missing rule.

### Root causes (confirmed by reading published HTML)

- The `pointermove` handler lived in `glasscss2.js`; the header embed only carried the matching CSS, never the JS. Removing `glasscss2` at site level removed the handler everywhere.
- `.cp-header` is not naturally `position: fixed` in the Webflow CSS. The rule `body.menu-lock .cp-header { position: fixed !important; top: 0; left: 0; right: 0; z-index: 600; }` used to be injected at runtime by `heroslider4.js`. After cleanup, only Home still carries that rule (through the Home hero embed).

### What the refactored embed adds

- Permanent CSS rule `body.menu-lock .cp-header { position: fixed !important; ... }` so the header stays pinned on every page when the menu opens.
- `MutationObserver` on `<body>` to keep the header aligned with the locked body width (replaces the `heroslider4` observer at the global header level).
- `pointermove` + `pointerleave` handlers that set `--gx` / `--gy` on `.cp-hero_caption` and `.svc_caption`, restoring the cursor-tracked highlight.
- All previous logic (menu open/close, theme toggle, lang toggle, on-dark detection, WebGL liquid-glass filter) preserved and reorganized into clearly labelled sections, with `window.__COP_HEADER_BOOTED__` guard so the script is idempotent.

### Notes

- Embed contents cannot be edited via the Webflow Data API; the change must be pasted into the Designer.
- After paste, Abdellah must re-publish for the change to be live.

## 2026-05-27 — Cleared all site-level script applications

### Changed

- Webflow MCP `data_scripts_tool > clear_site_scripts` called on site `6a08929c8a27708945c53a0d`. API response: "All scripts removed from the site." This removed the 15 previously applied site-level scripts (heroslider4, projhovercss2, projcarousel4, enhancecss2, enhancejs3, contactfx4, footerfx2, glasscss2, glassjs6, togglecss, togglejs2, menucss2, menujs3, uifxcss2, uifxjs).
- The embeds inside Home, Seeblick, the Global Header component and the Footer Animated component were intentionally NOT touched.
- Memory files updated to reflect the new state.

### Verified through Webflow MCP

- `get_site_scripts` → 404 "Custom code block not found" (= zero applied site scripts)
- `get_page_scripts` for Home `6a0892a08a27708945c53a25`, Seeblick `6a110292a4b44b5af561fa98`, Contact `6a09d8751e5baf5ab32394aa` → 404 "Custom code block not found" (= zero applied page scripts everywhere)
- `get_registered_scripts` → still 60 entries (the API does not expose a delete action for registered scripts; confirmed in Webflow Developer Docs)

### Notes

- The change is in Webflow's data layer. The currently published HTML still references the old scripts. Abdellah needs to re-publish the site for the change to be live.
- The 60 registry entries cannot be removed via API. Cleanest path: uninstall the MCP Bridge App in Webflow Workspace settings, which removes every script it had registered. They are dormant in the meantime and have no runtime cost.
- Rollback snapshot stored at `/opt/cursor/artifacts/wf_audit_scripts/_BACKUP_state_before_cleanup.json` plus the 15 raw script files. Each application can be re-added one by one with `data_scripts_tool > add_site_script` if a regression appears after publish.
