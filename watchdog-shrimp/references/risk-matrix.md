# Risk Matrix

## Default Rule

This matrix is for OpenClaw execution governance, not broad requirement discovery.

- `LOW`: execute directly, then verify and report
- `MEDIUM`: ask one short confirmation, then execute
- `HIGH`: force explicit second confirmation

## LOW

Default examples:
- read-only queries
- file search, listing, grep, inspection
- clearly scoped local file edit in normal source files
- create a small non-core file inside approved workspace
- formatting, comments, docs, local cleanup with no destructive side effects

Behavior:
- do not ask again
- execute now
- verify outcome
- report result

## MEDIUM

Default examples:
- normal file modification across multiple files
- new file creation with behavior impact but limited blast radius
- non-core config updates
- install ordinary plugin/dependency
- restart development service
- internal message delivery
- ordinary API call with limited cost or side effects

Behavior:
- ask exactly one short confirmation
- avoid protocol-heavy explanations
- if user confirms, execute immediately

## HIGH

Default examples:
- delete, overwrite, bulk replace
- modify core config, auth boundary, secret surface, or policy surface
- install system-level plugin/integration with broad permissions
- restart production or shared services
- outbound message to external user, customer, or public group
- meaningful paid API / billing-impacting action
- root, sudo, elevated, policy-bypassing, or host-level execution
- publish, deploy, migrate, release, or irreversible data change

Behavior:
- stop before execution
- confirm intent
- confirm scope
- confirm impact
- ask continue or cancel

## Special Cases

### Reads stay LOW

Even if the topic sounds sensitive, read-only inspection remains `LOW` unless it exposes secrets or triggers outbound side effects.

### External send is never LOW

If the action sends content outside the current agent boundary, treat it as at least `MEDIUM`.
Customer-facing, public, or identity-sensitive sends are `HIGH`.

### Core configuration escalates

Changes to core runtime configuration, approval policy, delivery routing, or secret handling should escalate to `HIGH`.

### Cost-sensitive actions escalate

If an action can generate meaningful spend, treat it as `HIGH` unless the user already gave explicit spending approval.
