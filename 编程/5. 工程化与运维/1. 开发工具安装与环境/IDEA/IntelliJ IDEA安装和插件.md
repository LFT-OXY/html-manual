# 激活方式一

## Mac激活流程

方法很简单，只是写的教程比较详细，并不复杂，耐心看完就可以激活

如果您是纯小白，或不想自己操作，联系客服付费5元，远程给您激活

按下面的方法操作99%都可以激活

### 第一步

先下载MAC文件，并解压放到桌面

打开mac文件夹，找到ja-netfilter.jar文件（具体位置如下图所示）

![CleanShot 2025-06-16 at 23.26.29@2x.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/CleanShot%202025-06-16%20at%2023.26.29@2x.png)

### 第二步

打开终端，把ja-netfilter.jar文件拖进终端里面，然后复制ja-netfilter.jar的路径（具体方法如下图所示）

![CleanShot 2025-06-16 at 23.27.43@2x.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/CleanShot%202025-06-16%20at%2023.27.43@2x.png)

（打开终端）

![CleanShot 2025-06-16 at 23.29.05@2x.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/CleanShot%202025-06-16%20at%2023.29.05@2x.png)

（ja-netfilter.jar文件拖进终端，复制路径）

### 第三步

先用下面的临时账号把软件激活，然后再打开软件的VM配置界面（如下图所示）

Ctrl+A全选复制下方临时账号复制到软件里（临时账号不能长期使用，只作为临时应急用）

```
账号nahlpilkhf@outlook.com 密码manongjishumi0421i
```

![CleanShot 2025-06-16 at 23.29.52@2x.png|425](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/CleanShot%202025-06-16%20at%2023.29.52@2x.png)

然后打开vm配置界面

打开vm方法1--点Help→Edit Custom VM Options..

![CleanShot 2025-06-16 at 23.30.18@2x.png|225](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/CleanShot%202025-06-16%20at%2023.30.18@2x.png)

打开vm方法二--点左下角，看图操作，看不懂问deepseek

![CleanShot 2025-06-16 at 23.30.42@2x.png|450](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/CleanShot%202025-06-16%20at%2023.30.42@2x.png)


先把下面这两行全选复制进去（一定要ctrl+a全选复制完整）

--add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED

--add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED

然后再加添刚才复制的ja-netfilter.jar的路径，最后再保存重启软件（最后效果如下图所示）

格式：-javaagent:填ja-netfilter.jar的路径=jetbrains

特别说明：最后一行的路径一定要写对！！不能有空格！！不然会打不开软件，重启软件前请仔细检查，如果是路径没写对导致软件打不开的需要付费远程解决，远程费用是5元（软件打不开就是你没操作对，不要找任何理由，下面有图）

![CleanShot 2025-06-16 at 23.31.21@2x.png|400](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/CleanShot%202025-06-16%20at%2023.31.21@2x.png)

### 第四步

重启软件，输入mac文件夹里面的激活码（你是什么软件就选择什么激活码，例如软件是idea就选择IntelliJ IDEA的激活码）激活后激活工具不要动，不要删！！！

激活后Mac文件夹不要动，不要删！！！

![CleanShot 2025-06-16 at 23.31.41@2x.png|350](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/CleanShot%202025-06-16%20at%2023.31.41@2x.png)


mac 激活，点击下方链接用电脑浏览器打开下载， https://jetb.lanzouv.com/iKgUR28wzlnc
mac 激活教程，击下方链接用电脑浏览器打开查看 https://docs.qq.com/doc/DSGFyS0FSSWx4eFRP


# 激活方式二

CodeKey Run 简介

CodeKey Run（[ckey.run](https://ckey.run/)）是一个专注于 JetBrains 系列软件及插件自动激活的网站，支持 Windows、Linux 和 Mac 操作系统，几乎涵盖了 JetBrains 全线产品及主流插件。其核心优势在于“一键激活”，无需繁琐配置，适合广大开发者和团队快速部署开发环境。

支持产品与插件

CodeKey Run 支持 JetBrains 全家桶，包括但不限于：

- IntelliJ IDEA
    
- PhpStorm
    
- PyCharm
    
- WebStorm
    
- GoLand
    
- Rider
    
- DataGrip
    
- CLion
    
- AppCode
    
- DataSpell
    
- RustRover
    
- dotMemory 等
    

并且还支持数百款 JetBrains 插件、主题和工具，如 JetBrains AI Assistant、Code With Me、Material Theme UI、Redis Client、LeetCode Editor Pro、数据库工具、语言包等，覆盖了开发、测试、DevOps、界面美化等多个领域。

## 使用方法

### Windows 系统

1. 按下 `Win + X`，选择 **Windows PowerShell(管理员)** 运行。
    
2. 1. 执行如下命令：
    
    ```bash
    irm ckey.run|iex
    ```
    
3. 程序会自动完成激活，无需手动干预。

### Linux 系统

打开终端，输入并执行：

```bash
wget -q ckey.run -O ckey.run && bash ckey.run
```

### Mac 系统

打开终端，输入并执行：

```bash
curl -Ls ckey.run -o ckey.run && bash ckey.run
```

# 激活原理与注意事项

CodeKey Run 的激活方式本质上是自动化脚本，通过远程下载并运行脚本，实现对 JetBrains 产品的许可证注册和插件授权。官方还支持自定义许可证信息，满足团队、多用户不同场景的需求。

# 插件

1. Alibaba Java Coding Guidelines   `代码格式`
2. CodeGlance Pro    `代码滚轮,上下快速翻滚`
3. Continue
4. Open in cursor
5. google-java-format   `格式化代码`
	- google-java-format 配置及应用
	- https://blog.csdn.net/youxijishu/article/details/135350055
6. HighlightBracketPair
7. Rainbow Brackets
	- https://blog.csdn.net/SunnerChen/article/details/122984222
8. Smart Input   `自动输入法`
9. ptg  `一键生成set和get方法`
10. Extra ToolWindow Colorful Icons  `更换idea中的图标的插件`
11. Nyan Progress Bar  `更换进度条插件`
12. Translation `翻译插件`
13. MyBatisCodeHelperPro  `支持mapper互跳，方法自动生成，代码自动生成`  
14. MyBatis Log Plugin `根据执行sql 替换掉 ？ 显示完整 sql, 直接复制粘贴到数据库，就可以执行`
15. Alibaba Cloud Toolkit  `快速部署到服务器`
16. Atom Material ICons `icon图标插件,可以优化idea流畅度`
17. GitToolBox `能在项目上提示你还有多少文件没提交，远程还有多少文件没更新下来。还能在每一行代码上提示上次提交的时间。查版本提交问题` 
18. Maven Helper
19. Codota `代码智能提示插件`
20.  `驼峰命名和下划线命名转换`
21. Save Actions `格式化代码插件`
22. Key promoter X `快捷键告知`
23. Jrebel for Intellij  `JVM插件，它使得Java代码修改后不用重启系统，立即生效`
24. Git Commit Message Helper
25. Git Commit Template
26. EditorJumper `跳转cursor`



# 主题

1. One Dark Theme
2. Cyan Light Theme
3. Espresso Light Theme
4. One Dark Theme
5. Dracula Theme (喜欢Dracula Colorful)
6. Dark Purple Theme
7. Material Theme UI (喜欢Dracula (Material))
8. ppy colours
9. Rider UI Theme Pack
10. Catppuccin Theme(喜欢)


#  IDEA中使用git，并且配置git忽略文件

当在IDEA中编写好一个项目，想要用git上传到远程仓库时。有一些文件是不用上传的比如.gradle、.idea和build目录下的。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/215a063c075b4e84a2e81b12b1ca50dc.png)


此时可以配置git忽略文件，从而忽略不需要上传的文件。  
1）第一步在C:\Users\用户 目录下新建一个.gitignore文件然后再文件中写入要忽视的文件或目录  
![在这里插入图片描述](https://img-blog.csdnimg.cn/0c271f1e079a41ccb96d71d928086fa9.png)


```java
	*.class
	*.log
	*.ctxt
	*.mtj.tmp/
	*.war
	*.nar
	*.ear
	*.zip
	*.tar.gz
	*.rar
	*.iml
	
	.classpath
	.project
	.settings
	target
	build
	.idea
	.gradle
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/820aa32a82224d6fa7a3883f5fde3422.png)  

2) 在C:\Users\用户 目录下的.gitconfig文件中写入

```java
[core]
	excludesfile = C:/Users/自己的用户名/.gitignore
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e7c0d62943814ac29f535e19bc99900a.png)

注意：在最开始用git时，如果用过下面的命令，那么就会有.gitconfig这个文件
git config --global user.name 用户名 《设置用户签名》
git config --global user.email 邮箱 《设置用户签名》
3) 打开本地的项目，设置项目git路径，为本地安装的git的路径。


![在这里插入图片描述](https://img-blog.csdnimg.cn/262fbb8452f74200bc60654810fdf8ff.png)

4) 新建git 点击Create Git Repository，然后选择本地项目目录，最后点OK  
![在这里插入图片描述](https://img-blog.csdnimg.cn/cd2efd8ef6fc4045a337174bb86bd85f.png)

6) 此时结果如下图（被git忽视的文件颜色偏淡）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2597a6210b274992b775d9557ffeef44.png)

此时被忽视的内容是不能add和commit的。![在这里插入图片描述](https://img-blog.csdnimg.cn/28fb5e26779b473f98ae53c02bfe366f.png)

没有被忽视的内容是可以add 和commit的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/d5e7f5eff6f54ed086fc4081d4cd2ca8.png)

7）将代码 **add** 暂存区  
右键项目 --> Git --> Add  
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ae6c53f708845bfb71c7b6774824de1.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ccd76e831e4540dcaf6bed540ce15048.png)

8) 将代码 **commit** 到 本地仓库  
右键项目–>Git–>Commit Directory  
![在这里插入图片描述](https://img-blog.csdnimg.cn/59a36fb1c9204c16b964438fcd4a5e7b.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/644b6c4019ca456983704877348bb574.png)

9）将代码 **push** 到远程库(代码托管中新) 此处是push到gitee  
在gitee上新建远程仓库  
![在这里插入图片描述](https://img-blog.csdnimg.cn/f05a6b514d7844bca896d2519240103c.png)

复制远程仓库地址  
![在这里插入图片描述](https://img-blog.csdnimg.cn/18cbbec991a2411fadbdfc000bf00f05.png)


IDEA中点击Git --> push  

![在这里插入图片描述](https://img-blog.csdnimg.cn/d298dd82cd204c1692371942c9389a01.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/415d07a8ae0e4510b56f47979daae1fd.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d83f93926ec4859a1f65549c9fe1935.png)

其中某一步骤应该会要求输入gitee的账号和密码。  
此处就可以看到，已将本地项目，提交到了远程库中  
![在这里插入图片描述](https://img-blog.csdnimg.cn/0e0dc6d94c3745ed8ba4aac4de3d0cb3.png)


补充说明：git常用命令

```java
1) git config --global user.name 用户名              《设置用户签名》
2) git config --global user.email 邮箱               《设置用户签名》
3) git init                                          《初始化本地库，管理项目》
4) git status      《查看本地库状态》
6) git add 文件名     《添加到暂存区》
7) git rm --cached 文件名      《将暂存区里的文件删除，工作区里还是有的》
8) git commit -m "日志信息" 文件名     《提交的本地库》
9) git reflog     《查看版本信息》
10) git log      《查看版本详细信息》
11) git reset --hard 版本号     《版本穿梭，回到任意版本(如git reset --hard 9aa3670)》
12) git branch 分支名      《创建分支》
13) git branch -v      《查看分支》 
14) git checkout 分支名    《更换分支》
15) git merge 分支名     《把指定的分支和并到当前分支上》
16) git remote -v       《查看当前所有远程地址别名》
17) git remote add 别名 远程地址    《创建远程地址别名》
18) git push 别名 分支        《将本地仓库内容推送到远程仓库(或者 git push 远程地址 分支)》
19) git pull 别名 分支      《将远程仓库内容拉取到本地仓库》
20) git clone 远程地址    《将远程仓库内容克隆到本地仓库》
```
