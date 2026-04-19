---
name: "SDD: Archive"
description: "归档完成的 spec，沉淀为可复用知识"
---

对完成的 spec 做归档沉淀，将"中间产物"提炼为可复用知识。

**Input**: `/sdd:archive [spec-path]` 或 `/sdd:archive`（默认当前活跃 spec）

**Steps**

1. 检查 spec 状态块，确认 `Phase` 不是 `implementing`（未完成的 spec 禁止归档）
2. 读取 spec 内容，提炼关键知识
3. 生成归档文档：
   - `human`: 汇报视角
   - `llm`: 后续开发参考视角
4. 每个归档文档必须包含 `Trace to Sources`（结论 → 来源文件）
5. 更新状态块 `Phase: Done`

**Output**

- `docs/archive/YYYY-MM-DD_HH-MM_<topic>_human.md`
- `docs/archive/YYYY-MM-DD_HH-MM_<topic>_llm.md`

**Guardrails**

- 有活跃执行中的 spec（Phase 为 implementing）时，禁止归档
- 默认只归档不删除原文件；删除/移动需用户显式授权
