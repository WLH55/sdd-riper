# SDD-RIPER Workflow Quick Reference

总纲：`Pre-Research -> RIPER`，全程按 SDD 维护 Spec。

## 启动顺序（按复杂度）

1. `Pre-Research Utilities（非 RIPER 阶段）`
2. `标准流（中大型任务）`：`codemap -> contextbundle -> bootstrap（启动 Research）`
3. `快速流（小任务/模糊需求）`：`bootstrap（启动 Research） -> 按需补 codemap/contextbundle`
4. `RIPER 正式开始于 Research`：`Research -> (Innovate, 可选) -> Plan -> Execute -> Review`

## 命令索引（12 个）

| 命令 | 定位 |
|------|------|
| `/sdd:codemap` | Pre-Research 代码索引（feature/project 级） |
| `/sdd:contextbundle` | Pre-Research 需求整理（Lite/Standard 粒度） |
| `/sdd:bootstrap` | RIPER 启动（进入 Research，产首版 spec） |
| `/sdd:plan` | Plan 阶段（File Changes + Signatures + Checklist） |
| `/sdd:reviewspec` | Plan 后建议性预审（GO/NO-GO，不阻塞） |
| `/sdd:execute` | Execute 阶段（按 Plan 逐项实施） |
| `/sdd:review` | 三轴审查（Spec 质量 + 代码一致性 + 代码质量） |
| `/sdd:ff` | 一键快进（micro-spec → checkpoint → 实现 → 回写） |
| `/sdd:fix` | 零 spec 快修（纯机械改动） |
| `/sdd:resume` | 跨会话恢复（从状态块读取断点） |
| `/sdd:archive` | 知识沉淀（human/llm 视角 + Trace to Sources） |
| `/sdd:status` | 查看所有活跃 spec 状态概览 |

## 产物命名规则（统一时间前缀）

| 命令 | 产物路径 |
|------|---------|
| codemap (feature) | `docs/codemap/YYYY-MM-DD_HH-MM_<scope>功能.md` |
| codemap (project) | `docs/codemap/YYYY-MM-DD_HH-MM_<scope>项目总图.md` |
| contextbundle | `docs/context/YYYY-MM-DD_HH-MM_<task>_context_bundle.md` |
| bootstrap | `docs/specs/YYYY-MM-DD_HH-MM_<TaskName>.md` |
| archive (human) | `docs/archive/YYYY-MM-DD_HH-MM_<topic>_human.md` |
| archive (llm) | `docs/archive/YYYY-MM-DD_HH-MM_<topic>_llm.md` |

规则：时间前缀不可省略，业务名不可擅改。

## 阶段完成标准（DoD）

- **RESEARCH DoD**：明确需求边界、现状链路、已知风险，写入 spec
- **INNOVATE（可选）DoD**：完成方案对比与取舍；跳过需写明原因
- **PLAN DoD**：产出 File Changes + Signatures + 原子 Checklist
- **EXECUTE DoD**：代码与 plan 对齐，执行日志回写 spec
- **REVIEW DoD**：三轴评审完整 + 外部证据 + Overall Verdict + 偏差清单

## 阶段门禁

- RESEARCH：优先引用 Requirement Source + Codemap + Context；小任务允许最小化输入
- INNOVATE：复杂任务默认 2-3 方案；小任务可跳过
- PLAN：必须包含 File Changes + Signatures + Checklist
- PLAN 后：建议 `/sdd:reviewspec`；NO-GO 为建议项
- EXECUTE：用户表达明确肯定 + 继续意图后方可执行
- REVIEW：必须三轴 + 外部证据 + Plan-Execution Diff
- ARCHIVE：默认只归档不删源文件

## Codemap 过期检测

| 节点 | 动作 |
|------|------|
| Execute 每步完成后 | 比对改动文件 vs §1.2 Key Index，命中则标记 Stale |
| Review 开始前 | 审计 §1.2 过期状态，WARN 用户 |
| Resume 恢复时 | Stale codemap 警告用户 |
| Archive 归档时 | 附注哪些 codemap 已过期 |

Spec §1.2 字段：`Staleness: Fresh/Stale/Refreshed` + `Stale Reason`

## Spec 章节 → 填写阶段

| 章节 | 填写阶段 |
|------|---------|
| §0 Open Questions | 全程维护 |
| §0.1 Change Log | 全程维护 |
| §1 Requirements + §1.1-1.3 | Research |
| §2 Research Findings | Research |
| §3 Innovate | Innovate（可选） |
| §4 Plan | Plan |
| §4.4 Spec Review Notes | Review Spec（可选） |
| §5 Execute Log | Execute |
| §5.1 Blockers & Deviations | Execute |
| §6 Review Verdict | Review |
| §7 Plan-Execution Diff | Review |
| §8 Archive Record | Archive |

## 关键约束

- No Spec, No Code
- Spec is Truth
- Reverse Sync
- Review FAIL → 回到 Research/Plan
- Review 必须包含外部证据
