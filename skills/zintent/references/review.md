# Review workflow

Accept an existing Intent selector understood by `zintent`. Canonical state comes from its verified
HEAD-selected revision, not conversation text or a copied JSON object.

1. Inspect with `zintent validate <intent> --output json` and `zintent show <intent> --output json`.
2. Launch `zintent review <intent>` in an interactive TTY. An explicit `--actor-id` is only a local
   attribution fallback and does not represent authentication.
3. Let the human accept, edit, comment, reject, resolve comments, and complete review. Text used for
   edits, rejection reasons, or comments must come from explicit human input.
4. After exit or failure, reload canonical state and report the Intent ID, current revision,
   lifecycle, and blocking findings.

If direct CLI assistance is needed, use only public review commands and always supply the exact
current `--expected-revision` plus a fresh operation ID for mutations. Edits require a fresh
`edit-preview` and its matching one-use preview token before applying the edit.

On `stale_revision`, discard the pending decision, operation ID, and preview token. Reload HEAD,
show the change to the human, and wait for a new explicit decision. Never replay or silently rebase
the mutation.

Do not invoke approval, planning, Spec Kit, or downstream execution from this workflow.
