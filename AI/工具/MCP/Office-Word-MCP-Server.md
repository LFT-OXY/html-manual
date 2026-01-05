地址：https://github.com/GongRzhe/Office-Word-MCP-Server

用于创建、读取和操作 Microsoft Word 文档的模型上下文协议 (MCP) 服务器。该服务器使 AI 助手能够通过标准化接口处理 Word 文档，并提供丰富的文档编辑功能。



## 概述

Office-Word-MCP-Server 实现了[模型上下文协议 (MCP)](https://modelcontextprotocol.io/)，将 Word 文档操作作为工具和资源公开。它充当 AI 助手和 Microsoft Word 文档之间的桥梁，支持文档创建、内容添加、格式设置和分析。

该服务器采用模块化架构，将关注点分离为核心功能、工具和实用程序，使其具有高度可维护性和可扩展性，以便进行未来的增强。



## 安装

### 通过 Smithery 安装

[通过Smithery](https://smithery.ai/server/@GongRzhe/Office-Word-MCP-Server)自动为 Claude Desktop 安装 Office Word 文档服务器：

```
npx -y @smithery/cli install @GongRzhe/Office-Word-MCP-Server --client claude
```

### 先决条件

- Python 3.8 或更高版本
- pip 包管理器

### 基本安装

```plain
# Clone the repository
git clone https://github.com/GongRzhe/Office-Word-MCP-Server.git
cd Office-Word-MCP-Server

# Install dependencies
pip install -r requirements.txt
```

### 使用安装脚本

或者，您可以使用提供的安装脚本，该脚本可以处理以下操作：

- 检查先决条件
- 建立虚拟环境
- 安装依赖项
- 生成 MCP 配置



```
python setup_mcp.py
```

## 与 Claude for Desktop 配合使用

### 配置

#### 方法一：本地安装后

1. 安装完成后，将服务器添加到 Claude for Desktop 的配置文件中：

```plain
{
  "mcpServers": {
    "word-document-server": {
      "command": "python",
      "args": ["/path/to/word_mcp_server.py"]
    }
  }
}
```

#### 方法二：无需安装（使用 uvx）

1. 您还可以使用 uvx 包管理器配置 Claude for Desktop，使其无需本地安装即可使用服务器：

```plain
{
  "mcpServers": {
    "word-document-server": {
      "command": "uvx",
      "args": ["--from", "office-word-mcp-server", "word_mcp_server"]
    }
  }
}
```

1. 配置文件位置：

- macOS：`~/Library/Application Support/Claude/claude_desktop_config.json`
- 视窗：`%APPDATA%\Claude\claude_desktop_config.json`

1. 重启 Claude for Desktop 以加载配置。



### 示例操作

配置完成后，您可以让 Claude 执行以下操作：

- “创建一个名为‘report.docx’的新文档，并添加标题页。”
- “在我的文档中添加一个标题和三个段落”
- “请在文档顶部用36号粗体Helvetica字体添加我的名字。”
- “添加标题为‘摘要’的章节标题，字体为 Helvetica，14 号粗体，并带有底部边框。”
- “添加一段文字，字体为 Times New Roman，14 号字，蓝色斜体”
- 在包含“引言”的段落之后插入一个项目符号列表。
- “插入一个编号列表，包含以下项目：‘第一步’、‘第二步’、‘第三步’”
- 在“摘要”标题后添加项目符号
- “插入一个包含销售数据的 4x4 表格”
- 将第二段中的“重要”一词设置为粗体红色。
- “查找并替换所有‘旧术语’为‘新术语’”
- “为章节标题创建自定义样式”
- “对文档中的表格应用格式”
- “从我的文档中提取所有注释”
- “显示 John Doe 的所有评论”
- “获取第三段的评论”
- 将表格单元格 (1,2) 中的文本设置为粗体蓝色，字体大小为 14 磅。
- “在表头单元格四周添加 10 点的内边距”
- “创建一个带有蓝色对勾图标和白色文本的标注表”
- “将第一列的宽度设置为 50 磅，其余列自动调整宽度。”
- “使用交替的行颜色使表格更易于阅读”