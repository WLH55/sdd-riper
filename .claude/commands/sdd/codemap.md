---
name: "SDD: Codemap"
description: "生成代码索引地图，支持 feature 和 project 两种模式"
---

生成代码索引地图。CodeMap 是代码上下文索引，用于后续按需加载，而不是每轮全仓扫描。

**Input**: `/sdd:codemap [scope]` 或 `/sdd:codemap mode=project scope=<name>`

- `mode`: `feature`（默认）或 `project`
- `scope`: 要索引的功能或项目名称
- `goal`: 可选，辅助聚焦

**Steps**

1. 确定模式和范围
2. 扫描代码库，提取关键信息
3. 生成 codemap 文件

**feature 模式**关注：入口、核心链路、依赖、风险

**project 模式**关注：架构层、核心模块、跨模块流程、外部依赖；图示建议优先 Mermaid（受限可降级为结构化文字图）

**Output**

- feature: `docs/codemap/YYYY-MM-DD_HH-MM_<scope>功能.md`
- project: `docs/codemap/YYYY-MM-DD_HH-MM_<scope>项目总图.md`
