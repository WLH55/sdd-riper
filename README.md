# SDD-RIPER: AI-Native Development Protocol

> **The Constitution for Human-AI Collaboration.**
> 像指挥军团一样指挥 AI，而不是像保姆一样修补代码。

---

## 30 秒读懂 SDD-RIPER

**SDD (Spec-Driven Development)** 是一套为大模型编程量身定制的研发协议。核心公式：

> **Spec (Truth) + AI (Execute) = Software 2.0**

解决大模型编程的四个工程痛点：

| 痛点 | SDD 解法 |
|------|---------|
| **上下文腐烂**：AI 聊着聊着就忘了前文约束 | **Spec 状态块**：状态持久化在文件里，不靠 AI 记忆 |
| **审查瘫痪**：AI 秒生成 500 行代码，人 Review 不过来 | **RIPER 门禁**：先审 Plan 再写 Code，审逻辑代替审代码 |
| **维护断层**：全是 AI 生成的陌生代码，不敢动 | **文档即源码**：代码是消耗品，Spec 才是资产 |
| **代码不信任**：不知道 AI 为什么这么写，不敢上线 | **三轴审查**：Spec + 执行日志 + 代码三方交叉验证 |

---

## 三条使用路径

| 路径 | 适用场景 | 命令 |
|------|---------|------|
| **标准流** | 中大型任务、架构改动、需求模糊 | `/sdd:codemap` → `/sdd:bootstrap` → `/sdd:plan` → `/sdd:execute` → `/sdd:review` |
| **快速流** | 小任务、需求明确 | `/sdd:ff <任务描述>` |
| **极速修** | 纯机械改动（typo、配置值） | `/sdd:fix <修改描述>` |

---

## 命令速查

| 命令 | 用途 |
|------|------|
| `/sdd:codemap [scope]` | 生成代码地图（feature/project 级） |
| `/sdd:contextbundle <目录>` | 构建需求上下文包 |
| `/sdd:bootstrap task=<...>` | 启动任务，进入 Research 阶段 |
| `/sdd:plan` | 规划阶段：产出 File Changes + Checklist |
| `/sdd:reviewspec` | Plan 后建议性预审（GO/NO-GO，不阻塞） |
| `/sdd:execute` | 执行阶段：按 Plan 逐项实施 |
| `/sdd:review` | 三轴审查：Spec 质量 + 代码一致性 + 代码质量 |
| `/sdd:ff <任务>` | 一键快进：任务 → micro-spec → 实现 → 回写 |
| `/sdd:fix <修改>` | 零 spec 快修：纯机械改动 |
| `/sdd:resume [spec]` | 跨会话恢复：从状态块读取断点继续 |
| `/sdd:archive [spec]` | 归档：将完成的 spec 沉淀为知识 |
| `/sdd:status` | 查看所有活跃 spec 的状态概览 |

---

## 状态块：AI 的"程序计数器"

每个 spec 文件顶部包含状态块，作为 AI 的执行状态存储。新会话通过 `/sdd:resume` 读取即可恢复，不需要 AI "记住"任何上下文。

```markdown
> **Phase**: plan | **Updated**: 2026-04-21 10:30
> **Goal**: 实现完整登录流程
> **Approval**: Pending
> **Scope**: In 登录页、JWT | Out 注册
> **Last**: 完成 Research | **Next**: 完成 Plan §4
```

---

## 安装

### 方式一：Claude Code（推荐）

```bash
TARGET=<目标项目路径>

# 1. 复制完整 skill 包到目标项目
cp -r skills/sdd-riper-one/ $TARGET/.claude/skills/sdd-riper-one/

# 2. 注册 slash commands
cp skills/sdd-riper-one/commands/*.md $TARGET/.claude/commands/sdd/
```

然后在目标项目的 `CLAUDE.md` 中编排 skill 使用规则（**不要复制 SKILL.md 内容**，只写引用路径）：

```markdown
## Skill 使用约定

- 默认使用 `sdd-riper-one`，读取 `.claude/skills/sdd-riper-one/SKILL.md`
- 除非我明确要求，否则不要主动安装依赖、升级依赖、删除依赖或修改锁文件
```

AI 会根据 CLAUDE.md 的编排自动读取对应 SKILL.md 的内容，不需要把协议复制进 CLAUDE.md。

安装后的目标项目结构：

```
目标项目/
  CLAUDE.md                              ← 编排规则（引用 skill 路径，不包含协议内容）
  .claude/
    commands/sdd/                        ← slash commands
      ff.md, fix.md, bootstrap.md, ...
    skills/
      sdd-riper-one/                     ← 完整 skill 包
        SKILL.md                         ← AI 按需读取
        commands/
        references/
```

**为什么需要完整包**：
- `commands/` 提供具体的 slash command 动作
- `references/` 提供详细的协议文档、模板、示例——SKILL.md 里的命令通过相对路径引用它们
- SKILL.md 里写了 `references/spec-template.md`、`references/multi-project.md` 等路径，如果 references 没装，AI 读不到

**为什么不要复制 SKILL.md 到 CLAUDE.md**：
- CLAUDE.md 负责编排"什么时候用哪个 skill"，不是协议本身
- AI 会在需要时自动读取 SKILL.md，不需要常驻在 CLAUDE.md 里
- 保持 CLAUDE.md 精简，多个 skill 可以灵活切换

### 方式二：Cursor

```bash
cp -r skills/sdd-riper-one/ <目标项目>/sdd-riper-one/
echo "默认读取 sdd-riper-one/SKILL.md" >> <目标项目>/.cursorrules
```

### 方式三：Claude Desktop / Claude.ai

在 Custom Instructions 中编排 skill 使用规则，指向项目中的 SKILL.md 文件路径。

### 验证安装

```text
输入 /sdd:ff 测试一下这个功能
如果 AI 识别并按协议执行（先复述理解、再写 spec、等批准后再改代码），说明安装成功
```

---

## 文件结构

```
skills/
  sdd-riper-one/
    SKILL.md                          # 核心协议入口（~120 行）
    commands/                         # 12 个 slash command
      codemap.md
      contextbundle.md
      bootstrap.md
      plan.md
      reviewspec.md
      execute.md
      review.md
      archive.md
      resume.md
      ff.md
      fix.md
      status.md
    references/                       # 详细参考文档（按需加载）
      sdd-riper-one-protocol.md       # 协议总索引
      protocol-core.md                # 核心法则、状态管理、门禁
      protocol-research.md            # Research + Codemap + Context Bundle
      protocol-plan-execute.md        # Plan + Execute + FAST
      protocol-review-archive.md      # Review + Archive
      protocol-debug.md               # Debug 协议
      multi-project.md                # 多项目协作
      spec-template.md                # Spec 模板（单项目/多项目/micro）
      workflow-quickref.md            # 工作流快速参考
      archive-template.md             # 归档模板
      usage-examples.md               # 使用示例
```

---

## 核心原则

- **No Spec, No Code**：没有 spec 不写代码（`/sdd:fix` 除外）
- **Spec is Truth**：Spec 是唯一真相源，聊天决议必须回源到 Spec
- **No Approval, No Execute**：没有明确批准不执行
- **Done by Evidence**：完成由证据证明，不是模型自行宣布
- **Reverse Sync**：执行后必须回写 Spec

## 安全底线

- 永远不要执行 `git clean`（任何参数），防止未提交数据丢失

---

## 推荐的 CLAUDE.md 配置

建议将以下规则写入目标项目的 `CLAUDE.md`：

```markdown
# 工作指南

- 使用中文交流。
- 永远不要执行 `git clean`（任何参数，尤其是 `-fdx`）。
- No Spec, No Code
- No Approval, No Execute
- Spec is Truth

## Skill 使用约定

- 默认使用 `sdd-riper-one`
- 除非我明确要求，否则不要主动安装依赖、升级依赖、删除依赖或修改锁文件
- 对可能造成不可逆后果的命令，先等待我确认

## Git 边界

- 默认严格尊重 `.gitignore` 与所有已忽略路径
- 不要主动使用 `git add -f` / `git add --force`

## Spec 同步

- 当改动影响需求、接口、行为、约束时，执行后同步更新 spec
- 纯机械性改动可不更新 spec

## 执行规则

- 代码修改前先提交方案并等待确认
- 优先小步、分段、少量多次修改
- 不要顺手修改与当前任务无关的内容
```
