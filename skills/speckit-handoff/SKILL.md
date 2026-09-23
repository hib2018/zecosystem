---
name: speckit-handoff
description: Hand an immutable zintent Approved Intent Snapshot or approved zintent Intent to the current project's official Spec Kit specify workflow. Use only when the human explicitly asks to turn an approved intent into a specification; do not use for unapproved requests, review, planning, task generation, or implementation.
---

# Approved Intent to Spec Kit

Connect the zintent Meaning Gate to the Project-owned Spec Kit Definition Pipeline without copying
or replacing Spec Kit's workflow.

## Invocation contract

This Skill may be invoked when the human explicitly asks to hand approved zintent artifact(s) to
Spec Kit. Accept any of:

- an immutable Approved Intent Snapshot path;
- an approved Intent directory/path whose fresh canonical state cross-references an approved
  snapshot; or
- a workspace/search root request, such as “list approved intents under `intents/` and let me choose”.

Do not accept a Draft, `in_review`, `review_complete`, `rejected`, chat summary, memory entry, raw
Human Request, or copied JSON blob as handoff input. If the human gives an Intent that is not already
approved, stop and direct them to the zintent review/approval flow. This Skill never performs
approval and never answers approval challenges.

If multiple approved artifacts are selected, run one independent Spec Kit `specify` handoff per
artifact. Do not merge multiple snapshots into one feature unless the human explicitly requests that
combined scope after seeing the list.

## Spec Kit bootstrap

Before creating a specification, make the current Project usable with Pi Spec Kit:

1. If `.specify/` is missing, stop and report that the Project must be initialized with Spec Kit.
   Ask before running initialization because it mutates the Project. If the human approves, run the
   Project's installed Specify CLI, e.g. `specify init --here --integration pi --script sh` (do not
   use `--force` unless separately and explicitly authorized).
2. If `.specify/` exists but `.pi/prompts/speckit.specify.md` is missing, install only the Pi
   integration with `specify integration install pi --script sh`. This is the minimal handoff
   bootstrap; do not refresh shared templates with `init --force`.
3. Run `specify integration status` when available. Stop on errors or missing managed files.
4. Use the Project-local `.pi/prompts/speckit.specify.md` as the authoritative workflow. Never
   substitute a global prompt copy.

## Discover and select approved artifacts

When the human provides a workspace/search root instead of an exact artifact:

1. Inspect only the requested root (default to `intents/` only when the human's request clearly
   refers to the current repository's zintent workspace). Do not search the whole home directory.
2. Prefer `zintent workspace`/workspace listing commands when they provide machine-readable approved
   state; otherwise do the smallest filesystem scan for direct child Intent directories and snapshot
   files.
3. For each candidate Intent, run `zintent validate <intent> --output json` and
   `zintent show <intent> --output json`; include only fresh canonical `approved` Intents with a
   resolvable approved snapshot. For snapshot files, validate the snapshot shape directly.
4. Present a numbered list with intent ID, short snapshot ID, path, approved revision, and first
   approved statement. Include rejected/unapproved counts only as skipped summary, not as choices.
5. Ask the human to choose one or more numbers/ranges (for example `1`, `1,3`, or `2-4`). Do not
   auto-select “latest” or “all” without an explicit human choice.
6. Resolve each selected entry to an approved snapshot and continue. If no approved candidates are
   found, stop with the root searched and the reason.

## Resolve the approved snapshot

For a snapshot path:

1. Read the snapshot file.
2. Validate that it has `schema_version: 1.0.0`, a content-addressed `snapshot_id`, non-empty
   `approved_content.items`, a human `approval.approving_actor`, and
   `approval.validation_result.eligible: true` with no blocking findings.

For an Intent path/directory:

1. Run `zintent validate <intent> --output json` and stop on blocking findings.
2. Run `zintent show <intent> --output json` and require fresh canonical state to be `approved`.
3. Obtain the canonical approved snapshot reference/path from the approved state/HEAD output.
4. Read and validate that snapshot as above. Do not inspect unrelated history.

If the CLI does not expose a trustworthy approved snapshot reference, stop and ask for the exact
snapshot path returned by `zintent approve`.

## Handoff

1. Read only the snapshot metadata, `approved_content.items`, and `source_references` needed for the
   specification. Do not load unrelated Intent history or repository-wide context.
2. Read the Project's `.pi/prompts/speckit.specify.md` completely and follow that version as the
   authoritative specification workflow.
3. Build the feature description from approved item statements and applicable source references.
   Preserve meaning; do not silently add, omit, weaken, or reinterpret an approved item. Write
   human-readable generated prose in the language the human uses to address the agent; if the
   interaction mixes languages, use its dominant language unless the human explicitly chooses
   another. Keep IDs, paths, commands, code, schema keys, and protocol literals unchanged, and carry
   this language convention into later Project-local Spec Kit artifacts, especially `tasks.md`.
4. Run the official Spec Kit specify workflow to create exactly one feature specification for the
   current snapshot. For multiple selections, repeat this step once per selected snapshot.
5. Record the snapshot path, snapshot ID, Intent ID, confirmed revision ID, and approved revision ID
   as provenance in the generated specification or adjacent Project-owned Spec Kit metadata, using
   the official format when it provides one. Do not wrap the snapshot or create a second Approved
   Intent artifact.
6. When the official workflow requires a materially ambiguous choice not resolved by the Approved
   Intent, stop at its clarification boundary and ask the human. The snapshot remains evidence, not
   permission to invent scope.
7. Report the generated feature directory, `spec.md`, provenance used, checklist result, bootstrap
   actions taken, and the next available Spec Kit stage. Do not automatically start clarify, plan,
   tasks, or implementation.

## Boundaries

- Never modify the Approved Intent Snapshot or zintent store.
- Never use an unapproved Intent revision, chat summary, memory entry, or raw Human Request as a
  replacement for the Approved Intent input.
- Never approve an Intent, answer an approval challenge, or infer human approval from this handoff
  request.
- Spec Kit owns `spec.md`, `plan.md`, and `tasks.md` in the Project repository. zintent does not plan,
  and this Skill does not create its own competing definition format.
- Project-local prompts and `.specify/` files stay Project-owned and versioned with that Project;
  this global Skill contains only the reusable handoff policy.
