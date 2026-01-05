地址：https://github.com/googleapis/genai-toolbox

# MCP数据库工具箱

[![img](https://camo.githubusercontent.com/e11e2dfa5831a43304a043ebaf6e842107e11fc9a98d0e0b54058207a90abeab/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f646f63732d4d43505f546f6f6c626f782d626c7565)](https://googleapis.github.io/genai-toolbox/) [![img](https://camo.githubusercontent.com/e8727eea5b2630f114f46079a69f6672ab2c166a9f950d6e51bf5db8d08cac93/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446973636f72642d2532333538363546322e7376673f7374796c653d666c6174266c6f676f3d646973636f7264266c6f676f436f6c6f723d7768697465)](https://discord.gg/Dmm69peqjh) [![img](https://camo.githubusercontent.com/475e8a8911200afe419a1b50abb2e29b08ba87d5d21c666f94eda8915b0785a8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4d656469756d2d3132313030453f7374796c653d666c6174266c6f676f3d6d656469756d266c6f676f436f6c6f723d7768697465)](https://medium.com/@mcp_toolbox) [![img](https://camo.githubusercontent.com/50fa33fe6486d45209e730e3f1ce90258c08e25f10135e6137cf3c202b0f47e0/68747470733a2f2f676f7265706f7274636172642e636f6d2f62616467652f6769746875622e636f6d2f676f6f676c65617069732f67656e61692d746f6f6c626f78)](https://goreportcard.com/report/github.com/googleapis/genai-toolbox)

**笔记**

MCP Toolbox for Databases 目前处于 beta 测试阶段，在第一个稳定版本 (v1.0) 发布之前可能会出现重大变更。

MCP Toolbox for Databases 是一个开源的 MCP 服务器，用于数据库。它能够处理连接池、身份验证等复杂问题，使您能够更轻松、更快速、更安全地开发工具。

本README文件提供简要概述。如需了解详细信息，请参阅[完整文档](https://googleapis.github.io/genai-toolbox/)。

**笔记**

该解决方案最初名为“数据库 Gen AI 工具箱”，因为它的初始开发早于 MCP，但为了与最近添加的 MCP 兼容性保持一致，它被重新命名。



```plain
"mysql": {
      "args": [
        "--prebuilt",
        "mysql",
        "--stdio"
      ],
      "command": "/opt/homebrew/Cellar/mcp-toolbox/0.19.1/bin/toolbox",
      "env": {
        "MYSQL_DATABASE": "project_manage",
        "MYSQL_HOST": "47.109.92.14",
        "MYSQL_PASSWORD": "Qiu#CHINHAE#",
        "MYSQL_PORT": "3306",
        "MYSQL_USER": "root"
      }
    },
```



### 安装服务器

如需最新版本，请查看[发布页面](https://github.com/googleapis/genai-toolbox/releases)，并根据您的操作系统和 CPU 架构使用以下说明。

二进制

要将 Toolbox 安装为二进制文件：

Linux (AMD64)

在 Linux (AMD64) 上以二进制文件形式安装 Toolbox：

```plain
# see releases page for other versions
export VERSION=0.21.0
curl -L -o toolbox https://storage.googleapis.com/genai-toolbox/v$VERSION/linux/amd64/toolbox
chmod +x toolbox
```

macOS（苹果芯片）

在 macOS（Apple Silicon）上以二进制文件形式安装 Toolbox：

```plain
# see releases page for other versions
export VERSION=0.21.0
curl -L -o toolbox https://storage.googleapis.com/genai-toolbox/v$VERSION/darwin/arm64/toolbox
chmod +x toolbox
```



自制啤酒

在 macOS 或 Linux 上使用 Homebrew 安装 Toolbox：

```
brew install mcp-toolbox
```



Gemini CLI 扩展

要为 MCP Toolbox 安装 Gemini CLI 扩展，请运行以下命令：

```
gemini extensions install https://github.com/gemini-cli-extensions/mcp-toolbox
```



### 

### 运行服务器

[配置](https://github.com/googleapis/genai-toolbox#configuration)a`tools.yaml`以定义您的工具，然后执行`toolbox`以启动服务器：

二进制

从二进制文件运行 Toolbox：

./toolbox --tools-file "tools.yaml"

ⓘ 注意：
工具箱默认启用动态重载。要禁用动态重载，请使用相应的 `--disable-reload`标志。

容器图像



自制啤酒

如果您使用[Homebrew](https://brew.sh/)安装了 Toolbox ，则其`toolbox` 二进制文件已添加到系统路径中。您可以使用相同的命令启动服务器：

toolbox --tools-file "tools.yaml"

Gemini CLI

使用自然语言与您的自定义工具进行交互。请查看 [gemini-cli-extensions/mcp-toolbox](https://github.com/gemini-cli-extensions/mcp-toolbox) 了解更多信息。

您可以使用`toolbox help`它来查看完整的标志列表！要停止服务器，请发送终止信号（`ctrl+c`在大多数平台上）。

[有关部署到不同环境的更详细文档，请查看“操作指南”部分](https://googleapis.github.io/genai-toolbox/how-to/)中的资源。



```yaml
"mysql": {
      "command": "/opt/homebrew/Cellar/mcp-toolbox/0.19.1/bin/toolbox",
      "args": ["--prebuilt", "mysql", "--stdio"],
      "env": {
        "MYSQL_HOST": "HOST",
        "MYSQL_PORT": "3306",
        "MYSQL_DATABASE": "project_manage",
        "MYSQL_USER": "root",
        "MYSQL_PASSWORD": "PASSWORD"
      }
    }
```