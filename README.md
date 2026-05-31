# COP Website

Shared project memory for the COP / CloudOnPoint Webflow website.

## Source of truth

- Repository to use: `sabir-art/COP-website`
- Branch to use: `cursor/setup-dev-environment-ee62`
- Do not use `sabir-art/Skills-Claude` for new work. It was created by mistake.
- Do not create new repositories, branches, or pull requests without asking Abdellah first.

## Important rule

The live website source of truth is Webflow, not GitHub.

GitHub is only used to keep a clean working memory between Claude, Cursor, and different computers.

Before changing anything in Webflow, inspect the live structure first. Do not rely only on old notes.

## Memory hierarchy

1. `VALIDATED_CONTEXT.md` — confirmed facts only.
2. `CURRENT_TASK.md` — current active work only.
3. `QUESTIONS.md` — unresolved questions only.
4. `CHANGELOG.md` — short record of important repo/documentation changes.
5. `FAILED_ATTEMPTS.md` — only verified, reproducible limitations. Never use this file to block a new attempt without re-checking the current context.
6. `AGENTS.md` — operating rules for Claude, Cursor, and other AI agents.

## Rule for future agents

Old notes are not automatically true.

If something says “it did not work”, treat it as historical context, not as a permanent rule. Re-check the current Webflow state, browser state, API capability, or user requirement before deciding.
