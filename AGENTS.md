# AI Agent Instructions

This repository is the shared memory for the COP Webflow website.

The live website source of truth is Webflow, not GitHub.
GitHub is used to synchronize context between Claude, Cursor, and different computers.

## Core rules

1. Never treat previous AI notes as absolute truth unless they are inside `VALIDATED_CONTEXT.md`.
2. Before modifying Webflow, always verify the current live structure using the Webflow MCP.
3. If something is uncertain, write it in `QUESTIONS.md` instead of assuming.
4. If a solution fails, document it in `FAILED_ATTEMPTS.md`.
5. Do not overwrite validated decisions without asking Abdellah.
6. Keep changes minimal and focused on the requested task.
7. Do not invent Webflow class names, component names, page structures, scripts, or variables.
8. Do not delete scripts, styles, classes, or components unless their usage has been verified.
9. If removing anything, keep a backup or document exactly what was removed and why.
10. Never commit API tokens, Webflow credentials, `.env` secrets, or private access keys.

## Start-of-session checklist

Before doing any task, read:

- `README.md`
- `AGENTS.md`
- `VALIDATED_CONTEXT.md`
- `CURRENT_TASK.md`
- `FAILED_ATTEMPTS.md`
- `QUESTIONS.md`

Only `VALIDATED_CONTEXT.md` should be treated as confirmed truth.
Everything else must be verified when relevant.

## End-of-session checklist

At the end of a work session, update the relevant files:

- `CURRENT_TASK.md` with what is currently in progress.
- `CHANGELOG.md` with what changed.
- `FAILED_ATTEMPTS.md` if an approach did not work.
- `QUESTIONS.md` if something still needs confirmation.

Do not update `VALIDATED_CONTEXT.md` unless Abdellah explicitly validates the information.

## Webflow MCP workflow

When working on Webflow:

1. Inspect the live Webflow structure through MCP first.
2. Identify whether an element is global, component-based, page-level, or custom-code based.
3. Make the smallest possible change.
4. Test the change on the relevant pages.
5. Document the result.

### HtmlEmbed editing workflow (mandatory)

Before proposing any change to a Webflow Embed element, the agent MUST:

1. Read the current embed content via the Webflow Data API (`get_component_content` or `get_page_content`) and show it to Abdellah, or summarize it precisely with line references.
2. Explain what will change and why.
3. Provide the full new embed content as a single, ready-to-paste block.
4. Tell Abdellah to paste it into the embed in the Webflow Designer, then re-publish.

The agent MUST NOT skip step 1. The agent MUST NOT ask Abdellah to paste new code without first confirming what is currently there.

Reason: the MCP cannot write the HTML/CSS/JS content of an existing embed (confirmed 2026-05-28). The only settable keys on an `HtmlEmbed` element are `domId` and `visibility`. The paste step is therefore always a human action.

## Cursor Cloud specific instructions

This repository is used mainly as project memory for a Webflow website.
There may be no application code, no package manager configuration, no tests, and no services defined unless Abdellah adds them later.

### Current technical state

- The live website is in Webflow.
- GitHub stores Markdown memory and documentation.
- Do not assume there is a local app to build unless project files are added later.

### For future agents

Once application code is added, update this file with:

- How to install dependencies.
- How to run the dev server.
- How to run lint and tests.
- Any non-obvious environment requirements or gotchas.

## Communication style

Be direct and practical.
Explain what was checked, what changed, and what remains uncertain.
Avoid long assumptions.
