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

## 双版本架构

| 版本 | 定位 | 适用场景 | 入口 |
|------|------|---------|------|
| `sdd-riper-one-light` | **快进模式**：checkpoint-driven | 日常编码、小任务、快速开发 | `/sdd:ff` |
| `sdd-riper-one` | **标准模式**：完整 RIPER 门禁 | 架构设计、复杂重构、跨项目协作 | `/sdd:bootstrap` |

- **默认推荐 Light 版**——适合大多数日常开发。
- **复杂任务升级到标准版**——需要完整 `Research -> Plan -> Execute -> Review` 门禁时使用。

---

## 命令速查

### Light 版（日常快进）

| 命令 | 用途 |
|------|------|
| `/sdd:ff <任务>` | 一键快进：任务 → spec → 实现 → 回写 |
| `/sdd:fix <修改>` | 零 spec 快修：纯机械改动（typo、配置值等） |
| `/sdd:resume [spec]` | 跨会话恢复：从状态块读取断点继续 |
| `/sdd:archive [spec]` | 归档：将完成的 spec 沉淀为知识 |

### 标准版（门禁流程）

| 命令 | 用途 |
|------|------|
| `/sdd:codemap [scope]` | 生成代码地图（feature/project 级） |
| `/sdd:bootstrap task=<...> goal=<...>` | 启动任务，进入 Research 阶段 |
| `/sdd:plan` | 规划阶段：产出 File Changes + Checklist |
| `/sdd:execute` | 执行阶段：按 Plan 逐项实施 |
| `/sdd:review` | 三轴审查：Spec 质量 + 代码一致性 + 代码质量 |
| `/sdd:resume [spec]` | 跨会话恢复 |
| `/sdd:archive [spec]` | 归档 |

---

## 状态块：AI 的"程序计数器"

每个 spec 文件顶部包含状态块，作为 AI 的执行状态存储。新会话通过 `/sdd:resume` 读取即可恢复，不需要 AI "记住"任何上下文。

```markdown
> **Phase**: Plan | **Updated**: 2026-04-19 14:30
> **Goal**: 实现完整登录流程
> **Approval**: Pending
> **Scope**: In 登录页、JWT | Out 注册
> **Last**: 完成 Research | **Next**: 完成 Plan §4
```

状态块统一格式，Light 版是标准版的子集（省略可选字段）。

---

## 安装

Skill 是一个完整包，包含 SKILL.md（协议层）+ commands/（命令层）+ references/（参考文档）+ scripts/（自动化脚本）。安装时需要完整复制，不能只装一部分。

### 方式一：Claude Code（推荐）

```bash
TARGET=<目标项目路径>

# 1. 复制完整 skill 包到目标项目
cp -r skills/sdd-riper-one/         $TARGET/.claude/skills/sdd-riper-one/
cp -r skills/sdd-riper-one-light/   $TARGET/.claude/skills/sdd-riper-one-light/

# 2. 注册 slash commands（两个版本都装，命令不冲突）
cp skills/sdd-riper-one/commands/*.md       $TARGET/.claude/commands/sdd/
cp skills/sdd-riper-one-light/commands/*.md $TARGET/.claude/commands/sdd/

# 3. 将 SKILL.md 内容追加到目标项目的 CLAUDE.md
cat skills/sdd-riper-one-light/SKILL.md >> $TARGET/CLAUDE.md
```

安装后的目标项目结构：

```
目标项目/
  CLAUDE.md                              ← 包含 SKILL.md 协议
  .claude/
    commands/sdd/                        ← slash commands
      ff.md, fix.md, bootstrap.md, ...
    skills/
      sdd-riper-one/                     ← 标准版完整包
        SKILL.md
        commands/
        references/                      ← 按需加载的详细文档
        scripts/                         ← 自动化脚本
      sdd-riper-one-light/               ← Light 版完整包
        SKILL.md
        commands/
        references/
```

**为什么需要完整包**：
- `commands/` 提供具体的 slash command 动作
- `references/` 提供详细的协议文档、模板、示例——SKILL.md 里的命令通过相对路径引用它们
- `scripts/` 提供 archive 等自动化工具
- SKILL.md 里写了 `references/spec-template.md`、`references/multi-project.md` 等路径，如果 references 没装，AI 读不到

### 方式二：Cursor

```bash
# 1. 复制完整 skill 包到项目根目录
cp -r skills/sdd-riper-one/ <目标项目>/sdd-riper-one/

# 2. 将 SKILL.md 作为 .cursorrules
cp skills/sdd-riper-one-light/SKILL.md <目标项目>/.cursorrules
```

### 方式三：Claude Desktop / Claude.ai

1. 复制 `SKILL.md` 内容到 Custom Instructions
2. 将 `references/` 和 `scripts/` 放到项目可访问的目录
3. AI 需要时按路径读取

### 方式四：其他 AI Agent

参考 `agents/openai.yaml` 配置，将完整 skill 包放到 Agent 可访问的路径。

### 验证安装

```text
输入 /sdd:ff 测试一下这个功能
如果 AI 识别并按协议执行（先复述理解、再写 spec、等批准后再改代码），说明安装成功
```

---

## 文件结构

```
skills/
  sdd-riper-one/                # 标准版
    SKILL.md                    # 核心协议（~70 行）
    commands/                   # 7 个 slash command
      codemap.md
      bootstrap.md
      plan.md
      execute.md
      review.md
      archive.md
      resume.md
    references/                 # 详细参考文档（按需加载）
    agents/openai.yaml          # OpenAI Agent 配置

  sdd-riper-one-light/          # Light 版
    SKILL.md                    # 核心协议（~50 行）
    commands/                   # 4 个 slash command
      ff.md
      fix.md
      resume.md
      archive.md
    references/                 # 按需加载的模块
    examples/                   # 示例 spec 和 codemap

.claude/commands/sdd/           # 安装即用副本（11 个命令）
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

建议将以下规则写入目标项目的 `CLAUDE.md`（Claude Code）、`.cursorrules`（Cursor）或 Custom Instructions（Claude Desktop）。这些规则与 SKILL.md 配合使用，作为项目级行为约束。

```markdown
# 工作指南

- 使用中文交流。
- 永远不要执行 `git clean`（任何参数，尤其是 `-fdx`）。
- `No Spec, No Code`
- `No Approval, No Execute`
- `Spec is Truth`

## Skill 使用约定

- 默认使用 `sdd-riper-one`
- 仅当任务明确需要轻量模式时切换到 `sdd-riper-one-light`
- 除非我明确要求，否则不要主动安装依赖、升级依赖、删除依赖或修改锁文件
- 对可能造成不可逆后果、批量删除、覆盖、重置、迁移或外部写操作的命令，先等待我确认

## Git 边界

- 默认严格尊重 `.gitignore` 与所有已忽略路径
- 不要主动使用 `git add -f` / `git add --force`
- 被 `.gitignore` 忽略的内容默认不提交；只有我明确点名路径时，才允许精确提交

## Spec 同步

- 当改动影响需求、接口、行为、约束、流程或实现决策时，执行后同步更新 spec
- 纯机械性改动可不更新 spec

## 执行规则

- 代码修改前先提交方案并等待我确认；文档修改可直接执行
- 等待确认期间，先将方案收敛到最小改动集，不要无限扩展搜索范围
- 修改文件时，不要一次性重写整个文件；优先小步、分段、少量多次修改
- 除非我明确要求，否则不要做大范围重构、整文件替换或超大 patch
- 多文件改动时，先完成最核心链路的最小可用修改，再逐步补齐
- 不要顺手修改与当前任务无关的内容
- 如果连续 2 轮以上仅阅读/搜索而没有实际修改，先停止继续探索，并明确说明准备修改哪些文件、先改哪一处、为什么
```

将上述内容与选定版本的 `SKILL.md` 内容合并后写入目标项目的 `CLAUDE.md` 即可。
