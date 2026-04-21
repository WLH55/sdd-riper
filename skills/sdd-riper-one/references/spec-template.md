# SDD Spec Template (Workflow-aligned)

本模板包含三个版本：**单项目模板**（默认）、**多项目模板**（`mode=multi_project`）和 **Micro Spec 模板**（`/sdd:ff` 快速流使用）。

---

## 状态块格式

每个 spec 文件顶部必须包含状态块（blockquote 格式），作为 AI 的"程序计数器"。

```markdown
> **Phase**: <research|innovate|plan|execute|review|done> | **Updated**: YYYY-MM-DD HH:MM
> **Goal**: <核心目标>
> **Approval**: Pending / Approved / N/A        ← 可选
> **Scope**: In <...> | Out <...>               ← 可选
> **Done Contract**: <完成定义>                  ← 可选
> **Last**: <上次完成什么> | **Next**: <下一步动作>
```

- `Phase`、`Updated`、`Goal`、`Last`、`Next` 为必填字段
- `Approval`、`Scope`、`Done Contract` 为可选字段
- 每轮 AI 回复结束后必须更新 `Updated`、`Last`、`Next`；阶段切换时更新 `Phase`

---

## Micro Spec 模板（`/sdd:ff` 使用）

适用于小任务、需求明确的快速开发。压缩流程：任务 → micro-spec → checkpoint → 实现 → 回写。

```markdown
# SDD Spec: <Task Name>

> **Phase**: plan | **Updated**: YYYY-MM-DD HH:MM
> **Goal**: <核心目标，1 句话>
> **Done Contract**: <什么算完成、由什么证明>
> **Last**: <初始化> | **Next**: <等待批准>

## micro-spec

- **目标**: ...
- **涉及文件**: ...
- **主要风险**: ...
- **验证方式**: ...

## Execute Log

- [ ] Step 1: ...

## Review Summary

- 结果: PASS/FAIL
- 外部证据: <git diff / 测试输出 / lint / 人工确认>
- 偏差: 无 / <描述>
```

**Guardrails**：
- 必须有最小 spec，不能裸改代码
- 必须等用户批准后再执行
- 完成由证据证明，不是模型自行宣布
- 执行后必须回写 spec
- 复杂度上升时立即升级到标准版 `/sdd:bootstrap`

---

## 单项目模板（默认）

<!-- 以下为 spec 文件的实际内容模板，直接复制使用 -->

# SDD Spec: <Task Name>

> **Phase**: research | **Updated**: YYYY-MM-DD HH:MM
> **Goal**: <核心目标>
> **Approval**: Pending
> **Scope**: In <...> | Out <...>
> **Last**: <初始化> | **Next**: <调研代码库>

## 0. Open Questions

- [ ] None

## 0.1 Change Log

| Date | Section | Change | Reason |
|------|---------|--------|--------|
|  |  |  |  |

## 1. Requirements (Context)

- **Goal**: ...
- **In-Scope**: ...
- **Out-of-Scope**: ...

## 1.1 Context Sources

- Requirement Source: ...
- Design Refs: ...
- Chat/Business Refs: ...

## 1.2 Codemap Used

- Codemap Mode: feature / project
- Codemap File: `docs/codemap/YYYY-MM-DD_HH-MM_<name>.md`
- Staleness: Fresh / Stale / Refreshed
- Stale Reason: <files changed that affect this codemap, or empty if Fresh>
- Key Index:
  - Entry Points / Architecture Layers: ...
  - Core Logic / Cross-Module Flows: ...
  - Dependencies / External Systems: ...

## 1.3 Context Bundle Snapshot

- Bundle Level: Lite / Standard
- Bundle File: `docs/context/YYYY-MM-DD_HH-MM_<task>_context_bundle.md`
- Key Facts: ...
- Open Questions: ...

## 2. Research Findings

- 事实与约束: ...
- 风险与不确定项: ...

## 2.1 Next Actions

- 下一步动作 1 ...
- 下一步动作 2 ...

## 3. Innovate (Optional)

### Option A
- Pros: ...
- Cons: ...

### Option B
- Pros: ...
- Cons: ...

### Decision
- Selected: ...
- Why: ...

### Skip (for small/simple tasks)
- Skipped: true/false
- Reason: ...

## 4. Plan (Contract)

### 4.1 File Changes

- `path/to/file`: 变更说明

### 4.2 Signatures

- `fn/class signature`: ...

### 4.3 Implementation Checklist

- [ ] 1. ...
- [ ] 2. ...

### 4.4 Spec Review Notes (Optional Advisory, Pre-Execute)

- Spec Review Matrix:
| Check | Verdict | Evidence |
|--------|---------|----------|
| Requirement clarity & acceptance | PASS/FAIL/PARTIAL | ... |
| Plan executability | PASS/FAIL/PARTIAL | ... |
| Risk / rollback readiness | PASS/FAIL/PARTIAL | ... |
- Readiness Verdict: GO/NO-GO (Advisory)
- Risks & Suggestions: ...
- Phase Reminders: ...
- User Decision (if NO-GO): Proceed / Revise

## 5. Execute Log

- [ ] Step 1: ...
- [ ] Step 2: ...

### 5.1 Blockers & Deviations

| Step | Blocker | Resolution | Status |
|------|---------|------------|--------|
|  |  |  |  |

## 6. Review Verdict

- Review Matrix (Mandatory):
| Axis | Key Checks | Verdict | Evidence |
|------|-----------|---------|----------|
| Spec Quality & Requirement Completion | Goal/In-Scope/Acceptance 是否完整清晰；需求是否达成 | PASS/FAIL/PARTIAL | ... |
| Spec-Code Fidelity | 文件、签名、checklist、行为是否与 Plan 一致 | PASS/FAIL/PARTIAL | ... |
| Code Intrinsic Quality | 正确性、鲁棒性、可维护性、测试、关键风险 | PASS/FAIL/PARTIAL | ... |
- External Evidence: <test output / lint / git diff / human confirmation, or "无外部验证 — Review 标记 PARTIAL">
- Overall Verdict: PASS/FAIL
- Blocking Issues: ...
- Regression risk: Low/Medium/High
- Follow-ups: ...

## 7. Plan-Execution Diff

- Any deviation from plan: ...

## 8. Archive Record (Recommended at closure)

- Archive Mode: snapshot / thematic
- Audience: human / llm / both
- Source Targets:
  - `docs/specs/...`
  - `docs/codemap/...`
- Archive Outputs:
  - `docs/archive/YYYY-MM-DD_HH-MM_<topic>_human.md`
  - `docs/archive/YYYY-MM-DD_HH-MM_<topic>_llm.md`
- Key Distilled Knowledge: ...

---

## 多项目模板（`mode=multi_project` 时使用）

多项目模板在单项目模板基础上增加以下章节。§4.4 Spec Review Notes 编号固定不变，跨项目契约为 §4.5。

### 额外章节

**§0.2 Project Registry**

| project_id | project_path | project_type | marker_file |
|------------|-------------|-------------|-------------|
| web-console | ./web-console | typescript | package.json |
| api-service | ./api-service | java | pom.xml |

**§0.3 Multi-Project Config**

- **workdir**: `./`
- **active_project**: `web-console`
- **active_workdir**: `./web-console`
- **change_scope**: `local`
- **related_projects**: `api-service`

**§1.2 Codemap Used** 按项目分组：

### web-console
- Codemap File: `docs/codemap/YYYY-MM-DD_HH-MM_web-console项目总图.md`
- Staleness: Fresh / Stale / Refreshed
- Stale Reason: <files changed, or empty if Fresh>
- Key Index: ...

### api-service
- Codemap File: `docs/codemap/YYYY-MM-DD_HH-MM_api-service项目总图.md`
- Staleness: Fresh / Stale / Refreshed
- Stale Reason: <files changed, or empty if Fresh>
- Key Index: ...

**§2 Research Findings** 增加跨项目依赖关系。

**§4.1 File Changes / §4.2 Signatures / §4.3 Checklist** 按项目分组：

#### [api-service] (provider first)
- ...

#### [web-console] (consumer second)
- ...

**§4.5 Contract Interfaces**（仅 `change_scope=cross` 时必填）

| Provider | Interface / API | Consumer(s) | Breaking Change? | Migration Plan |
|----------|-----------------|-------------|------------------|----------------|
| api-service | `POST /api/release` | web-console | No | N/A |

**§5 Execute Log** 按项目分组（同 §4）。

**§6.1 Touched Projects**

| project_id | Files Changed | Reason |
|------------|---------------|--------|
| api-service | `ReleaseController.java` | 新增发布接口 |
| web-console | `release.tsx` | 对接发布接口 |

**§6 Review Verdict** 必须包含：
- per-project regression risk
- cross-project consistency: PASS/FAIL

**§7 Plan-Execution Diff** 增加：
- Orphan changes (files outside registered projects): None

---

## 使用要求

### 通用

- 中大型任务建议先具备 Codemap + Context Bundle + 首版 Spec；小任务可先出首版 Spec 再补齐
- 首版 Spec 允许先完成 Research 最小章节，后续章节按 RIPER 阶段逐步补齐
- 未经用户批准禁止进入 Execute
- `/sdd:reviewspec` 为建议性预审：NO-GO 不强制阻塞；若继续执行需记录用户决策
- Review 未通过则返回 Research/Plan 重新闭环
- 任务收口时建议填写 §8 Archive Record 并执行 archive
- Review 必须包含外部证据；无外部证据的 Review 标记为 PARTIAL

### 多项目专属

- §0.2 Project Registry 和 §0.3 Multi-Project Config 在 bootstrap 时由 Agent 自动生成
- §4 Plan 的 File Changes、Signatures、Checklist 必须按项目分组，Provider 优先于 Consumer
- §4.5 Contract Interfaces 仅在 change_scope=cross 时必填
- §6.1 Touched Projects 在任何跨项目改动后必填
- §6 Review Verdict 必须包含 per-project regression risk 和 cross-project consistency
