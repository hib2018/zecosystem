---
name: zintent-interpret
description: Generate a Japanese zintent Draft JSON from a human natural-language request and prepare the workspace/draft files needed before launching zintent. Use when the human asks to create, generate, or interpret an Intent/Draft from a request; do not use for review, approval, planning, or implementation.
---

# zintent Draft Interpretation

Create only an external Draft input for zintent. The zintent core still owns import, review,
transitions, validation, snapshots, and store persistence.

## Invocation contract

Use this Skill when the human asks to generate an Intent Draft from a natural-language request.
The Draft content must be written in Japanese, even if the source request mixes languages. Preserve
meaning; do not add implementation plan, architecture, tasks, acceptance criteria, or scope not
present in the request.

If the request is too ambiguous to split into reviewable items, ask the smallest clarifying question.
Otherwise create the minimal useful Draft: usually 1-5 items.

## Files to prepare before running zintent

Before launching `zintent workspace <workspace>`, ensure every file/directory zintent needs exists:

1. Determine the Project root and workspace path. Default to `<project>/intents` unless the human
   names another workspace.
2. Create the workspace directory: `<project>/intents/`.
3. Create the sibling Draft directory discovered by the TUI: `<project>/draft/`.
4. Save the original human request as `<project>/draft/<slug>.source.md` for provenance.
5. Generate the external Draft JSON as `<project>/draft/<slug>.json`.
6. Run a cheap local check such as `python3 -m json.tool <draft>.json >/dev/null`.
7. Launch `zintent workspace <workspace>` in an interactive TTY so the human can inspect/import the
   Draft. The import preview and final import must be performed through zintent, not filesystem
   edits.

Do not create or edit zintent store internals (`HEAD.json`, `revisions/`, `snapshots/`, capabilities,
or imported Intent directories). The external Draft under `draft/` is the only skill-authored zintent
input artifact.

## Draft JSON shape

Generate a schema-valid version `1.0.0` revision compatible with zintent Draft import:

- `lifecycle_state`: `draft`
- `parent_revision_id`: `null`
- `hash_algorithm`: `sha-256`
- `canonicalization`: `jcs-rfc8785`
- `actor.actor_type`: `human`
- `actor.identity_source`: `explicit_fallback`
- `actor.authenticated`: `false`
- `operation.type`: `start_review`
- each item has `review_status: "unreviewed"`, `included_in_approval: true`, and Japanese
  `statement`
- provenance should use `content_origin: "source"`, `operation_type: "draft_interpretation"`, and
  reference the generated revision/operation IDs

Use standard-library UUID/timestamp generation; do not add dependencies. `revision_hash` may be a
64-character zero placeholder for external Draft input; zintent import will publish governed state.

## Japanese item style

- Write each `statement` as one concise Japanese sentence.
- Make items individually reviewable by the human.
- Keep requirements at meaning level: what outcome is intended, not how to implement it.
- Mark uncertainty as its own reviewable item only when the source explicitly contains it; otherwise
  ask before generating.

## Boundaries

- Never approve, review, accept, reject, or edit items on the human's behalf.
- Never hand off to Spec Kit or implementation from Draft generation.
- Never claim the Draft is canonical until zintent imports it.
- Report the created source file, Draft file, workspace path, and the next command to run.
