# Confirmation Templates

## Output Protocol

Use these machine-readable fields whenever the user, harness, or downstream system expects stable structure:
- `risk_level`
- `blocked`
- `missing_fields`
- `approval_mode`
- `continue_or_cancel`
- `itemized_actions`

`HIGH` and `CRITICAL` must output the machine-readable fields plus the human-readable headings in the same block.
`LOW` and `MEDIUM` may stay human-readable unless a machine-readable consumer is explicitly in play.

## LOW

No confirmation.

Pattern:
- Execute
- Verify
- Report

## MEDIUM

No confirmation.

Pattern:
- Action
- Verify
- Result

Stable execution report:
- `Action`
- `Verify`
- `Result`

Optional machine-readable fields when requested:
- `risk_level: MEDIUM`
- `blocked: false`

### Result Template

```markdown
Action
[what changed]
Verify
[how it was checked]
Result
[final state]
```

## HIGH

Use a blocked confirmation with this fixed field order:
- `Risk: HIGH`
- `Action`
- `Scope`
- `Impact`
- `Possible Consequence`
- `Continue or Cancel`
- `Missing Fields`
- `Blocked Until`

Rules:
- Start by explicitly stating `Risk: HIGH`.
- Do not use ordinary clarification instead of this template for plugin install + config mutation + restart.
- If any information is missing, keep the missing-fields prompt inside the blocked confirmation block.
- Do not degrade into ordinary Q&A or clarification-first style questioning.
- Do not output a default execution plan, ordered implementation steps, or fallback actions before explicit confirmation.
- State authorization granularity explicitly: this approval covers this exact high-risk action only.
- State `Continue or Cancel` explicitly.

Required machine-readable fields:
- `risk_level: HIGH`
- `blocked: true`
- `approval_mode: explicit_confirmation`
- `missing_fields`
- `continue_or_cancel`
- `itemized_actions: []`

### English

```markdown
Risk: HIGH
risk_level: HIGH
blocked: true
approval_mode: explicit_confirmation
Action
Install the named plugin, mutate `plugins.entries`, and restart the gateway
Scope: install plugin + mutate `plugins.entries` + restart gateway on the named target
Impact: OpenClaw runtime wiring and gateway health may change for this target
Possible Consequence: a bad install, config mutation, or restart can leave the instance unhealthy
Continue or Cancel: continue or cancel
continue_or_cancel: continue or cancel
missing_fields:
- plugin source
- target instance
Missing Fields:
- plugin source
- target instance
Blocked Until: explicit continue/cancel confirmation is given for this exact action
itemized_actions: []
Authorization Granularity: this approval covers this exact high-risk action only, not later restart / delete / outbound send / paid-loop steps outside the named scope
```

### Chinese

```markdown
Risk: HIGH
risk_level: HIGH
blocked: true
approval_mode: explicit_confirmation
Action
安装命名插件、修改 `plugins.entries`、并重启 gateway
Scope: 在指定目标上安装插件 + 修改 `plugins.entries` + 重启 gateway
Impact: OpenClaw 运行时接线与 gateway 健康状态可能发生变化
Possible Consequence: 如果安装、配置或重启判断错误，实例可能变得不健康
Continue or Cancel: continue or cancel
continue_or_cancel: continue or cancel
missing_fields:
- 插件来源
- 目标实例
Missing Fields:
- 插件来源
- 目标实例
Blocked Until: 已对这一组高风险动作给出明确 continue/cancel 之前不得执行
itemized_actions: []
Authorization Granularity: 本次授权仅覆盖这一组已命名的高风险动作，不覆盖后续新增的重启 / 删除 / 外发 / 付费循环
```

### High-Risk But Information Is Missing

Use this exact skeleton when a request already hits `HIGH` but key fields are missing:

```markdown
Risk: HIGH
risk_level: HIGH
blocked: true
approval_mode: explicit_confirmation
Action
Install one plugin, mutate `plugins.entries`, and restart the gateway
Blocked Until: the exact missing information is provided and the exact action receives explicit continue/cancel confirmation
Continue or Cancel: continue or cancel
continue_or_cancel: continue or cancel
missing_fields:
- plugin source
- plugin id
- install method
Missing Fields:
- plugin source
- plugin id
- install method
Scope: plugin install + `plugins.entries` mutation + gateway restart
Impact: OpenClaw runtime wiring and gateway availability may change
Possible Consequence: guessing any missing field can break plugin wiring or leave the gateway unhealthy
itemized_actions: []
```

Rules:
- Keep `Missing Fields` and `Blocked Until` inside the same blocked confirmation block.
- Do not ask a free-form follow-up question outside this template.
- Do not propose a default install plan before explicit confirmation.

## CRITICAL

Use a blocked itemized confirmation with this fixed field order:
- `Risk: CRITICAL`
- `Critical Action Items`
- `Authorization Granularity`
- `Approve Each Item`
- `Continue or Cancel`
- `Blocked Until`

Rules:
- Always start by explicitly stating `Risk: CRITICAL`.
- Never merge approvals for multiple critical items.
- Use `Approve Each Item` and require a separate approval or cancellation for every numbered item.
- For broadcast or public-channel work, list each audience or destination separately.
- Do not output a bundled execution plan before itemized approval is granted.
- Include an explicit refusal block:
  - `I will not execute this on a general confirmation`
  - `Merged approval is not accepted`
  - `Each item must be approved separately`

Required machine-readable fields:
- `risk_level: CRITICAL`
- `blocked: true`
- `approval_mode: itemized`
- `missing_fields`
- `continue_or_cancel`
- `itemized_actions`

### Critical Example

### Critical Fixed Skeleton

```markdown
Risk: CRITICAL
risk_level: CRITICAL
blocked: true
approval_mode: itemized
missing_fields: []
Critical Action Items:
Item 1: Delete shared user-data directory
Item 2: Rotate shared router configuration
Item 3: Notify affected users
Authorization Granularity: approve each item separately; do not merge authorization across items
Approve Each Item: reply item-by-item with approve or cancel
Continue or Cancel: continue or cancel
continue_or_cancel: continue or cancel
Blocked Until: each item receives separate approval or cancellation
I will not execute this on a general confirmation.
Merged approval is not accepted.
Each item must be approved separately.
itemized_actions:
- Delete shared user-data directory
- Rotate shared router configuration
- Notify affected users
```

### Broadcast / Public Channel Template

Use this exact template for customer mailing lists, public channels, or broad external delivery:

```markdown
Risk: CRITICAL
risk_level: CRITICAL
blocked: true
approval_mode: itemized
missing_fields: []
Destinations:
- customer mailing list `A`
- public channel `B`
Audience:
- customers in mailing list `A`
- viewers in public channel `B`
Message Content:
- [exact announcement text or approved message identifier]
Authorization Granularity: approve each destination separately; `Approve all external destinations` is not accepted
Approve Each Item: reply destination-by-destination with approve or cancel
Approve Each Destination: reply destination-by-destination with approve or cancel
Continue or Cancel: continue or cancel
continue_or_cancel: continue or cancel
Blocked Until: each destination receives separate approval or cancellation
I will not execute this on a general confirmation.
Merged approval is not accepted.
Each item must be approved separately.
itemized_actions:
- send to customer mailing list `A`
- send to public channel `B`
```

Rules:
- Approvals must be destination-by-destination, not audience-wide.
- `Approve all external destinations` or similar bundled approval is not accepted.
- Do not replace `Approve Each Destination` with a general warning paragraph.

## OpenClaw-Specific Notes

- if the action touches `~/.openclaw/openclaw.json`, plugins, gateway, delivery, or shared routing, say that directly
- if the action is only a read or recommendation, do not ask for confirmation just because the surface is sensitive
- if a medium-risk action is repeatedly approved, shorten result reporting when useful
- do not treat vague enthusiasm or unrelated acknowledgment as approval for a high-risk action
- if a recovery workflow is unavailable, say so and output the minimum recovery handoff instead of improvising manifest/config surgery

## Tone Rules

- low risk: no permission speech
- medium risk: no permission speech
- high risk: concise but explicit
- critical risk: itemized, explicit, and non-bundled
- no filler, no duplicate confirmation blocks
- never mix Chinese and English headings in the same reply
