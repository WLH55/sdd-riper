---
name: sdd-riper-one
description: 将 SDD-RIPER 方法论落地为严格可执行流程的技能。用于代码/架构任务中的“功能级与项目级 CodeMap 生成、全模态需求上下文打包、Spec 驱动研发、RIPER 阶段门禁推进”，适用于 Claude/Codex/其他 CLI Agent 的多轮协作开发。
---

# SDD-RIPER-ONE Skill

## 核心定位

- 先读：`references/sdd-riper-one-protocol.md`
- 总纲：`Pre-Research -> RIPER`，全程遵循 SDD 并持续维护 Spec
- 三条底线：`No Spec, No Code`、`Spec is Truth`、`Reverse Sync`
- `create_codemap` / `build_context_bundle` 是 Pre-Research 输入准备；`sdd_bootstrap` 是 RIPER 启动命令（进入 Research 第一步，同时完成 Pre-Research 收口）
- RIPER 主流程：`Research -> (Innovate, 可选) -> Plan -> Execute -> Review`

## 推荐流程（直接执行）

- 标准流（中大型任务）：
  - `create_codemap -> build_context_bundle -> sdd_bootstrap -> Research -> (Innovate, 可选) -> Plan -> Execute -> Review`
- 快速流（小任务/需求模糊）：
  - `sdd_bootstrap -> （按需补）create_codemap/build_context_bundle -> Research -> Plan -> Execute -> Review`
- 门禁：
  - 首版 spec 落盘前，不进入实现
  - 未收到精确字样 `Plan Approved`，禁止进入 `Execute`
  - `Review` 不通过，回到 `Research/Plan` 修正

## 多项目协作（自动发现 + 作用域隔离）

- 目标：多项目场景下保持"局部理解 + 局部执行 + 显式边界"，**用户零额外配置**。
- 详细协议：`references/sdd-riper-one-protocol.md` → `MULTI-PROJECT PROTOCOL` 段
- Spec 模板：`references/spec-template.md` → 多项目模板

### 自动发现（Auto-Discovery）

- 触发：`sdd_bootstrap: mode=multi_project` 或触发词 `MULTI / 多项目`
- Agent 自动扫描 `workdir` 下子目录，通过标志文件识别子项目：
  - JS/TS: `package.json` | Java/Kotlin: `pom.xml`, `build.gradle` | Go: `go.mod` | Python: `pyproject.toml`, `setup.py` | Rust: `Cargo.toml` | 通用: `.git`
  - Monorepo: 额外检查 `workspaces`、`settings.gradle`、`pnpm-workspace.yaml`
- 产出 `Project Registry`（`§0.1`），报告给用户确认后继续
- 用户也可显式提供 `projects=[...]` 跳过自动发现
- 智能降级：仅 1 个子项目 → 自动降级为单项目模式；0 个子项目 → workdir 本身作为单项目

### 自动 Codemap

- 发现项目后，自动为每个子项目检查/生成 `create_codemap(project)`
- 产物路径：`mydocs/codemap/YYYY-MM-DD_hh-mm_<project_id>项目总图.md`

### 作用域隔离规则（必须）

- 每轮先声明 `active_project` 与 `active_workdir`
- 默认 `change_scope=local`，只允许修改 `active_project` 下的文件
- 仅在显式 `change_scope=cross`（或触发词 `CROSS / 跨项目`）时允许跨项目改动
- 始终 `codemap-first`：切换到任何项目前，必须先加载该项目的 codemap/context
- 跨项目执行后，在 spec `§6.1 Touched Projects` 记录改动项目、文件、原因

### 跨项目依赖与契约

- 跨项目改动时，必须在 spec `§4.4 Contract Interfaces` 记录：Provider → Interface → Consumer → 是否 Breaking Change → 迁移方案
- 跨项目 Plan 的 checklist 按项目分组，Provider 优先于 Consumer 执行
- 修改前检查目标项目是否有活跃 Spec，有冲突则 STOP 等待用户决策

### 多项目 Review（扩展）

- 校验跨项目契约一致性（Provider 与 Consumer 接口匹配）
- 校验 Touched Projects 完整性
- 校验无孤立改动（所有改动文件都在已注册项目内）
- 按项目分别评估回归风险

### 触发词

- `MULTI / 多项目` → 进入多项目模式，运行自动发现
- `CROSS / 跨项目` → 当前轮 `change_scope=cross`
- `SWITCH <project_id> / 切换 <project_id>` → 切换 `active_project`，自动加载 codemap
- `REGISTRY / 项目列表` → 显示当前 Project Registry
- `SCOPE LOCAL / 回到本地` → 重置为 `change_scope=local`

### 最小启动示例

```text
sdd_bootstrap: mode=multi_project, task=<任务名>, goal=<目标>, requirement=<需求文档或描述>
```

- 无需手动列项目，Agent 自动发现并确认。
- 也可显式指定：`projects=[{id:web-console,path:./web-console},{id:api-service,path:./api-service}]`

## 30 秒上手（首页可直接复制）

```text
create_codemap: mode=project, scope=<项目名>, goal=输出项目总图与主流程
build_context_bundle: <需求资料目录路径>
sdd_bootstrap: task=<任务名>, goal=<目标>, requirement=<需求文档或描述>
```

- 小任务可直接从第 3 条开始（先 `sdd_bootstrap`，后补 codemap/context）。

## 原生命令动作（可直接输入）

### 1) `create_codemap`

- 用途：生成代码索引地图，支持 `feature` / `project`（默认 `feature`）
- **本质**：CodeMap 不只是一张"地图"，而是**上下文的索引与切片**。它把庞大的代码库压缩为可按需加载的局部上下文，让后续对话不必每次全仓扫描，而是通过索引精准定位到相关代码片段。对于老项目/大项目，建议按功能切片逐步沉淀 CodeMap，形成可复用的长期资产。
- 输入：`scope`（建议明确）；`mode` 可选；`goal` 可选
- 输出：
  - `feature`：`mydocs/codemap/YYYY-MM-DD_hh-mm_<feature>功能.md`
  - `project`：`mydocs/codemap/YYYY-MM-DD_hh-mm_<project>项目总图.md`
- 要点：
  - `feature` 关注入口、核心链路、依赖、风险
  - `project` 关注架构层、核心模块、跨模块流程、外部依赖；图示建议优先 Mermaid（受限可降级为结构化文字图）

### 2) `build_context_bundle`

- 用途：整理需求上下文，替用户读资料并提炼细节
- 输入：目录路径
- 解析策略：best effort，支持文本/文档/图片；不可解析文件进入 `Unparsed Sources`，不阻塞产出
- 输出：`mydocs/context/YYYY-MM-DD_hh-mm_<task>_context_bundle.md`
- 输出级别：
  - `Lite`：`Source Index`、`Requirement Snapshot`、`Open Questions`、`Next Actions`
  - `Standard`：`Requirement Facts`、`Business Rules`、`Acceptance Criteria`、`Constraints`、`Conflicts & Ambiguities` 等

### 3) `sdd_bootstrap`

- 用途：RIPER 启动命令（进入 Research 第一步，并产出第一版 spec）
- 输入：只要是“有意义且真实的需求”即可（口述/文档/聊天记录/context bundle 均可）
- 执行动作：
  - 汇总用户输入 + 代码事实 + 历史资产（codemap/context/spec）
  - 冲突处理：先落首版 spec 标记冲突，再给 `Option A/B` 和推荐决策
  - 形成首版研究结论与下一步动作
- 输出：`mydocs/specs/YYYY-MM-DD_hh-mm_<TaskName>.md`
- 首版最小内容：`Context Sources`、`Codemap Used`、`Research Findings`、`Open Questions`、`Next Actions`

## 阶段约束（最小集合）

- 每轮先同步 spec 再推进阶段
- `Innovate` 可选：复杂任务建议 2-3 方案；小任务可跳过但要写原因
- `Plan` 必须可执行：文件路径 + 签名 + 原子 checklist
- `Review` 必须回写结论：`Review Verdict`、`Plan-Execution Diff`

## Debug 模式（日志驱动排查与功能验证）

- 用途：基于日志 + Spec + 代码三角定位 Bug，或用全链路日志验证功能是否正常
- 触发词：`DEBUG / 排查 / 日志分析 / 验证功能`
- 输入：
  - `log_path`：日志文件或日志目录路径（必须）
  - `issue`：发现的问题描述 / 报错信息（可选，排查模式时建议提供）
  - `spec`：关联的 Spec 文件路径（可选，验证模式时建议提供）
- 两种子模式：
  - **排查模式**（默认）：用户提供日志 + 问题描述，Agent 结合 Spec 和代码定位可能的 Bug 根因
  - **验证模式**：用户提供全链路日志 + Spec，Agent 逐条比对 Spec 中的预期行为与日志中的实际行为，确认功能是否正常
- 工作流：
  1. 读取日志文件/目录，提取关键错误、异常、调用链信息
  2. 加载关联的 Spec 和 CodeMap（如有），建立"预期行为 vs 实际行为"的对照
  3. 定位代码中的可疑逻辑（结合 CodeMap 索引精准跳转）
  4. 输出结论：Bug 根因分析 / 功能验证报告
  5. 如需修复，自动进入 RIPER 流程（Research → Plan → Execute → Review）
- 约束：
  - Debug 模式本身不直接改代码，只做分析和定位
  - 需要改代码时，必须走 RIPER 流程（或 FAST 通道处理小修复）
  - 分析结论回写到 Spec 的 `§ Debug Log` 段（如有活跃 Spec）

## 触发词

- `MAP / Code Map / 链路梳理 / 只看代码` -> `create_codemap(feature)`
- `PROJECT MAP / 全局地图 / 项目总图 / MAP ALL` -> `create_codemap(project)`
- `MULTI / 多项目` -> 多项目轻量模式（父目录 workdir + 局部执行）
- `CROSS / 跨项目` -> 允许跨项目改动（强制记录 `Touched Projects`）
- `FAST / 快速 / >>` -> 小改快速通道（改后同步 spec）
- `DEBUG / 排查 / 日志分析 / 验证功能` -> Debug 模式（日志驱动排查与功能验证）
- `EXIT SDD / 退出协议` -> 退出状态机

## 命名规则（统一时间前缀）

- 时间前缀：`YYYY-MM-DD_hh-mm_`
- `create_codemap(feature)`：`mydocs/codemap/YYYY-MM-DD_hh-mm_<feature>功能.md`
- `create_codemap(project)`：`mydocs/codemap/YYYY-MM-DD_hh-mm_<project>项目总图.md`
- `build_context_bundle`：`mydocs/context/YYYY-MM-DD_hh-mm_<task>_context_bundle.md`
- `sdd_bootstrap`：`mydocs/specs/YYYY-MM-DD_hh-mm_<TaskName>.md`

## 参考

- `references/sdd-riper-one-protocol.md`
- `references/spec-template.md`
- `references/workflow-quickref.md`
- `references/usage-examples.md`
