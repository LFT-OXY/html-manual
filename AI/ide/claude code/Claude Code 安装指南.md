## 简介

Claude Code 是 Anthropic 官方推出的 AI 编程助手命令行工具，可以在终端中与 Claude 对话，进行代码编写、项目管理、Git 操作等。它基于 Node.js 开发，支持多平台运行。

## 系统要求

- **操作系统**: macOS 10.14+ 或 Windows 10/11
- **Node.js**: 版本 18.0.0 或更高
- **内存**: 至少 4GB RAM
- **网络**: 稳定的互联网连接

## macOS 安装教程

### 安装 Node.js

#### 方法一：官方安装包（推荐新手）

1. 访问 [Node.js — 在任何地方运行 JavaScript](https://nodejs.org/)
2. 下载 macOS 版本的 LTS 安装包 (.pkg 文件)
3. 运行安装包，按照向导完成安装
4. 打开终端验证安装：

```plain
node --version
npm --version
```

#### 方法二：使用 Homebrew

1. 如果您已安装 Homebrew，在终端中执行：

```plain
brew install node
```

1. 验证安装：

```plain
node --version
npm --version
```

### 安装 Claude Code

在终端中执行以下命令进行全局安装：

```plain
npm install -g @anthropic-ai/claude-code
```

### 配置环境变量

根据您使用的 shell 类型配置环境变量：

#### 对于 zsh 用户（macOS Catalina 及以后版本默认）

```plain
echo 'export ANTHROPIC_BASE_URL="https://api.apiqik.com"' >> ~/.zshrc
echo 'export ANTHROPIC_AUTH_TOKEN="sk-your-token-here"' >> ~/.zshrc
source ~/.zshrc
```

#### 对于 bash 用户（较老的 macOS 版本）

```plain
echo 'export ANTHROPIC_BASE_URL="https://api.apiqik.com"' >> ~/.bash_profile
echo 'export ANTHROPIC_AUTH_TOKEN="sk-your-token-here"' >> ~/.bash_profile
source ~/.bash_profile
```

请将 `sk-your-token-here` 替换为您实际的 API 密钥。

## Windows 安装教程

### 方案一：通过 WSL 安装（推荐）

#### 步骤一：启用 WSL 功能

1. 以管理员身份打开 PowerShell
2. 执行以下命令：

```plain
wsl --install
```

1. 重启计算机

#### 步骤二：安装 Linux 发行版

重启后，系统会自动安装 Ubuntu。您也可以手动选择其他发行版：

```plain
wsl --install -d Ubuntu-24.04
```

#### 步骤三：安装 Node.js

在 WSL 环境中执行：

```plain
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
node --version  # 验证安装
```

#### 步骤四：安装 Claude Code

在 WSL 环境中执行：

```plain
npm install -g @anthropic-ai/claude-code
```

#### 步骤五：配置环境变量

在 WSL 环境中配置：

```plain
echo 'export ANTHROPIC_BASE_URL="https://api.anyrouter.top"' >> ~/.bashrc
echo 'export ANTHROPIC_AUTH_TOKEN="sk-your-token-here"' >> ~/.bashrc
source ~/.bashrc
```

### 方案二：原生 Windows 安装

#### 步骤一：安装 Node.js

1. 访问 [Node.js — 在任何地方运行 JavaScript](https://nodejs.org/)
2. 下载 Windows 版本的安装包
3. 运行安装包，按照向导完成安装

#### 步骤二：安装 Claude Code

以管理员身份打开 PowerShell 或命令提示符：

```plain
npm install -g @anthropic-ai/claude-code
```

#### 步骤三：配置环境变量

1. 右键点击"此电脑" → "属性" → "高级系统设置"
2. 点击"环境变量"按钮
3. 在"系统变量"部分，点击"新建"
4. 添加以下变量：

1. 变量名：`ANTHROPIC_BASE_URL`
2. 变量值：`https://api.anyrouter.top/v1/claude`

1. 再次点击"新建"添加第二个变量：

1. 变量名：`ANTHROPIC_AUTH_TOKEN`
2. 变量值：`sk-your-api-key-here`（替换为实际API密钥）

## 获取 API 密钥

1. 访问 https://anyrouter.top/ 注册账号
2. 登录后进入 "API 令牌" 页面
3. 点击 "添加令牌" 按钮
4. 复制生成的 API Key（以 sk- 开头）
5. 将密钥用于环境变量配置中

## 验证安装

安装完成后，在两个系统上都可以通过以下命令验证：

```plain
claude --version
```

如果安装成功，将显示 Claude Code 的版本号。

## 基本使用

1. 打开终端（macOS）或 WSL/PowerShell（Windows）
2. 导航到您的项目目录：

```plain
cd /path/to/your/project
```

1. 启动 Claude Code：

```plain
claude
```

1. 按照提示开始与 Claude 交互

常用命令：

- `claude help` - 查看帮助信息
- `claude "你的问题"` - 直接提问
- `Ctrl+D` - 退出 Claude Code

## 常见问题解决

### 网络连接问题

如果您在中国大陆地区，可能需要使用网络代理或镜像源：

```plain
# 使用 npm 镜像源
npm config set registry https://registry.npmmirror.com

# 或使用代理
npm config set proxy http://proxy.company.com:8080
```

### 权限问题

如果在安装过程中遇到权限错误：

**macOS/Linux:**

```plain
sudo npm install -g @anthropic-ai/claude-code
```

**Windows:**

以管理员身份运行 PowerShell 或命令提示符

### 命令找不到错误

如果提示 `command not found: claude`，可能是 npm 全局路径未配置：

```plain
# 检查 npm 全局安装路径
npm config get prefix

# 将该路径添加到 PATH 环境变量中
# 例如，如果输出是 /usr/local，则添加：
export PATH="/usr/local/bin:$PATH"
```

### API 连接失败

1. 确保环境变量配置正确
2. 验证 API 密钥是否有效
3. 检查网络连接是否正常

## 卸载指南

### macOS 卸载

```plain
npm uninstall -g @anthropic-ai/claude-code
# 同时删除环境变量
```

### Windows 卸载

```plain
npm uninstall -g @anthropic-ai/claude-code
# 同时删除系统环境变量
```

## 资源链接

- [Node.js — 在任何地方运行 JavaScript](https://nodejs.org/)
- https://github.com/anthropics/anthropic-codel
- https://anyrouter.top/
- [Windows Subsystem for Linux 文档](https://docs.microsoft.com/zh-cn/windows/wsl/)