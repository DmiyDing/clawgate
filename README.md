# watchdog-shrimp

OpenClaw-specific execution governance for agents that are either too chatty or too reckless.

[中文说明](./README.zh-CN.md)

## What It Is

`watchdog-shrimp` is an OpenClaw skill for a very specific operational problem:

- low-risk work gets slowed down by too many confirmations
- high-risk work can still move too fast once tools are available

This skill fixes that by acting as a risk-decision layer.

It does not try to make the agent smarter at everything.
It makes one behavior better: deciding when to execute, when to ask once, and when to stop.

## Core Behavior

- `LOW` risk: execute directly, verify, then report
- `MEDIUM` risk: ask one short confirmation, then execute
- `HIGH` risk: require explicit second confirmation on intent, scope, impact, and whether to continue

## Why It Exists

The usual tradeoff in agent execution is bad in both directions.

If the agent asks for confirmation on every small step, it becomes slow and irritating.
If the agent gets broad autonomy with weak governance, it can make destructive, expensive, or embarrassing decisions too easily.

`watchdog-shrimp` is built for the middle ground:

- fewer interruptions on safe work
- harder stops on dangerous work
- compact confirmations instead of long protocol speeches

## What It Is Good At

Use it when the main question is not *how to implement something*, but *how much autonomy is appropriate right now*.

Examples:
- small local edits that should just run
- multi-file normal changes that need a quick confirmation
- delete / overwrite / deploy / outbound send / root-like actions that must not slip through casually

## What It Is Not

- not a generic coding advisor
- not a replacement for deep requirement discovery
- not a clone of `clarify-first`
- not a runtime enforcement layer

If the real problem is unresolved ambiguity, use `clarify-first` first.
If you need truly non-bypassable enforcement, that belongs in OpenClaw runtime and policy.

## Skill Structure

- `watchdog-shrimp/SKILL.md`: main skill
- `watchdog-shrimp/references/risk-matrix.md`: executable risk rules
- `watchdog-shrimp/references/confirmation-templates.md`: compact confirmation formats
- `watchdog-shrimp/references/examples.md`: trigger and non-trigger examples
- `watchdog-shrimp/references/checklist.md`: execution checklist
- `watchdog-shrimp/evals/evals.json`: seed evaluation cases

## Risk Model

### LOW

Execute now. Verify. Report.

Typical examples:
- read-only inspection
- clearly scoped local edits
- small non-core file creation

### MEDIUM

Ask once, briefly. Then execute.

Typical examples:
- normal multi-file changes
- development restarts
- non-core config edits
- internal sends

### HIGH

Stop and require explicit approval.

Typical examples:
- delete / overwrite / migrate / deploy
- external sends
- paid API usage
- privileged execution
- secrets or core policy surfaces

## Positioning

This project is intended to start as a standalone open-source skill.

That is deliberate.
The behavior model needs real-world tuning before any part of it should move into OpenClaw core.

## Validation

The skill validates against the current skill-creator validator.

## Status

This is an execution-governance skill for OpenClaw.
It is designed to be useful immediately at the skill layer, while staying honest about which guarantees require runtime enforcement later.
