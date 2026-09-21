# Architecture Boundaries

## Source of Truth split

| Area | Owner |
|---|---|
| Z Ecosystem global skills and principles | `zecosystem` |
| zintent domain, artifact lifecycle, CLI/TUI, schemas, tool-specific skills | `zintent` |
| ztasks task runtime, event protocol, dependency extraction, source adapters | `ztasks` |
| zconfig proposal/revision/apply review protocol, schemas, security model | `zconfig` |
| Pi/Codex/macOS settings and symlink wiring | `dotfiles` |

## Current migration decisions

- `skills/zintent` moved here from dotfiles because it is a global Meaning Gate usage policy rather than Pi configuration.
- `skills/speckit-handoff` moved here because it defines a reusable Z handoff boundary between an Approved Intent Snapshot and project-owned Spec Kit workflows.
- Project-local Spec Kit skills remain in each tool repository for now. They are upstream Spec Kit workflow copies, not yet Z Ecosystem-specific global skills.
- zintent-specific `zintent-review` and `zintent-approve` remain in `zintent` because they encode tool-specific CLI and lifecycle rules.
- ztasks and zconfig contracts remain in their tool repositories until a shared cross-tool contract is explicitly extracted.

## Future candidates

- A Z Ecosystem Spec Kit profile or overlay, if repeated customization needs emerge.
- Shared envelope, stable-code, actor, capability, and redaction principles, but only after comparing current zintent, ztasks, and zconfig contracts.
- zconfig integration guidance for final configuration review in the ecosystem artifact flow.
