# CodeMap: sdd-riper-one skill 功能索引

> 生成时间: 2026-04-20 15:30 | 模式: feature

## 入口

| 文件 | 角色 |
|------|------|
| `SKILL.md` | 技能入口，全局行为约束 + 状态管理 + 命令索引 |
| `references/sdd-riper-one-protocol.md` | 完整协议定义，6 条 CORE LAW + RIPER 状态机 + 所有模块 |

## 核心链路

```
SKILL.md (入口)
├── commands/           ← 10 个命令文件，按 RIPER 阶段 + 快捷通道
│   ├── codemap.md      Pre-Research
│   ├── bootstrap.md    Research 启动
│   ├── plan.md         Plan
│   ├── review-spec.md  Plan 后预审 (新)
│   ├── execute.md      Execute
│   ├── review.md       Review (三轴)
│   ├── archive.md      归档
│   ├── resume.md       跨会话恢复
│   ├── ff.md           一键快进 (新，来自 light 版)
│   └── fix.md          零 spec 快修 (新，来自 light 版)
├── references/         ← 参考文档
│   ├── spec-template.md          Spec 模板 (单项目 + 多项目)
│   ├── workflow-quickref.md      工作流快速参考
│   ├── multi-project.md          多项目协作规则
│   ├── archive-template.md       归档模板
│   ├── usage-examples.md         使用示例
│   └── commands.md               命令详细说明
└── agents.md           ← Agent 配置（如有）
```

## 关键依赖

- `spec-template.md` 被 `bootstrap.md`、`plan.md`、`review-spec.md`、`review.md` 引用
- `workflow-quickref.md` 汇总所有门禁规则，被 SKILL.md 引用
- `sdd-riper-one-protocol.md` 是唯一真相源，所有命令文件是其子集

## 风险点

- `SKILL.md` 与 `sdd-riper-one-protocol.md` 存在内容重复（约束规则、门禁），需保持同步
- 命令文件的 frontmatter `skill: sdd-riper-one` 依赖宿主框架解析，裸文件直接读可能被忽略
- `spec-template.md` 含 Staleness 字段，但旧 spec 不含此字段，需向下兼容
