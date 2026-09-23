# zecosystem

Z Ecosystem global knowledge, skills, contracts, protocols, and architecture boundaries shared across `zintent`, `ztasks`, and `zconfig`.

This repository is the Source of Truth for Z Ecosystem elements that are not owned by a single tool and are not machine/harness configuration.

## Prerequisites

Required environment:

- A normal Git-managed text repository.
- An agent, harness, or editor that can read Markdown documentation.
- When using `skills/`, an agent harness that can load Markdown-based skills or an equivalent reference configuration.
- For actual workflow execution, the required Z tools (`zintent`, `ztasks`, and `zconfig`) must be installed separately and available through their public CLI/protocol surfaces.

Constraints:

- The Z Ecosystem is treated as a human-controlled artifact pipeline.
- Tool repositories remain separate: `zintent`, `ztasks`, and `zconfig` each own their domain logic and normative contracts.
- Agent skills orchestrate workflow stages; tool CLIs/protocols/cores own state transitions, validation, persistence, and invariants.
- Harness and machine configuration such as Pi, Codex, macOS, shell, and package-manager settings must not be stored here.
- Runtime state, sessions, locks, caches, snapshots, and credentials must not be stored here.

## Scope

In scope:

- cross-tool agent principles and process boundaries
- global skills that connect Z tools or define ecosystem-wide use
- shared contract/protocol/schema/capability notes once they are truly common
- architecture and artifact-flow documentation

Out of scope:

- tool-specific domain logic and skills owned by `zintent`, `ztasks`, or `zconfig`
- project-local Spec Kit prompts and generated definition artifacts
- Pi, Codex, macOS, shell, package, or other machine/harness settings
- runtime state, caches, locks, sessions, and local stores

## Initial contents

- `skills/zintent-interpret/`: generate a Japanese external Draft from a natural-language request and prepare the `intents/` and `draft/` files needed before running `zintent`.
- `skills/zintent/`: global Meaning Gate orchestration policy for using public `zintent` CLI/TUI safely.
- `skills/zintent-ingest/`: import already-approved zintent output into project/context only, without Spec Kit or implementation.
- `skills/speckit-handoff/`: reusable handoff policy from an Approved Intent Snapshot to a project-owned Spec Kit specify workflow.
- `instructions/agent-principles.md`: shared agent principles for Z Ecosystem work.
- `docs/artifact-flow.md`: high-level artifact pipeline boundaries.
- `docs/architecture.md`: ownership boundaries between this repository and tool repositories.
- `docs/glossary.md`: Japanese cross-tool glossary for zecosystem, zintent, ztasks, and zconfig.
- `docs/zconfig-notes.md`: notes for future extraction candidates from zconfig.

## Standard workflow

```text
Human Request
  -> zintent-interpret: create Japanese Draft and prepare <project>/intents + <project>/draft
  -> zintent workspace: import Draft, human reviews/approves meaning
  -> Approved Intent Snapshot
  -> zintent-ingest: optionally import approved output only and stop
  -> speckit-handoff: pass to project-local Spec Kit specify workflow
  -> spec.md -> plan.md -> tasks.md
  -> ztasks: monitor execution against tasks.md
  -> Verification / Execution Result
  -> zconfig only when a configuration change needs guarded review
```

Key boundaries:

- `zintent-interpret` creates only external Draft input and never edits zintent store internals.
- Draft item statements are Japanese and individually reviewable.
- After review/approval, `zintent-ingest` can record approved output only when explicitly requested.
- Review, approval, handoff, planning, and execution each require explicit human intent.
- `spec.md`, `plan.md`, and `tasks.md` belong to the project repository's Spec Kit workflow.

## Related repositories

- `hib2018/zintent`: Intent review and approval domain, CLI/TUI, contracts, schemas, and tool-specific skills.
- `hib2018/ztasks`: task execution monitoring domain, runtime protocol, contracts, and source adapters.
- `hib2018/zconfig`: configuration-change review domain, proposal/revision/apply protocol, schemas, and security rules.
