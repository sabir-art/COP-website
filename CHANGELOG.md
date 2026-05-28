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
