网页与APP设计工具，可以让团队在浏览器上合作设计，而不必依赖复杂笨重的本地软件

Figma里面有上百款，可以免费使用的UI设计模板

github项目： https://github.com/GLips/Figma-Context-MCP/blob/main/README.zh-cn.md

# 使用方式

选择你想要的页面模版，右键复制copy link，然后交给大模型



# claude code安装

```
claude mcp add Figma npx figma-developer-mcp --scope user
```

然后进入 `~/.claude.json` ,改成下面的参数

```plain
{
  "mcpServers": {
    "Framelink MCP for Figma": {
      "command": "npx",
      "args": ["-y", 
      "figma-developer-mcp", 
      "--figma-api-key=YOUR-KEY", 
      "--stdio"]
    }
  }
}
```

`"--figma-api-key=YOUR-KEY"` **是需要改为自己的key的**