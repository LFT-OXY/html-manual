### 第一步：安装 SDKMAN!

在终端中执行以下命令：

```bash
curl -s "https://get.sdkman.io" | bash
```

安装完成后，**重新打开一个终端窗口**，或者执行：

```bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

验证安装是否成功：

```bash
sdk version
```

### 核心常用指令（你会每天用到的）

#### 1. 列出所有可安装的软件（候选版本）

查看 SDKMAN! 支持管理的所有软件包和它们的版本。

```bash
sdk list
```

#### 2. 安装软件

这是最核心的功能。基本语法是 `sdk install <软件名> [版本]`。

- **安装最新的稳定版**（最常用）：

```bash
sdk install java
sdk install maven
sdk install gradle
```

- **安装特定版本**（需要知道精确版本号）：

```bash
# 先使用 list 命令查找可用的版本
sdk list java

# 然后选择其中一个版本安装，例如 Temurin 的 JDK 17.0.9
sdk install java 17.0.9-tem
```

#### 3. 查看当前使用的版本 & 切换版本

这是 SDKMAN! 的杀手级功能。

- **查看当前所有软件的使用情况**：

```bash
sdk current
```

- **切换某个软件的版本**（仅在当前终端会话有效）：

```bash
sdk use java 11.0.21-tem
```

- **设置某个软件版本的默认值**（全局生效，新开的终端也会用这个版本）：

```bash
sdk default java 17.0.9-tem
```

#### 4. 查看某个软件的所有可用版本

在安装或切换前，先看看有什么版本。

```bash
sdk list java
```

这个命令会显示一个长长的列表，当前正在使用的版本前面会有 `>` 符号，已安装的版本前面有 `>` 符号。

#### 5. 卸载软件

移除某个软件的特定版本。

```bash
sdk uninstall java 8.0.392-tem
```

### 进阶实用指令（项目级环境管理）

这个功能非常强大，可以确保每个项目都使用正确的SDK版本。

#### 为项目创建版本配置文件

进入你的项目根目录，然后执行：

```bash
sdk env init
```

这会在当前目录创建一个 `.sdkmanrc` 文件。

#### 编辑配置文件

用文本编辑器打开 `.sdkmanrc` 文件，它会包含类似以下内容，你可以修改它：

```bash
# Enable auto-env through the sdkman_auto_env config
# Add key=value pairs of SDKs to use below
java=17.0.9-tem
maven=3.9.5
```

#### 在项目目录中自动启用配置

当你进入包含 `.sdkmanrc` 文件的目录时，SDKMAN! 可以自动切换到文件中指定的版本。

1. **首先，启用自动配置功能**：

```bash
sdk config
```

这会在默认编辑器中打开配置文件，确保以下设置是 `true`：

```bash
sdkman_auto_env=true
```

2. **之后，每当你 `cd` 到这个项目目录**，SDKMAN! 会自动切换到 `.sdkmanrc` 中定义的版本。
    
3. **当你离开项目目录**，它会自动切换回你全局默认的版本。
    

#### 手动为当前目录应用配置

如果你不想用自动模式，可以手动执行：

```bash
sdk env
```

### 其他有用指令

- **检查SDKMAN!自身更新**：

```bash
sdk selfupdate
```

- **离线模式**（当没有网络时）：

```bash
sdk offline enable  # 开启离线模式
sdk offline disable # 关闭离线模式
```

- **查看某个软件的描述信息**：

```bash
sdk home java 17.0.9-tem
```


### 实战流程示例

假设你要开始一个Spring Boot新项目，要求使用Java 17和Maven。

1. **检查并安装所需SDK**：

```bash
# 查看可用的 Java 17 版本
sdk list java | grep 17

# 安装一个（例如 Temurin）
sdk install java 17.0.9-tem

# 安装 Maven（最新版）
sdk install maven
```

2. **创建项目目录并进入**：

```bash
mkdir my-spring-project && cd my-spring-project
```

3. **为该项目创建环境配置**：

```bash
sdk env init
```

4. **编辑 `.sdkmanrc` 文件**，指定版本：

```bash
# 用VSCode编辑，或者用nano/vim
code .sdkmanrc
```

内容修改为：

```bash
java=17.0.9-tem
maven=3.9.5
```

5. **启用该配置**：

```bash
sdk env use
```

或者，如果你开启了 `sdkman_auto_env`，以后每次进入这个文件夹都会自动切换。    

现在，你在这个项目里执行 `java -version` 和 `mvn -version`，确保它们都是正确的版本。这样就能保证团队所有成员和CI/CD环境都使用完全一致的构建环境。