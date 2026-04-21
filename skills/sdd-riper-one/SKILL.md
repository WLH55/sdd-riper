---
name: sdd-riper-one
description: 将 SDD-RIPER 方法论落地为严格可执行流程的技能。用于代码/架构任务中的"功能级与项目级 CodeMap 生成、全模态需求上下文打包、Spec 驱动研发、RIPER 阶段门禁推进"，适用于 Claude/Codex/其他 CLI Agent 的多轮协作开发。
---

# SDD-RIPER-ONE Skill

> Version: 2.0.0 | Updated: 2026-04-21

## 全局行为与安全底线

- **高危操作阻断**：永远不要静默或提议执行 `git clean`（包含任何参数，特别是 `-fdx`），防止用户未提交的工作区数据不可逆丢失。
- **研发纪律**：
  - Spec is Truth：spec 是持久化上下文、压缩记忆与协作真相源。
  - No Spec, No Code：未形成或更新最小 spec 前，不进入代码实现。
  - No Approval, No Execute：未得到明确执行许可，不进入实现或高影响变更。批准确认不要求精确字符串匹配，用户表达明确肯定 + 继续意图即可。
  - Reverse Sync：执行后必须把结果、偏差、验证结论回写 spec。
  - Restate First：用户输入任务后，先用模型自己的话复述理解，再进入 spec 或计划。

## 核心定位

- 总纲：`Pre-Research -> RIPER`，全程遵循 SDD 并持续维护 Spec
- RIPER 主流程：`Research -> (Innovate, 可选) -> Plan -> Execute -> Review`
- 不要在每轮对话里重载整份 Skill / Spec；默认只回读当前阶段需要的小节
- **Spec 受众分层与上下文保护**：Spec 的第一受众是人类（持久化的任务上下文与组织记忆），第二受众才是模型。协议对模型的核心价值：**注意力聚焦**、**信息索引**、**防止上下文腐烂**、**辅助 Review**。协议绝不应导致上下文被塞满挤爆。

## 状态管理

Spec 文件顶部包含状态块，作为 AI 的"程序计数器"。每轮回复结束后必须更新状态块。

```markdown
> **Phase**: <research|innovate|plan|execute|review|done> | **Updated**: YYYY-MM-DD HH:MM
> **Goal**: <核心目标>
> **Approval**: Pending / Approved / N/A        ← 可选
> **Scope**: In <...> | Out <...>               ← 可选
> **Done Contract**: <完成定义>                  ← 可选
> **Last**: <上次完成什么> | **Next**: <下一步动作>
```

- `Phase`、`Updated`、`Goal`、`Last`、`Next` 为必填字段
- 阶段切换时更新 `Phase`；用户批准时更新 `Approval`
- 新会话使用 `/sdd:resume` 读取状态块即可恢复执行

## 上下文读取规则

- **热上下文**：从 spec 状态块读取（Phase、Goal、Last、Next）——每次读 spec 即自动获得
- **温上下文**：从 spec 正文对应章节读取（§2 Research → Plan 切换时；§4 Plan → Execute 切换时；§5 Execute → Review 切换时）
- **冷上下文**：从外部文件按需加载（codemap、context bundle、archive），spec 内只存路径引用

不再需要 AI 在上下文中"记住"状态——所有状态从 spec 文件读取。

## 门禁规则

- 没有 spec，不进入代码实现
- `Phase` 与 `Approval` 必须是显式状态，不允许根据语气推断
- 批准确认：用户表达明确肯定 + 继续意图即可，不要求精确字符串匹配
- `Review` 时必须基于 Plan 与 Validation，而不是只看聊天摘要
- Review 必须包含外部证据（测试输出、lint、git diff、人工确认）
- 发现冲突、字段缺失或记忆不确定时，立即回读完整 spec

## 命令索引

| 命令 | 用途 | 详细定义 |
|------|------|---------|
| `/sdd:codemap` | 生成代码地图 | `commands/codemap.md` |
| `/sdd:contextbundle` | 构建需求上下文包 | `commands/contextbundle.md` |
| `/sdd:bootstrap` | 启动任务（Research） | `commands/bootstrap.md` |
| `/sdd:plan` | 规划阶段 | `commands/plan.md` |
| `/sdd:reviewspec` | Plan 后建议性预审 | `commands/reviewspec.md` |
| `/sdd:execute` | 执行阶段 | `commands/execute.md` |
| `/sdd:review` | 三轴审查（Execute 后） | `commands/review.md` |
| `/sdd:archive` | 归档 | `commands/archive.md` |
| `/sdd:resume` | 跨会话恢复 | `commands/resume.md` |
| `/sdd:ff` | 一键快进（小任务） | `commands/ff.md` |
| `/sdd:fix` | 零 spec 快修（机械改动） | `commands/fix.md` |
| `/sdd:status` | 查看所有 spec 状态 | `commands/status.md` |

## 可选模块

| 模块 | 用途 | 详细定义 |
|------|------|---------|
| 多项目协作 | 跨项目发现 + 作用域隔离 | `references/multi-project.md` |
| Debug 模式 | 日志驱动排查 | `references/protocol-debug.md` |

## 参考

- `references/sdd-riper-one-protocol.md` - 协议总索引（阶段协议拆分为独立文件）
- `references/protocol-core.md` - 核心法则、状态管理、门禁规则
- `references/protocol-research.md` - Research + Codemap + Context Bundle + Staleness Detection
- `references/protocol-plan-execute.md` - Plan + Review Spec + Execute + FAST
- `references/protocol-review-archive.md` - Review + Archive
- `references/protocol-debug.md` - Debug 协议
- `references/spec-template.md` - Spec 模板（含状态块、micro-spec、多项目）
- `references/workflow-quickref.md` - 工作流快速参考
- `references/usage-examples.md` - 使用示例
- `references/archive-template.md` - 归档模板
- `references/multi-project.md` - 多项目协作详细规则

## 快速入门

### 选择使用路径

| 路径 | 适用场景 | 命令 |
|------|---------|------|
| **标准流** | 中大型任务、架构改动、需求模糊 | `/sdd:codemap` → `/sdd:bootstrap` → `/sdd:plan` → `/sdd:execute` → `/sdd:review` |
| **快速流** | 小任务、需求明确 | `/sdd:ff <任务描述>` |
| **极速修** | 纯机械改动（typo、配置值） | `/sdd:fix <修改描述>` |

### 五步基本流程

1. **描述任务**：用自然语言告诉 AI 你要做什么
2. **AI 复述**：AI 会用自己的话复述理解，确认无误后继续
3. **产出 Spec**：AI 将任务转化为结构化的 spec 文件（自动落盘到 `docs/specs/`）
4. **用户批准**：审查 spec 和 plan，表达确认即可授权执行
5. **执行 + 回写**：AI 按 plan 执行代码变更，完成后自动回写 spec

> 中断后恢复：新会话输入 `/sdd:resume` 即可从上次断点继续。查看所有 spec 状态：`/sdd:status`。
