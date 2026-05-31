# Repository Consolidation

Date: 2026-05-31

## Decision

`COP-website` is the only GitHub repository to use for the COP Webflow website project.

The branch `cursor/setup-dev-environment-ee62` is the single source of truth for project memory and documentation.

Do not create new repositories, new branches, or pull requests without asking Abdellah first and receiving explicit approval.

Do not modify the live Webflow website as part of repository consolidation work.

## Secondary repository

The repository `sabir-art/Skills-Claude` was created by mistake.

Its useful content was compared against `sabir-art/COP-website` before making changes here.

Do not delete `sabir-art/Skills-Claude` until Abdellah confirms that everything useful has been safely migrated.

## Files checked in `sabir-art/Skills-Claude`

The following files were checked:

- `README.md`
- `AGENTS.md`
- `VALIDATED_CONTEXT.md`
- `CURRENT_TASK.md`
- `CHANGELOG.md`
- `FAILED_ATTEMPTS.md`
- `QUESTIONS.md`
- `contact-form-embed.html`

## Matching files already present in `sabir-art/COP-website`

The same files were already present in `sabir-art/COP-website` on branch `cursor/setup-dev-environment-ee62`.

## Comparison result

The checked files were identical between `sabir-art/Skills-Claude` and `sabir-art/COP-website` by Git blob SHA:

| File | Result |
|---|---|
| `README.md` | Identical |
| `AGENTS.md` | Identical |
| `VALIDATED_CONTEXT.md` | Identical |
| `CURRENT_TASK.md` | Identical |
| `CHANGELOG.md` | Identical |
| `FAILED_ATTEMPTS.md` | Identical |
| `QUESTIONS.md` | Identical |
| `contact-form-embed.html` | Identical |

No content from `COP-website` was deleted or overwritten with a conflicting version from `Skills-Claude`.

## Merge action performed

Because all checked useful files were already present and identical in `COP-website`, no file content from `Skills-Claude` needed to be manually merged.

The safe consolidation work performed in `COP-website` was documentation-only:

1. Updated `README.md` to state that:
   - `COP-website` is the only repository to use.
   - `cursor/setup-dev-environment-ee62` is the single source of truth.
   - New branches or pull requests require Abdellah's explicit approval.
   - `Skills-Claude` was created by mistake and should not be deleted yet.
2. Updated `AGENTS.md` with the same repository-source-of-truth rules for future agents.
3. Added this `REPOSITORY_CONSOLIDATION.md` file as a permanent record of the comparison and consolidation.

## Important preservation note

The live Webflow website was not modified.

The `Skills-Claude` repository was not deleted.
