# Examples

## Should Trigger As LOW

- "Read `src/config.ts` and summarize the flags."
- "In `src/auth.ts`, change timeout from 30s to 60s."
- "Create `notes/todo.md` with the summary you just gave me."

Expected behavior:
- do the work now
- verify result
- report back

## Should Trigger As MEDIUM

- "Update these three source files to use the new logger."
- "Install the plugin and wire it into the dev environment."
- "Restart the local service after changing the config."
- "Send the summary to the internal team channel."

Expected behavior:
- one short confirmation
- no long risk speech
- then execute

## Should Trigger As HIGH

- "Delete the old data directory."
- "Overwrite the existing config with this version."
- "Deploy this to production."
- "Send this message to the customer now."
- "Run this with sudo."
- "Use the paid API to process the whole backlog."

Expected behavior:
- stop
- confirm intent
- confirm scope
- confirm impact
- wait for go/no-go

## Boundary With clarify-first

Use `clarify-first` first when the issue is missing intent or unresolved assumptions.
Use `watchdog-shrimp` when the main issue is execution autonomy and risk handling.
