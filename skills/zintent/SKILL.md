---
name: zintent
description: Orchestrate human review and approval of an existing zintent Intent through the public CLI. Use when inspecting, reviewing, resuming, or approving an Intent; do not use for specification planning, task generation, or direct artifact mutation.
---

# zintent Meaning Gate

Use `zintent` as the Human Gate that answers: “Is this the meaning the human approves?” Keep it
separate from Spec Kit planning and downstream execution.

## Route the request

- For inspection, run `zintent validate <intent> --output json` and `zintent show <intent> --output
  json`. Treat their structured fields and stable codes as authoritative.
- For item review or resuming an incomplete review, read [references/review.md](references/review.md).
- For final approval of an exact `review_complete` revision, read
  [references/approval.md](references/approval.md).
- For Draft creation from natural-language input, use the `zintent-interpret` Skill first. It must
  generate a Japanese external Draft and prepare the workspace/draft files before `zintent` is run.
- For a workspace containing multiple Intents or an external schema-valid Draft, launch
  `zintent workspace <workspace>` in an interactive TTY. Do not invent a canonical Intent or write
  zintent store files directly.
- For post-approval import/recording without Spec Kit or implementation, use the `zintent-ingest`
  Skill. It consumes only already-approved output.

## Invariants

- Use only the public `zintent` frontend found on `PATH`; never invoke `zintent-core` directly.
- Never edit Intent revisions, `HEAD.json`, capabilities, snapshots, or other zintent persistent
  state through filesystem operations.
- Human review decisions and approval challenge entry belong to the human. Never infer, automate,
  prefill, relay, or submit them.
- A successful approval yields the immutable Approved Intent Snapshot reported by the CLI. Do not
  wrap or rewrite it as a competing canonical artifact.
- Stop on stale revision, invalid artifact, failed persistence, missing TTY, or unresolved human
  choice. Report the stable error/finding code and current canonical state.
- Do not start Spec Kit or implementation merely because approval succeeded. Handoff to the next
  stage requires an explicit human request.
