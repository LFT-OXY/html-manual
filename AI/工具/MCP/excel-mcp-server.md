地址：https://github.com/haris-musa/excel-mcp-server



一款模型上下文协议 (MCP) 服务器，无需安装 Microsoft Excel 即可操作 Excel 文件。使用您的 AI 代理创建、读取和修改 Excel 工作簿。

## 特征

- 📊 **Excel 操作**：创建、读取、更新工作簿和工作表
- 📈**数据处理**：公式、格式设置、图表、数据透视表和 Excel 表格
- 🔍**数据验证**：内置数据范围、公式和数据完整性验证
- 🎨**格式设置**：字体样式、颜色、边框、对齐方式和条件格式
- 📋**表格操作**：创建和管理具有自定义样式的 Excel 表格
- 📊**图表创建**：生成各种图表类型（折线图、柱状图、饼图、散点图等）
- 🔄**数据透视表**：创建动态数据透视表进行数据分析
- 🔧**工作表管理**：轻松复制、重命名和删除工作表
- 🔌**三重传输支持**：stdio、SSE（已弃用）和流式 HTTP
- 🌐**远程和本地**：既可本地使用，也可作为远程服务使用



## 用法

服务器支持三种传输方式：

### 1. 工作室运输（供本地使用）

```
uvx excel-mcp-server stdio
```



```bash
{
   "mcpServers": {
      "excel": {
         "command": "uvx",
         "args": ["excel-mcp-server", "stdio"]
      }
   }
}
```



### 2. 可流式HTTP传输（推荐用于远程连接）

```
uvx excel-mcp-server streamable-http
```



**可流式HTTP传输连接**：

```plain
{
   "mcpServers": {
      "excel": {
         "url": "http://localhost:8000/mcp",
      }
   }
}
```

## 环境变量和文件路径处理

### SSE 和可流式 HTTP 传输

**使用SSE 或 Streamable HTTP 协议**运行服务器时，必须**在服务器端****设置**`**EXCEL_FILES_PATH**`环境变量。该变量指示服务器在哪里读取和写入 Excel 文件。

- 如果未设置，则默认值为`./excel_files`。

您还可以设置`FASTMCP_PORT`环境变量来控制服务器监听的端口（`8017`如果未设置，则默认为 0）。

- 示例（Windows PowerShell）：

```plain
$env:EXCEL_FILES_PATH="E:\MyExcelFiles"
$env:FASTMCP_PORT="8007"
uvx excel-mcp-server streamable-http
```

- 示例（Linux/macOS）：`EXCEL_FILES_PATH=/path/to/excel_files FASTMCP_PORT=8007 uvx excel-mcp-server streamable-http`

### 斯坦迪奥运输

使用**标准 I/O 协议**时，每次工具调用都会提供文件路径，因此无需**在**服务器端进行设置`EXCEL_FILES_PATH`。服务器将使用客户端发送的路径执行每次操作。