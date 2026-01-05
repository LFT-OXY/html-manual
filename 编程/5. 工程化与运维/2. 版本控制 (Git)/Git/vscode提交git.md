VsCode使用Git上传代码至GitHub上
一、软件安装
1、安装Git

2、安装VsCode

Git具体安装方法见 Git的下载安装

VsCode直接安装即可，这里不多做赘述了

二、在GitHub上创建空的仓库/项目
1、在GitHub登录界面登录自己的账户，进入GitHub主页后，点击右上角的“+”号，选择New repository新建一个项目
![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/ba22bd4280918efcb09db8c7cd05ed87.png)

2、在新建界面，输入项目名称，其它默认即可（描述为可选项），如图所示，点击最下面的Create repository 即可创建一个新的项目

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/55bc5b1a42486355edb2bdd6edf90a4b.png)

3、新建成功后跳出的界面显示一些基本信息以及基本的Git操作，方便测试git的克隆，我们需要在该项目中新建一个文件，点击以下蓝色字体，生成一个readme.md文档

也可以在新建项目时直接勾选 Add a README file 按钮，那么在新建项目的同时即可生成一个readme文件

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/786f301d501c2c8d9e9a1cf4916ca8c8.png)

4、在新弹出来的界面中输入信息，然后点击最下面的创建即可

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/da7e8681a2b501348acc792f28fd10ea.png)

5、文件生成后如下图所示

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/b42483a3b8f5b56580661ec3499b5503.png)

#### 三、Git首次安装需要进行的设置

1、查看我的git配置列表

鼠标右键点击Git Bash Here，弹出git命令行终端，输入指令：git config --list，会在命令行出现当前的 git 配置内容，如下图

![在这里插入图片描述](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/110ee5b38acc9a8b9f59e575f5d6ecbd.jpeg)

2、配置全局用户名

在终端命令行输入指令：**git config --global user.name “你的用户名”**

3、配置全局邮箱

在终端命令行输入指令：**git config --global user.email “你的邮箱”**

> 注意：设置的用户名及邮箱尽量同GitHub中一致

#### 四、通过HTTPS协议链接GitHub

##### 方法1-从GitHub仓库克隆代码

1、从GitHub仓库克隆代码

（1）打开VsCode，按快捷键“ctrl+shift+P”，然后输入git，选择克隆（clone）

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/55b64f2443edf199dc598499484fcb58.png)

（2）弹出输入框，提示我们输入存储库的URL地址，我们直接去GitHub中复制仓库的HTTPS地址粘贴即可。

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/e49aab0d31b8148dff50a8a902f17004.png)

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/f6abaab3c7bd0f4c0af8d21580de4d2f.png)

（3）输入存储库URL地址确认后，会让我们选择需要克隆到本地的位置，直接选择自己放项目的文件夹即可（不要中文），确认后开始克隆，如图所示。

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/53ffc49802de4da9c9f4401a0f23e265.png)

（4）克隆完成后，会出现如下界面，选择打开即可，同时对应的项目文件夹里面已经存在了克隆过来的内容

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/297c69851de825186cd70e41dfb41bb0.png)

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/a36d0fc252f4b23a873d11977367666b.png)

2、在本地修改代码

打开项目后，我们看到如下图所示，点击文件夹管理，弹出我们克隆下来的代码，选择README.md文件，可以看到对应的文件内容，对该文件进行一些修改并保存，以便接下来测试是否可以回传到GitHub。

保存文件后，可以看到左侧出现了一个蓝色的1图标，点击它进入Git管理界面。![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/94c907968cdd65db5c79e76b41387c37.png)

3、Git版本管理

（1）进入Git管理界面后，可以看到已经有了一个更改，文件的后面的字母 M 表示文件发生了更改，意味着 modified，点击该文件，如下图所示，可以看出我们对文件的更改是增加了部分内容。

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/44e44eb921795d8a0dc697ebaa2b4b25.png)

（2）点击图标 M 左边的 + 号，可以看到如下图所示，说明我们把更改暂存了，但是此时还没有提交
![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/dba5f806746f51dd83084f83adf1d06c.png)

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/b1eefb83157c32897c951ac6ba817099.png)

（3）按照提示，我们在箭头处填写信息，此处的信息内容为提交的备注，这里可以写修复了什么内容，写完后点击右上角的 √ 提交。

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/649ed63cf419c76bdfc182b3c86b6a6c.png)

（4）提交完成后即已经成功修改、并且把代码提交到了本地仓库。接下来我们需要把代码提交到远程仓库。

4、从本地仓库上传到GitHub远程仓库
点击如下图的箭头处，若出现登录窗口则根据具体需要进行填写即可

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/743fde632832f873ddf09f2fb741255e.png)

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/90d273044d2dfce90ea7f46c30af1a96.png)

5、在GitHub远程仓库查看更改

（1）代码已经修改了

（2）备注信息显示为我们提交时填写的信息

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/0a3ece30e46236beeb9e180586b23e91.png)

##### 方法二-使用VSCode终端提交

终端提交代码到Github一共需要两步：提交到本地仓库 —> 从本地仓库再提交到远程仓库

、提交到本地仓库

（1）首选创建一个本地仓库，即右键新建一个空文件夹，接下来在VSCode中打开终端，进入当前新建的空文件夹，进行本地仓库初始化，之后这个空文件夹里会出现一个 .git 文件夹，如下图：

```
本地仓库初始化：
    git init
```

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/466dc187944059e939fa3671ef8b742f.png)

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/68bfa0d0ed5dfe5b2254ca88542149de.png)

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/702b2d982e10e5e33d6b48bbf980e43e.png)

（2）接下来向当前文件夹里添加一些文件用于测试提交的步骤

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/1b085483cf57517e6969944e2b7d13ff.png)

（3）现在来进行提交的操作—提交单个文件步骤

```
提交单个文件：
    git add readme.txt
    git commit -m "第一次提交readme"
```

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/a297b8daf17235194251a56b69642442.png)

```
提交全部文件：
    git add . 
    git commit -m "全部提交"
```

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/038ce595e7c2401c116d98c231d63fb2.png)

2、提交到远程仓库

本地仓库提交完毕后，就需要将内容提交到远程仓库了，如图：

```cmd
提交远程仓库：
    git remote add origin https://github.com/LL-Elsie/git_test.git
    git remote set-url origin https://github.com/LL-Elsie/git_test.git 
    git branch -M test1
    git push -u origin test1
```

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/9343dc012ee459f6b6fa11b2c134d33c.png)

这样就在 git_test 远程仓库新建了一个 test1 分支，并且把本地 git_test 中的文件全都推到 git_test 仓库中的 test1 分支中了。

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/df17a72cce25db191a8ab30d5ba15d1d.png)

**常用git指令**

```cmd
git branch------------------查看当前所处分支 
git checkout -b main--------在当前仓库中新建一个main分支，并切换到这个分支。 
git branch -M main----------新建一个main分支 
git checkout main-----------切换到main分支
```

#### 五、使用git上传代码至于Github（远程仓库已存在内容）

1、该仓库下有内容更新，如图所示，打开终端，使用git进行上传

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/508525c70b1ed5c10ebb66bde38c34fb.png)

2、在终端中输入命令，提交全部文件至本地仓库后，再进行提交到远程仓库，在提交远程仓库后，会出现一行错误信息：error: remote origin already exists.，如下图

```cmd
提交全部文件至本地仓库
git add . 
git commit -m "全部提交"
git remote add origin https://github.com/LL-Elsie/Practice.git
```

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/d6329d1d630c31a3cb5cc8dad217a455.png)

3、出现该错误信息，翻译过来就是（致命：远程来源已经存在），此时，我们可以先 git remote -v 查看远程库信息

```cmd
查看远程库信息
git remote -v
```

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/0df3a9b737fbbe76a7b311012d2bed00.png)

4、出现该问题表示远程仓库已存在，因此我们先删掉再进行链接即可，解决方法如下：

```cmd
删除远程仓库
    git remote rm origin
将远程仓库添加到分支
    git remote add origin https://github.com/LL-Elsie/Practice.git
重新推送
    git push origin master
```

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/a88ffc943359b8bf3092d6c9b7d86277.png)

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/d881b3fa5e5328b681961177b405c02a.png)

注意：若推送代码至远程仓库时，有如下报错信息：the remote end hung up unexpectedly，则是因为推送代码的大小超出了git默认的范围，可以调整git推送代码的最大值

![39d33c17c6a7020cc4af160e6e1cf5f.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/603232345a49688d580a4784b35ef1f4.png)

解决办法：

找到该本地仓库下的.git文件夹里面的config文件，打开config文件，将postBuffer改为1024mb，此时重新进行推送，代码就可以成功的推送到远程仓库

```cmd
[http]
postBuffer = 1024288000
```

![885a9d18fd5bda65aa6f4523e4e5382.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/27bf3bab5d8ba5700abbd43d2640a3ac.png)

![7482f5fb4d9f4b38eac6d37614e5ba0.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/15c2391a30a3c0a4998b81ff91955452.png)