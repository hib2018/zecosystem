---
name: task-only
description: Create or update a minimal task-only tasks.md when the human explicitly wants to skip the normal Spec Kit spec/plan phases but still keep a canonical task definition for ztasks/execution monitoring. Use for "task only", "tasks.md only", "skip spec", "lightweight tasks", or "just track current work" requests.
---

# Task-Only Definition

Create the smallest useful `tasks.md` so execution can be tracked without claiming the full Spec Kit
pipeline was completed.

## Invocation contract

Use this Skill only when the human explicitly chooses the shortcut, e.g. “仕様駆動は飛ばして
tasks.mdだけ作る”, “task-onlyで進める”, or “今の作業taskだけ把握したい”.

Do not use this as the default replacement for Spec Kit. If the request needs durable product
requirements, architecture decisions, compliance/security review, or cross-team acceptance criteria,
recommend the normal `speckit-specify -> speckit-plan -> speckit-tasks` lane instead.

## Inputs

Accept any of:

- a human task description;
- an approved zintent Intent/Snapshot, resolved using the same public-boundary rules as
  `speckit-handoff`;
- existing current work context in the repository.

Never treat an unapproved zintent Draft or chat summary as approved intent. If using zintent input,
read only the approved snapshot fields needed to derive tasks.

## Output location

Prefer the current Spec Kit feature directory when one is active:

1. If `.specify/scripts/bash/check-prerequisites.sh` exists, run it without requiring plan/tasks when
   useful to discover the current `FEATURE_DIR`.
2. If no feature directory is active or discovery fails, create a new directory under `specs/` with
   the next available numeric prefix, e.g. `specs/003-task-only-<slug>/`.
3. Write or update only `tasks.md` unless the human explicitly asks for more.

If a `tasks.md` already exists, preserve completed tasks and unrelated user edits. Append or minimally
edit; do not rewrite the whole file unless the human asks.

## Required `tasks.md` shape

The file must clearly say it is not a completed Spec Kit pipeline:

```md
# Tasks: <short title>

Mode: task-only
Spec: intentionally skipped
Plan: intentionally skipped
Source: <human request or approved snapshot/intent>
Provenance: <snapshot path/id if applicable, else human request timestamp/context>

## Tasks

- [ ] T001 <small actionable task>
- [ ] T002 <small runnable verification>
- [ ] T003 <record result / next blocker if needed>
```

Rules:

- Write the human-readable parts of `tasks.md` in the language the human uses to address the agent,
  including its title, metadata descriptions, task text, and notes. If the request mixes languages,
  use its dominant language unless the human explicitly chooses another. Keep required IDs, markers,
  file paths, commands, and protocol literals unchanged.
- Tasks are the canonical task definition for monitoring.
- Keep tasks small, dependency ordered, and directly executable.
- Include at least one verification task for non-trivial code changes.
- Do not invent architecture, requirements, or acceptance criteria beyond the provided input.
- Mark known shortcuts inline, e.g. `task-only: spec/plan skipped; promote to Spec Kit if scope grows`.

## Handoff to execution

After writing `tasks.md`, report:

- path to `tasks.md`;
- skipped phases (`spec.md`, `plan.md`);
- source/provenance used;
- next command/action for ztasks or manual execution.

Do not automatically start implementation unless the human explicitly asks.

## Promotion path

If the work grows or ambiguity blocks execution, stop and recommend promotion:

```text
task-only tasks.md -> speckit-handoff/specify -> plan -> tasks
```

Do not silently convert task-only artifacts into full Spec Kit artifacts.
