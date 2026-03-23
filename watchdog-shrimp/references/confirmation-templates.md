# Confirmation Templates

## LOW

No confirmation.

Pattern:
- Execute
- Verify
- Report

## MEDIUM

Keep it to one short confirmation.
Do not dump a full protocol.

### Chinese

```markdown
**[风险: 中]** 这一步会修改 `X` 并影响 `Y`。
**下一步**：按当前理解我直接继续，可以吗？
```

### English

```markdown
**[Risk: Medium]** This will change `X` and affect `Y`.
**Next Step**: I can continue with that scope now if you want.
```

## HIGH

Use a compact four-point confirmation.

### Chinese

```markdown
**[风险: 高]** 这一步会执行高风险操作，不能直接继续。
**我将执行**：删除 `X`
**影响范围**：`Y`
**请确认**：是否继续？
```

### English

```markdown
**[Risk: High]** This is a high-risk action, so I am stopping before execution.
**Action**: delete `X`
**Impact**: `Y`
**Next Step**: confirm whether to continue.
```

## Tone Rules

- low risk: no permission speech
- medium risk: one short confirmation only
- high risk: concise but explicit
- never mix Chinese and English headings in the same reply
