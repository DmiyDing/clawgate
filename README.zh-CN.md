# watchdog-shrimp

面向 OpenClaw 的执行治理 skill，用来解决 Agent 不是太啰嗦、就是太冒进的问题。

[English README](./README.md)

## 它是什么

`watchdog-shrimp` 是一个 OpenClaw 专用的执行治理 skill。

它解决的是一个很具体的问题：

- 低风险任务反复确认，效率低
- 高风险任务拿到权限后又可能推进过快，边界不稳

它不是想让 Agent 在所有事情上都更聪明。
它只做一件事：把“什么时候直接执行，什么时候问一句，什么时候必须硬停”这件事做对。

## 核心行为

- `LOW`：直接执行，验证结果，再汇报
- `MEDIUM`：一句话确认，然后执行
- `HIGH`：必须二次确认意图、范围、影响和是否继续

## 为什么要做它

Agent 执行里常见的问题有两个极端：

1. 什么都问，导致效率很差
2. 什么都敢做，导致风险很高

`watchdog-shrimp` 的目标就是把这两种极端拉回中间：

- 安全的事情少打扰
- 危险的事情拦得住
- 中间态的事情用最短确认完成

## 它适合什么

当问题的核心不是“怎么实现”，而是“现在这一步该不该直接干”的时候，就适合用它。

例如：
- 小范围本地修改应直接执行
- 普通多文件修改应快速确认
- 删除、覆盖、部署、外发、提权操作必须谨慎处理

## 它不是什么

- 不是通用编程顾问
- 不是深度需求澄清器
- 不是 `clarify-first` 的复制版
- 不是 OpenClaw runtime 的硬策略替代品

如果问题核心是需求模糊或假设过多，先用 `clarify-first`。
如果你需要真正不可绕过的硬阻断，那必须进入 OpenClaw runtime / policy。

## 目录结构

- `watchdog-shrimp/SKILL.md`：主 skill
- `watchdog-shrimp/references/risk-matrix.md`：风险分级规则
- `watchdog-shrimp/references/confirmation-templates.md`：紧凑确认模板
- `watchdog-shrimp/references/examples.md`：触发与非触发示例
- `watchdog-shrimp/references/checklist.md`：执行检查清单
- `watchdog-shrimp/evals/evals.json`：初始评测样例

## 风险模型

### LOW

直接执行，验证，汇报。

典型场景：
- 只读查询
- 范围明确的本地小改动
- 非核心文件的小规模新增

### MEDIUM

问一句，用户确认后继续。

典型场景：
- 普通多文件修改
- 开发环境重启
- 非核心配置变更
- 对内发送信息

### HIGH

硬停，要求显式确认。

典型场景：
- 删除 / 覆盖 / 迁移 / 部署
- 对外发送消息
- 付费 API 调用
- 提权执行
- secrets 或核心策略面修改

## 项目定位

这个项目应该先作为独立开源 skill 迭代。

原因很简单：
它的行为模型需要先通过真实使用打磨，再决定哪些能力应该上收进 OpenClaw 核心。

## 当前状态

这是一个已经可以直接用于 OpenClaw 执行治理场景的 skill 草案。
它追求的是：
- 低风险少打扰
- 高风险不偷跑
- 对 skill 能做到的事情保持诚实
- 对必须进 runtime 的能力边界保持清晰
