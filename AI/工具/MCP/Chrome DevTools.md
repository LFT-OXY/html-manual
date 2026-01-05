谷歌官方开发的mcp工具，可以通过ai大模型来操作谷歌浏览器

github地址： https://github.com/ChromeDevTools/chrome-devtools-mcp



# claude code安装

Use the Claude Code CLI to add the Chrome DevTools MCP server ([guide](https://docs.anthropic.com/en/docs/claude-code/mcp)):

```
claude mcp add chrome-devtools npx chrome-devtools-mcp@latest
```

目前这个指令只是针对当前项目有效，如果想要修改为用户级别

```
claude mcp add chrome-devtools npx chrome-devtools-mcp@latest --scope user
```



# codex安装

### Windows

Follow the [configure MCP guide](https://github.com/openai/codex/blob/main/docs/advanced.md#model-context-protocol-mcp) using the standard config from above. You can also install the Chrome DevTools MCP server using the Codex CLI:

```
codex mcp add chrome-devtools -- npx chrome-devtools-mcp@latest
```

**On Windows 11**

Configure the Chrome install location and increase the startup timeout by updating `.codex/config.toml` and adding the following `env` and `startup_timeout_ms` parameters:

```plain
[mcp_servers.chrome-devtools]
command = "cmd"
args = [
    "/c",
    "npx",
    "-y",
    "chrome-devtools-mcp@latest",
]
env = { SystemRoot="C:\\Windows", PROGRAMFILES="C:\\Program Files" }
startup_timeout_ms = 20_000
```



### mac

mac在 `~/.codex` 中的config文件最后添加

```plain
[mcp_servers.chrome-devtools]
command = "npx"
args = [
    "-y",
    "chrome-devtools-mcp@latest"
]
```