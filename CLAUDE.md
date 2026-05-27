# CLAUDE.md — CoSiMo System Architecture

This is a documentation-only repository. No production code lives here.

## What this repo is
A living guide and decision archive for the CoSiMo system architecture. See `README.md` for full context.

## Mandatory rules
- Always update `changelog.md` after making any change (format: `## YYYY-MM-DD`, most recent at top)
- All files in English
- File names: lowercase, hyphenated (e.g. `my-document.md`)
- ADRs in `docs/decisions/` follow the naming pattern `NNN-short-title.md`
- Before using a term that may be project-specific, check `glossary.md` first and use definitions as written
- When introducing a new term in any document, add it to `glossary.md` before or alongside the change

## What not to do
- Do not make or imply architectural decisions without explicit instruction
- Do not create files outside the established folder structure without asking
- Do not add production code of any kind

## Key files
- `how-to.md` — conventions, folder guide, ADR template
- `roadmap.md` — open work and milestones
- `glossary.md` — shared terminology
