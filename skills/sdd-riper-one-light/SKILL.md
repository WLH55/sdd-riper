---
name: sdd-riper-one-light
description: 面向强模型的轻量 spec-driven / checkpoint-driven coding skill。用于高输入、高频、多轮的代码与 agentic coding 任务，默认短输出、中文沟通、模型自行分解任务；常驻只保留最小 spec、先复述理解、执行前 checkpoint、批准后执行、执行后回写五类关键约束。
---

# SDD-RIPER-ONE Light

## 核心定位

- 这是 `checkpoint-driven` 版本，不是 `phase-heavy` 版本。
- 默认假设强模型已经能自行分解任务、补足局部计划、按需追溯上下文。
- 目标不是减少控制力，而是减少低价值常驻 token。

## 硬约束

- `Spec is Truth`：spec 是持久化上下文、压缩记忆与协作真相源。
- `No Spec, No Code`：未形成或更新最小 spec 前，不进入代码实现（`/sdd:fix` 除外）。
- `No Approval, No Execute`：未得到明确执行许可，不进入实现或高影响变更。
- `Done by Evidence`：完成应由验证结果与外部反馈证明，而不是由模型自行宣布。
- `Reverse Sync`：执行后必须把结果、偏差、验证结论回写 spec。

## 状态管理

Spec 文件顶部包含状态块，作为 AI 的"程序计数器"。每轮回复结束后必须更新状态块。

```markdown
> **Phase**: <implementing|done> | **Updated**: YYYY-MM-DD HH:MM
> **Goal**: <核心目标>
> **Done Contract**: <完成定义>
> **Last**: <上次完成什么> | **Next**: <下一步>
```

- 所有字段为必填
- 新会话使用 `/sdd:resume` 读取状态块即可恢复执行

## 命令索引

| 命令 | 用途 | 详细定义 |
|------|------|---------|
| `/sdd:ff` | 一键快进：任务 → spec → 实现 → 回写 | `commands/ff.md` |
| `/sdd:fix` | 零 spec 快修：纯机械改动 | `commands/fix.md` |
| `/sdd:resume` | 跨会话恢复 | `commands/resume.md` |
| `/sdd:archive` | 归档 | `commands/archive.md` |

## 何时暂停

遇到以下情况，先暂停并说明原因：

- 需求存在会改变实现方向的关键歧义
- 需要破坏性、高风险、不可逆操作
- 涉及架构级改动、公共接口变更
- 复杂度超出 `/sdd:ff` 范围，建议升级到标准版 `/sdd:bootstrap`

## 输出风格

- 默认中文，默认短输出
- 优先给"当前理解 + 核心目标 + 下一步 + 必要风险"
- 不强制打印阶段状态机

## 参考

- `references/spec-lite-template.md` - 最小 spec 模板（含状态块）
- `references/conventions.md` - 落盘目录、命名规则
- `references/modules.md` - Deep Planning / Debug / Review 模块（按需加载）
