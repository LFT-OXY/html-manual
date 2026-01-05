## **1.2软件安装**

### **1.2.1官网下载安装**

官方下载地址：

[iTerm2官网​iTerm2.com/downloads.html](https://link.zhihu.com/?target=https%3A//iTerm2.com/downloads.html)

![|500](https://pic1.zhimg.com/80/v2-d9f240ecf1b1dd2c433987252838375c_1440w.webp)

根据个人需求下载正式稳定版或者beta版，下载完成之后,双击打开zip包，解压至当前文件夹，

![|500](https://pic2.zhimg.com/80/v2-ecf43aa7af190e833ab14a3da7ab8c9d_1440w.webp)

双击打开iTerm.app,根据弹窗提示点击Move to Applications Folder即可。

![|500](https://pic3.zhimg.com/80/v2-3eb3948dbc44c225a6e7593377075bee_1440w.webp)

### **1.2.2 Homebrew安装**

打开电脑终端，命令行输入如下命令回车：

```bash
brew install iTerm2  
```

如提示未安装Homebrew，参考这篇文章进行安装

[金牛肖马：Homebrew国内如何自动安装（国内地址）（Mac & Linux）6237 赞同 · 1353 评论文章![|755](https://pic4.zhimg.com/v2-c40d2ef7e08afbb2750f2c5b4b45c923_180x120.jpg)](https://zhuanlan.zhihu.com/p/111014448)

OK,是不是贼简单，iTerm2到这里就安装完毕，大家可以关闭退出了。哈哈哈，别急，咱们仅仅完成了第一步，iTerm2真正的魅力在配置和扩展后才能发挥出来，接着往下看。

## **02基本设置**

### **2.1设置iTerm2为默认终端**

打开iTerm2，左上角选择iTerm2，红框内容点击即可

![|500](https://pic4.zhimg.com/80/v2-6031ecab0f244dd18c50e5e92731fb93_1440w.webp)

### **2.2配置主题**

访问iTerm2主题网站

[https://github.com/mbadolato/iTerm2-Color-Schemes​github.com/mbadolato/iTerm2-Color-Schemes](https://link.zhihu.com/?target=https%3A//github.com/mbadolato/iTerm2-Color-Schemes)

![|500](https://pic2.zhimg.com/80/v2-50aa1374ef9e39a185a26cdccd8fe665_1440w.webp)

下载zip包并解压到本地，进入解压缩的文件目录，找到schemes文件夹

![|500](https://pic4.zhimg.com/80/v2-3fa46f9a940c99c53c38ed6bde00a927_1440w.webp)

在schemes文件夹中找到Solarized Dark Higher Contrast.itermcolors文件，此款主题配色最为流行，下面就以此主题为例进行导入和修改，这里我是下载了全量的配色方案，大家可以根据自己喜好选择不同的主题进行导入。

![|350](https://pic4.zhimg.com/80/v2-7b7c026733ea5c8c7238a619a35aca2b_1440w.webp)

导入主题配色：iTerm2 -> Preferences

![|400](https://pic4.zhimg.com/80/v2-d891cc4b768e0dd732f5e64a5ca2c407_1440w.webp)

上图给大家演示了如何找到iTerm2 -> Preferences选项，后续所有关于iTerm2的配置均在此选项下进行，不再赘述。

打开配置页面，Profiles -> Colors -> Color Presets -> Import，选择到刚刚解压的主题文件。

![|875](https://pic3.zhimg.com/80/v2-3d454d8c0e164c0f5f895363d3716bb2_1440w.webp)

![|325](https://pic2.zhimg.com/80/v2-732a7b9817db56fa1e18174cbf631519_1440w.webp)

导入完成后在Color Presets中找到Solarized Dark Higher Contrast选项勾选即可。

## **03安装oh-my-zsh**

### **3.1关于zsh Shell**

mac下默认的Shell是bash，虽然bash的功能已经很强大了，但是zsh拥有更多的自定义空间，并支持各种丰富的扩展，可以实现更强大的命令补全，命令高亮，自动跳转等一系列功能，这些功能极大的提升了我们操作命令行的效率。其实吧，我觉得以上的都不是很重要，对大家最最最重要的一点就是：

> “不管我行不行，我的工具要够炫，只要你被我的工具装到了，我至少胜利了一大半。”

工具胜利法，这一波又赢麻了。

当然使用zsh Shell 的代价是牺拖慢了一点点启动速度，不过这点几乎可以直接忽略不计，因为默认的 zsh 配置起来比较麻烦，一个叫 robbyrussel 的用户在 GitHub 上制作了一个配置文件 oh-my-zsh，该文件逐步的被大家接受，目前也是公认的较为流行的zsh 配置方式。

### **3.2下载oh-my-zsh**

访问oh-my-zsh官网：

[Oh My Zsh - a delightful & open source framework for Zsh​ohmyz.sh/#install![|955](https://pic3.zhimg.com/v2-b5f6554a8bd0752f2dc2d7b4307819ce_180x120.jpg)](https://link.zhihu.com/?target=https%3A//ohmyz.sh/%23install)

![|500](https://pic3.zhimg.com/80/v2-66d101899de20d62ed4fe4fec91ea9a6_1440w.webp)

官网提供了两种安装方式，博主都贴在了下面，方便大家直接复制使用：

```bash
#Install oh-my-zsh via curl
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

#Install oh-my-zsh via wget
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

咱们在1.2.2小节提供了Homwbrew的安装办法，如果使用wget安装方式，若终端提示未安装wget，直接在Shell输入以下命令行点击回车进行安装即可

```text
brew install wget
```

### **3.3设置默认 Shell**

安装完成后我们需要把zsh设为默认的Shell，下面提供几个常用的命令，供大家参考：

```bash
#查看系统安装的所有Shell
cat /etc/Shells
```

![|250](https://pic3.zhimg.com/80/v2-63a209bb88b0825e14bc15a92f9dc4fa_1440w.webp)

```bash
#查看当前使用的Shell
echo $SHELL
```

![|500](https://pic4.zhimg.com/80/v2-c088d1ada4e5f3df38f0a2884c57b347_1440w.webp)

```bash
#修改默认Shell为zsh
chsh -s /bin/zsh 
```

修改默认Shell需要输入电脑密码，输入后回车即修改成功，博主默认使用的就是zsh，所以这里显示未修改。

![|500](https://pic3.zhimg.com/80/v2-df64712b88ae43b7237e9b344000772e_1440w.webp)

## **04配置oh-my-zsh主题**

### **4.1 主题下载及修改**

### **4.1.1自带主题**

安装完成后接下来我们对zsh主题进行修改，首先咱们可以使用以下的命令查看所有的oh-my-zsh自带主题

```bash
ls ~/.oh-my-zsh/themes
```

![|500](https://pic3.zhimg.com/80/v2-7d493de62a43888fc559fd6a9556523a_1440w.webp)

是不是一脸懵逼，你肯定要问这么多主题，每个主题都是什么样的，哪个适合我呢？咱们可以进入下面的网站看下oh-my-zsh自带的部分主题显示效果：

[Themes · ohmyzsh/ohmyzsh Wiki​github.com/ohmyzsh/ohmyzsh/wiki/Themes![|755](https://pic3.zhimg.com/v2-cc0414c1c2bbc0874f575e40c03e0682_180x120.jpg)](https://link.zhihu.com/?target=https%3A//github.com/ohmyzsh/ohmyzsh/wiki/Themes)

![|500](https://pic1.zhimg.com/80/v2-8bf62b32ec9af298d261da66b08aba5c_1440w.webp)

找到自己喜欢的主题，接下来就可以进行主题修改了，博主以agnoster为例，展示下如何修改oh-my-zsh主题：

- 首先在终端输入

```bash
vim ~/.zshrc
```

进入如下界面，可以看到红框圈出来的即为主题配色方案：ZSH_THEME="ys"

![|500](https://pic3.zhimg.com/80/v2-784688806393ae88059e9f35b638abe2_1440w.webp)

- 接下来键盘输入 i ,进入编辑模式
- 光标定位到ZSH_THEME=""这一行，把ys删除，agnoster写入，显示为ZSH_THEME="agnoster"
- 然后按下ecs键，输入:wq退出编辑模式
- 设置完成输入以下命令更新配置，使之生效。

```bash
source ~/.zshrc
```

**后面涉及到的对此配置文件的修改均采用上面的方法，只是修改的内容不一样，不再赘述具体的操作。**

当然如果你特别**变态**，对所有的主题都很喜欢，可以按照上面的修改步骤，把主题修改为

```bash
ZSH_THEME="random"
```

这样你每启动一次终端，就会随机切换一个主题。

当然你也可以选择几个最喜欢的主题配置在图片中红框这一行

```bash
ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" "ys")
```

这样你的主题会在括号中配置的这几个主题中随机切换。

![|500](https://pic4.zhimg.com/80/v2-110e8a6f46ba4e69a8e9216da259400b_1440w.webp)

除此之外，你也可以排除你不想要的主题，在配置文件增加如下代码，在括号中写上自己不喜欢的主题名称，以空格隔开：

```bash
ZSH_THEME_RANDOM_IGNORED=(pygmalion tjkirch_mod)
```

### 4.1.2第三方主题

如果对自带的主题不太满意，可以进入下面的网站看下oh-my-zsh的部分第三方主题显示效果

[External themes · ohmyzsh/ohmyzsh Wiki · GitHub​github.com/ohmyzsh/ohmyzsh/wiki/External-themes](https://link.zhihu.com/?target=https%3A//github.com/ohmyzsh/ohmyzsh/wiki/External-themes)

![|500](https://pic4.zhimg.com/80/v2-d54322c047133cd067fd58913cecc6df_1440w.webp)

可以根据自己的喜好进行下载安装。

### **4.2 Powerline字体下载安装**

iTerm2 修改主题之后，因为某些主题含有特殊字符或者表情，在操作的时候会出现乱码的情况，因此需要安装Meslo字体来兼容解决。

### **4.2.1官网安装字体**

字体下载地址：

[GitHub - powerline/fonts: Patched fonts for Powerline users.​github.com/powerline/fonts![|855](https://pic4.zhimg.com/v2-3960d2bfd11fa91903f421362b9bdd1f_180x120.jpg)](https://link.zhihu.com/?target=https%3A//github.com/powerline/fonts)

在fonts目录下找到 **Meslo Slashed/Meslo LG M Regular for Powerline.ttf** 字体

![|725](https://pic2.zhimg.com/80/v2-b7e6a13eeeb1d6c9cb3cf656a844cfc9_1440w.webp)

下载后直接安装即可。

### **4.2.2 git安装字体**

```bash
#先使用git命令克隆
git clone https://github.com/powerline/fonts.git --depth=1

# 进入克隆到本地的fonts目录进行安装
cd fonts
./install.sh

# 删除克隆到本地的目录
cd .. 
rm -rf fonts
```

### **4.2.3修改字体**

打开iTerm2，打开Preferences配置界面，Profiles -> Text -> Font，选择 Meslo LG M Regular for Powerline 字体。

![|725](https://pic2.zhimg.com/80/v2-03659834002c4ca712359bc35372d691_1440w.webp)

## **05安装插件**

插件的安装，博主基本都提供了Homebrew或者git两种安装方式，大家根据习惯任选其一即可，无需重复安装。

### **5.1 声明高亮插件zsh-syntax-highlighting**

此款插件在我们使用命令行的时候如果遇到特殊命令或者错误命令，会有高亮显示，可以及时进行提醒。

### **5.1.1 Homebrew安装**

博主这边已经安装过了，这里直接提示已安装，未安装的同学直接输入以下命令等待安装完成即可：

```bash
brew install zsh-syntax-highlighting 
```

![|500](https://pic3.zhimg.com/80/v2-5758aec20101f3df39b66e38b08e7c46_1440w.webp)

安装成功后，进行如下操作：

```bash
#编辑配置文件
vim ~/.zshrc

#在最后一行增加下面的代码
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh 

#退出编辑后执行使配置生效
source ~/.zshrc 
```

这里如果还不清楚如何操作，请参考4.1.1自带主题这一小节，进行文件修改即可。

![|500](https://pic1.zhimg.com/80/v2-341ce407d3fbf2d38c86d8dd058069d0_1440w.webp)

### **5.1.2 git命令安装**

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

![|500](https://pic1.zhimg.com/80/v2-f550957d9089febba81bddc76a345700_1440w.webp)

安装成功后，进行如下操作：

```bash
#编辑配置文件
vim ~/.zshrc

#找到plugins配置，在括号内增加zsh-syntax-highlighting,与其他插件之间使用空格分隔开
plugins=(zsh-syntax-highlighting)

#退出编辑后执行使配置生效
source ~/.zshrc
```

![|150](https://pic4.zhimg.com/80/v2-0e52e52924cdc0f7851bf147fb032663_1440w.webp)

### **5.2 自动填充建议插件zsh-autosuggestions**

此款插件非常实用，大大加快了我们敲命令的速度。

### **5.2.1 Homebrew安装**

直接输入以下命令等待安装完成即可：

```bash
 brew install zsh-autosuggestions 
```

![|500](https://pic4.zhimg.com/80/v2-2b06971c9abcd0792c4a2740357b6963_1440w.webp)

安装成功后，进行如下操作：

```bash
#编辑配置文件
vim ~/.zshrc

#在最后一行增加下面的代码
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh 

#退出编辑后执行使配置生效
source ~/.zshrc
```

### **5.2.2 git命令安装**

```bash
 git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
```

![|725](https://pic3.zhimg.com/80/v2-7f80ce1b335a3611a42a38e645840152_1440w.webp)

安装成功后，进行如下操作：

```bash
#编辑配置文件
vim ~/.zshrc

#找到plugins配置，在括号内增加zsh-autosuggestions,与其他插件之间使用空格分隔开
plugins=(zsh-autosuggestions)

#退出编辑后执行使配置生效
source ~/.zshrc
```


### **5.3 快速跳转插件autojump**

此款插件可以帮助我们快速跳到常用的目录。

### **5.3.1 Homebrew安装**

直接输入以下命令等待安装完成即可：

```bash
brew install autojump
```

![|500](https://pic3.zhimg.com/80/v2-fe5eed3f22588ec6b19a81f55ebb1a7a_1440w.webp)

安装成功后，进行如下操作：

```bash
#编辑配置文件
vim ~/.zshrc

#在最后一行增加下面的代码
[ -f /usr/local/etc/profile.d/autojump.sh ] && . /usr/local/etc/profile.d/autojump.sh

#退出编辑后执行使配置生效
source ~/.zshrc 
```

### **5.3.2 git命令安装**

```bash
#github镜像
git clone git://github.com/joelthelion/autojump.git

#进入目录，执行安装命令
./install.py
```

在安装过程中，会在～/下建立.autojump文件夹，如果github镜像无法下载，请使用gitee镜像下载

```bash
#gitee镜像
git clone https://gitee.com/null_454_5218/autojump.git $ZSH_CUSTOM/plugins/autojump

#进入目录autojump中
cd $ZSH_CUSTOM/plugins/autojump

#执行安装命令
./install.py
```

![|500](https://pic2.zhimg.com/80/v2-6202bcb52ec7c547f93ca55eb189b5d1_1440w.webp)

安装成功后，进行如下操作：

```bash
#编辑配置文件
vim ~/.zshrc

#找到plugins配置，在括号内增加autojump,与其他插件之间使用空格分隔开
plugins=(autojump)

#在文件最后一行或者plugins=()后另起一行添加如下内容
[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && . ~/.autojump/etc/profile.d/autojump.sh

#退出编辑后执行使配置生效
source ~/.zshrc 
```

## **06一些骚操作**

### **6.1 iTerm2快速隐藏和显示窗体**

打开iTerm2，打开Preferences配置界面，Profiles → Keys →configure Hotkey window，自定义一个快捷键就可以了。

![|725](https://pic1.zhimg.com/80/v2-deb1be11c328b150b8b5287895db42a8_1440w.webp)

![|725](https://pic2.zhimg.com/80/v2-a41eb29eb69d59657b685f38b957bd41_1440w.webp)

### **6.2 iTerm2隐藏用户名和主机名**

通常在Shell中默认的我们的用户名和主机名，这两者加在一起会很长，操作的时候很影响观感，我们可以手动去除。

首先使用命令查看当前用户名称

```bash
whoami
```

![|750](https://pic2.zhimg.com/80/v2-d8ba873b4e0478fba3c6be71ced78ad9_1440w.webp)

```bash
#编辑配置文件
vim ~/.zshrc

#在文件最后增加 DEFAULT_USER="xxxxx" 配置
DEFAULT_USER="xxxxx"

#退出编辑后执行使配置生效
source ~/.zshrc 
```

再次打开终端姓名和主机名就隐藏掉了。

### **6.3 设置 Status bar**

iTerm2 提供了不少的 Status bar，开启后我们可以在终端的最上方非常方便的实时查看本机的一些信息。

![|525](https://pic4.zhimg.com/80/v2-be59567f727b2b39bb312111c60024cf_1440w.webp)

打开iTerm2，打开Preferences配置界面，Profiles -> session-> 勾选 Status bar enable-> configure Status bar，选择自己想要的展示内容即可。

![|525](https://pic3.zhimg.com/80/v2-5eddca9e5c9734b4b126eb3fb1960556_1440w.webp)

![|500](https://pic4.zhimg.com/80/v2-e4fba4eeb8aa9f4c723a367d83f53523_1440w.webp)

### **6.4 光标选择**

iterm提供了三种光标可供选择：_、|、[]。

打开iTerm2，打开Preferences配置界面，Profiles -> text-> cursor，选择自己想要的光标即可。

![|650](https://pic1.zhimg.com/80/v2-f3ece66ba88708891e970e28cd8fba38_1440w.webp)

### **6.5 窗口设置**

打开iTerm2，打开Preferences配置界面，Profiles -> Window，根据自己的需求设置窗口透明度、背景图片、行列数以及风格等。

![|875](https://pic2.zhimg.com/80/v2-d6ce097cad7939176ef15c99991c2395_1440w.webp)

设置好后的效果如下：

![|750](https://pic2.zhimg.com/80/v2-93371c594dc31de5b05ddeba8f9ac3ed_1440w.webp)

### **6.6 Badge、Title、Icon**

打开iTerm2，打开Preferences配置界面，Profiles -> General ，根据自己的需求设置Badge，点击edit按钮调整Badge位置和大小，Title和Icon选项是设置标签页标题和图标的，博主习惯性采用图片中的设置，各位看官可以根据自己的需求灵活设置。

![|725](https://pic2.zhimg.com/80/v2-deedd31111e9f806557b8f7815e6ab1d_1440w.webp)

设置效果如下：

![|825](https://pic4.zhimg.com/80/v2-845f8eb452aae10db19bc334d429ce9f_1440w.webp)

### **6.7 标签页配色**

标签配色默认为黑色，不能与操作页面保持统一

![|700](https://pic3.zhimg.com/80/v2-0a1d7dc0373f02001aeed8aa0a7555d6_1440w.webp)

打开iTerm2，打开Preferences配置界面，Appearence -> General，将 Theme 改为 Minimal

![|725](https://pic4.zhimg.com/80/v2-2048b535eb8455018f593b25918fec33_1440w.webp)

修改后效果：

![|725](https://pic2.zhimg.com/80/v2-85d0649680f35b2379dfe28a9e814499_1440w.webp)

### **6.8 配置SSH快速连接**

博主以连接腾讯云服务器为例：

```bash
#首先在/Users目录下按照如下命令创建sh脚本
cd /Users/

#创建iterm文件夹
mkdir iterm
 
#进入iterm文件夹
cd iterm

#创建myserver.sh文件
touch myserver.sh

#编辑myserver.sh文件
vi myserver.sh
```

键盘输入i编辑文件，插入以下内容：

```bash
#!/usr/bin/expect
set timeout 30
spawn ssh -p [lindex $argv 0] [lindex $argv 1]@[lindex $argv 2]
expect {
        "(yes/no)?"
        {send "yes\n";exp_continue}
        "password:"
        {send "[lindex $argv 3]\n"}
}
interact
```

myserver.sh文件中变量解释：

```text
[lindex $argv 0]：端口号
[lindex $argv 1]：服务器用户名
[lindex $argv 2]：服务器IP地址
[lindex $argv 3]：服务器密码
```

插入完成后键盘esc 然后输入:wq退出，接下来给文件赋权

```bash
chmod 777 myserver.sh
```

打开iTerm2，打开Preferences配置界面，Profiles -> general，左下角点击+号，新建profile，参考下面图片在对应位置输入内容即可。

![|775](https://pic1.zhimg.com/80/v2-1b60a24ce966f3a9e49eda304a3a9038_1440w.webp)

Name:根据需求输入，通常选择标识性较强的内容便于区分，例如服务器的IP地址

Command：这里选择login Shell

Send text at start ：填写格式形如A B C D E这样，每一个部分之间用空格隔开，根据自己实际情况填写,下面是对每一部分内容的解释

```text
A代表咱们上面写的本机保存sh脚本的路径：/Users/iterm/myserver.sh

B代表服务器端口号一般远程连接端口为：22

C代表服务器用户名一般为：root

D代表服务器IP：根据腾讯云服务器对外暴露的公网IP填写

E代表服务器密码：根据自己实际的服务器密码填写
```

设置好之后打开iTerm2，点击profiles，点击前面自己新增的连接远程服务器的profile的名字

![|500](https://pic4.zhimg.com/80/v2-44b72b68d158985edf07a5d6a5d62bff_1440w.webp)

首次连接需要输入一次服务器密码，之后再连接就免密码登陆了

![|650](https://pic3.zhimg.com/80/v2-238d8878e236ee47a0a70f08610bb472_1440w.webp)

### **6.9 设置终端历史行数**

打开iTerm2，打开Preferences配置界面，Profiles -> Terminal，根须需求进行修改，如果想不限制行数可以勾选Unlimited scrollback

![|800](https://pic3.zhimg.com/80/v2-d25b656b77d0b13f37e28a23e3909112_1440w.webp)

## 07快捷键

安装CheatSheet软件,长按⌘键即可查看所有快捷键。

[CheatSheetfor Mac 快捷键：- Mac软件分享【腾讯柠檬精选】​lemon.qq.com/lab/app/CheatSheet.html![|730](https://pic2.zhimg.com/v2-cdeec05c4114a4c72a5de2a4172d98a5_180x120.jpg)](https://link.zhihu.com/?target=https%3A//lemon.qq.com/lab/app/CheatSheet.html)

![](https://pic2.zhimg.com/80/v2-0e0fb92ead6f736e3ca7aed53c887515_1440w.webp)