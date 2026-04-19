# sdd-riper-one-light

面向强模型的轻量 SDD / RIPER coding skill——**快进模式**。

核心定位：
- `checkpoint-driven`，不是 `phase-heavy`
- 默认假设强模型能自行分解任务
- 保留硬约束，去掉仪式感

## 命令列表

| 命令 | 用途 | 定义文件 |
|------|------|---------|
| `/sdd:ff <任务>` | 一键快进：任务 → spec → 实现 → 回写 | `commands/ff.md` |
| `/sdd:fix <修改>` | 零 spec 快修：纯机械改动 | `commands/fix.md` |
| `/sdd:resume [spec]` | 跨会话恢复 | `commands/resume.md` |
| `/sdd:archive [spec]` | 归档 | `commands/archive.md` |

## 状态管理

Spec 文件顶部包含状态块，作为 AI 的"程序计数器"：

```markdown
> **Phase**: implementing | **Updated**: YYYY-MM-DD HH:MM
> **Goal**: <核心目标>
> **Done Contract**: <完成定义>
> **Last**: <上次完成什么> | **Next**: <下一步>
```

- 每轮结束后必须更新状态块
- 新会话通过 `/sdd:resume` 从状态块恢复
- 与标准版共享 `/sdd:resume` 命令，状态块格式兼容

## 硬约束

- `Spec is Truth`
- `No Spec, No Code`（`/sdd:fix` 除外）
- `No Approval, No Execute`
- `Done by Evidence`
- `Reverse Sync`

## 快速上手

```text
/sdd:ff 修改登录按钮颜色为蓝色
```

AI 会自动：复述理解 → 写 micro-spec → 给 checkpoint → 等你批准 → 实现 → 回写验证。

如果复杂度超出预期，AI 会暂停并建议升级到标准版的 `/sdd:bootstrap`。

## 安装

```bash
# 复制命令文件到目标项目
cp commands/*.md <目标项目>/.claude/commands/sdd/

# 复制 SKILL.md 到目标项目的 CLAUDE.md
cp SKILL.md <目标项目>/CLAUDE.md
```

## 文件命名规则

- spec：`docs/specs/YYYY-MM-DD_HH-MM_<TaskName>.md`
- codemap：`docs/codemap/YYYY-MM-DD_HH-MM_<name>.md`
- archive：`docs/archive/YYYY-MM-DD_HH-MM_<topic>_{human|llm}.md`
