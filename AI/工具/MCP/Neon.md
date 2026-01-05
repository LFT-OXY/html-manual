这是一个云端数据库，可以使用大模型通过Neon的mcp来操作Neon的云端数据库

官方文档： https://neon.com/docs/ai/connect-mcp-clients-to-neon



# claude code

安装

```
claude mcp add --transport http neon https://mcp.neon.tech/mcp
```



# codex

### windows

 `.codex/config.toml` 

```plain
[mcp_servers.Neon]
command = "cmd"
args = [
    "/c",
    "npx",
    "-y",
    "mcp-remote@latest",
    "https://mcp.neon.tech/mcp"
]
env = { SystemRoot="C:\\Windows", PROGRAMFILES="C:\\Program Files" }
startup_timeout_ms = 20_000
```



### mac

```plain
[mcp_servers.Neon]
command = "npx"
args = [
    "-y",
    "mcp-remote@latest",
    "https://mcp.neon.tech/mcp"
]
```