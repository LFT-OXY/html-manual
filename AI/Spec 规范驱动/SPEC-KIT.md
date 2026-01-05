地址： 

https://github.com/github/spec-kit



[spec-kit-工作流程](https://lft-oxy.github.io/html-manual/spec-kit-工作流程.html)

# 🌱 Spec Kit  🌱 规格手册

### *更快地构建高质量软件。*

**一个开源工具包，让您能够专注于产品场景和可预测的结果，而非从头开始逐行编写代码。**

[![img](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/badge-20260105162204086.svg)](https://github.com/github/spec-kit/actions/workflows/release.yml) [![img](https://camo.githubusercontent.com/79f37a502abead3ac4b07bf851b94433d6b2f57d19e7eecd4a18900bee6c740b/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f73746172732f6769746875622f737065632d6b69743f7374796c653d736f6369616c)](https://github.com/github/spec-kit/stargazers) [![img](https://camo.githubusercontent.com/de69478c402ec49440743f91e46921baae6e0a83279421a10890fbd6b16da67c/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f6769746875622f737065632d6b6974)](https://github.com/github/spec-kit/blob/main/LICENSE) [![img](https://camo.githubusercontent.com/2ec2efdf26b99b1c38c6bcabe6224a90fcc4ca2102350935e18d4ac434b288af/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f646f63732d4769744875625f50616765732d626c7565)](https://github.github.io/spec-kit/)



## 🤔 什么是规格驱动开发？

规格驱动开发颠覆了传统软件开发模式。数十年来，代码始终占据主导地位——规格书不过是我们在开始"真正的工作"即编码时搭建的临时框架，完成后便弃之不用。规格驱动开发改变了这一现状：规格书变得可执行，能够直接生成可运行的实现方案，而非仅仅提供指导。

## ⚡ 立即开始

### 1. 安装 Specify CLI

选择您偏好的安装方式：

#### 选项 1：持久化安装（推荐）

安装一次，随处可用：

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

然后直接使用该工具：

```plain
specify init <PROJECT_NAME>
specify check
```

要升级 Specify，请参阅[升级指南](https://github.com/github/spec-kit/blob/main/docs/upgrade.md)获取详细说明。快速升级：

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

#### 选项 2：一次性使用

直接运行无需安装：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

**持久安装的优势：**

- 工具保持安装状态并可在 PATH 中使用
- 无需创建 shell 别名
- 通过 `uv tool list` 、 `uv tool upgrade` 、 `uv tool uninstall` 实现更优的工具管理
- 更简洁的 shell 配置

### 2. 确立项目原则

在项目目录中启动您的 AI 助手。助手中可使用 `/speckit.*` 命令。

使用 `/speckit.constitution` 命令创建项目的指导原则与开发规范，这些规范将指导后续所有开发工作。

```bash
/speckit.constitution 制定以代码质量、测试标准、用户体验一致性和性能要求为重点的原则
```

### 3. 创建规格说明书

使用 `/speckit.specify` 命令描述您想要构建的内容。重点阐述构建目标和原因，而非技术栈。

```bash
/speckit.specify 创建一个应用程序，帮助我将照片整理到不同的相册中。相册按日期分组，并可通过在主页上拖放来重新组织。相册永远不会出现在其他嵌套相册中。在每个相册中，照片可以在类似磁贴的界面中预览。
```

### 4. 制定技术实施计划

使用 `/speckit.plan` 命令提供您的技术栈和架构选择。

```bash
/speckit.plan 应用程序使用 Vite，并使用最少的库。尽可能使用虚幻的 HTML、CSS 和 JavaScript。图片不会上传到任何地方，元数据存储在本地 SQLite 数据库中。
```

### 5. 分解为任务

使用 `/speckit.tasks` 将您的实施计划转化为可操作的任务清单。

```bash
/speckit.tasks
```

### 6. 执行实施

使用 `/speckit.implement` 执行所有任务，并按计划构建您的功能。

```bash
/speckit.implement
```

有关详细的分步说明，请参阅我们的[综合指南](https://github.com/github/spec-kit/blob/main/spec-driven.md)。



## 🔧 指定命令行界面（CLI）引用

`specify` 命令支持以下选项：

### Commands  命令

| **Command**  **命令** | **描述**                                                     |
| --------------------- | ------------------------------------------------------------ |
| `init`                | 从最新模板初始化一个新的 Specify 项目                        |
| `check`               | 检查已安装工具（ `git` 、 `claude` 、 `gemini` 、 `code` / `code-insiders` 、 `cursor-agent` 、 `windsurf` 、 `qwen` 、 `opencode` 、 `codex` 、 `shai` ） |

### `specify init` 参数与选项

| **Argument/Option**  **参数/选项** | **Type**  **类型** | **Description**  **描述**                                    |
| ---------------------------------- | ------------------ | ------------------------------------------------------------ |
| `<project-name>`                   | Argument  论点     | 为新项目目录命名（使用 `--here` 时可选，或使用 `.` 表示当前目录） |
| `--ai`                             | Option  选项       | 需使用的 AI 助手： `claude` 、 `gemini` 、 `copilot` 、 `cursor-agent` 、 `qwen` 、 `opencode` 、 `codex` 、 `windsurf` 、 `kilocode` 、 `auggie` 、 `roo` 、 `codebuddy` , `amp` , `shai` , 或 `q` |
| `--script`                         | Option  选项       | 应使用的脚本变体： `sh` （bash/zsh）或 `ps` （PowerShell）   |
| `--ignore-agent-tools`             | Flag  旗帜         | 跳过对 Claude Code 等 AI 代理工具的检查                      |
| `--no-git`                         | Flag  旗帜         | 跳过 Git 仓库初始化                                          |
| `--here`                           | Flag  旗帜         | 在当前目录初始化项目，而非创建新项目                         |
| `--force`                          | Flag  旗帜         | 在当前目录初始化时强制合并/覆盖（跳过确认）                  |
| `--skip-tls`                       | Flag  旗帜         | 跳过 SSL/TLS 验证（不推荐）                                  |
| `--debug`                          | Flag  旗帜         | 启用详细调试输出以进行故障排除                               |
| `--github-token`                   | Option  选项       | 用于 API 请求的 GitHub 令牌（或设置GH_TOKEN/GITHUB_TOKEN 环境变量） |

### 示例

```plain
# Basic project initialization
specify init my-project

# Initialize with specific AI assistant
specify init my-project --ai claude

# Initialize with Cursor support
specify init my-project --ai cursor-agent

# Initialize with Windsurf support
specify init my-project --ai windsurf

# Initialize with Amp support
specify init my-project --ai amp

# Initialize with SHAI support
specify init my-project --ai shai

# Initialize with PowerShell scripts (Windows/cross-platform)
specify init my-project --ai copilot --script ps

# Initialize in current directory
specify init . --ai copilot
# or use the --here flag
specify init --here --ai copilot

# Force merge into current (non-empty) directory without confirmation
specify init . --force --ai copilot
# or
specify init --here --force --ai copilot

# Skip git initialization
specify init my-project --ai gemini --no-git

# Enable debug output for troubleshooting
specify init my-project --ai claude --debug

# Use GitHub token for API requests (helpful for corporate environments)
specify init my-project --ai claude --github-token ghp_your_token_here

# Check system requirements
specify check
```

### 可用斜杠命令

运行 `specify init` 后，您的 AI 编码助手将能够使用以下斜杠命令进行结构化开发：

#### 核心命令

面向规格驱动开发工作流的核心命令：

| **Command**  **命令**   | **Description**  **描述**              |
| ----------------------- | -------------------------------------- |
| `/speckit.constitution` | 制定或更新项目管理原则与开发指南       |
| `/speckit.specify`      | 定义您想要构建的内容（需求和用户故事） |
| `/speckit.plan`         | 根据您选择的技术栈制定技术实施计划     |
| `/speckit.tasks`        | 生成可执行的任务清单以供实施           |
| `/speckit.implement`    | 执行所有任务，按计划构建该功能。       |

#### 可选命令

增强质量与验证的附加指令：

| **Command**  **命令** | **Description**  **描述**                                    |
| --------------------- | ------------------------------------------------------------ |
| `/speckit.clarify`    | 澄清未明确说明的区域（建议在 `/speckit.plan`之前；原为 `/quizme`） |
| `/speckit.analyze`    | 跨工件一致性与覆盖率分析（在 `/speckit.tasks`之后、 `/speckit.implement`之前运行） |
| `/speckit.checklist`  | 生成自定义质量检查清单，用于验证需求完整性、清晰度和一致性（例如"英文单元测试"）。 |

### Environment Variables  环境变量

| **Variable**  **变量** | **Description**  **描述**                                    |
| ---------------------- | ------------------------------------------------------------ |
| `SPECIFY_FEATURE`      | Git branches. 覆盖非 Git 仓库的功能检测。设置为功能目录名称（例如 `001-photo-albums` ），以便在不使用 Git 分支时处理特定功能。 在使用 `/speckit.plan` 或后续命令之前，必须结合您正在合作的代理的具体情况来设定。 |

## 📚 核心理念

规格驱动开发是一种结构化流程，其核心在于强调：

- 意图驱动开发，即在定义"如何实现"之前先明确"实现什么"
- 基于防护栏与组织原则的丰富规格创建
- 基于提示的多步优化而非一次性代码生成
- 高度依赖先进人工智能模型能力进行规格说明解读

## 🌟 开发阶段

| **Phase**  **阶段**                                         | **Focus**  **聚焦** | **Key Activities**  **关键活动**                             |
| ----------------------------------------------------------- | ------------------- | ------------------------------------------------------------ |
| 0 到 1 **Development** 开发（"绿地项目"）                   | 从头开始生成        | 从高层级需求开始生成规格说明书计划实施步骤构建可投入生产环境的应用程序 |
| **Creative Exploration**  **创意探索**                      | 并行实现            | 探索多样化的解决方案支持多种技术栈与架构尝试用户体验模式     |
| **Iterative Enhancement** ("Brownfield") 迭代增强（"棕地"） | 棕地现代化改造      | 逐步添加功能现代化改造传统系统调整流程                       |



## 📖 Learn More  📖 了解更多

- [**完整的规格驱动开发方法论**](https://github.com/github/spec-kit/blob/main/spec-driven.md) - 深度解析全流程
- [**详细操作指南**](https://github.com/github/spec-kit#-detailed-process) - 逐步实施手册

------



## 📋 详细流程



您可以使用 Specify CLI 工具为项目进行初始化，该工具将把所需的构建产物引入您的环境。运行：

```bash
specify init <project_name>
```

或在当前目录中初始化：

```bash
specify init .
# or use the --here flag
specify init --here
# Skip confirmation when the directory already has files
specify init . --force
# or
specify init --here --force
```



系统将提示您选择正在使用的 AI 代理。您也可以主动在终端中直接指定：

```bash
specify init <project_name> --ai claude
specify init <project_name> --ai gemini
specify init <project_name> --ai copilot

# Or in current directory:
specify init . --ai claude
specify init . --ai codex

# or use --here flag
specify init --here --ai claude
specify init --here --ai codex

# Force merge into a non-empty current directory
specify init . --force --ai claude

# or
specify init --here --force --ai claude
```



命令行界面将检查您是否安装了 Claude Code、Gemini CLI、Cursor CLI、Qwen CLI、opencode、Codex CLI 或 Amazon Q 开发者命令行工具。若未安装，或您希望跳过工具检测直接获取模板，请在命令中使用 `--ignore-agent-tools` ：

```bash
specify init <project_name> --ai claude --ignore-agent-tools
```

### 步骤 1：确立项目原则

前往项目文件夹并运行您的 AI 代理。在本示例中，我们使用的是 `claude` 。

若能看到 `/speckit.constitution` 、 `/speckit.specify` 、 `/speckit.plan` 、 `/speckit.tasks` 和 `/speckit.implement` 命令可用，则说明配置正确。



第一步应是使用 `/speckit.constitution` 命令确立项目的治理原则。这有助于确保在后续所有开发阶段中保持决策的一致性：

```bash
/speckit.constitution 制定以代码质量、测试标准、用户体验一致性和性能要求为重点的原则。包括对这些原则如何指导技术决策和实施选择的管理。
```

此步骤将创建或更新 `.specify/memory/constitution.md` 文件，其中包含项目的基础指导方针，AI 代理将在规格制定、规划和实施阶段参考这些内容。



### 步骤 2：创建项目规范



在确立项目原则后，您现在可以开始制定功能规格说明书。请使用 `/speckit.specify` 命令，并为拟开发项目提供具体需求。

[!重要] 请尽可能明确说明你试图构建的内容及其原因。现阶段请勿聚焦于技术栈。

示例提示：

```plain
开发团队生产力平台 Taskify。它应允许用户创建项目、添加团队成员、

分配任务、发表评论并在看板风格的板块间移动任务。在该功能的初始阶段

让我们把它称为 "创建 Taskify"，让我们有多个用户，但用户将提前声明并预先定义。

我需要两个不同类别的五个用户，一个产品经理和四个工程师。让我们创建三个

不同的示例项目。

"进行中"、"审核中 "和 "已完成"。此应用程序将不需要登录，因为这仅仅是

第一项测试工作，以确保我们的基本功能已设置完毕。在任务卡的用户界面中，对于每项任务

您应该能够在看板工作板的不同列之间更改任务的当前状态。

您应该可以为某个任务卡留下不限数量的评论。您应该能够从该任务

卡中指定一个有效用户。首次启动 Taskify 时，它将为您提供一份可供选择的五个用户的列表

供你选择。无需密码。点击用户后，会进入主视图，显示以下用户列表

项目列表。点击项目后，就会打开该项目的看板。你将看到各栏。

你可以在不同列之间来回拖放卡片。你会看到任何卡片

与其他所有卡片的颜色不同，这样你就可以快速地

你的卡片。您可以编辑自己发表的任何评论，但不能编辑其他人发表的评论。您可以

删除自己发表的任何评论，但不能删除其他人发表的评论。
```



输入此提示后，您将看到 Claude Code 启动规划和规格起草流程。Claude Code 还将触发部分内置脚本以配置代码仓库。



完成此步骤后，您应已创建一个新分支（例如 `001-create-taskify` ），并在 `specs/001-create-taskify` 目录中生成一个新规范。



生成的规格说明书应包含一组用户故事和功能需求，具体定义详见模板。



在此阶段，您的项目文件夹内容应类似如下：

```plain
└── .specify
    ├── memory
    │  └── constitution.md
    ├── scripts
    │  ├── check-prerequisites.sh
    │  ├── common.sh
    │  ├── create-new-feature.sh
    │  ├── setup-plan.sh
    │  └── update-claude-md.sh
    ├── specs
    │  └── 001-create-taskify
    │      └── spec.md
    └── templates
        ├── plan-template.md
        ├── spec-template.md
        └── tasks-template.md
```



### 步骤 3：功能规格说明澄清（规划前必备）



基准规格书创建完成后，您可以着手澄清首次尝试中未能准确捕捉到的任何需求。

在制定技术方案前，应执行结构化澄清工作流，以减少后续环节的返工。

首选顺序：

1. 使用 `/speckit.clarify` （结构化）——顺序式、覆盖式提问，将回答记录在"澄清"部分。
2. 若仍有模糊之处，可酌情进行临时自由形式的细化。

若您有意跳过澄清环节（例如快速原型或探索性原型），请明确说明此情况，以免代理因缺少澄清信息而阻塞流程。

示例自由格式优化提示（若仍需使用，请置于 `/speckit.clarify` 之后）：

```plain
对于您创建的每个示例项目或项目，任务数量应介于 5 到 15 个之间。

每个任务随机分配到不同的完成状态。确保至少有

每个完成阶段至少有一项任务。
```



您还应请克劳德·科德核验《审查与验收清单》，对已通过验证/满足要求的事项打勾确认，未通过者则保持未勾选状态。可使用以下提示语：

```plain
阅读审核和验收核对表，如果功能规格符合标准，则勾选核对表中的每个项目。如果不符合，则留空。
```

与 Claude Code 的交互应视为澄清规范并提出疑问的契机——切勿将其首次尝试视为最终版本。



### 步骤 4：制定计划

现在您可以具体说明技术栈及其他技术要求。可使用项目模板内置的 `/speckit.plan` 命令，例如以下提示：

```plain
我们将使用.NET Aspire，以 Postgres 作为数据库来生成该文件。前台应使用

Blazor 服务器，可拖放任务板，实时更新。应使用项目 API 创建 REST API、

任务 API 和通知 API。
```



此步骤的输出将包含若干实现细节文档，您的目录树结构将类似于以下形式：

```plain
.
├── CLAUDE.md
├── memory
│  └── constitution.md
├── scripts
│  ├── check-prerequisites.sh
│  ├── common.sh
│  ├── create-new-feature.sh
│  ├── setup-plan.sh
│  └── update-claude-md.sh
├── specs
│  └── 001-create-taskify
│      ├── contracts
│      │  ├── api-spec.json
│      │  └── signalr-spec.md
│      ├── data-model.md
│      ├── plan.md
│      ├── quickstart.md
│      ├── research.md
│      └── spec.md
└── templates
    ├── CLAUDE-template.md
    ├── plan-template.md
    ├── spec-template.md
    └── tasks-template.md
```



检查 `research.md` 文档，确保根据您的指示使用了正确的科技栈。若存在突出的组件，可要求 Claude Code 进行优化，甚至让其核查本地安装的所需平台/框架版本（例如.NET）。

此外，若所选技术栈属于快速迭代的领域（例如.NET Aspire、JS 框架），您可考虑让 Claude Code 深入研究其细节，建议使用如下提示语：

```plain
我希望你们仔细研究实施计划和实施细节，寻找可以

由于 .NET Aspire 是一个快速变化的库，因此需要进行更多的研究。对于您发现的那些领域

需要进一步研究的领域，我希望您更新研究文件，提供有关具体

版本的更多细节，并催生并行研究任务，以澄清

任何细节。
```



在此过程中，你可能会发现克劳德代码陷入了错误的研究方向——你可以通过这样的提示来引导它回归正轨：

```plain
我认为我们需要将其分解为一系列步骤。首先，确定任务清单

你在实施过程中需要完成的、你不确定的或会对你有帮助的任务清单。

进一步研究。写下这些任务的清单。然后针对每项任务

我希望你能为每项任务设计一个独立的研究任务，这样我们的研究结果就是

所有这些非常具体的任务同时进行。我看到你在做的事情看起来是

研究 .NET Aspire 的总体情况，我不认为这对我们的工作有什么帮助。

这样的研究太没有针对性了。研究需要帮助您解决一个具体的目标问题。
```

[!注意] Claude Code 可能过于热心，添加了你未曾要求的功能组件。请它说明变更的依据和来源。



### 步骤 5：让克劳德代码验证该方案

计划制定后，应让克劳德·科德执行该计划，确保没有遗漏环节。可使用如下提示：

```plain
现在，我要你们去审核实施计划和实施细节文件。

通读这些文件，确定是否有一连串的任务是你需要的

因为我不知道这里的内容是否足够。因为我不知道这里的内容是否足够。举个例子

当我查看核心实现时，最好能参考实现中的适当位置

在核心实现或改进中的每个步骤中，都能找到相关信息。
```



这有助于完善实施计划，并帮助您规避 Claude Code 在规划周期中可能遗漏的潜在盲点。初步优化阶段完成后，请要求 Claude Code 再次核对检查清单，方可进入实施阶段。



您也可以要求 Claude Code（前提是您已安装 GitHub CLI）直接从当前分支向 `main` 创建拉取请求，并附上详细说明，以确保工作成果得到妥善追踪。

[!注意] 在让代理人执行计划前，建议先让克劳德代码交叉核对细节，排查是否存在过度设计的组件（请谨记——它可能过于积极）。若发现过度设计的组件或决策，可要求克劳德代码予以修正。确保克劳德代码在制定计划时，将宪法作为必须遵循的基础准则。



### 步骤 6：使用 /speckit.tasks 生成任务分解

在实施计划获得验证后，您现在可以将计划分解为具体可执行的任务，并按正确顺序执行。使用 `/speckit.tasks` 命令可从您的实施计划中自动生成详细的任务分解：

```plain
/speckit.tasks
```



此步骤将在特性规范目录中创建一个 `tasks.md` 文件，其中包含：

- 按用户故事组织的任务分解——每个用户故事成为独立的实施阶段，并拥有其专属的任务集
- 依赖管理——任务按组件间的依赖关系排序（例如：模型先于服务，服务先于端点）
- 并行执行标记 - 可并行运行的任务使用 `[P]` 标记，以优化开发工作流
- 文件路径规范 - 每个任务都包含应执行实现的精确文件路径
- 测试驱动开发结构——若需测试，则包含测试任务，并要求在实现前编写测试代码。
- 检查点验证 - 每个用户故事阶段都包含检查点，用于验证独立功能

生成的 tasks.md 为 `/speckit.implement` 命令提供了清晰的路线图，确保系统化实施既能维持代码质量，又可实现用户故事的增量交付。



### 步骤 7：实施

准备就绪后，使用 `/speckit.implement` 命令执行您的实施计划：

```plain
/speckit.implement
```



`/speckit.implement` 命令将：

- 验证所有先决条件是否就绪（包括章程、规范、计划和任务）
- 解析任务分解结构，源自 `tasks.md`
- 按正确顺序执行任务，同时遵守依赖关系和并行执行标记
- 遵循任务计划中定义的测试驱动开发方法
- 及时提供进度更新并妥善处理错误

[!重要] AI 代理将执行本地命令行命令（例如 `dotnet` 、 `npm` 等）——请确保您的机器已安装所需工具。

完成实施后，请测试应用程序并解决任何可能未在命令行界面日志中显示的运行时错误（例如浏览器控制台错误）。您可以将此类错误复制粘贴回人工智能代理进行处理。