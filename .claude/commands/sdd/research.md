---
name: "SDD: Research"
description: "进入 Research 阶段，基于任务描述产出首版 spec"
skill: sdd-riper-one
---

本命令属于 `sdd-riper-one` 技能。未加载技能时请先执行 `/sdd-riper-one`。

进入 RIPER 的 Research 阶段。接收用户需求，产出首版 spec 文件，严格遵循 `references/spec-template.md` 的单项目模板格式。

**Input**: `/sdd:research <任务描述>`

- 输入只要是"有意义且真实的需求"即可（口述/文档/聊天记录/context bundle 均可）
- 如有 `mode=multi_project` 或触发词 `MULTI / 多项目`，使用多项目模板

**Steps**

1. **复述理解**：用自己的话复述任务，确认核心目标（延续性指令跳过）
2. **汇总输入**：用户输入 + 代码事实 + 已有资产（codemap / context bundle）
3. **生成首版 spec**，严格按 `references/spec-template.md` 格式，至少填写：
   - 顶部状态块：`Phase: research`、`Goal`、`Last`、`Next`
   - `§1 Requirements`：Goal、In-Scope、Out-of-Scope
   - `§1.1 Context Sources`：来源引用
   - `§1.2 Codemap Used`：引用已有 codemap 或标记"待生成"
   - `§2 Research Findings`：事实、约束、风险
   - `§2.1 Next Actions`：下一步动作
   - `§0 Open Questions`：未解决问题
4. 冲突处理：先落首版 spec 标记冲突，再给 `Option A/B` 和推荐决策
5. 立即持久化到 `docs/specs/YYYY-MM-DD_HH-MM_<TaskName>.md`

**Output**

- `docs/specs/YYYY-MM-DD_HH-MM_<TaskName>.md`
- 格式严格遵循 spec-template.md

**Guardrails**

- 首版 spec 落盘前，不进入代码实现
- spec 格式必须与 spec-template.md 一致，不允许自由发挥结构
- 如果需要 codemap / context bundle 但尚未准备，在 spec 中标记"待补齐"，不阻塞 spec 落盘
