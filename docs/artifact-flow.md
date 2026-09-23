# Z Ecosystem Artifact Flow

```text
Human Request
  -> zintent-interpret Draft Preparation
  -> zintent Meaning Gate
  -> Approved Intent Snapshot
  -> optional zintent-ingest Output Import Only
  -> Project-owned Spec Kit Definition Pipeline
  -> spec.md / plan.md / tasks.md
  -> ztasks Execution Monitor
  -> Verification and Execution Result
  -> optional zconfig Configuration Review Boundary
```

## Stage responsibilities

| Stage | Artifact | Owner |
|---|---|---|
| `zintent-interpret` | External Draft JSON in the human's interaction language plus `<project>/draft/<slug>.source.md` | Skill-authored external input only |
| `zintent workspace` | imported Intent revisions and Approved Intent Snapshot | public `zintent` CLI/TUI and core |
| `zintent-ingest` | minimal approved meaning/provenance record when requested | Global ingest skill |
| `speckit-handoff` | one feature request passed to Project Spec Kit | Global handoff skill |
| Spec Kit | `spec.md`, `plan.md`, `tasks.md` | Project repository |
| `ztasks` | execution monitoring state and result evidence | public `ztasks` tool |
| `zconfig` | configuration proposal/revision/apply artifacts | public `zconfig` tool |

## Boundaries

- `zintent-interpret` prepares only external Draft input: `<project>/intents/`, `<project>/draft/`, source note, and Draft JSON. It never edits zintent store internals.
- `zintent` answers whether the interpreted meaning is approved by the human. It does not plan implementation.
- `zintent-ingest` may record approved output only; it does not create Spec Kit artifacts or tasks.
- Spec Kit owns project-local definition artifacts. Global skills may hand off approved content but must not replace the project's installed workflow.
- `ztasks` monitors execution against a task definition. Runtime state does not redefine `tasks.md`.
- `zconfig` reviews and applies structured configuration-change proposals near the end of work. It is a guarded review boundary, not a generic chat planner.

## Shared invariants

- Use stable structured result fields and codes, not display prose, as integration contracts.
- Do not directly edit tool-owned stores, HEAD files, event logs, capabilities, or snapshots.
- Approval and confirmation challenges belong to the human at the responsible interface.
- A downstream stage requires an explicit human request; success in one stage is not automatic authorization for the next.
