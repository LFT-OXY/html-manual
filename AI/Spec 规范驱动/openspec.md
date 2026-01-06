https://github.com/Fission-AI/OpenSpec



[openspec-工作流程](https://note.3oxy.com/html/openspec-工作流程.html)



[![img](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/openspec_pixel_light.svg)](https://github.com/Fission-AI/OpenSpec)

Spec-driven development for AI coding assistants.
面向 AI 编码助手的规范驱动开发

[![img](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/badge.svg)](https://github.com/Fission-AI/OpenSpec/actions/workflows/ci.yml) [![img](https://camo.githubusercontent.com/92a019ce6fd31fc491a16699f7aa9c4335ef1524dfdfff013ff2f452ee0c0a12/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f4066697373696f6e2d61692f6f70656e737065633f7374796c653d666c61742d737175617265)](https://www.npmjs.com/package/@fission-ai/openspec) [![img](https://camo.githubusercontent.com/e3280a4368d596307a8efc84c8c1450d894636fb32f1765b4bc2511431389c37/68747470733a2f2f696d672e736869656c64732e696f2f6e6f64652f762f4066697373696f6e2d61692f6f70656e737065633f7374796c653d666c61742d737175617265)](https://nodejs.org/) [![img](https://camo.githubusercontent.com/a7e65aee57b11d28e4caff8b945729a66be0bb663f7f93bd24c5aa65699f148e/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4c6963656e73652d4d49542d626c75652e7376673f7374796c653d666c61742d737175617265)](https://github.com/Fission-AI/OpenSpec/blob/main/LICENSE) [![img](https://camo.githubusercontent.com/a491ce4d03b4fa0f547c517517c4dc4f3c12d4b1f598926d973b3f34f638820d/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f436f6e76656e74696f6e616c253230436f6d6d6974732d312e302e302d79656c6c6f772e7376673f7374796c653d666c61742d737175617265)](https://conventionalcommits.org/) [![img](https://camo.githubusercontent.com/4542ca7bf3c2ba49dcedcd83c610e6878ad8aab88df3ed342cad2f81feddfbc7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446973636f72642d4a6f696e253230746865253230636f6d6d756e6974792d3538363546323f6c6f676f3d646973636f7264266c6f676f436f6c6f723d7768697465267374796c653d666c61742d737175617265)](https://discord.gg/YctCnvvshC)

[![img](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/openspec_dashboard.png)](https://github.com/Fission-AI/OpenSpec/blob/main/assets/openspec_dashboard.png)



# 为何选择 OpenSpec？

当需求存在于聊天记录中时，AI 编码助手虽强大却难以预测。OpenSpec 引入轻量级规范工作流，在实现前锁定意图，为您提供确定性且可审查的输出结果。

关键成果：

- 人类与人工智能相关方在工作开始前就规格达成一致。
- 结构化变更文件夹（提案、任务和规格更新）确保范围明确且可追溯。
- 共享可见性，可查看已提议、正在处理或已归档的项目。
- 与您现有的 AI 工具协同工作：在支持的场景下使用自定义斜杠命令，其他场景则通过上下文规则实现。

# Openspec 规范驱动的原理是什么？

## 工作原理

```plain
┌────────────────────┐
│ Draft Change       │  修改草案
│ Proposal           │  建议
└────────┬───────────┘
         │ share intent with your AI（与人工智能共享意图）
         ▼
┌────────────────────┐
│ Review & Align     │  审查和对齐（编辑规格/任务）
│ (edit specs/tasks) │◀──── feedback loop ──────┐
└────────┬───────────┘      反馈回路             │
         │ approved plan（核准计划）              │
         ▼                                      │
┌────────────────────┐    执行任务               │
│ Implement Tasks    │──────────────────────────┘
│ (AI writes code)   │  （人工智能编写代码）
└────────┬───────────┘
         │ ship the change（运送）
         ▼
┌────────────────────┐
│ Archive & Update   │  存档和更新
│ Specs (source)     │  规格（来源）
└────────────────────┘

1. 起草一份变更建议书，其中包含您想要的规格更新。
2. 与您的人工智能助理一起审查提案，直到大家都同意为止。
3. 参照商定的规格执行任务。
4. 将更改归档，以便将已批准的更新合并回真实源规范中。
```

## 怎么触发的

执行完 openspec init 之后 会生成几个重要的文件(命令/工作流)

```
Agents.md
openspec-apply.md
openspec-proposal
```

## 怎么使用

- 使用关键词触发，比如 proposal,spec 之类的
- 使用 slash command(需要 AI 编程工具支持）指定触发， claude code 比较特殊，“/”命令后，输入，不要直接 enter 键
- 引用文件到对话框中，主要针对不支持 Agents.md 、Claude.md以及 slash command的工具

## 这些工具内置了 OpenSpec 命令。在系统提示时选择 OpenSpec 集成。

| **Tool**  **工具**           | **Commands**  **命令**                                       |
| ---------------------------- | ------------------------------------------------------------ |
| **Claude Code  克劳德·科德** | `/openspec:proposal`, `/openspec:apply`, `/openspec:archive` `/openspec:提案 `,`/openspec:申请 `,`/openspec:归档` |
| **Codex  法典**              | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (global: `~/.codex/prompts`, auto-installed) `/openspec-proposal`,`/openspec-apply`,`/openspec-archive`(全局:`~/.codex/prompts`, 自动安装) |
| **Cursor  光标**             | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` `/openspec-proposal`,`/openspec-apply`,`/openspec-archive` |
| **Gemini CLI**               | `/openspec:proposal`, `/openspec:apply`, `/openspec:archive` (`.gemini/commands/openspec/`) `/openspec:proposal`,`/openspec:apply`,`/openspec:archive`(`.gemini/commands/openspec/`) |
| **Kilo Code  千码**          | `/openspec-proposal.md`, `/openspec-apply.md`, `/openspec-archive.md` (`.kilocode/workflows/`) `/openspec-proposal.md`,`/openspec-apply.md`,`/openspec-archive.md`(`.kilocode/workflows/`) |
| **RooCode**                  | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (`.roo/commands/`) `/openspec-proposal`,`/openspec-apply`,`/openspec-archive`(`.roo/commands/`) |
| **Windsurf**  **风帆冲浪**   | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (`.windsurf/workflows/`) `/openspec-proposal`,`/openspec-apply`,`/openspec-archive`(`.windsurf/工作流/`) |

Kilo Code 可自动发现团队工作流。将生成的文件保存至 `.kilocode/workflows/目录，` 并通过命令面板`使用/openspec-proposal.md` 、`/openspec-apply.md` ` 或/openspec-archive.md` 触发。



### 安装与初始化

#### 先决条件

- **Node.js >= 20.19.0**- 使用 `node --version` 检查您的版本

#### 步骤 1：全局安装命令行界面

```plain
npm install -g @fission-ai/openspec@latest
```



 验证安装：

```plain
openspec --version
```



#### 步骤 2：在项目中初始化 OpenSpec

导航至您的项目目录：

```plain
cd my-project
```



运行初始化：

```plain
openspec init
```



**初始化过程中会发生什么：**

- 系统将提示您选择任何原生支持的 AI 工具（Claude Code、CodeBuddy、Cursor、OpenCode、Qoder 等）；其他助手始终依赖共享`的 AGENTS.md` 模板文件。
- OpenSpec 会自动为选定的工具配置斜杠命令，并始终在项目根目录生成受管理的 `AGENTS.md` 移交文件。
- 在您的项目中创建了一个新的 `openspec/` 目录结构

**设置完成后：**

- 基础 AI 工具无需额外配置即可触发 `/openspec` 工作流
- 运行 `openspec list 命令`以验证配置并查看任何正在进行的更改
- 如果您的编程助手未能立即显示新的斜杠命令，请重新启动它。斜杠命令在启动时加载，因此重新启动可确保它们显示出来。

### 可选：填充项目上下文

在 `openspec init` 完成后，您将收到一个建议提示，帮助您填充项目上下文：

```plain
填充项目上下文：
"请阅读 openspec/project.md，并帮我填写有关我的项目、技术栈和约定的详细信息"。
```

使用 `openspec/project.md` 文件定义项目级别的规范、标准、架构模式及其他指导原则，这些规范应贯穿所有变更过程。



### 创建您的首次变更

以下是一个真实案例，展示完整的 OpenSpec 工作流。该流程适用于任何 AI 工具。支持原生斜杠命令的工具将自动识别快捷方式。

#### 1. 起草提案

首先请你的 AI 创建一份变更提案：

```plain
你：创建 OpenSpec 变更提案（或者直接使用/openspec:proposal），按角色和团队添加配置文件搜索筛选器

(使用斜线命令的工具的快捷方式：/openspec:proposal 添加配置文件搜索筛选器）

人工智能：我将为配置文件筛选器创建 OpenSpec 变更提案。

*用 proposal.md、tasks.md、spec deltas.* 搭建 openspec/changes/add-profile-filters/ 的支架。
```



#### 2. 核查与复核

请确认变更已正确创建，并审核该提案：

```plain
$ openspec list                             # 确认更改文件夹存在
$ openspec validate add-profile-filters     # V验证规格格式
$ openspec show add-profile-filters         # 审查提案、任务和规格 delta
```



#### 3. 优化规格

反复调整规格，直至完全符合您的需求：

```plain
你：能否为角色和团队筛选器添加验收标准？

人工智能：我会更新规格 delta，加入角色和团队筛选器的方案。

*编辑 openspec/changes/add-profile-filters/specs/profile/spec.md 和 tasks.md.*
```



#### 4. 实施变更

一旦规格看起来不错，就开始实施：

```plain
你规格看起来不错。让我们实现这一改变吧。

(使用斜线命令的工具快捷方式：/openspec:apply add-profile-filters）

人工智能：我将完成添加配置文件过滤器变更中的任务。

*整合 openspec/changes/add-profile-filters/tasks.md* 中的任务

*标记任务已完成：任务 1.1 ✓, 任务 1.2 ✓, 任务 2.1 ✓...*
```



#### 5. 归档已完成的变更

实施完成后，归档变更：

```plain
AI: 所有任务均已完成。实施准备就绪。

你请将更改存档

(带斜线命令的工具的快捷方式：/openspec:archive add-profile-filters)

人工智能：我会归档 add-profile-filters 更改。

*运行：openspec archive add-profile-filters --yes*

✓ 更改已成功存档。规格已更新。为下一个功能做好准备！
```



或者在终端中自行运行该命令：

```plain
$ openspec archive add-profile-filters --yes  # 将已完成的更改存档，无需提示
```

**注：** 具备原生斜杠命令的工具（Claude Code、CodeBuddy、Cursor、Codex、Qoder、RooCode）可使用所示快捷方式。其他工具均支持通过自然语言请求执行操作，例如"创建 OpenSpec 提案"、"应用 OpenSpec 变更"或"归档变更"。



## 命令参考

```plain
openspec list               # 查看活动更改文件夹
openspec view               # 规格和变更的交互式仪表板
openspec show <change>      # 显示变更详情（建议、任务、规格更新）
openspec validate <change>  # 检查规格格式和结构
openspec archive <change> [--yes|-y]   # 将已完成的更改移至存档/（使用 --yes 时为非交互式）
```