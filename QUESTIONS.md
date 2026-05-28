# Open Questions

Use this file for anything that is uncertain and needs confirmation from Abdellah or verification through the Webflow MCP.

Do not transform questions into confirmed facts unless Abdellah validates them or they are verified directly in Webflow.

## Format

```md
## YYYY-MM-DD — Question topic

### Question

- [What is uncertain?]

### Why it matters

- [Why this needs to be answered before changing anything]

### How to verify

- [Ask Abdellah / check Webflow MCP / test on page]
```

## Current open questions

### Resolved during the 2026-05-27 audit

- The header IS a reusable Webflow component (Global Header `7214b4c4-a3c9-af66-6bba-a6e063092594`) used on Home, Seeblick and Contact.
- The header overlay (menu + theme/lang toggle + glass effect + on-dark detection) lives inside one embed (`4a9e9a90-2e99-fd97-a7d5-49cee5771f42`) inside the Global Header component, NOT in Site Settings custom code.
- The footer used by the three main pages is the component `Footer Animated` `a125598e-ebde-50d8-fd45-3d389fb4ee6c` (not the older `Global Footer` `55f89729-ddba-dfd9-8201-af010ac8f5f7`). The footer marquee + reveal animation lives inside one embed (`38c1eca2-4d51-7694-a158-48e7d98f3038`) inside that component.

### New questions to confirm with Abdellah

- 2026-05-27 — Approval to call `clear_site_scripts` on site `6a08929c8a27708945c53a0d` and remove all 15 applied site scripts in one shot? Each is fully duplicated by an embed; removal is reversible via `add_site_script`.
- 2026-05-27 — After clearing, should the site be re-published immediately, or only after manual visual check in the Designer?
- 2026-05-27 — Should the Contact page placeholder content ("This is some text inside of a div block.") be treated in the same workstream, or is it a separate task?
- 2026-05-27 — Is it OK to flag the 45 orphan registered scripts (older versions, never applied) so Abdellah removes them from the Webflow Apps UI of the MCP Bridge app? The Data API has no programmatic way to delete them.
- 2026-05-27 — Are the Site Settings → Custom Code "Head Code" and "Footer Code" boxes empty? The Data API only returns app-registered scripts. The two manual boxes are not exposed by MCP and need a visual check.
