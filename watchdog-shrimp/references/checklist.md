# Execution Checklist

## Before Classifying

- Is this request about execution governance rather than broad requirement discovery?
- Is the task read-only, mutating, external, privileged, or cost-sensitive?
- Does the action touch normal files, core config, services, secrets, outbound delivery, or system surfaces?

## LOW Check

Proceed directly only if all are true:
- scope is explicit enough
- no destructive action
- no outbound send
- no paid or privileged action
- no core runtime or secret surface

## MEDIUM Check

Use one short confirmation if any are true:
- multiple file edits with limited blast radius
- new behavior but not core runtime/policy
- internal send or limited-side-effect API call
- install/restart in development context

## HIGH Check

Force second confirmation if any are true:
- delete / overwrite / migrate / deploy / publish
- external send
- sudo / root / elevated / bypass policy
- paid API usage with meaningful cost
- core config / auth / secret / delivery-routing surface

## After Execution

- verify the result, not just the action
- report what changed
- if anything deviated from expected scope, say so explicitly
- if a destructive action failed halfway, report rollback status
