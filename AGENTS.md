# AGENTS.md

## Repository purpose

This repository owns Z Ecosystem global elements shared across `zintent`, `ztasks`, and `zconfig`.

## Boundaries

- Keep tool-specific domain logic, transitions, schemas, and skills in the owning tool repository until a concrete cross-tool reuse need is established.
- Do not create a shared runtime, common library, or framework here without separate human approval.
- Do not store machine-specific Pi/Codex/macOS settings here; those belong in dotfiles.
- Do not store runtime state, locks, caches, sessions, snapshots, or secrets.
- Prefer documentation, skills, contracts, protocols, and schemas that are inspectable and versioned.

## Change safety

- Preserve external tool repositories unless the human explicitly asks to edit them.
- When moving global skills, keep Pi/Codex compatibility through symlinks or documented harness configuration.
- If a common element is derived from a tool repository, record provenance and leave the normative tool-specific contract in that tool until migration is explicitly approved.
