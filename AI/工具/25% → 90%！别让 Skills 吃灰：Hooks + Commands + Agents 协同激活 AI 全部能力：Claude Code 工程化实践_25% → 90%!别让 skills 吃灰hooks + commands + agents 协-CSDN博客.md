## 25% → 90%！别让 Skills 吃灰：[Hooks](https://so.csdn.net/so/search?q=Hooks&spm=1001.2101.3001.7020) + Commands + Agents 协同激活 AI 全部能力：Claude Code 工程化实践

### 一、引言：AI 编程助手的"失忆症"

你有没有遇到过这样的场景？

明明给 AI 配置了详细的项目规范，它却视而不见，生成的代码完全不符合架构要求。你告诉它"我们用四层架构"，它偏要写成三层。你强调"禁止使用 [el-input](https://so.csdn.net/so/search?q=el-input&spm=1001.2101.3001.7020)"，它下一秒就给你来一个 `<el-input v-model="value" />`。

这不是 AI 不够聪明，而是它患上了"选择性失忆症"。

我们团队在使用 Claude Code 开发 RuoYi-Plus 项目时，做过一个统计：**在没有任何干预的情况下，AI 主动调用项目 Skills（技能配置）的概率只有 25% 左右**。也就是说，你精心编写的项目规范，有 75% 的概率被 AI 直接忽略。

这就像你给新员工发了一本 500 页的开发手册，但他从来不翻，全凭自己的"经验"写代码。

**问题的本质是：AI 有能力，但缺乏"制度约束"。**

于是我们开始思考：能不能设计一套机制，让 AI 在每次回答前，**强制**检查应该调用哪些技能？

答案是：**Hooks（钩子）**。

在原先基础上经过一个多月的迭代，我们构建了一套完整的 `.claude` 配置体系，核心是 **4 个生命周期钩子 + 26 个专业技能 + 多个斜杠命令 + 自定义代理**。这套体系让开发场景下的技能激活率从约 25% 飙升到 90% 以上，彻底解决了 AI "不听话"的问题。

本文将完整拆解这套配置的设计思路和实现细节。

___

### 二、破局：一个 Hook 的魔法

#### 问题根源

为什么 AI 不主动调用 Skills？

因为在默认情况下，Claude Code 的工作流程是这样的：

```
用户提问 → AI 直接回答
```

Skills 只是"可选参考"，AI 会根据自己的判断决定是否调用。而 AI 的判断标准往往是"这个问题看起来需不需要"，而不是"项目规范要求我必须这样做"。

#### 解决方案：强制评估钩子

我们在 `.claude/hooks/` 目录下创建了一个关键文件：`skill-forced-eval.js`。

这个钩子挂载在 `UserPromptSubmit` 事件上，也就是**用户每次提交问题时自动触发**。它的作用是：在 AI 开始思考之前，强制输出一段"技能评估指令"。

核心代码逻辑：

```csharp
// skill-forced-eval.js 核心逻辑
const instructions = `## 指令：强制技能激活流程（必须执行）

### 步骤 1 - 评估
针对以下每个技能，陈述：[技能名] - 是/否 - [理由]

可用技能列表：
- crud-development: CRUD/业务模块开发
- api-development: API设计/RESTful规范
- database-ops: 数据库/SQL/建表
- ui-pc: 前端组件/AForm/AModal
- ui-mobile: 移动端/WD UI组件
...（共26个技能）

### 步骤 2 - 激活
如果任何技能为"是" → 立即使用 Skill() 工具激活
如果所有技能为"否" → 说明"不需要技能"并继续

### 步骤 3 - 实现
只有在步骤 2 完成后，才能开始实现。`;

console.log(instructions);
```

#### 效果对比

**优化前（无 Hook）：**

```
用户：帮我开发一个优惠券管理功能
AI：好的，我来帮你写代码...（直接开始，忽略项目规范）
```

**优化后（有 Hook）：**

```markdown
用户：帮我开发一个优惠券管理功能

AI 评估结果：
- crud-development: 是 - 涉及业务模块CRUD开发
- database-ops: 是 - 需要建表和字典配置
- ui-pc: 否 - 用户未要求前端页面

激活技能：
> Skill(crud-development)
> Skill(database-ops)

[加载技能知识后开始实现...]
```

**数据验证：** 经过两周的实际使用统计，开发类任务的技能激活率从约 25% 提升到 90% 以上，提升了 **3.6 倍**。

#### 为什么是 90% 而不是 100%？

因为我们设计了一个"逃生通道"：当用户输入的是斜杠命令（如 `/dev`、`/crud`）时，钩子会跳过评估，直接执行命令逻辑。

```javascript
// 检测斜杠命令，跳过技能评估
const isSlashCommand = /^\/[^\/\s]+$/.test(prompt.split(/\s/)[0]);
if (isSlashCommand) {
  process.exit(0); // 跳过
}
```

这样既保证了常规问答的技能激活率，又不干扰命令的执行效率。

___

### 三、Hooks 全景：四个生命周期钩子

除了核心的 `skill-forced-eval.js`，我们还设计了另外三个钩子，覆盖 AI 工作的完整生命周期。

#### 3.1 SessionStart：开工先看项目状态

**触发时机**：每次启动 Claude Code 会话时

**核心功能**：

-   显示当前 Git 分支和未提交变更
-   加载项目待办事项
-   展示快捷命令菜单

**实际效果**：

```markdown
## 🚀 RuoYi-Plus-Uniapp 会话已启动
**时间**: 2024/12/23 14:14:36
**Git 分支**: `master`

⚠️ **未提交变更** (5 个文件):
  M plus-ui/src/views/user/index.vue
  M ruoyi-modules/.../UserService.java

📋 **待办事项**: 3 未完成 / 12 已完成

💡 **快捷命令**:
| /dev  | 开发新功能 |
| /crud | 快速生成CRUD |
| /check| 代码规范检查 |
```

**价值**：让 AI 在开始工作前，先了解项目的当前状态，避免"两眼一抹黑"。

#### 3.2 UserPromptSubmit：强制技能评估（核心）

这就是前面详细讲解的 `skill-forced-eval.js`，不再赘述。

#### 3.3 PreToolUse：安全防护层

**触发时机**：AI 执行 Bash 命令或写入文件之前

**核心功能**：

-   拦截危险命令（`rm -rf /`、`drop database`、`format`）
-   警告敏感操作（删除文件、修改系统配置）
-   阻止运行未经授权的脚本

**代码示例**：

```javascript
// 危险命令黑名单
const dangerousPatterns = [
  /rm\s+(-rf?|--recursive).*\//,  // rm -rf /
  /drop\s+(database|table)/i,      // SQL删库
  /format\s+[a-z]:/i,              // 格式化磁盘
  />\s*\/dev\/sda/,                // 写入磁盘设备
];

// 检测到危险命令，直接阻止
if (dangerousPatterns.some(p => p.test(command))) {
  console.log(JSON.stringify({
    decision: "block",
    reason: "检测到危险命令，已阻止执行"
  }));
  process.exit(0);
}
```

**价值**：防止 AI 因为"理解偏差"执行灾难性操作。我们实际遇到过 AI 试图用 `rm -rf` 清理临时文件的情况，幸好被这个钩子拦截了。

#### 3.4 Stop：总结反馈 + 下一步建议

**触发时机**：AI 完成回答后

**核心功能**：

-   播放完成音效（提醒用户）
-   分析本次代码变更类型
-   智能推荐下一步操作
-   清理临时文件（如 Windows 下误创建的 `nul` 文件）

**实际效果**：

```markdown
---
✅ **任务完成** | 检测到 8 个文件变更

**建议操作**:
- 使用 `@code-reviewer` 审查后端代码
- 新增了 Service，记得更新项目文档
- SQL 文件有变更，确保更新所有数据库脚本

**项目管理**:
- `/update-status` 更新项目状态
- `git add . && git commit -m "feat: xxx"` 提交代码
```

**价值**：每次任务结束后给出明确的"收尾指引"，避免遗漏重要步骤。

#### 四个钩子的协同工作流

```css
┌─────────────────────────────────────────────────────────┐
│                    用户启动会话                          │
└─────────────────────────────────────────────────────────┘
                           ↓
              ┌────────────────────────┐
              │  [Hook] SessionStart   │
              │  展示项目状态、待办事项  │
              └────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│                     用户提问                             │
└─────────────────────────────────────────────────────────┘
                           ↓
              ┌────────────────────────┐
              │[Hook] UserPromptSubmit │
              │  强制评估 26 个技能      │
              │  激活率 25% → 90%+      │
              └────────────────────────┘
                           ↓
              ┌────────────────────────┐
              │    AI 开始执行任务      │
              │  （调用工具、写代码）    │
              └────────────────────────┘
                           ↓
              ┌────────────────────────┐
              │  [Hook] PreToolUse     │
              │  拦截危险命令           │
              │  警告敏感操作           │
              └────────────────────────┘
                           ↓
              ┌────────────────────────┐
              │      AI 完成回答        │
              └────────────────────────┘
                           ↓
              ┌────────────────────────┐
              │     [Hook] Stop        │
              │  播放音效、分析变更     │
              │  推荐下一步操作         │
              └────────────────────────┘
```

___

### 四、Skills：被激活的知识库

有了钩子保证"激活率"，接下来的问题是：**被激活的技能里面有什么？**

#### 4.1 技能分类

我们设计了 26 个专业技能，覆盖开发的各个领域：

|  分类  |         技能          |         职责          |
|------|---------------------|---------------------|
| **后端开发** |  crud-development   |   四层架构 CRUD 开发规范    |
|      |   api-development   |  RESTful API 设计规范   |
|      |    database-ops     |     数据库操作、建表、字典     |
|      | backend-annotations |       注解使用规范        |
|      |    error-handler    |       异常处理规范        |
| **前端开发** |        ui-pc        | Element Plus 封装组件使用 |
|      |      store-pc       |     Pinia 状态管理      |
| **移动端**  |      ui-mobile      |      WD UI 组件库      |
|      |  ui-design-mobile   |       移动端设计规范       |
|      |    store-mobile     |       移动端状态管理       |
|      |   uniapp-platform   |       跨平台条件编译       |
| **业务集成** | payment-integration |       支付功能集成        |
|      | wechat-integration  |       微信生态集成        |
|      | file-oss-management |      文件上传与 OSS      |
|      |   ai-langchain4j    |      AI 大模型集成       |
| **质量保障** |    bug-detective    |       Bug 排查        |
|      | performance-doctor  |        性能优化         |
|      |   security-guard    |        安全防护         |
|      |    code-patterns    |        代码规范         |
| **工程管理** | architecture-design |        架构设计         |
|      |  project-navigator  |       项目结构导航        |
|      |    git-workflow     |       Git 工作流       |
|      |    tech-decision    |        技术选型         |
|      |     brainstorm      |        头脑风暴         |

#### 4.2 SKILL.md 的结构设计

每个技能都有一个 `SKILL.md` 文件，遵循统一的结构：

```markdown
# [技能名称]

## 触发条件
- 关键词：xxx、xxx、xxx
- 场景：当用户需要 xxx 时

## 核心规范
### 规范1：xxx
（详细说明 + 代码示例）

### 规范2：xxx
（详细说明 + 代码示例）

## 禁止事项
- ❌ 禁止 xxx
- ❌ 禁止 xxx

## 参考代码
- 文件路径：`xxx/xxx.java`
- 文件路径：`xxx/xxx.vue`

## 检查清单
- [ ] 是否遵循 xxx
- [ ] 是否使用 xxx
```

#### 4.3 示例：crud-development 技能

以最常用的 `crud-development` 技能为例：

```markdown
# CRUD 开发技能

## 触发条件
- 关键词：CRUD、增删改查、业务模块、Entity、Service、DAO
- 场景：开发新的业务功能模块

## 核心规范

### 四层架构
Controller → Service（不继承）→ DAO（buildQueryWrapper）→ Mapper

### Entity 规范
- 必须继承 TenantEntity
- 主键使用雪花 ID，不用自增

### Service 规范
- 禁止继承 ServiceImpl
- 禁止在 Service 层构建查询条件
- 对象转换必须用 MapstructUtils.convert()

### DAO 规范
- 查询条件必须在 buildQueryWrapper() 中构建
- String 类型用 like()，其他类型用 likeCast()

## 禁止事项
- ❌ Service 继承 ServiceImpl
- ❌ 使用 BeanUtil.copyProperties()
- ❌ Controller 使用通用路径如 /page、/{id}

## 参考代码
- Entity: `ruoyi-modules/.../domain/Ad.java`
- Service: `ruoyi-modules/.../service/impl/AdServiceImpl.java`
- DAO: `ruoyi-modules/.../dao/impl/AdDaoImpl.java`
```

**价值**：当 AI 被激活这个技能后，它会先"学习"这些规范，然后再生成代码。这就像给 AI 发了一本"岗位手册"，而且强制它在开工前必须阅读。

___

### 五、Commands：复杂流程的编排器

Skills 解决的是"知识储备"问题，而 Commands（斜杠命令）解决的是"流程编排"问题。

#### 5.1 /dev：7 步智能开发流程

当用户输入 `/dev 开发用户管理功能` 时，AI 会按照预设的 7 个步骤执行：

```markdown
步骤 1：需求澄清
  - AI 主动询问功能细节
  - 确认业务规则和边界条件

步骤 2：技术设计
  - 确定涉及的数据库表
  - 设计 API 接口
  - 规划前端页面

步骤 3：数据库设计
  - 生成建表 SQL
  - 配置数据字典
  - 插入菜单数据

步骤 4：后端开发
  - 按四层架构生成代码
  - Entity → BO/VO → DAO → Service → Controller

步骤 5：前端开发
  - 生成 API 定义
  - 生成页面组件
  - 配置路由和权限

步骤 6：测试验证
  - 启动服务
  - 执行功能测试

步骤 7：文档更新
  - 更新项目进度
  - 记录待办事项
```

**价值**：把"最佳实践"固化成可重复执行的流程，确保每次开发都遵循相同的标准。

#### 5.2 /crud：一键生成四层架构代码

`/crud` 是使用频率最高的命令。它能从现有数据库表，一键生成完整的四层架构代码：

```yaml
输入：/crud b_coupon

输出：
✅ 生成 Entity: Coupon.java
✅ 生成 BO: CouponBo.java
✅ 生成 VO: CouponVo.java
✅ 生成 DAO: ICouponDao.java / CouponDaoImpl.java
✅ 生成 Service: ICouponService.java / CouponServiceImpl.java
✅ 生成 Controller: CouponController.java
✅ 生成 Mapper: CouponMapper.java
✅ 生成前端 API: couponApi.ts / couponTypes.ts
✅ 生成前端页面: coupon.vue
```

**效率对比**：

-   手动开发：2-4 小时
-   使用 /crud：5-10 分钟

#### 5.3 /check：全栈代码规范检查

```shell
输入：/check

输出：
## 后端检查
✅ Entity 继承 TenantEntity
✅ Service 未继承 ServiceImpl
⚠️ UserDao.java:45 - 建议使用 likeCast() 替代 like()

## 前端检查
✅ 使用 AFormInput 替代 el-input
⚠️ user.vue:23 - 建议使用 AModal 替代 el-dialog

## 移动端检查
✅ 使用 wd-form 组件
✅ useToast 从 @/wd 导入
```

#### 5.4 命令与技能的关系

命令和技能不是孤立的，它们相互配合：

```bash
/dev 命令执行时：
  ├── 步骤3（数据库设计）→ 激活 database-ops 技能
  ├── 步骤4（后端开发）→ 激活 crud-development 技能
  └── 步骤5（前端开发）→ 激活 ui-pc 技能
```

**命令是流程的骨架，技能是填充的血肉。**

___

### 六、Agents：并行处理的子任务专家

有些任务比较独立，不需要在主对话中处理，这时候就可以派发给 Agent（代理）。

#### 6.1 code-reviewer：自动代码审查

**触发方式**：

-   完成代码编写后自动触发
-   手动调用 `@code-reviewer`

**审查内容**：

```markdown
## 后端审查清单
- [ ] Entity 是否继承 TenantEntity
- [ ] Service 是否禁止继承 ServiceImpl
- [ ] DAO 是否实现 buildQueryWrapper
- [ ] Controller 路径是否包含实体名
- [ ] 是否使用 MapstructUtils 转换对象

## 前端审查清单
- [ ] 是否使用 A* 封装组件
- [ ] API 调用是否使用 [err, data] 格式
- [ ] 是否存在 el-* 原生组件

## 移动端审查清单
- [ ] 是否使用 wd-* 组件
- [ ] useToast 是否从 @/wd 导入
- [ ] 样式单位是否使用 rpx
```

#### 6.2 project-manager：项目进度管理

**触发方式**：

-   手动调用 `@project-manager`
-   通过 `/update-status` 命令

**核心功能**：

-   维护 `docs/项目状态.md`
-   更新 `docs/待办清单.md`
-   统计开发进度

#### 6.3 代理的工作模式

```css
主对话                          代理
  │                              │
  │  "帮我审查刚写的代码"          │
  │  ─────────────────────────→  │
  │                              │ [code-reviewer 启动]
  │                              │ 读取变更文件
  │                              │ 逐项检查规范
  │                              │ 生成审查报告
  │  ←─────────────────────────  │
  │  收到审查报告                  │
  │                              │
  │  [继续主对话]                 │
```

**价值**：代理可以并行处理，不阻塞主对话。你可以一边让 AI 继续开发下一个功能，一边让代理审查上一个功能的代码。

___

### 七、协同架构：完整工作流

现在，让我们把所有组件串联起来，看看它们是如何协同工作的：

```bash
┌─────────────────────────────────────────────────────────────────┐
│                         .claude 目录                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐                                                │
│  │settings.json│ ← 核心配置：定义钩子、权限、MCP服务器            │
│  └──────┬──────┘                                                │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    hooks/ (钩子层)                       │   │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐     │   │
│  │  │session-start │ │skill-forced- │ │ pre-tool-use │     │   │
│  │  │    .js       │ │   eval.js    │ │     .js      │     │   │
│  │  │  项目状态     │ │ 技能激活率    │ │  安全防护    │     │   │
│  │  │  感知        │ │ 25%→90%+     │ │              │     │   │
│  │  └──────────────┘ └──────────────┘ └──────────────┘     │   │
│  │                          ┌──────────────┐               │   │
│  │                          │   stop.js    │               │   │
│  │                          │ 总结反馈     │               │   │
│  │                          └──────────────┘               │   │
│  └─────────────────────────────────────────────────────────┘   │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                   skills/ (技能层)                       │   │
│  │                                                         │   │
│  │   crud-development   api-development   database-ops     │   │
│  │   ui-pc              ui-mobile         payment-...      │   │
│  │   security-guard     performance-...   ...（共26个）     │   │
│  │                                                         │   │
│  └─────────────────────────────────────────────────────────┘   │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                  commands/ (命令层)                      │   │
│  │                                                         │   │
│  │   /dev (7步开发)   /crud (代码生成)   /check (规范检查)   │   │
│  │   /init-docs       /progress         /add-todo          │   │
│  │                                                         │   │
│  └─────────────────────────────────────────────────────────┘   │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                   agents/ (代理层)                       │   │
│  │                                                         │   │
│  │      code-reviewer (代码审查)    project-manager (项目管理)│   │
│  │                                                         │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### 一个实际场景的完整演示

**用户需求**：开发一个优惠券管理功能

```markdown
1. [SessionStart Hook] 会话启动
   → 显示：当前分支 master，3个未提交文件，2个高优先级待办

2. 用户输入：帮我开发一个优惠券管理功能

3. [UserPromptSubmit Hook] 强制技能评估
   → 评估结果：
     - crud-development: 是
     - database-ops: 是
     - ui-pc: 否（用户未要求）
   → 激活 Skill(crud-development)、Skill(database-ops)

4. AI 加载技能知识，开始工作
   → 参考 crud-development 的四层架构规范
   → 参考 database-ops 的建表规范

5. AI 执行 Bash 命令创建文件
   → [PreToolUse Hook] 检查命令安全性 → 通过

6. AI 完成代码生成

7. [Stop Hook] 任务完成
   → 播放提示音
   → 分析：新增 8 个 Java 文件，1 个 SQL 文件
   → 建议：使用 @code-reviewer 审查代码

8. 用户：@code-reviewer

9. [Agent] code-reviewer 启动
   → 审查所有新增文件
   → 返回报告：2 个警告，0 个错误

10. 用户：提交代码

11. AI 执行 git commit
    → [PreToolUse Hook] 检查 → 通过
    → 提交成功
```

___

### 八、效率量化

经过两个月的实际使用，我们统计了以下数据：

|     指标      |    优化前     |    优化后    |   提升   |
|-------------|------------|-----------|--------|
| **技能激活率**（开发场景） |    ~25%    |   90%+    | **+260%**  |
|  **CRUD 模块开发**  |   2-4 小时   |  5-10 分钟  | **\-95%**  |
|   **代码规范问题**    | 每模块 5-10 个 | 每模块 0-2 个 | **\-80%**  |
|   **危险操作事故**    |     偶发     |     0     | **\-100%** |
|   **新人上手周期**    |   1-2 周    |   2-3 天   | **\-75%**  |

#### 最显著的改变

1.  **AI 终于"懂"项目了**：不再生成不符合架构的代码
2.  **开发流程标准化**：每个人的代码风格趋于一致
3.  **安全兜底**：再也不用担心 AI 执行危险命令
4.  **知识沉淀**：项目规范不再口口相传，全部固化到 Skills

___

### 九、快速复用指南

如果你想在自己的项目中复用这套体系，以下是核心文件清单：

#### 必备文件（最小化配置）

```php
.claude/
├── settings.json          # 核心配置
├── hooks/
│   ├── skill-forced-eval.js   # 强制技能评估（最重要）
│   └── pre-tool-use.js        # 安全防护
├── skills/
│   └── your-skill/
│       └── SKILL.md           # 至少一个技能
└── CLAUDE.md                  # 项目规范（可选但推荐）
```

#### 3 步快速部署

**步骤 1**：创建 settings.json

```json
{
  "hooks": {
    "UserPromptSubmit": [{
      "matcher": "",
      "hooks": [{
        "type": "command",
        "command": "node .claude/hooks/skill-forced-eval.js"
      }]
    }]
  }
}
```

**步骤 2**：创建 skill-forced-eval.js

复制本文第二章的代码，修改技能列表为你自己的技能。

**步骤 3**：创建至少一个 SKILL.md

按照第四章的模板，编写你项目最核心的开发规范。

#### 扩展建议

-   **逐步迭代**：先从 1-2 个核心技能开始，根据实际使用情况逐步增加
-   **收集反馈**：记录 AI 的"错误行为"，转化为新的技能规范
-   **团队共建**：让每个开发者贡献自己领域的技能知识

___

### 十、总结

回到开篇的问题：**如何让 AI 从"通用助手"变成"项目专家"？**

答案是：**用制度约束能力，用知识填充认知**。

-   **Hooks 是纪律**：确保 AI 在每个关键节点执行正确的动作
-   **Skills 是知识**：把项目规范固化成 AI 可学习的文档
-   **Commands 是流程**：把最佳实践编排成可重复执行的步骤
-   **Agents 是分工**：让专业的子任务交给专业的代理处理

这套体系的核心洞察是：**AI 的能力需要"制度"来激发**。

就像一个聪明的新员工，如果没有明确的规章制度和岗位手册，他可能会按自己的理解乱来。但如果你给他一套完善的制度体系，他就能快速成长为团队的中坚力量。

AI 也是一样。

___

**最后**，附上我们项目的 .claude 目录结构，供参考：

```php
.claude/
├── settings.json              # 核心配置
├── CLAUDE.md                  # 项目总规范
├── hooks/                     # 4 个生命周期钩子
│   ├── session-start.js
│   ├── skill-forced-eval.js
│   ├── pre-tool-use.js
│   └── stop.js
├── skills/                    # 26 个专业技能
│   ├── crud-development/
│   ├── api-development/
│   ├── ui-pc/
│   ├── ui-mobile/
│   └── ...
├── commands/                  # 斜杠命令
│   ├── dev.md
│   ├── crud.md
│   └── check.md
├── agents/                    # 自定义代理
│   ├── code-reviewer.md
│   └── project-manager.md
└── templates/                 # 模板文件
    └── 项目状态模板.md
```

