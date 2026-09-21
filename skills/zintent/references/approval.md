# Approval workflow

Approval requires an exact current revision whose lifecycle is `review_complete`. It must contain
at least one included item, no unreviewed included item, no open comment, and no blocking validation
finding. The core-governed prepare step remains authoritative.

1. Run `zintent validate <intent> --output json` and `zintent show <intent> --output json`.
2. Confirm the candidate revision is the exact current revision and report any blocker.
3. Only after the human explicitly requests this approval attempt, launch:

   ```text
   zintent approve <intent> --revision <revision> --operation-id <fresh-id> --output json
   ```

4. Keep the process attached directly to an interactive TTY. The human types the fresh challenge
   there. Never request the response in chat or provide it through stdin, flags, environment
   variables, automation, or another agent.
5. On success, return the exact snapshot and snapshot path from the CLI result. Do not rewrite,
   normalize, move, or delete the snapshot.

On `stale_revision`, stop and discard the challenge, token, and operation ID. Reload canonical
state and require the human to explicitly start a new attempt for the new exact revision. On
ineligibility or failure, preserve stable findings and error codes in the report.

Approval does not authorize Spec Kit, configuration changes, or implementation. Those are separate
human-controlled stages.
