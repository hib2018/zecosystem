# zecosystem

Z Ecosystem global knowledge, skills, contracts, protocols, and architecture boundaries shared across `zintent`, `ztasks`, and `zconfig`.

This repository is the Source of Truth for Z Ecosystem elements that are not owned by a single tool and are not machine/harness configuration.

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

- `skills/zintent/`: global Meaning Gate orchestration policy for using public `zintent` CLI/TUI safely.
- `skills/speckit-handoff/`: reusable handoff policy from an Approved Intent Snapshot to a project-owned Spec Kit specify workflow.
- `instructions/agent-principles.md`: shared agent principles for Z Ecosystem work.
- `docs/artifact-flow.md`: high-level artifact pipeline boundaries.
- `docs/architecture.md`: ownership boundaries between this repository and tool repositories.

## Related repositories

- `hib2018/zintent`: Intent review and approval domain, CLI/TUI, contracts, schemas, and tool-specific skills.
- `hib2018/ztasks`: task execution monitoring domain, runtime protocol, contracts, and source adapters.
- `hib2018/zconfig`: configuration-change review domain, proposal/revision/apply protocol, schemas, and security rules.
- `hib2018/dotfiles`: machine and harness configuration. Dotfiles may symlink to this repository for global skills.
