**1、新功能开发的提示词**

当你完成了一个组件的开发，准备进入下一个组件时，为了避免AI“跑偏”或者遗漏关键细节，需要让它重新理解当前的工作范围，并提前规划实现步骤。这时候，可以使用以下提示词和方法：

**英文提示词：**
*"Refer to [document-name] to understand the scope of work for this feature. Before implementation, write an implementation plan. If you need clarification or have any questions, ask me!"*

**中文提示词：**
*"请参考[文档名称]，以了解该功能的工作范围。在开始实现之前，请编写实现计划。如果有任何疑问，请向我询问！"*

**使用方法：**

1. **引导AI参考文档**：明确指定需要参考的文档，比如`frontend-guidelines.md`，确保AI理解每个组件的功能需求和范围。

1. **要求提前计划**：让AI在执行前先写出实现计划，包括关键步骤、可能的挑战和解决方法。

1. **答疑后执行**：如果AI提出问题，及时解答后再让它继续实施。

**效果：**
这种方法可以显著减少AI在组件开发过程中的**“幻觉”（Hallucination）**问题，让每一步执行都更可靠、更贴近实际需求。通过一步步拆解和规划，确保新功能开发的高效和精准！



**2.任务更新与切换的提示词结构**

在开发过程中，每完成一个任务，都需要让AI知道当前任务已经完成，并明确指示它接下来需要处理的任务，同时关联相关文档以确保理解准确。这一结构可以有效地引导AI逐步推进工作，避免遗漏或偏离方向。

**英文提示词结构：**

1. **提供任务更新：**
   *"Header menu is now aligned in center perfectly."*
   （当前任务已完成的简短总结）

1. **提出下一个任务：**
   *"Now we need a sign-in and sign-up buttons."*
   （明确说明接下来的任务目标）

1. **关联相关文档：**
   *"Check @[document name] and explain how you will implement this."*
   （标记相关文档，并让AI先阐述实现方案）

**使用方法：**

- 每次完成一个任务后，按照上述步骤逐条向AI说明当前状态和下一步计划。

- 通过关联正确的文档，可以减少AI对需求范围的误解，并引导它先提出解决方案，确保实现路径清晰。

**效果：**
这种分步骤提示的方法可以让AI在任务切换时保持清晰的思路，减少误操作，提高开发效率。试试这个结构，让你的开发流程更加流畅吧！



**3、记录工作进度的提示词**

在开发过程中，记录每一步的工作进度对于避免重复工作和追踪问题至关重要。通过让AI在`progress.md`文件中记录完成情况，我们可以提供明确的上下文，减少重复错误的发生，并为后续工作打下坚实的基础。

**英文提示词：**
"At the end of every completed step, record your work log in @progress.md file. Answer these 3 questions step by step:

1. What features we implemented?

1. What errors we encountered?

1. How did we fix all those errors?
   DO NOT miss any information."

**中文提示词：**
"在每个步骤完成后，将工作日志记录在 @progress.md 文件中。按照以下三个问题逐步回答：

1. 我们实现了哪些功能？

1. 我们遇到了哪些错误？

1. 我们是如何解决这些错误的？
   请确保不遗漏任何信息。"

**使用方法：**

1. **完成一个任务后**，指示AI记录日志到`progress.md`文件中。

1. **回答核心问题**：让AI逐条回答实现的功能、遇到的错误及解决方法。

1. **确保详细性**：通过提示词强调不能遗漏任何关键细节。

**效果：**

- **进度透明**：清晰记录每个步骤，便于团队或个人回溯。

- **减少重复错误**：明确问题和解决方案，避免反复犯同样的错误。

- **优化上下文理解**：为AI提供持续的任务背景，提升其可靠性和精准度。

这个方法是高效管理开发进度的利器，尤其适合团队协作和长期项目开发！快试试，让AI成为你工作中的记录助手吧！



**5、项目状态记录的提示词**

为了让AI能够从我们上一次的工作中接续任务，需要在`project-status.md`文件中记录每次会话的详细工作日志。这种方法不仅能帮助AI跟踪完成的内容，还能为下一次工作提供完整的上下文信息。

**英文提示词：**
*"At the end of the session, record your work log in @project-status.md file.
**First, check @progress.md file to understand all the features we have implemented in this session.
**Then, write a detailed session report which should provide detailed context for the next work session."*

**中文提示词：**
*"在本次会话结束时，将你的工作日志记录在@project-status.md文件中。
**首先，检查@progress.md文件，以了解本次会话中已实现的所有功能。
**然后，撰写一份详细的会话报告，为下一次工作会话提供完整的上下文信息。"*

**使用方法：**

1. **检查**`progress.md`**文件**：获取当前会话中已经完成的所有功能细节。

1. **撰写会话报告**：详细描述本次完成的内容、遇到的问题、解决方案，以及未完成的部分。

1. **记录到**`project-status.md`：确保每次会话结束后，AI都能在下一次工作中理解当前的进度。

**效果：**

- **保持工作连续性**：让AI清楚知道已经完成的任务和接下来的工作重点。

- **减少重复**：避免重复解决已完成的任务或遗漏关键细节。

- **提升效率**：通过详细的上下文帮助AI快速进入状态，减少误解和错误。

这个方法适用于长时间、多阶段的项目开发，是保持任务衔接和高效执行的关键！

### 6、Cursor Agent 精准执行提示词

在使用 Cursor Agent 时，有时它可能“过度执行”，导致代码库被不必要地修改。通过一个精准的提示词，可以引导 AI 理解功能范围，并制定清晰的实现计划，避免混乱和无意义的编辑。

**英文提示词：**
*"Read instructions in @ (document name) to understand the scope of work for this feature.
**Use chain of thought reasoning to create a step-by-step implementation plan.
**Ensure you explain how every section of this feature works providing macro-level details.
**Break these items into detailed numbered steps."*

**中文提示词：**
*"请阅读 @(文档名称) 中的说明，以了解该功能的工作范围。
**使用链式思考推理方法，制定逐步实现计划。
**确保解释每个功能部分的工作原理，并提供宏观层面的细节。
**将这些内容分解为详细的编号步骤。"*

**使用方法：**

1. **明确功能范围**：让 AI 先参考指定文档，准确理解当前任务的边界和要求。

1. **链式思考**：要求 AI 分析每个部分的作用，并制定包含宏观细节的计划。

1. **细分为步骤**：让 AI 将任务分解为可操作的编号步骤，以便逐步实现。

1. **验证计划**：在 AI 执行前，检查生成的计划是否与需求完全一致，确认无误后再批准执行。

**效果：**

- **避免不必要的修改**：通过详细计划，确保 AI 只做所需的工作。

- **更高的代码质量**：明确每一部分的功能和目的，减少潜在错误。

- **可控性更高**：计划生成后可提前审核，确保执行方向正确。

通过这一方法，你可以更好地控制 Cursor Agent 的行为，减少不必要的工作，提高整体开发效率！



**7、附加文档统一管理**

为了确保 AI 有足够的上下文并能准确执行任务，可以创建一个专门的“Documentation”或“Instructions”文件夹，将所有核心文档集中存放，方便调用和引用。合理的文档结构配合精准提示词，可以避免多余的内容和不必要的复杂信息。

文档目录建议：

- Project Requirements Doc (PRD)：项目需求文档，清晰定义功能目标和需求。

- **App Flow Doc**：应用流程文档，展示应用的整体工作流和交互逻辑。

- **Frontend Guidelines Doc**：前端开发指南文档，规范 UI/UX 以及前端实现的最佳实践。

- **Backend Structure Doc**：后端结构文档，描述 API、数据库设计及逻辑流。

- **Tech Stack Doc**：技术栈文档，明确开发所用的工具、框架和依赖。

- **File Structure Doc**：文件结构文档，规范代码和资源文件的组织方式。

**英文提示词：**
*"Use the attached documentation in the 'Documentation' folder to understand the scope of work. Refer to relevant files such as PRD, App Flow, or Backend Structure to avoid unnecessary details. Ensure precise implementation aligned with project goals."*

**中文提示词：**
*"使用‘Documentation’文件夹中的附加文档来理解工作范围。参考相关文档，例如 PRD、App Flow 或 Backend Structure，避免不必要的细节。确保实现内容与项目目标保持一致。"*

**注意事项：**

1. **避免信息冗余：** 使用提示词引导 AI 专注于文档中的关键内容，避免生成无用或“过度解释”的部分。

1. **按需引用：** 根据任务需求指定文档，例如实现前端功能时只需参考`Frontend Guidelines`和`App Flow`。

1. **保持一致性：** 每次会话都清楚指明要引用的文档，减少误解或遗漏。

**效果：**
通过整合文档和高效的提示词管理，可以显著提高任务执行的准确性和效率，同时确保项目的各个部分有条不紊地推进！