# 为什么要用？

- 体验Claude Code 这款AI编程工具的强大功能
- GLM 4.6 编程能力非常不错，接近 Claude 4.0
- 实在是太便宜了
- 国内节点，速度快，完全不用魔法

# 套餐介绍

记住是去购买套餐，不是去买token,点击下方链接

[智谱AI开放平台](https://bigmodel.cn/claude-code?utm_source=bigmodel&utm_medium=link&utm_term=套餐概览页&utm_campaign=Platform_Ops&_channel_track_key=RYqdAnEv)

# Claude Code 搭配GLM 4.6

![img](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/1764123783484-c4259609-510a-4ed7-9bfe-6d23e3a0cb02.png)

如果你使用中出现上面这个问题，那你就按照我发的安装方法安装，就不会有这个问题了

- 先安装Claude Code ， Windows 建议安装WSL

```plain
# 进入命令行界面，安装 Claude Code
npm install -g @anthropic-ai/claude-code

# 创建您的工作目录，例如 `your-project`，使用 `cd` 命令导航到您的项目
cd your-project

# 安装完成，运行命令 `claude` 即可进入 Claude Code 交互界面
claude
```

### Windows CMD 

```plain
# 在 Cmd 中运行以下命令
# 注意替换里面的 `your_zhipu_api_key` 为您上一步获取到的 API Key
setx ANTHROPIC_AUTH_TOKEN your_zhipu_api_key
setx ANTHROPIC_BASE_URL https://open.bigmodel.cn/api/anthropic
```

### Windows powershell

```plain
# 在 PowerShell 中运行以下命令
# 注意替换里面的 `your_zhipu_api_key` 为您上一步获取到的 API Key
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_AUTH_TOKEN', 'your_zhipu_api_key', 'User')
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_BASE_URL', 'https://open.bigmodel.cn/api/anthropic', 'User')
```

### Mac Linux

```plain
vi ~/.claude/settings.json

{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "你自己的APIKEY",
    "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",
    "API_TIMEOUT_MS": "3000000",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "glm-4.6",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "glm-4.6",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "glm-4.5-air"
  }
}
```

![img](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/1764123783587-0b21e1e4-f5b1-4148-a7a1-64700b8b7f62.png)

推荐终端使用工具vscode, 将终端命令行拖到右边，方便查看代码

![img](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/1764123783682-56b1235e-b6f4-4569-9953-9250b92d3d85.png)

也可以使用Warp工具，非常好用的终端AI编程工具

![img](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/1764123783705-41a4ad07-2c2b-4638-854c-83e8422ba063.png)