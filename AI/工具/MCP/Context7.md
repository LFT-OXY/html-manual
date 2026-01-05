永远保持最新同步全网最全的代码文档库

github项目： https://github.com/upstash/context7

官网：[https://context7.com](https://context7.com/)

目前主流的大语言模型（如 [OpenAI](https://openai.com/)，[Claude](https://claude.ai/)，[DeepSeek](https://deepseek.com/)）的训练数据往往滞后于技术的更新，导致生成的代码常常基于已经废弃的旧版 API。尤其是前端组件甚至每两周都更新一次，使用context7能够实时文档获取，减少AI幻觉，context7覆盖了所有主流库，并且完全免费！

## 使用教程

### 获取API Key

首先访问官网：[https://context7.com](https://context7.com/) 点击Sign In进行登录，如果没有注册过，那么先注册一下账号

点击 Click here to generate an API key 生成属于自己的专属API Key

点击复制按钮，需要保存好API key，因为只会出现一次，忘记后需要重新生成。

# claude code

```
claude mcp add -s user --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: ctx7sk-d3c46061-466d-4401-a6aa-c5fee650d4e3"
```