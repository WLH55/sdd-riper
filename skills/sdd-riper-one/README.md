# sdd-riper-one

SDD-RIPER ONE Skill（标准版 - Spec 驱动研发，完整 RIPER 门禁）。

该 Skill 用于约束大模型严格按以下流程执行研发任务：

`Research -> (Innovate, 可选) -> Plan -> (Plan Approved) -> Execute -> Review`

## 工作方式优先

推荐把 `sdd-riper-one` 当成一种**工作方式**来使用，而不是一组命令：

- `No Spec, No Code`
- `No Approval, No Execute`
- `Spec is Truth`
- 先形成最小 spec，再进入执行
- 复杂任务走完整阶段门禁，小任务也不能裸改

## 命令列表

所有命令已拆分为独立的 slash command，定义在 `commands/` 目录：

| 命令 | 用途 | 定义文件 |
|------|------|---------|
| `/sdd:codemap` | 生成代码地图 | `commands/codemap.md` |
| `/sdd:bootstrap` | 启动任务（Research） | `commands/bootstrap.md` |
| `/sdd:plan` | 规划阶段 | `commands/plan.md` |
| `/sdd:execute` | 执行阶段 | `commands/execute.md` |
| `/sdd:review` | 三轴审查 | `commands/review.md` |
| `/sdd:archive` | 归档 | `commands/archive.md` |
| `/sdd:resume` | 跨会话恢复 | `commands/resume.md` |

## 状态管理

Spec 文件顶部包含状态块，作为 AI 的"程序计数器"：

```markdown
> **Phase**: Plan | **Updated**: YYYY-MM-DD HH:MM
> **Goal**: <核心目标>
> **Approval**: Pending
> **Scope**: In <...> | Out <...>
> **Last**: <上次完成什么> | **Next**: <下一步>
```

- 每轮结束后必须更新状态块
- 新会话通过 `/sdd:resume` 从状态块恢复
- 不需要 AI 在上下文中"记住"状态——所有状态从 spec 文件读取

## 推荐流程

标准流（中大型任务）：
```
/sdd:codemap -> /sdd:bootstrap -> /sdd:plan -> /sdd:execute -> /sdd:review -> /sdd:archive
```

快速流（小任务）：
```
/sdd:bootstrap -> /sdd:plan -> /sdd:execute -> /sdd:review
```

关键门禁：
- 首版 spec 落盘前，不进入实现
- 未收到 `Plan Approved`，禁止进入 Execute
- Review 不通过，回到 Research/Plan 修正

## Multi-Project 协作

详见 `references/multi-project.md`。触发方式：

```text
/sdd:bootstrap mode=multi_project task=<任务> goal=<目标>
```

## 安装

```bash
# 复制命令文件到目标项目
cp commands/*.md <目标项目>/.claude/commands/sdd/

# 复制 SKILL.md 到目标项目的 CLAUDE.md
cp SKILL.md <目标项目>/CLAUDE.md
```

## 文件命名规则

统一使用 `YYYY-MM-DD_HH-MM_` 前缀：

- codemap：`docs/codemap/YYYY-MM-DD_HH-MM_<name>.md`
- context bundle：`docs/context/YYYY-MM-DD_HH-MM_<task>_context_bundle.md`
- spec：`docs/specs/YYYY-MM-DD_HH-MM_<TaskName>.md`
- archive：`docs/archive/YYYY-MM-DD_HH-MM_<topic>_{human|llm}.md`

## archive 自动化

```bash
python3 scripts/archive_builder.py \
  --targets docs/specs docs/codemap \
  --kind mixed \
  --audience both \
  --mode thematic \
  --topic 主题名
```
