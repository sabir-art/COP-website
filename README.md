# COP Website

This repository is used as a shared project memory for the COP Webflow website.

The live website source of truth is Webflow.
GitHub is used to keep documentation, validated context, current tasks, changelog, failed attempts, and open questions synchronized between Claude, Cursor, and different computers.

## Repository source of truth

`COP-website` is the only GitHub repository to use for this project.

The branch `cursor/setup-dev-environment-ee62` is the single source of truth for the project memory and documentation.

Do not create new branches or pull requests without asking Abdellah first and receiving explicit approval.

The repository `sabir-art/Skills-Claude` was created by mistake. Its useful content has been checked against this repository and merged/preserved here. Do not delete `Skills-Claude` until Abdellah confirms that everything useful has been safely migrated.

## Important principle

Do not treat every note in this repository as confirmed truth.
Only `VALIDATED_CONTEXT.md` contains information explicitly validated by Abdellah.

Before changing the live Webflow project, Claude or Cursor must verify the current structure through the Webflow MCP.

## Main files

- `AGENTS.md` — permanent rules for Claude, Cursor, and AI agents.
- `VALIDATED_CONTEXT.md` — confirmed project facts only.
- `CURRENT_TASK.md` — current active task and working context.
- `CHANGELOG.md` — log of changes made or validated.
- `FAILED_ATTEMPTS.md` — things that were tried and should not be repeated.
- `QUESTIONS.md` — uncertainties that need confirmation.
- `REPOSITORY_CONSOLIDATION.md` — record of the safe merge from `sabir-art/Skills-Claude` into `sabir-art/COP-website`.
