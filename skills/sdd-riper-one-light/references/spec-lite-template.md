# Spec Lite Template

spec 是持久化上下文与压缩记忆层；没有最小 spec，不进入代码实现。

```markdown
# Spec: <Task Name>

> **Phase**: implementing | **Updated**: YYYY-MM-DD HH:MM
> **Goal**: <核心目标>
> **Done Contract**: <什么算完成、由什么证明、哪些情况仍算未完成>
> **Last**: <上次完成什么> | **Next**: <下一步>

## Goal
- 要解决什么问题：
- 验收结果：

## Done Contract
- 什么算完成：
- 由什么证明：
- 哪些情况仍算未完成：

## Scope
- In:
- Out:

## Facts / Constraints
- 已确认事实：
- 技术/业务约束：
- 已知风险：

## Open Questions
- [ ] 待确认项 1

## Restated Understanding
- 我理解当前任务是：
- 当前核心目标是：
- 当前边界是：
- 暂不处理：

## Checkpoint Summary
- 当前任务理解：
- 当前核心目标：
- 当前进度：
- 下一步 1:
- 下一步 2:
- 涉及文件 / 模块：
- 风险：
- 验证方式：
- Execution Approval: `Pending` / `Approved`

## Change Log
- YYYY-MM-DD: 决策/改动摘要

## Validation
- Self-check:
- Static checks:
- Runtime / Test:
- Human confirmation:
- 结果汇总：
- 核心目标是否已由证据证明完成：
- 若未完成，当前剩余差距：

## Resume / Handoff
- 当前状态：
- 当前卡点：
- 下一步唯一动作：
```

建议：

- `fast` 任务至少保留 `Goal / Done Contract / Checkpoint Summary / Change Log / Validation`。
- `standard` 任务补齐全部区块。
- spec 一旦形成或更新，应尽快落盘。
- 用户输入任务后，先写 `Restated Understanding`，再进入后续动作。
- `Done Contract` 保持 1-3 行，不要写成长计划。
- 执行前先把 `Execution Approval` 置为 `Pending`，获批后再改为 `Approved`。
- 暂停或交接前，更新顶部状态块和 `Resume / Handoff`。
- 状态块是 AI 的"程序计数器"——新会话 `/sdd:resume` 从状态块恢复，不需要 AI "记住"任何上下文。
