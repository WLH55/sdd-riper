# SDD-RIPER-ONE Protocol

> Version: 2.0.0 | Updated: 2026-04-21

本文件是协议总索引。详细协议按阶段拆分为独立文件，AI 按需加载当前阶段对应的协议文件，不需要一次性加载全部。

## 核心规则

核心法则、状态管理、门禁规则、上下文读取规则、优先级与自动切换见 **`references/protocol-core.md`**。

## 阶段协议索引

| 阶段 | 协议文件 | 覆盖内容 |
|------|---------|---------|
| Pre-Research | `references/protocol-research.md` | Codemap、Context Bundle |
| Research | `references/protocol-research.md` | RESEARCH 模式、INNOVATE 模式 |
| Plan | `references/protocol-plan-execute.md` | PLAN 模式、REVIEW_SPEC 预审 |
| Execute | `references/protocol-plan-execute.md` | EXECUTE 模式、FAST 模式 |
| Review | `references/protocol-review-archive.md` | 三轴审查、ARCHIVE |
| Multi-Project | `references/multi-project.md` | 多项目自动发现与作用域隔离 |
| Debug | `references/protocol-debug.md` | 日志驱动诊断与验证 |

## 加载策略

- 启动时加载 `protocol-core.md`（约 100 行）
- 进入具体阶段时加载对应协议文件
- 不需要每轮重载已加载的协议
- Spec 模板见 `references/spec-template.md`
- Codemap Staleness Detection 详见 `references/protocol-research.md`

## Spec 章节 → 协议文件映射

| Spec 章节 | 填写阶段 | 协议文件 |
|-----------|---------|---------|
| §0-§1.3 (Open Questions, Requirements, Codemap, Context) | Research | protocol-research.md |
| §2 Research Findings | Research | protocol-research.md |
| §3 Innovate | Innovate (可选) | protocol-research.md |
| §4 Plan | Plan | protocol-plan-execute.md |
| §4.4 Spec Review Notes | Review Spec (可选) | protocol-plan-execute.md |
| §5 Execute Log | Execute | protocol-plan-execute.md |
| §6 Review Verdict | Review | protocol-review-archive.md |
| §7 Plan-Execution Diff | Review | protocol-review-archive.md |
| §8 Archive Record | Archive | protocol-review-archive.md |
