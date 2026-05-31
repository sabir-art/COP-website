# AI Agent Instructions

This repository is shared working memory for the COP / CloudOnPoint Webflow website.

## Repository rules

- Use only `sabir-art/COP-website`.
- Use only branch `cursor/setup-dev-environment-ee62` unless Abdellah explicitly approves another branch.
- Do not create new repositories, branches, or pull requests without asking Abdellah first.
- Do not use `sabir-art/Skills-Claude` for new work. It was created by mistake.
- Do not delete `Skills-Claude` unless Abdellah explicitly confirms it can be deleted.

## Source of truth

The live website source of truth is Webflow, not GitHub.

GitHub stores working memory only. Old notes can be useful, but they are not automatically true.

## Memory hierarchy

Read files in this order:

1. `README.md`
2. `AGENTS.md`
3. `VALIDATED_CONTEXT.md`
4. `CURRENT_TASK.md`
5. `QUESTIONS.md`
6. `FAILED_ATTEMPTS.md`
7. `CHANGELOG.md`

Interpret them like this:

- `VALIDATED_CONTEXT.md` = confirmed facts only.
- `CURRENT_TASK.md` = what is active now, not the full project history.
- `QUESTIONS.md` = unresolved items that need Abdellah or live verification.
- `FAILED_ATTEMPTS.md` = warnings, not permanent bans.
- `CHANGELOG.md` = historical record, not instructions.

## Anti-bias rule

Do not let old failed attempts overrule the current situation.

If a note says something failed, do not assume it still fails. First check:

- Was it a temporary bug?
- Was it caused by the wrong branch, wrong selector, wrong page, wrong plan, or wrong Webflow state?
- Has the Webflow structure changed since then?
- Has the API, browser, or user requirement changed?

Only treat something as a hard limitation if it is reproducible, current, and clearly documented as such.

## Webflow rules

Before modifying Webflow:

1. Inspect the current live/page/component structure through the Webflow MCP or current available source.
2. Identify whether the target is global, component-based, page-level, or custom-code based.
3. Make the smallest possible change.
4. Do not delete scripts, styles, classes, components, or embeds unless their usage has been verified.
5. If something is uncertain, ask Abdellah or write it in `QUESTIONS.md`.

## HtmlEmbed workflow

The current known workflow is:

1. Read the existing embed content first when possible.
2. Explain what should change and why.
3. Provide the full replacement code as a ready-to-paste block.
4. Abdellah manually pastes it into Webflow Designer and republishes.

Do not assume an embed can or cannot be edited only from old notes. Re-check the current tool/API capability when the task depends on it.

## File update rules

- Keep `CURRENT_TASK.md` short and current.
- Keep `VALIDATED_CONTEXT.md` factual and stable.
- Keep `FAILED_ATTEMPTS.md` short and only for verified reproducible limitations.
- Do not store long exploratory reasoning in memory files.
- Do not add speculative conclusions as facts.
- Never commit API tokens, credentials, `.env` secrets, or private keys.

## Communication style

Be direct, practical, and precise.
Say what was checked, what changed, and what remains uncertain.
