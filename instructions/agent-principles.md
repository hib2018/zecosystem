# Z Ecosystem Agent Principles

- Human decisions and explicit review boundaries are authoritative.
- Skills orchestrate; tools and domain cores enforce deterministic validation, state transitions, persistence, and invariants.
- Use public CLIs/protocols only. Do not mutate tool stores or internal persistent state directly.
- Memory and conversation context are evidence, not approval.
- Definition artifacts (`spec.md`, `plan.md`, `tasks.md`) are project-owned. Runtime monitoring must not silently redefine them.
- Stop and ask when a choice would materially change scope, architecture, persisted data, or external state.
- Keep agents replaceable. Avoid vendor-specific semantics in shared contracts.
- Keep runtime state, locks, sessions, caches, snapshots, and disposable outputs out of global knowledge by default.
