## OpenSkills 完整安装与使用

[GitHub - numman-ali/openskills: Universal skills loader for AI coding agents - npm i -g openskills](https://github.com/numman-ali/openskills)

### 📋 安装步骤

### 第一步：全局安装OpenSkills CLI

```bash
npm i -g openskills
```

验证安装成功：

```bash
openskills --version
```

### 第二步：创建AGENTS.md文件

在项目根目录创建AGENTS.md（OpenSkills sync命令的必需文件）：

```bash
cd /Users/oxy/Documents/program/CompanyProjects/bank
touch AGENTS.md
```

在AGENTS.md中添加基础内容：

```markdown
# AI Agent Instructions

This file contains instructions and capabilities for AI coding agents.

<!-- Skills will be automatically injected here by openskills sync -->
```

### 第三步：安装技能

#### 选项A：交互式安装（推荐）

从Anthropic官方市场安装：

```bash
cd /Users/oxy/Documents/program/CompanyProjects/bank
openskills install anthropics/skills
```

这会显示交互式界面，让你选择要安装的技能：

- ✅ pdf — PDF处理（提取、合并、拆分）
- ✅ xlsx — Excel电子表格操作
- ✅ docx — Word文档处理
- ✅ pptx — PPT演示文稿
- ✅ mcp-builder — MCP服务器构建
- ✅ canvas-design — 视觉设计
- ✅ skill-creator — 技能创建指南

#### 选项B：非交互式安装

```bash
openskills install anthropics/skills -y
```

#### 选项C：安装特定技能

从GitHub安装自定义技能仓库：

```bash
openskills install your-username/custom-skills
```

### 第四步：同步技能到AGENTS.md

```bash
openskills sync
```

这会：

1. 扫描已安装的技能
2. 显示交互式选择界面
3. 将选中的技能信息注入到AGENTS.md的`<available_skills>`部分

------

## 🎯 使用方法

### 在Windsurf中使用技能

#### 1. 查看已安装的技能

```bash
openskills list
```

#### 2. 在对话中调用技能

当你在Windsurf中与AI对话时，系统会自动读取AGENTS.md中的技能列表。

**用户发起请求：**

```plain
"帮我从这个PDF文件中提取文本内容"
```

**AI代理会：**

1. 扫描`<available_skills>`，发现pdf技能
2. 执行命令加载技能详细指令：

```bash
openskills read pdf
```

1. 根据加载的指令完成任务

#### 3. 手动加载技能（供参考）

```bash
openskills read pdf
```

这会输出技能的完整指令内容，包括：

- 基础目录路径
- 详细使用说明
- 捆绑资源位置（scripts/、references/、assets/）

------

## 📁 安装模式详解

### 默认模式：项目级安装

```bash
openskills install anthropics/skills
```

- 安装位置：`./.claude/skills/`
- 特点：项目独立，会被gitignore
- 适用场景：单项目使用

### 全局模式：跨项目共享

```bash
openskills install anthropics/skills --global
```

- 安装位置：`~/.claude/skills/`
- 特点：所有项目共享
- 适用场景：多项目使用相同技能

### 通用模式：多代理共享

```bash
openskills install anthropics/skills --universal
```

- 安装位置：`./.agent/skills/`
- 特点：Claude Code + Windsurf等多代理共享
- 适用场景：同时使用多个AI代理

------

## 🛠️ 常用命令速查

| 命令                             | 说明                    |
| -------------------------------- | ----------------------- |
| `openskills install <source>`    | 安装技能（交互式）      |
| `openskills install <source> -y` | 安装技能（非交互式）    |
| `openskills sync`                | 同步技能到AGENTS.md     |
| `openskills sync -y`             | 同步技能（非交互式）    |
| `openskills list`                | 列出已安装技能          |
| `openskills read <name>`         | 加载特定技能            |
| `openskills manage`              | 管理/删除技能（交互式） |
| `openskills remove <name>`       | 删除特定技能            |

------

## 💡 针对你的项目建议

基于你的项目（银行系统，涉及PDF处理），我建议：

### 推荐安装的技能：

1. **pdf** — 用于PDF解析和处理（与你的PdfController相关）
2. **xlsx** — 用于生成贷款报表等Excel文档
3. **docx** — 用于生成合同文档

### 完整安装命令序列：

```bash
# 1. 安装CLI
npm i -g openskills

# 2. 进入项目目录
cd /Users/oxy/Documents/program/CompanyProjects/bank

# 3. 创建AGENTS.md
cat > AGENTS.md << 'EOF'
# AI Agent Instructions for Bank System

This project is a banking system with PDF processing capabilities using MinerU.

## Project Context
- Framework: Spring Boot
- Main features: PDF parsing, loan management
- PDF service: MinerU integration

<!-- Skills section - managed by openskills -->
EOF

# 4. 安装技能（交互式选择）
openskills install anthropics/skills

# 5. 同步到AGENTS.md
openskills sync

# 6. 验证安装
openskills list
```

------



所有技能标记为`(project)`，说明它们只在当前项目中有效。

------

## 转换为全局模式的方案

### 方案一：重新全局安装（推荐）

这是最干净的方法，将技能安装到全局位置`~/.claude/skills/`，所有项目都能访问。

### 详细执行步骤

#### **步骤1：备份当前AGENTS.md**（已完成 ✓）

当前AGENTS.md已存在，包含技能信息。

#### **步骤2：移除当前项目级技能**

可以选择保留或删除项目级技能：

**选项A：完全删除项目级技能**

```bash
cd /Users/oxy/Documents/program/CompanyProjects/bank
openskills manage
# 在交互界面中选择要删除的技能
```

**选项B：批量删除所有项目级技能**

```bash
# 删除项目级技能目录（如果存在.claude/skills/）
rm -rf .claude
```

#### **步骤3：全局安装技能**

```bash
# 从Anthropic官方市场全局安装（交互式选择）
openskills install anthropics/skills --global

# 或者非交互式安装所有技能
openskills install anthropics/skills --global -y
```

这会将技能安装到：`~/.claude/skills/`

#### **步骤4：验证全局安装**

```bash
openskills list
```

应该看到技能标记为`(global)`而不是`(project)`。

#### **步骤5：更新项目AGENTS.md**

由于技能现在是全局的，每个项目的AGENTS.md都可以引用它们：

```bash
cd /Users/oxy/Documents/program/CompanyProjects/bank
openskills sync
```

## 