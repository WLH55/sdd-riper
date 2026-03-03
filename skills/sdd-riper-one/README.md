# SDD-RIPER-ONE Skill 安装指南

> 将 SDD-RIPER 方法论落地为严格可执行流程的技能

---

## 📦 关于 Skill 安装

**重要说明**：目前业界**没有统一的 "Skill 一键安装" 标准**。不同 AI 平台的配置方式各不相同。

本指南提供了主流平台的配置方法，请根据你使用的工具选择对应的安装方式。

---

## 🚀 快速开始

### 方式一：Claude Desktop / Claude.ai（推荐）

1. **复制 Skill 内容**

   ```bash
   # 在本项目根目录执行
   cat skills/sdd-riper-one/SKILL.md | pbcopy
   ```

2. **配置 Custom Instructions**
   - **Claude Desktop**：
     - 打开设置 → Custom Instructions
     - 粘贴 SKILL.md 的完整内容

   - **Claude.ai**：
     - 在对话开始时，先发送一条消息：

       ```
       请阅读并遵循以下 Skill 协议：
       [粘贴 SKILL.md 内容]
       ```

3. **验证安装**

   ```text
   输入：create_codemap: mode=project, scope=my-project
   ```

---

### 方式二：Cursor

1. **创建 .cursorrules 文件**

   ```bash
   # 在你的项目根目录
   cp skills/sdd-riper-one/SKILL.md .cursorrules
   ```

2. **或者添加到全局配置**

   ```bash
   # macOS/Linux
   mkdir -p ~/.cursor
   cp skills/sdd-riper-one/SKILL.md ~/.cursor/rules.md
   
   # Windows
   mkdir %USERPROFILE%\.cursor
   copy skills\sdd-riper-one\SKILL.md %USERPROFILE%\.cursor\rules.md
   ```

3. **验证安装**
   - 重启 Cursor
   - 在 Chat 中输入：`sdd_bootstrap: task=测试任务`

---

### 方式三：其他 AI Agent（通用方法）

如果你使用的是其他支持自定义 Prompt 的 AI 工具（如 Windsurf、Cline 等）：

1. **找到自定义 Prompt 配置位置**
   - 通常在：设置 → System Prompt / Custom Instructions / Agent Rules

2. **复制 Skill 内容**

   ```bash
   cat skills/sdd-riper-one/SKILL.md
   ```

3. **粘贴到配置中**
   - 完整粘贴 SKILL.md 的内容
   - 保存并重启工具

---

## 📖 使用示例

安装完成后，你可以直接使用以下命令：

### 1. 生成代码地图

```text
create_codemap: mode=project, scope=my-project, goal=输出项目总图与主流程
```

### 2. 整理需求上下文

```text
build_context_bundle: ./docs/requirements/
```

### 3. 启动 SDD 任务

```text
sdd_bootstrap: task=用户登录功能, goal=实现完整的登录流程, requirement=支持邮箱和手机号登录
```

### 4. 快速修改（小改动）

```text
FAST: 修改登录按钮颜色为蓝色
```

### 5. Debug 模式

```text
DEBUG: log_path=./logs/error.log, issue=用户登录失败
```

---

## 📚 完整文档

- **核心协议**：[`references/sdd-riper-one-protocol.md`](./references/sdd-riper-one-protocol.md)
- **Spec 模板**：[`references/spec-template.md`](./references/spec-template.md)
- **工作流速查**：[`references/workflow-quickref.md`](./references/workflow-quickref.md)
- **使用示例**：[`references/usage-examples.md`](./references/usage-examples.md)

---

## 🔧 高级配置

### 多项目模式

如果你的工作区包含多个子项目（Monorepo）：

```text
sdd_bootstrap: mode=multi_project, task=跨项目重构
```

Skill 会自动发现并管理多个子项目。

### 自定义工作目录

```text
# 在 SKILL.md 中修改默认路径
mydocs/
├── codemap/    # 代码地图存储位置
├── context/    # 上下文 bundle 存储位置
└── specs/      # Spec 文档存储位置
```

---

## ❓ 常见问题

### Q1: 为什么没有"一键安装"脚本？

**A**: 因为不同 AI 平台的配置方式差异太大，目前业界没有统一标准。我们提供了主流平台的详细配置指南，大多数情况下只需要复制粘贴即可。

### Q2: 我的 AI 工具不在上述列表中怎么办？

**A**: 只要你的工具支持自定义 System Prompt 或 Custom Instructions，就可以使用。找到配置入口，将 `SKILL.md` 的内容粘贴进去即可。

### Q3: Skill 会自动更新吗？

**A**: 不会。如果 Skill 有更新，你需要：

1. 从 GitHub 拉取最新版本
2. 重新复制 `SKILL.md` 到你的配置中

### Q4: 可以同时使用多个 Skill 吗？

**A**: 可以，但需要注意：

- 确保不同 Skill 的命令不冲突
- 建议将多个 Skill 合并到一个配置文件中
- 注意 Token 限制（过长的配置可能影响性能）

---

## 🤝 贡献

如果你发现了更好的安装方法，或者适配了新的 AI 平台，欢迎提交 PR！

---

## 📄 许可

本 Skill 遵循 [MIT License](../../LICENSE)（如果有的话）。
