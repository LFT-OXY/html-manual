#### 安装插件

####  1、安装插件-高亮插件highlighting

```java
cd ~/.oh-my-zsh/custom/plugins/
 
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
vi ~/.zshrc
```

这时我们再次打开zshrc文件进行编辑。找到plugins，此时plugins中应该已经有了git，我们需要把高亮插件也加上：

![|375](https://img-blog.csdnimg.cn/cd5f281d59414a059b0aee6a2aae8e26.png)

请务必保证插件顺序，zsh-syntax-highlighting必须在最后一个。

然后在文件的最后一行添加：
```java
source ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

也可以用一下脚本自动添加到末尾:
```java
echo "source $(brew --prefix)/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```

执行：
`source ~/.zshrc`

#### 2、安装插件-命令补全zsh-autosuggestion，用于命令建议和补全

```java
cd ~/.oh-my-zsh/custom/plugins/
 
git clone https://github.com/zsh-users/zsh-autosuggestions
vi ~/.zshrc
```

找到plugins，加上这个插件即可：

![|500](https://img-blog.csdnimg.cn/8020be5c2ba647f694be89461af60ed5.png)



![|325](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38e4c074d4d~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

怎么感觉不太对，虽然你的背景变黑了，但依然掩盖不了你的丑啊。

没事儿，先天不足，后天努力嘛。

告别黑底白字，整出最骚终端，开始吧。

# 一. on my zsh

![|337](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38e4bb59dc0~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

主角是它，拥有了它，你一定是你们组最靓的仔。

> Oh My Zsh is an open source, community-driven framework for managing your zsh configuration.

### 安装

官网提供了两种安装方式：

  ```java
# via curl sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" 
# via wget sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

如果，由于一些原因，上面两种方法你都没能安装成功，可以试一下手动安装：
```java
# 下载 oh-my-zsh 源码 
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh 

# 并且把 .zshrc 配置文件拷贝到根目录下 
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc 

# 让 .zshrc 配置文件生效 
source ~/.zshrc
```

嗯... 你和我说，`clone` 也不行啊，不可描述的原因，网速不允许啊。

那你这样做。

在 [👉oh-my-zsh GitHub](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fohmyzsh%2Fohmyzsh "https://github.com/ohmyzsh/ohmyzsh") 上下载 `zip` -> 解压 -> 移动 oh-my-zsh 目录到根目录：
```java
cd ~/Downloads 
mv ohmyzsh-master ~/.oh-my-zsh 
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc 
source ~/.zshrc
```

如果还不行，你来找我。

好了，重新启动 `iTerm2`，是不是已经变了。

### .zshrc

这个文件非常关键，是 `oh-my-zsh` 的配置文件，它的位置在根目录下，可以通过 `vim ~/.zshrc` 查看。

每一次修改它之后，如果想要立即生效需要手动执行 `source ~/.zshrc`。

### 修改配色方案

一打开 `.zshrc`，就可以看到关于配色方案的配置：
```java
# Set name of the theme to load --- if set to "random", it will 
# load a random theme each time oh-my-zsh is loaded, in which case, 
# to know which specific one was loaded, run: echo $RANDOM_THEME 
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes ZSH_THEME="agnoster"
```

`oh-my-zsh` 提供了很多内置的配色方案，可以通过命令来查看：

`ls ~/.oh-my-zsh/themes`

也可以打开 [👉https://github.com/ohmyzsh/ohmyzsh/wiki/Themes](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fohmyzsh%2Fohmyzsh%2Fwiki%2FThemes "https://github.com/ohmyzsh/ohmyzsh/wiki/Themes") 更为直观的查看所有的配色方案。

只要修改 `ZSH_THEME` 的值就可以设置对应的配色方案了。

如果你想每天都过得不一样，可以设置成 `random`，每次打开 `iTerm2` 的都会随机使用一种配色方案。

我曾经有一段时间，由于不想折腾，使用的是这个配色方案：`agnoster`，它是这样的：

  
![|400](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38e81512de6~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

当然，有一天，我突然想造作一下，就开始自己配色。（没备份... 找不着了...）

如果你觉得默认的配色方案不够骚，并且觉得自己的审美 ok，也可以自己来搭配颜色。

### 自定义配色方案

入口：菜单栏 -> Profiles -> Open Profiles -> Edit Profiles -> + 一个配置 -> 选择 Colors

![|500](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38e9c55e312~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

像我这样审美不行的人，花了一整天的时间搞这个，到头来发现，还是默认的更好看一点...

⚠️ 别摸一下午鱼搞这个被老板发现，还是下班了再搞吧。

### 第三方配色方案

当然，不是只有你和我想要自己搞一套最骚的配色方案，大家都有这样的想法。

[👉iTerm2-Color-Schemes](https://link.juejin.cn?target=https%3A%2F%2Fiterm2colorschemes.com%2F "https://iterm2colorschemes.com/") 这里有非常多的配色方案题，也已经在 [👉GitHub](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fmbadolato%2FiTerm2-Color-Schemes "https://github.com/mbadolato/iTerm2-Color-Schemes") 上开源。

你可以像我一样这样做：

```java
# 找一个目录存放 iterm2 相关的文件 
mkdir Code/other/iterm2 

# 下载 iTerm2-Color-Schemes 
git clone https://github.com/mbadolato/iTerm2-Color-Schemes 

# schemes 文件夹就是真实存放配色方案的目录 
cd iTerm2-Color-Schemes/schemes
```

同样，如果 `clone` 不下来就下载 `zip` 解压就好了。

通过以下操作路径可以导入所有配色方案：

菜单栏 -> Profiles -> Open Profiles -> Edit Profiles -> 选择 Colors -> 右下角 Color Presets -> Import...

找到 `schemes` 文件夹选中所有配色方案就好了，然后你就 ~~眼花缭乱~~ 会收获满满的幸福。

没事，等等会有更高级的方案。

### 安装字体 PowerFonts

为什么要安装字体呢？有些主题是会设置图标的，我们电脑上的字体一般都不支持这些图标，会出现乱码。

![|475](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38e926f293e~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

打开 [👉Fonts](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fpowerline%2Ffonts "https://github.com/powerline/fonts") 下载 `zip` 包都本地解压，就会得到很多字体。

```
# 将下载好的 fonts 移动到之前建的目录 
mv ~/Downlaods/fonts-master ~/Code/other/iterm2/fonts 
cd ~/Code/other/iterm2/fonts 
# 执行安装文件 
./install.sh
```

![|415](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38ea1ca82aa~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

这样就安装好了，然后通过以下操作路径设置字体：

菜单栏 -> Profiles -> Open Profiles -> Edit Profiles -> 选择 Text

![|750](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38e9270bc86~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

可以选择 `Meslo` 这个字体，乱码的图标就正常了。

### 毛玻璃效果/窗口大小

如果想要更高逼格的毛玻璃效果，并且找到自己舒服的大小（？？？），可以在这里设置：

![|825](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38e8e560d08~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

操作路径：菜单栏 -> Profiles -> Open Profiles -> Edit Profiles -> 选择 Window

### 自定义背景

激动人心的时刻，你可以为你的终端设置一个自己喜欢的 ~~小姐姐~~ 图片作为背景，敲命令的时候都会更带劲吧：

![|825](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1727006a618d65bb~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

~~咳咳，Dota 云玩家们，你是更喜欢冰女还是火女？~~

操作路径：菜单栏 -> Profiles -> Open Profiles -> Edit Profiles -> 选择 Window

![|700](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38ed62ae7e7~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

### 状态栏

可以为每个打开的终端都设置一个状态栏，显示一些系统信息（比如 CPU、RAM、当前目录等）。

操作路径：菜单栏 -> Profiles -> Open Profiles -> Edit Profiles -> 选择 Session

![|650](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38ed8591b66~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

### 总结

经过这一番折腾，一个属于你自己的高颜值终端就诞生了。

不过，总感觉这样还是有点麻烦，有没有更厉害的玩意儿？有的，我们这就用起来。

## 神器 Powerlevel10k

[👉Powerlevel10k](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fromkatv%2Fpowerlevel10k "https://github.com/romkatv/powerlevel10k") 简单来说就是一个 `ZSH` 的主题，只不过它的功能很强大，以下简称 `p10k`。

![|650](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38ed3e607ea~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

### 安装 P10k

我们用的是 `Oh My Zsh`，所以这样安装 `p10k` 即可：

```java
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
```

然后需要打开 `~/.zshrc` 设置 `ZSH_THEME`:
```java
ZSH_THEME="powerlevel10k/powerlevel10k
```

### 安装字体 Nerd Fonts

上文我们已经安装了 `PowerFonts`，如果需要使用一些图标，这个字体是不够用的，我们需要一个强大的字体：[👉Nerd Fonts](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fryanoasis%2Fnerd-fonts "https://github.com/ryanoasis/nerd-fonts")，它支持下面这么多种图标：

![|650](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38edb57e111~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

#### 安装字体

你可以如官网所说，通过 `brew` 来安装：
```java
brew tap homebrew/cask-fonts 
brew cask install font-hack-nerd-font
```

但是我不建议这样，包括不建议你下载 `zip` 包，因为这个文件太大了，太大了，太大了。。。

我们可以这样：

打开 [👉https://github.com/ryanoasis/nerd-fonts/releases](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fryanoasis%2Fnerd-fonts%2Freleases "https://github.com/ryanoasis/nerd-fonts/releases")，滑动页面找到 `Assets` 区域，如图：

![|500](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38f049950df~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

我们只要下载箭头所指的 `Hack.zip` 这个字体包，解压缩之后就会获得一些 `ttf` 字体文件，双击安装即可。

#### zshrc 设置字体

```java
POWERLEVEL9K_MODE="nerdfont-complete" ZSH_THEME="powerlevel10k/powerlevel10k"
```

注意，需要设置在 `ZSH_THEME` 之前。

#### iTerm2 设置字体

操作路径：菜单栏 -> Profiles -> Open Profiles -> Edit Profiles -> 选择 Text

![|775](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38f0a297b08~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

这样，所有的图标就都可以正常显示了。

### 自动配置

如果你指定了 `ZSH_THEME="powerlevel10k/powerlevel10k"` 但是在 `zshrc` 里没进行任何手动的配置，打开 `iTerm2` 的时候就会触发自动配置的流程。

也可以通过以下命令再次进入自动配置的流程：

```
p10k configure
```

![|950](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38f0b954299~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

问题大致如下：

1. 这个符号看起来像钻石（旋转的正方形）吗？
2. 这个符号看起来像锁吗？
3. 这个符号看起来像 Debian logo 吗？
4. 这些图标都交叉分布在 X 之间吗？
5. 风格
6. 编码
7. 是否显示时间
8. 目录层级分隔符
9. 头部（左边）
10. 尾部（右边）
11. 是否换行
12. 左边和右边是否有连接线
13. 命令行和提示是否连接
14. 两行命令之间分布稀疏还是松散
15. 是否需要图标

后面几个选项随意，执行完命令之后，就会初始化 `p10k`：在根目录下生成 `~/.p10k.zsh`，并且在 `~/.zshrc` 底部写入：

![|675](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38f222c259e~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

如果想废除 `p10k` 的配置，只需要删除 `~/.p10k.zsh`，并且删除上面这条命令即可。

### 自定义配置

如果你想当高玩，也可以在 `~/.zshrc` 里手动配置 `p10k`，或者在 `~/.p10k.zsh` 基础上进行修改。

这个得要自己看文档摸索啦，这里我简单说几个配置：

- POWERLEVEL9K_LEFT_PROMPT_ELEMENTS
- POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS
- POWERLEVEL9K_VCS_GIT_GITHUB_ICON

**POWERLEVEL9K_LEFT_PROMPT_ELEMENTS**

显示在命令行左边区域的元素：

![|709](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1726f38f28292856~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

和上图相对应的配置为：

复制代码

`POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(user dir vcs newline)`

**POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS**

显示在命令行右边区域的元素：

![|800](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/172700254728bca0~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

和上图相对应的配置为：


复制代码

`POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(time)`

可以在 `POWERLEVEL9K_LEFT_PROMPT_ELEMENTS` 和 `POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS` 里用的字段有：

|字段|含义|
|---|---|
|user|用户名|
|dir|当前目录名|
|vcs|远程仓库信息|
|os_icon|系统图标|
|date|日期|
|host|主机名|
|status|上一条命令的执行状态|
|time|当前时间|
|...|...|

如果还想了解更多，自行前往[👉文档](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fromkatv%2Fpowerlevel10k%23batteries-included "https://github.com/romkatv/powerlevel10k#batteries-included")查看。

**POWERLEVEL9K_VCS_GIT_GITHUB_ICON**

如果它是一个 `Github` 目录，就会显示这个图标：

![|317](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1727002547966d58~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

所以出现在窗口里的图标都可以自定义，可以通过命令查看目前正在使用的图标：

复制代码

`get_icon_names`

![|500](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1727002547767d90~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

找到想要修改的 `KEY` 就可以修改图标了。

> 注意：需要使用 `Nerd Fonts` 才能收货这满满的快乐。

有人问，这个图标的代码该去哪找呢？

在这里：[👉https://www.nerdfonts.com/cheat-sheet](https://link.juejin.cn?target=https%3A%2F%2Fwww.nerdfonts.com%2Fcheat-sheet "https://www.nerdfonts.com/cheat-sheet")

这是 `Nerd Fonts` 能够支持的所有图标，可以直接使用关键字进行搜索。

比如，我想修改 `Git` 的图标：

![|725](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/172700254d522d05~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

找到喜欢的图标之后，右下角的 `f113` 就是这个图标的值，只需要这样就了：

复制代码

`POWERLEVEL9K_VCS_GIT_GITHUB_ICON=$'\uf113'`

快造作起来~

## 插件

到了这一步，你的 `iTerm2` 应该已经颜值爆表，足够好看了。

毕竟这是我们的饭碗，光好看不行，得好用，来了解一下强大的插件体系。

首先，我们先了解一下插件在 `~/.zshrc` 的哪个位置，找到下面这个字段就不会错了：

复制代码

`plugins=(git)`

### git

`git` 插件是自带插件，默认已经开启，它可以让我们使用非常~~骚~~好用的的 `git` 命令，提高开发效率：

|用了插件之前的 git 命令|用了插件之后的 git 命令|
|---|---|
|git add --all|gaa|
|git branch -D|gbD|
|git commit -a -m|gcam|
|git checkout -b|gcb|
|git checkout master|gcm|

是不是简单多了。可以通过命令查看所有配置：

复制代码

`vim ~/.oh-my-zsh/plugins/git/git.plugin.zsh`

### 自动跳转对应目录

如果你像我一样是一个整理狂魔，会把文件、目录一层一层的整理好。

> 整理一时爽，用时就不爽

目录层级深了，年龄大了，就找不到文件放哪了，`cd` 起来也不方便了，有什么办法可以解决呢？教你两招。

#### 设置别名 alias

打开 `~/.zshrc` 输入别名，比如：

复制代码

`alias articles='~/Code/GitHub/articles'`

然后执行 `articles` 就会自动跳到 `~/Code/GitHub/articles` 了。

这样还是比较麻烦的，得为每个目录都配置 `alias`。

#### autojump 插件

`autojump` 插件会记录你所有的访问记录，不同单独配置，直接访问即可。

##### 安装

复制代码

`brew install autojump`

##### 配置

打开 `~/.zshrc` 加一行代码：

复制代码

`[[ -s $(brew --prefix)/etc/profile.d/autojump.sh ]] && . $(brew --prefix)/etc/profile.d/autojump.sh`

然后就是 `source` 一下就生效了。

##### 使用

使用 `j` 命令就可以执行 `auto-jump`，比如 `j articles`：

![|500](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/1727002559305cd1~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

前提是你访问过 `articles` 目录，也就是你得让它记住。

### zsh-autosuggestions

这个插件的作用很简单，就是像它名字一样，会在你输入命令的时候提示并且自动完成：

![|500](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/172700255ed582c4~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

复制代码

`brew install zsh-autosuggestions`

### colors

这是一个文件目录美化插件，如图所示：

![|500](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/172700257e52ae07~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

复制代码

`gem install colorls`

然后执行 `colors` 就好了，你也可以设置 `alias` 更高效一点：

复制代码

`alias lc='colorls -lA --sd'`

设置了别名之后，就像我一样，输入 `lc` 就好了。

我就只用了以上几个插件，已经能够大幅度提升工作效率了，如果有其它好用的插件，一定要告诉我呀。

## VS Code 配置

如果你用的是 `VS Code`，需要再配置一下字体:

复制代码

`{   "terminal.integrated.fontFamily": "Hack Nerd Font" }`

## homebrew 安装

上面的几个插件都用的是 `brew` 命令安装，应该不在少数的人刚开始电脑上是没有 `brew` 的：

复制代码

`brew: command not found`

然后就百度了一下，说要装一个叫 `Homebrew` 的东西，然后就按照官网的方式执行安装：

复制代码

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`

如果安装成功了，恭喜你，你的运气真的很好。如果没安装成功，那你一定会各种百度如何安装，然后还是安装不成功：

复制代码

`curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused`

![|250](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/1/17270025999f6fa0~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

有人告诉你，换一个中科大的源试试：

复制代码

`/usr/bin/ruby -e "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install)"`

然后，你可能会卡在这：

复制代码

`==> Tapping homebrew/core Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core'...`

也就是因为不可描述的原因，下载 `homebrew-core` 这个库的时候网络不行了，那我们就手动 `clone` 一个吧，或者下载一个 `zip` 包解压到对应目录：

复制代码

`cd "$(brew --repo)/Library/Taps/" mkdir homebrew && cd homebrew git clone git://mirrors.ustc.edu.cn/homebrew-core.git`

然后再执行上面的命令安装就好了：

复制代码

`/usr/bin/ruby -e "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install)"`

会看到成功安装的提示：

复制代码

`==> Installation successful!`
