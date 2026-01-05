github 项目地址：https://github.com/oraios/serena



[Serena](https://lft-oxy.github.io/html-manual/serena-documentation.html)



# Connecting Your MCP Client 连接您的 MCP 客户端#

In the following, we provide default configurations for popular MCP-enabled clients.
以下为支持 MCP 功能的常用客户端提供的默认配置。

Depending on your needs, you might want to customize Serena’s behaviour by
根据您的需求，您可能需要通过以下方式自定义 Serena 的行为：

- [adding command-line arguments添加命令行参数](https://oraios.github.io/serena/02-usage/020_running.html#mcp-args)
- [adjusting configuration](https://oraios.github.io/serena/02-usage/050_configuration.html).
  [调整配置](https://oraios.github.io/serena/02-usage/050_configuration.html)。

## Claude Code  克劳德代码#

Serena is a great way to make Claude Code both cheaper and more powerful!
塞蕾娜是让克劳德代码既更便宜又更强大的绝佳方式！

From your project directory, add serena with a command like this,
在您的项目目录中，使用如下命令添加 serena：

```plain
claude mcp add serena -- <serena> start-mcp-server --context ide-assistant --project "$(pwd)"
```

where `<serena>` is [your way of running Serena](https://oraios.github.io/serena/02-usage/020_running.html).
其中 `<serena>` 是[您运行 Serena 的方式](https://oraios.github.io/serena/02-usage/020_running.html)。
For example, when using `uvx`, the above command becomes
例如，使用 `uvx` 时，上述命令变为

```plain
claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context ide-assistant --project "$(pwd)"
```

Note:  注：

- We use the `ide-assistant` context to disable unnecessary tools (avoiding duplication with Claude Code’s built-in capabilities).
  我们使用 `ide-assistant` 上下文来禁用不必要的工具（避免与 Claude Code 内置功能重复）。
- We specify the current directory as the project directory with `--project "$(pwd)"`, such that Serena is configured to work on the current project from the get-go, following Claude Code’s mode of operation.
  我们通过 `--project "$(pwd)"` 将当前目录指定为项目目录，使 Serena 从一开始就配置为处理当前项目，遵循 Claude Code 的运作模式。

Be sure to use at least `v1.0.52` of Claude Code (as earlier versions do not read MCP server system prompts upon startup).
请务必使用`至少 v1.0.52 版本`的 Claude Code（因早期版本在启动时无法读取 MCP 服务器的系统提示）。

## Codex  法典#

Serena works with OpenAI’s Codex CLI out of the box, but you have to use the `codex` context for it to work properly. (The technical reason is that Codex doesn’t fully support the MCP specifications, so some massaging of tools is required.).
Serena 开箱即用即可与 OpenAI 的 Codex 命令行界面配合工作，但必须使用 `codex` 上下文才能正常运行。（技术原因在于 Codex 尚未完全支持 MCP 规范，因此需要对工具进行适配处理。）

Add a [run command](https://oraios.github.io/serena/02-usage/020_running.html) to `~/.codex/config.toml` to configure Serena for all Codex sessions; create the file if it does not exist. For example, when using `uvx`, add the following section:
在 `~/.codex/config.toml` [中添加运行命令以](https://oraios.github.io/serena/02-usage/020_running.html)配置所有 Codex 会话的 Serena；若文件不存在则创建。例如使用 `uvx` 时，添加以下配置段：

```plain
[mcp_servers.serena]
command = "uvx"
args = ["--from", "git+https://github.com/oraios/serena", "serena", "start-mcp-server", "--context", "codex"]
```

After codex has started, you need to activate the project, which you can do by saying:
代码库启动后，您需要激活项目，操作方式如下：

“Activate the current dir as project using serena”
使用 Serena 将当前目录设为项目

If you don’t activate the project, you will not be able to use Serena’s tools!
若不激活该项目，您将无法使用 Serena 的工具！

That’s it! Have a look at `~/.codex/log/codex-tui.log` to see if any errors occurred.
就这样！查看 `~/.codex/log/codex-tui.log 文件 `，看看是否出现任何错误。

Serena’s dashboard will run if you have not disabled it in the configuration, but due to Codex’s sandboxing, the web browser may not open automatically. You can open it manually by going to `http://localhost:24282/dashboard/index.html` (or a higher port, if that was already taken).
若您未在配置中禁用该功能，Serena 仪表板将正常运行。但受 Codex 沙盒机制限制，网页浏览器可能不会自动打开。您可手动访问 `http://localhost:24282/dashboard/index.html` （若该端口已被占用，请使用更高端口）进行操作。

Codex will often show the tools as `failed` even though they are successfully executed. This is not a problem, seems to be a bug in Codex. Despite the error message, everything works as expected.
Codex 常会将工具显示为`失败状态 `，即使它们已成功执行。这并非问题，似乎是 Codex 的错误。尽管出现错误提示，所有功能仍可正常运行。

## Claude Desktop  克劳德桌面#

On Windows and macOS there are official [Claude Desktop applications by Anthropic](https://claude.ai/download), for Linux there is an [open-source community version](https://github.com/aaddrick/claude-desktop-debian).
在 Windows 和 macOS[平台上，Anthropic](https://claude.ai/download) 提供了官方的 [Claude 桌面应用程序](https://claude.ai/download)；而 Linux 平台则有[开源社区版本](https://github.com/aaddrick/claude-desktop-debian)。

To configure MCP server settings, go to File / Settings / Developer / MCP Servers / Edit Config, which will let you open the JSON file `claude_desktop_config.json`.
要配置 MCP 服务器设置，请前往文件/设置/开发者/MCP 服务器/编辑配置，这将允许您打开 JSON 文件 `claude_desktop_config.json`。

Add the `serena` MCP server configuration, using a [run command](https://oraios.github.io/serena/02-usage/020_running.html) depending on your setup.
添加 `Serena`MCP 服务器配置，根据您的设置使用相应的[运行命令](https://oraios.github.io/serena/02-usage/020_running.html)。

- local installation:  本地安装：

```plain
{
    "mcpServers": {
        "serena": {
            "command": "/abs/path/to/uv",
            "args": ["run", "--directory", "/abs/path/to/serena", "serena", "start-mcp-server"]
        }
    }
}
```

- uvx:

```plain
{
    "mcpServers": {
        "serena": {
            "command": "/abs/path/to/uvx",
            "args": ["--from", "git+https://github.com/oraios/serena", "serena", "start-mcp-server"]
        }
    }
}
```

- docker:

```plain
{
     "mcpServers": {
         "serena": {
             "command": "docker",
             "args": ["run", "--rm", "-i", "--network", "host", "-v", "/path/to/your/projects:/workspaces/projects", "ghcr.io/oraios/serena:latest", "serena", "start-mcp-server", "--transport", "stdio"]
         }
     }
 }
```

If you are using paths containing backslashes for paths on Windows (note that you can also just use forward slashes), be sure to escape them correctly (`\\`).
若在 Windows 系统中使用包含反斜杠的路径（请注意也可直接使用正斜杠），请确保正确转义反斜杠（`\）`。

That’s it! Save the config and then restart Claude Desktop. You are ready for activating your first project.
就这样！保存配置后重启 Claude Desktop。您已准备好激活首个项目。

**Attention**  **注意**

Be sure to fully quit the Claude Desktop application via File / Exit, as regularly closing the application will just minimize it.
请务必通过“文件/退出”菜单完全关闭 Claude 桌面应用程序，因为常规关闭操作只会将其最小化。

After restarting, you should see Serena’s tools in your chat interface (notice the small hammer icon).
重启后，您应能在聊天界面看到 Serena 的工具（注意小锤子图标）。

For more information on MCP servers with Claude Desktop, see [the official quick start guide](https://modelcontextprotocol.io/quickstart/user).
有关搭载 Claude Desktop 的 MCP 服务器的更多信息，请参阅[官方快速入门指南](https://modelcontextprotocol.io/quickstart/user)。

## Other Clients  其他客户#

In general, Serena can be used with any MCP-enabled client. To connect Serena to your favourite client, simply
通常，Serena 可与任何支持 MCP 的客户端配合使用。要将 Serena 连接到您喜欢的客户端，只需

1. determine how to add a custom MCP server to your client (refer to the client’s documentation).
   确定如何将自定义 MCP 服务器添加到客户端（请参阅客户端文档）。
2. add a new MCP server entry by specifying either
   通过指定以下任一方式添加新的 MCP 服务器条目：

- a [run command](https://oraios.github.io/serena/02-usage/020_running.html#start-mcp-server) that allows the client to start the MCP server in stdio mode as a subprocess, or
  一个[运行命令，](https://oraios.github.io/serena/02-usage/020_running.html#start-mcp-server) 允许客户端以子进程形式在标准输入输出模式下启动 MCP 服务器，或
- the URL of the HTTP/SSE endpoint, having started the [Serena MCP server in HTTP/SSE mode](https://oraios.github.io/serena/02-usage/020_running.html#streamable-http) beforehand.
  HTTP/SSE 端点的 URL，[需事先以 HTTP/SSE 模式](https://oraios.github.io/serena/02-usage/020_running.html#streamable-http)启动 [Serena MCP 服务器](https://oraios.github.io/serena/02-usage/020_running.html#streamable-http)。

Below, we list some popular categories of clients that Serena can be used with.
下面，我们列出一些可与 Serena 配合使用的常见客户类别。

### Terminal-Based Clients  基于终端的客户端#

There are many terminal-based coding assistants that support MCP servers, such as
有许多基于终端的编码助手支持 MCP 服务器，例如：

- [Gemini-CLI](https://github.com/google-gemini/gemini-cli),
  [Gemini](https://github.com/google-gemini/gemini-cli)-[CLI](https://github.com/google-gemini/gemini-cli)，
- [Qwen3-Coder](https://github.com/QwenLM/Qwen3-Coder),
  [Qwen3 编码器，](https://github.com/QwenLM/Qwen3-Coder)
- [rovodev](https://community.atlassian.com/forums/Rovo-for-Software-Teams-Beta/Introducing-Rovo-Dev-CLI-AI-Powered-Development-in-your-terminal/ba-p/3043623),
  [报告](https://community.atlassian.com/forums/Rovo-for-Software-Teams-Beta/Introducing-Rovo-Dev-CLI-AI-Powered-Development-in-your-terminal/ba-p/3043623)，
- [OpenHands CLI](https://docs.all-hands.dev/usage/how-to/cli-mode) and
  [OpenHands 命令行界面](https://docs.all-hands.dev/usage/how-to/cli-mode)和
- [opencode](https://github.com/sst/opencode).

They generally benefit from the symbolic tools provided by Serena. You might want to customize some aspects of Serena by writing your own context, modes or prompts to adjust it to the client’s respective internal capabilities (and your general workflow).
他们通常受益于 Serena 提供的符号化工具。您可能需要通过编写自定义上下文、模式或提示词来调整 Serena 的部分功能，使其适应客户各自的内部能力（以及您的常规工作流程）。

### MCP-Enabled IDEs and Coding Clients (Cline, Roo-Code, Cursor, Windsurf, etc.) 支持 MCP 的集成开发环境与编码客户端（Cline、Roo-Code、Cursor、Windsurf 等）#

Most of the popular existing coding assistants (e.g. IDE extensions) and AI-enabled IDEs themselves support connections to MCP Servers. Serena generally boosts performance by providing efficient tools for symbolic operations.
现有的主流编码助手（如 IDE 扩展）以及具备 AI 功能的 IDE 本身大多支持连接 MCP 服务器。Serena 通常通过提供高效的符号运算工具来提升性能。

We generally **recommend to use the** `**ide-assistant**` **context** for these integrations by adding the arguments `--context ide-assistant` in order to reduce tool duplication.
我们通常**建议**通过添加参数 `--context ide-assistant` **来使用** `**ide-assistant**` **上下文**进行这些集成。 为减少工具重复。