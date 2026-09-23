---
name: zintent-ingest
description: Import an already-approved zintent output into the current project or agent context without starting Spec Kit, planning, task generation, or implementation. Use after zintent review/approval is complete when the human asks to take in, record, ingest, or consume the zintent result only.
---

# zintent Output Ingest

Use this Skill only after zintent has produced an Approved Intent or Approved Intent Snapshot and the
human wants to consume that result without moving to Spec Kit or execution.

## Scope

This Skill answers: “What approved meaning should this project/context remember now?” It does not
interpret a new request, review items, approve anything, plan implementation, create tasks, or run
Spec Kit.

## Inputs

Accept one of:

- an Approved Intent Snapshot path or ref reported by `zintent`
- an Intent path whose canonical state is already approved
- a workspace/search root, only to list approved candidates for the human to choose

Never pick “latest” or “all” automatically. If more than one approved candidate exists, show a
numbered list and ask the human to select.

## Validation

Use only the public `zintent` CLI on `PATH`:

1. Run `zintent validate <input> --output json`.
2. Run `zintent show <input> --output json`.
3. Confirm from structured fields that the artifact is approved or is an immutable Approved Intent
   Snapshot.
4. If the input is an Intent, use the approved snapshot/ref reported by the CLI as the imported
   source of record.

Stop on invalid, unapproved, stale, ambiguous, or missing artifacts. Report the stable code/finding
when available.

## Ingest behavior

Import only the approved meaning and provenance. The smallest acceptable ingest record contains:

- source snapshot path/ref
- intent id and approved revision id/hash when available
- approval time/actor when available
- approved item statements, copied verbatim
- excluded/rejected items only if the human asks or the destination already tracks them

Destination rules:

- If the human names a file, append or update that file with a clearly labeled ingest record.
- If no file is named, do not create one by default; present the ingest record in the response and
  ask where to store it if persistence is needed.
- Keep project-owned records in the project repository when persistence is requested.
- Do not copy runtime sessions, locks, caches, credentials, or raw command logs.

## Boundaries

- Never edit zintent store internals (`HEAD.json`, revisions, snapshots, capabilities, or indexes).
- Never rewrite the Approved Snapshot as a competing canonical artifact; preserve its ref/path as
  provenance.
- Never hand off to Spec Kit, create `spec.md`/`plan.md`/`tasks.md`, or start implementation unless
  the human explicitly asks for that separate next step.
- Keep transformations minimal: verbatim approved statements plus provenance, not a plan or summary
  that changes meaning.
