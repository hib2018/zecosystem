# Z Ecosystem Artifact Flow

```text
Human Request
  -> zintent Meaning Gate
  -> Approved Intent Snapshot
  -> Project-owned Spec Kit Definition Pipeline
  -> spec.md / plan.md / tasks.md
  -> ztasks Execution Monitor
  -> Verification and Execution Result
  -> optional zconfig Configuration Review Boundary
```

## Boundaries

- `zintent` answers whether the interpreted meaning is approved by the human. It does not plan implementation.
- Spec Kit owns project-local definition artifacts. Global skills may hand off approved content but must not replace the project's installed workflow.
- `ztasks` monitors execution against a task definition. Runtime state does not redefine `tasks.md`.
- `zconfig` reviews and applies structured configuration-change proposals near the end of work. It is a guarded review boundary, not a generic chat planner.

## Shared invariants

- Use stable structured result fields and codes, not display prose, as integration contracts.
- Do not directly edit tool-owned stores, HEAD files, event logs, capabilities, or snapshots.
- Approval and confirmation challenges belong to the human at the responsible interface.
- A downstream stage requires an explicit human request; success in one stage is not automatic authorization for the next.
