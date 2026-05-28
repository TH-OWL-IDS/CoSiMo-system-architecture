# How to Work with this Repository

This guide explains how to navigate, add, change, and remove content in the CoSiMo system architecture repository. It is intended for project contributors as well as AI assistants working in this repository.

---

## What Lives Where

| Path | What goes here |
|---|---|
| `README.md` | Project overview and repo orientation — keep it high-level |
| `roadmap.md` | Milestones, open work, and upcoming decisions |
| `glossary.md` | Shared definitions for terms used across the project |
| `changelog.md` | A running log of all changes made to this repository |
| `docs/vision/` | Goals, guiding principles, scope, and non-goals |
| `docs/architecture/` | How the system is designed — components, boundaries, data flows |
| `docs/governance/` | Who is responsible for what, and how decisions are made |
| `docs/workflows/` | Step-by-step processes for recurring activities |
| `docs/security/` | Security requirements, threat models, compliance notes |
| `docs/decisions/` | Architectural Decision Records (ADRs) |
| `system/` | Structured definitions: actors, services, permissions, data models |
| `infrastructure/` | Hosting, deployment, Docker, monitoring, backup specs |
| `diagrams/` | Visual models — ecosystem, flows, permissions, exports |
| `research/` | References, Ideas, working notes |

---

## Adding a Document

1. Choose the correct folder based on the table above.
2. Create a new `.md` file with a clear, lowercase, hyphenated name (e.g. `voice-input-component.md`).
3. Start the file with a `# Title` and a short description of what it covers.
4. Update `changelog.md` with the date and what was added.

---

## Changing a Document

1. Edit the file directly.
2. If the change affects how something works at a system level, consider whether an ADR is needed (see below).
3. Update `changelog.md`.

---

## Removing a Document

1. Before deleting, check whether anything else references the file (search for its filename).
2. If it contains a decision that was once active, consider moving it to an archive folder rather than deleting it.
3. Update `changelog.md`.

---

## Writing an Architectural Decision Record (ADR)

ADRs live in `docs/decisions/`. They capture the context, options, and reasoning behind significant architectural choices — so future contributors understand *why* the system works the way it does, not just *how*.

**File naming:** `NNN-short-title.md` (e.g. `001-authentication-approach.md`)

**Template:**

```markdown
# NNN — Title

**Date:** YYYY-MM-DD  
**Status:** Proposed | Accepted | Deprecated

## Context
What situation or problem triggered this decision?

## Options Considered
- Option A — short description
- Option B — short description

## Decision
What was decided and why.

## Consequences
What does this decision enable or constrain going forward?
```

---

## Updating the Roadmap

`roadmap.md` tracks what is planned and what is open. When work is completed, mark it as done rather than deleting it — the history of completed milestones is useful context.

---

## Updating the Glossary

`glossary.md` is alphabetically sorted. When introducing a new term in any document, check whether it belongs in the glossary. If it does, add a definition before merging or finalizing the document.

---

## General Conventions

- All files are written in **English**.
- File names are **lowercase and hyphenated** (no spaces, no camelCase).
- Every change to this repository should have a corresponding entry in `changelog.md`.
- Prefer editing existing documents over creating new ones unless the topic is genuinely distinct.
