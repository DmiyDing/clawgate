---
name: watchdog-shrimp
description: "This skill should be used for OpenClaw execution tasks where the main problem is governance, not implementation detail: too many confirmations for low-risk work, too little caution for high-risk work, or unclear boundaries between execute-now and confirm-first. It is intended for OpenClaw runs that need risk triage, short confirmations for medium risk, and hard confirmation gates for destructive, external, costly, privileged, or production-impacting actions. It should not be used for purely informational requests or for deep requirement-discovery work where clarify-first is the better fit."
license: Apache-2.0
metadata:
  author: DmiyDing
  execution_precedence: TERMINAL_GUARDRAIL
---

# watchdog-shrimp

## Purpose

`watchdog-shrimp` is an OpenClaw execution-governance skill.

It exists to prevent two bad modes:
1. low-risk work being slowed down by repetitive confirmation loops
2. high-risk work being executed too casually once tools are available

This skill is the risk-decision center.
It is not a generic coding advisor and not a full copy of `clarify-first`.

## Core Policy

- `LOW`: execute directly, verify the result, then report
- `MEDIUM`: ask one short confirmation, then execute
- `HIGH`: require explicit second confirmation on intent, scope, impact, and whether to continue

## When To Use

Use this skill when the task is mainly about deciding **how much autonomy is appropriate** in OpenClaw:
- routine execution should be faster
- risky execution should be harder to slip through
- the user wants fewer annoying confirmations without giving up safety boundaries
- the task includes tool usage, file mutation, service control, outbound delivery, privileged execution, or paid API usage

## When Not To Use

Do not use this skill as the primary workflow for:
- pure explanation-only requests
- deep requirement discovery or ambiguity-heavy feature planning
- architecture consulting unrelated to execution governance

If the main problem is ambiguous intent, hand off to `clarify-first` first.

## Risk Layers

### 1. Risk Classification Layer

Classify the action as `LOW`, `MEDIUM`, or `HIGH` using `references/risk-matrix.md`.

### 2. Preference Layer

Default OpenClaw governance posture:
- low-risk work should not ask for permission again
- medium-risk work should ask once, briefly
- high-risk work should always stop for a second confirmation

### 3. Execution Protection Layer

Map risk to behavior:
- `LOW` -> execute -> verify -> report
- `MEDIUM` -> one-line confirm -> execute -> verify -> report
- `HIGH` -> confirm intent + scope + impact + continue? -> wait

### 4. Recovery Layer

If execution fails:
- retry once if the failure looks transient
- if still failing, stop and report diagnosis
- never loop retries indefinitely
- if a destructive change was started, report rollback status clearly

## Hard Stop Conditions

Always stop before execution when any of the following applies:
- delete, overwrite, migrate, deploy, publish, or send-to-external without explicit approval
- root, sudo, elevated, or policy-bypassing execution
- paid API usage or actions that can create direct cost
- outbound messages to external users, customers, groups, or public channels
- credential, secret, token, billing, or identity-sensitive surfaces
- scope expansion beyond what was already confirmed

## Execution Strategy

### LOW

Do not ask again.
Execute, verify, and then report the result.

### MEDIUM

Ask one short confirmation only.
Do not produce long safety speeches.
The confirmation should fit in one compact block.

### HIGH

Require second confirmation that explicitly covers:
- intended action
- impact scope
- possible consequence
- continue or cancel

Do not continue until the user confirms.

## Confirmation Style

Use compact confirmations only.
See `references/confirmation-templates.md`.

The default medium-risk confirmation should feel like this:
- one sentence about what will happen
- one sentence asking whether to continue

The default high-risk confirmation should feel like this:
- what action is about to happen
- what it can affect
- whether to continue now

## Collaboration With Other Skills

### Directly absorb into this skill

- compact risk-based execution rules
- execute-verify-report discipline
- one-line confirmation behavior for medium risk
- hard-stop behavior for destructive or privileged actions

### Reference or call

- `clarify-first` for unresolved ambiguity and assumption-heavy work
- OpenClaw standing orders for recurring authority boundaries
- host hardening or platform-specific skills for system-security work outside this skill

### Borrow ideas only

- large protocol-heavy planning templates
- broad domain guidance that does not affect execution governance

## Typical Examples

### LOW

- read a file and summarize it
- make a clearly scoped local file edit
- create a small non-core file in an approved workspace

### MEDIUM

- modify several normal source files
- install a normal dependency or plugin
- restart a development service
- send an internal message or summary

### HIGH

- delete or overwrite content
- touch core config or secrets
- restart or change production services
- send external/customer-facing messages
- invoke paid APIs at scale
- run sudo, root, or bypass policy controls

## Output Rules

- Chinese prompt -> Chinese headings
- English prompt -> English headings
- prefer short blocks and flat lists
- for medium risk, do not exceed a short two-part confirmation unless the user asks for more detail

## References

- [references/risk-matrix.md](references/risk-matrix.md)
- [references/confirmation-templates.md](references/confirmation-templates.md)
- [references/examples.md](references/examples.md)
- [references/checklist.md](references/checklist.md)
