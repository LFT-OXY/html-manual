
# gitbash常用命令

git init 初始化 git，只有初始化了以后才可以使用 git 相关命令。

git clone 获取远程项目，并下载到本地。远程库的地址在 GITHUB 项目中会有提供。

git status 查看本地修改与服务器的差异。

git add . 将这些差异文件添加，这样就可以提交了。

git commit –m “这里是注释” 提交更改到服务器。

git checkout master 更改到master库。

git pull 将服务器最新的更改获取到本地。

git merge local master 将本地的local合并到远程的master上。

git push origin master 正式提交到远程的master服务器上.

还有“git tag”，“git diff”，“git show”，“git log”，“git remote”等。

一、git命令整理

> git config --global user.email “邮箱名”：绑定GitHub邮箱
> 
> git config --global user.name “Github名”：绑定GitHub
> 
> git init：初始化一个空的git仓库
> 
> git status：检查仓库中的文件状态
> 
> git add：添加文件到暂存区
> 
> git commit -m “备注信息”：提交到版本库并记录提交信息
> 
> git log：查看git日志
> 
> git log --pretty=oneline：简化日志信息
> 
> git log --graph --pretty=oneline --abbrev-commit 查看日志信息（暂时不知跟上面有什么区别）
> 
> git checkout – 文件名：撤销对本地文件的更改（未add时）
> 
> git reset HEAD 文件名：撤销对暂存区的修改（add后）（相当于撤销add操作）
> 
> git reset --hard HEAD^：回退版本信息
> 
> （HEAD^：一个版本 HEAD~50 五十个版本）
> 
> git reset --hard 标识符：恢复到标识符版本
> 
> git rm 文件名：删除仓库中的文件
> 
> git clone GitHub仓库地址：将远程的仓库下载到本地
> 
> git push origin master：将本地仓库的内容与远程仓库同步 推过去
> 
> git pull origin master：拉取远程仓库的代码到本地，使本地与远程仓库的代码同步 拉过来
> 
> git remote add origin 仓库地址：将本地仓库同步（连接）到远程仓库
> 
> git push -u origin master：将本地仓库的内容推送到远程仓库中
> 
> （-u参数，Git会把本地的master分支与远程的master分支关联起来）
> 
> git branch 分支名：创建新分支
> 
> git checkout 分支名：切换到分支
> 
> git checkout -b 分支名：创建并切换分支
> 
> git branch：查看分支
> 
> git branch -D(d高版本小写也可以) 分支名：删除分支
> 
> git push origin本地分支名:远程分支名：将分支推送到远程仓库
> 
> （如果没有冒号那么本地分支名将会与远程分支名相同）

二、本地连接远程仓库

　　初始化仓库：git init

　　建立连接：git remote add origin 远程仓库地址

　　远程仓库pull到本地：git pull origin master

**安装完成git后**

# 方法一:

## 1.获取ssh密钥

打开输入：ssh-keygen -t rsa -C “git账号”  
输入之后一路Enter（确认）就可以了
![](https://img-blog.csdnimg.cn/20210111010036926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

以上截图就证明成功了，这个时候打开以下地址：  
id_rsa.pub就是我们需要的ssh密钥了

![](https://img-blog.csdnimg.cn/20210110214035273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

**注意**：有的可能以前生成过，就会报这个错了。

![](https://img-blog.csdnimg.cn/20210110150417301.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

报错解决: [https://blog.csdn.net/weixin_43888891/article/details/112429980](https://blog.csdn.net/weixin_43888891/article/details/112429980)

## 2.绑定ssh密钥

现在你就需要登录到你的GitHub上边添加这个密匙

![](https://img-blog.csdnimg.cn/20210109192647110.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

将整个id_rsa.pub内容复制

![](https://img-blog.csdnimg.cn/20210111005907399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

添加成功

![](https://img-blog.csdnimg.cn/20210110214812416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

之后你就可以回到你的Git bash上边了  
输入：ssh -T git@github.com  
然后输入上边的代码，来检查是否成功绑定。如果输入之后选择yes出来是这样说明就成功了。

![](https://img-blog.csdnimg.cn/20210110215503352.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

接下来还需要简单的设置一些东西。  
git config --global user.name “git账号”  
git config --global user.email “git邮箱，注册时候的邮箱”

![](https://img-blog.csdnimg.cn/20210110224155331.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

## 3.代码克隆

下面就要将你的库克隆下来到本地电脑中，方便以后进行上传代码。

链接: [https://github.com/](https://github.com/)

![](https://img-blog.csdnimg.cn/20210110224822436.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

下面就要将你的库克隆下来到本地电脑中，方便以后进行上传代码。

在库创建完成之后 会有一个网址出现在网页中，这个地址就是代码地址。  
git clone 命令会用的到

![](https://img-blog.csdnimg.cn/20210110225411595.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

接下来就开始选择文件存储地方了。
![](https://img-blog.csdnimg.cn/20210110225903563.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

git clone后边的网址就是你创建库成功之后的网址

git clone 地址（这个地址就是刚刚创建的库那个页面上代码地址）

**在执行命令过程有时候会让你输入账号密码啥的，这个不要输错了就行！**


![](https://img-blog.csdnimg.cn/20210111010201542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

可以看到，指定目录已经存在了我们的库文件

![](https://img-blog.csdnimg.cn/20210110231511349.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

## 4.测试提交文件

打开这个文件夹，然后在其中创建一个任意格式，任意名称的文件。

![](https://img-blog.csdnimg.cn/20210110232359167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

然后在这个文件里面右键git bash进黑框框  
git add我们新增的文件
![](https://img-blog.csdnimg.cn/20210110232529407.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

之后输入然后git commit -m “cc” 引号内的内容可以随意改动，这个语句的意思是 给你刚刚上传的文件一个备注，方便查找记忆而已

![](https://img-blog.csdnimg.cn/20210110232623696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

然后在输入git push origin master  
这个就代表成功了
![](https://img-blog.csdnimg.cn/20210111010300987.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)

现在打开你的GitHub网站，找到你创建的库。  
文件上传成功。
![](https://img-blog.csdnimg.cn/20210110233509888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjEwMTgzOQ==,size_16,color_FFFFFF,t_70)



# 方法二:
## 步骤1：创建仓库

1. 登录你的GitHub账号。
    
2. 在主页或仓库页面，点击"Create repository"按钮。
    
3. 在弹出的对话框中，填写仓库名称（Repository name）和可选的描述（Description）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/d0d97f25e6e4431d977e8ba4612f232b.jpeg#pic_center)

4. 选择公开（Public）或私有（Private）的仓库类型。
![在这里插入图片描述](https://img-blog.csdnimg.cn/5df5f137f8424319848c090a434cf344.jpeg#pic_center)

5. 点击"Create repository"按钮，完成仓库的创建。
![在这里插入图片描述](https://img-blog.csdnimg.cn/4340e6680d6f4730821ee73a444e894e.jpeg#pic_center)

这样你就拥有了自己的仓库

## 步骤2：克隆仓库

1. 在本地选择一个存放代码的目录。
2. 打开命令行终端（Terminal）。
3. 使用cd命令进入你选择的目录。
4. 在GitHub仓库页面中，点击绿色的"Code"按钮。
5. 复制仓库的URL。
6. 在命令行终端中，使用git clone命令，后面跟上仓库的URL。
```
git clone https://github.com/你的用户名/仓库名称.git
```
执行命令后，远程仓库的代码将被克隆到本地目录中。

## 步骤3：添加和提交代码

1. 将你的代码文件复制到本地仓库目录中。
2. 在命令行终端中进入仓库目录。
3. 使用git add命令将文件添加到暂存区。你可以使用git add .将所有修改过的文件添加到暂存区，或使用git add 文件名添加指定文件。
4. 使用git commit命令提交更改到本地仓库，并提供相关的提交说明。
```
git commit -m "提交说明"
```


## 步骤4：推送代码

1. 使用`git push`命令将本地仓库的代码推送到GitHub远程仓库
```
git push origin 主分支名称
```

注意替换"主分支名称"为你要推送到的分支名称，通常是"main"或"master"。


## 步骤5：分支管理

1. 创建新分支：
    - 使用`git branch`命令查看分支列表。
    - 使用`git branch 分支名称`命令创建一个新分支。
2. 切换分支：
    - 使用`git checkout 分支名称`命令切换到指定分支。

## 步骤6：合并代码

1. 在GitHub网站上进行合并：
    - 进入仓库页面，点击"New pull request"按钮。
    - 选择要合并的源分支和目标分支。
    - 进行代码审查和讨论。
    - 确认无误后，点击"Merge pull request"按钮进行合并。


## 步骤7：代码同步

1. 保持本地仓库与GitHub远程仓库的代码同步：
    - 在本地仓库目录下，使用`git pull`命令拉取远程仓库的最新代码。
```
git pull origin 主分支名称
```

注意替换"主分支名称"为你的主分支名称。





# 方法三:

## 一、在IDEA中配置Git

### 1.1 配置Git
在IDEA中使用Git，本质上是使用安装在本地的git软件，（并不是使用远程仓库，或者说使用码云Gitee）。

打开IDEA，点击File -> Setting （快捷键是CTRL+alt+S），搜索Git并配置Git的安装目录
![9bba069ca56c45be9f7a5c4e3d724a9f.png](https://img-blog.csdnimg.cn/9bba069ca56c45be9f7a5c4e3d724a9f.png)

### 1.2 获取Git仓库
在IDEA中使用Git获取仓库的两种方式：

|                                                            |                                                            |
| ---------------------------------------------------------- | ---------------------------------------------------------- |
| 本地初始化仓库                                                    | 从远程仓库克隆                                                    |
| 相当于  git init 命令，其实就是把自己的项目变成一个git仓库，Create Git Repository | 相当于  git clone 命令，其实是把远程仓库的项目拷贝下来，Get From Version Control |


本地初始化仓库步骤：

（1）新建一个项目

这里以新建Maven项目为例，不知道如何新建Maven项目可以查看这篇文章，IDEA创建Maven项目

![2ebcc94a73a242ea916a10702e406120.png](https://img-blog.csdnimg.cn/2ebcc94a73a242ea916a10702e406120.png)

 （2）将这个项目所在目录变成一个Git仓库，从而实现用Git管理这个项目
![d35c99e22a1a4b68a12ae276afdc956d.png](https://img-blog.csdnimg.cn/d35c99e22a1a4b68a12ae276afdc956d.png)

![91d4e0e41dbb48f8b04ec0bf3dfa2367.png](https://img-blog.csdnimg.cn/91d4e0e41dbb48f8b04ec0bf3dfa2367.png)


![cc95cf6ab261494da2170f92aa750490.png](https://img-blog.csdnimg.cn/cc95cf6ab261494da2170f92aa750490.png)

（3）创建好本地仓库后，IDEA中会出现几个Git操作的图标

![ea24934f060941e9b70e8eb4a617dce4.png](https://img-blog.csdnimg.cn/ea24934f060941e9b70e8eb4a617dce4.png)

**从远程仓库克隆步骤：**

从远程仓库获取是以后工作中用的更多的，因为当你进入到某家公司的时候，往往项目已经开发到一定阶段了，因此此时我们要做的就是从远程仓库获取项目。

（1）点击VCS，点击Get From Version Control

当使用IDEA新建项目并添加到本地仓库之后，有些版本的IDEA认为你已经使用了VCS，所以不会出现VCS了。

此时可以点击File -> New -> Project From Version Control进行从远程仓库克隆代码。

![7557d5def73949f7bd680776e8e92f73.png](https://img-blog.csdnimg.cn/7557d5def73949f7bd680776e8e92f73.png)

### 1.3 将本地项目推送到远程仓库

**将本地项目推送到远程仓库时遇到的问题：**

**1.拒绝Push推送**

![bbf15bed2a544ef3a7c5c64fdd4da6c8.png](https://img-blog.csdnimg.cn/bbf15bed2a544ef3a7c5c64fdd4da6c8.png)

 这个报错的意思大概是，更新被拒绝是因为远程仓库的部分文件本地仓库没有，这通常发生在本地新建仓库之后第一次要推送到远程仓库时。

**解决方案：**

（1）先拉取远程仓库的分支（一般为master，origin一般指远程仓库的名字）

**2.在Pull时遇到的问题-拒绝合并不相关的请求**

![8abc07b744c147db93fa59f2cd815723.png](https://img-blog.csdnimg.cn/8abc07b744c147db93fa59f2cd815723.png)

 出现这个问题的最主要原因是本地仓库和远程仓库实际上是两个独立的仓库。
 
 **解决方案：**

 点击IDEA中的命令提示符，输入下面的命令，执行即可。

![f0c950fc8c6e4f0eab7dc4e140eaa8cf.png](https://img-blog.csdnimg.cn/f0c950fc8c6e4f0eab7dc4e140eaa8cf.png)

```java
git pull origin master --allow-unrelated-histories
```

![9a34f95890c24e0aa97d46af09c36d32.png](https://img-blog.csdnimg.cn/9a34f95890c24e0aa97d46af09c36d32.png)

（2）再次push

![607c737468be4913828a4e9a2c14e695.png](https://img-blog.csdnimg.cn/607c737468be4913828a4e9a2c14e695.png)

 至此，将本地仓库的项目推送至远程仓库成功！


### 1.4 .gitignore文件的作用

.gitignore文件用于定义哪些文件不需要交给Git管理，文件名是固定的，不可修改。

（1）.gitignore文件的生成

在创建远程仓库时，可以创建这个文件。

![e0d3d06ed801432098f8aade2dc3381f.png](https://img-blog.csdnimg.cn/e0d3d06ed801432098f8aade2dc3381f.png)

（2）.gitignore对应的常用Java模板

```java
# Compiled class file
*.class
 
# Eclipse
.project
.classpath
.settings/
 
# Intellij
*.ipr
*.iml
*.iws
.idea/
 
# Maven
target/
 
# Gradle
build
.gradle
 
# Log file
*.log
log/
 
# out
**/out/
 
# Mac
.DS_Store
 
# others
*.jar
*.war
*.zip
*.tar
*.tar.gz
*.pid
*.orig
temp/
```

## 二、本地仓库操作

### 2.1 将文件加入暂存区

![cb8f922903274b3ba0794f541282f10f.png](https://img-blog.csdnimg.cn/cb8f922903274b3ba0794f541282f10f.png)

 此时如果要将文件加入暂存区，就是相当于 git add 这个命令，

未暂存的文件是红色，已暂存的文件是蓝色。

（1）可以点击IDEA中的这个按钮：

![92adc393fb2e4d138b13a05b2b8cd85e.png](https://img-blog.csdnimg.cn/92adc393fb2e4d138b13a05b2b8cd85e.png)

2） 或者右击文件选择Add：

![fa71241a83fc498bb1b2be811755c031.png](https://img-blog.csdnimg.cn/fa71241a83fc498bb1b2be811755c031.png)

（3）IDEA设置版本控制工具自动Add

![421c7fc6d94e49abad271731d42a8b12.png](https://img-blog.csdnimg.cn/421c7fc6d94e49abad271731d42a8b12.png)

### 2.2 将暂存区的文件提交到版本库

这个操作相当于 git commit -m 这个命令，点击IDEA中的图标或者右击都可以提交。

![09cfd8a4bdb64fcfb4eff02fb703d5a6.png](https://img-blog.csdnimg.cn/09cfd8a4bdb64fcfb4eff02fb703d5a6.png)



### 2.3 [查看日志](https://so.csdn.net/so/search?q=%E6%9F%A5%E7%9C%8B%E6%97%A5%E5%BF%97&spm=1001.2101.3001.7020)

相当于 git log 这个命令，点击IDEA中的Git图标

![ee13c0bf38af489690d08be7d472af47.png](https://img-blog.csdnimg.cn/ee13c0bf38af489690d08be7d472af47.png)

![fba9b4e8683a4eb6ab60f0d262a243f0.png](https://img-blog.csdnimg.cn/fba9b4e8683a4eb6ab60f0d262a243f0.png)

## 三、远程仓库操作

### 3.1 查看和添加远程仓库

![c3f96192c70b47e7977174c617bf205f.png](https://img-blog.csdnimg.cn/c3f96192c70b47e7977174c617bf205f.png)

![b72e85da398546ef979a5e769695c904.png](https://img-blog.csdnimg.cn/b72e85da398546ef979a5e769695c904.png)


### 3.2 推送至远程仓库

![7758c001abe34add8d8d092f2fbed5ae.png](https://img-blog.csdnimg.cn/7758c001abe34add8d8d092f2fbed5ae.png)


### 3.3 从远程仓库拉取

![f06b2f422a0d45a697309cc3b89b73c6.png](https://img-blog.csdnimg.cn/f06b2f422a0d45a697309cc3b89b73c6.png)

![c1efca0a9fb6403e9c15f89e56fb720f.png](https://img-blog.csdnimg.cn/c1efca0a9fb6403e9c15f89e56fb720f.png)

## 四、分支操作

在IDEA中可以进行查看分支，创建分支，切换分支，推送分支到远程仓库，合并分支的操作。

![997e3d12fa0d40b9908618aa0b1e7984.png](https://img-blog.csdnimg.cn/997e3d12fa0d40b9908618aa0b1e7984.png)

![885b5ce2bb914e6e99c8576df3d30818.png](https://img-blog.csdnimg.cn/885b5ce2bb914e6e99c8576df3d30818.png)


切换分支时遇到的问题：

 1.如果当前分支还有未提交的内容，即unchanges files

那么切换分支后当前分支未修改的内容会带到另外一个分支上去。

解决方案：

分支修改后要先提交再切换

2.切换分支后弹出提示  Workspace associated with branch 'master' has been restored

与分支“master”关联的工作区已恢复

解决方案：

暂不理会，x掉即可

![84dce61ec74749d2a4138ea1bc9ac2ab.png](https://img-blog.csdnimg.cn/84dce61ec74749d2a4138ea1bc9ac2ab.png)

![f857af27ade640578d4b1430e610e9dc.png](https://img-blog.csdnimg.cn/f857af27ade640578d4b1430e610e9dc.png)



# 方法四:

## 1. 配置 Git

想要在 IDEA 中使用 Git，首先就需要在 IDEA 工具中对 Git 进行相关的配置。

打开 IDEA 设置，之后按照下面图片中的步骤进行操作即可。需要注意的是，在 Path Git executable 中，也就是设置 Git 可执行路径中，找到本机电脑上 Git 的安装位置，根据情况进行路径选择。例如，我这里的路径是 E:\Git\cmd\git.exe，而在 IDEA 中这里默认是 C 盘路径下的目录，这里要根据个人情况而定，不可忽视。

![在这里插入图片描述](https://img-blog.csdnimg.cn/adfe03b70e9443cd818c0102fac00691.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)

## 2. 创建项目远程仓库

这里使用代码托管平台 Gitee 为例，进行创建远程仓库。如下图所示，新建仓库，仓库名称为 redis-demo（自定义），仓库介绍自定义没有影响，其他都按照默认设置即可，最后点击创建。

![在这里插入图片描述](https://img-blog.csdnimg.cn/43a40387f0f24048910400d42bf5c5ce.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)  

这样一个远程仓库的项目就创建完成了。

## 3. 初始化本地仓库

创建完远程仓库之后，还要通过 IDEA 开发工具来初始化一个本地仓库，相比于 Git Bash 这样的命令行来说，IDEA 非常方便可以进行可视化的 Git 管理。

如下图所示，在工具栏中的 [VCS](https://so.csdn.net/so/search?q=VCS&spm=1001.2101.3001.7020)，鼠标光标移动到 Import Version Control -> Create Git Repository，点击 Create Git Repository。

![在这里插入图片描述](https://img-blog.csdnimg.cn/da5d9f4aed2d44f6a32c4fc83488f03a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)  

点击 Create Git Repository 之后，弹出一个小窗口，选择要初始化的本地项目。这里就以打开的项目 redis-demo 为例，对 redis-demo 进行 Git 本地初始化。

![在这里插入图片描述](https://img-blog.csdnimg.cn/412d433b89dd4180b835d2d29c9f9ef0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)

## 4. 连接远程仓库

初始化本地仓库，并且创建完远程仓库项目之后，下一步就需要将本地和远程进行对接交互了。那么在交互之前，肯定是要进行建立连接的，就像打电话之前先呼号建立通话连接一样，之后再进行信息交换。

工具栏 VCS -> Git -> Remotes，点击 Remotes 添加远程仓库连接信息。

![在这里插入图片描述](https://img-blog.csdnimg.cn/931a697024a046ed9e5f0dc46b52571e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)  

添加 Remote，输入 Name 和 URL，这里的 Name 如果没有特殊设置就是按照默认的 origin 来，也是 IDEA [自动补全](https://so.csdn.net/so/search?q=%E8%87%AA%E5%8A%A8%E8%A1%A5%E5%85%A8&spm=1001.2101.3001.7020)的，建议在创建远程项目的时候不要轻易修改，这里的 URL 就是在远程项目信息中可以看到，我就不放出来了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/cd748ca18f6e4a9ab8201c7441e55f63.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)  

之后弹出一个登录 gitee 的窗口，输入自己在注册 gitee 的账号和密码即可，然后点击 Log In.


![在这里插入图片描述](https://img-blog.csdnimg.cn/5d261c9b68b94f30b6efc3f264fdc701.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_16,color_FFFFFF,t_70,g_se,x_16)  

验证通过之后，Git Remotes 中便出现了远程项目的 Name 和 URL，最后点击 OK，这样就正式连接远程仓库项目了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/5c325bec6a7f4de195281f2efd6b239b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)

## 5. 提交到本地仓库

在工具栏中，点击绿色的图标，表示 commit 到本地仓库。

- 弹出一个 Commit Changes 窗口，然后，选择我们的项目 redis-demo，会自动选中项目中的有变化的文件，第一次 commit 整个项目，会把所有项目文件进行 commit.
- **一般地，在 commit 操作之前，需要仔细查看每个文件细节内容，避免因为鼠标键盘无意按压动作导致对代码文件带来非本意的修改，这是要养成的习惯。**
- 在 commit Message 中填入提交记录说明，最后点击右下角的 commit，提交项目 redis-demo 到本地仓库。

![在这里插入图片描述](https://img-blog.csdnimg.cn/df22a23b1472434bbb818c4412c571a8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)

如果项目存在 [warnings](https://so.csdn.net/so/search?q=warnings&spm=1001.2101.3001.7020)，即使项目没有错误可以正常运行，IDEA 还是会给出提示，询问是否要 review 一下，这里我直接忽略，直接 Commit 提交即可。

![在这里插入图片描述](https://img-blog.csdnimg.cn/464fa7139467441389868d28d6d37da5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_14,color_FFFFFF,t_70,g_se,x_16)


提交完成之后，在 IDEA 界面下边信息提示栏中，Log 日志选择 Version Control，可以看到提交记录，表示提交成功。

![img](https://img-blog.csdnimg.cn/746359808a274489a7e0926e84ec1f08.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)

## 6. 推送到远程仓库

推送项目到远程仓库，按照下图操作即可。**在 push 之前需要对每个文件内容进行检查，避免因为鼠标键盘无意按压动作导致对代码文件带来非本意的修改。**推送成功之后，可以在 IDEA 中看到提示信息，也可以在远程仓库中查看结果。

![在这里插入图片描述](https://img-blog.csdnimg.cn/5bc3ff4214b24170bc66677df28a39b5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)

## 7. 克隆远程仓库到本地

![在这里插入图片描述](https://img-blog.csdnimg.cn/6536acb966dd4ba18201a433232a45ee.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/da31b572839e4b55beb806d17917fdb1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2O5LuU5LuUY29kaW5n,size_20,color_FFFFFF,t_70,g_se,x_16)

## 8. 分支操作

### 8.1 新建分支

在实际项目开发过程中，当遇到一个需求时，我们并不会直接就在主分支(master)上面进行开发，而是在主分支的基础上单独创建一个新的分支进行开发，当新建分支的代码经过代码评审以及测试验证通过，确认准确无误之后才会将新增的代码合并到主分支master，这才是一个规范的git操作流程。

因此，必须要掌握新建分支的过程，那么创建一个分支的过程如下图所示：

1、首先，单击 “Remote Branches” 下的 “origin/master” 分支，单击选择 “New Branch from Selected…”

![在这里插入图片描述](https://img-blog.csdnimg.cn/d72b52a9b4c54c1b9574e4100c7f5a96.png)  

2、其次，弹出对话框，输入你要新建的分支名称。例如，feature-1.1.0，单击“Checkout”，表示创建该分支同时切换到 feature-1.1.0 分支。

![在这里插入图片描述](https://img-blog.csdnimg.cn/8bc4effb38bc4503991748c9352c1061.png)


2、然后，创建分支 feature-1.1.0 成功之后，可以在 “Local Branches” 下看到此时有两个分支分别为feature-1.1.0 和 master，这表明本地仓库中已经有了新建的分支了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/c8def4057857406fb0c25d891168c937.png)

3、但是 “Remote Branches” 下依然只有一个 master 分支，表明在我们的远程仓库依然只有一个master 分支，因此我们还需要把在本地仓库新建的分支推送到远程仓库中，这样就可以在远程仓库中看到新建的分支了。

单击 feature-1.1.0 ，然后在左边单击 “Push” ，弹出推送对话框。单击 “Push” 推送到远程仓库，这样分支新建操作彻底完成，可以看到在"Remote Branches"分支下面出现了“feature-1.1.0”，我们可以在该分支进行编码并进行拉取和推送操作。

![在这里插入图片描述](https://img-blog.csdnimg.cn/749041e34a884629b8f17a246bbe5a35.png)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/f5b3e2c5a1cc4c2ebf54fbf85d7f9b64.png)

### 8.2 删除分支

在删除分支之前，建议先切换到(Checkout)其他分支，例如，Checkout到master分支，当然这并不强制，只是为了方便删除分支而已。

1、首先，在 “Local Branches” 下单击分支 feature-1.1.0，左边弹出选项选中 “Delete”.

![在这里插入图片描述](https://img-blog.csdnimg.cn/8e6abf29e280484abc908604775eb81f.png)

2、然后，在对话框中勾选“Delete tracking local feature-1.1.0 as well”，意思就是同时删除远程和本地仓库中的 feature-1.1.0 分支。

![在这里插入图片描述](https://img-blog.csdnimg.cn/25b0054b0056437e9e04ce31a5c5c6e6.png)  

删除成功之后，出现提示信息 “Deleted Branch:feature-1.1.0”，并且可以在 Git Branches 下看到最新的分支情况。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3767a2c46582474ea96e92a74fa41f48.png)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/af649e371763435488b9f191fa8f934c.png)

### 8.3 比较分支

分支比较就是对比不同分支之间的差异。例如，需要比较当前所在分支与 master 分支之间的差异，可以选择需要对比的 master 分支，然后单击 “Compare with Current”，在弹出的窗口可以看到具体的差异。

![在这里插入图片描述](https://img-blog.csdnimg.cn/78b2bcd319ab4bdda0e511c386f4179c.png)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/c89627cd14714d8b8c8c9557780fab00.png)

### 8.4 合并分支

合并分支是将 A 分支合并到 B 分支，A 分支称为源分支，B 分支称为目标分支。

> **注意事项**：master 分支是我们部署在生产环境服务器的分支，master 分支的代码往往是最新的代码。通常我们有一个新的需求时不会直接在 master 分支进行实施编码，而是基于 master 分支拉出一个新的分支，在拉出来的新分支进行实施编码。只有当新分支的代码经过 code review 以及测试联调通过之后，才会将代码合并到 master 分支等待部署上线。合并分支的流程务必规范：先将 master 分支合并到新分支，之后再将新分支合并到 master 分支。 这样做的原因是因为一个项目通常由多个人负责，可能在你开发的过程中，有新的代码已经合并到 master 分支并且部署上线了，但是你拉的新分支上的代码依旧是 master 分支的老代码。所以需要先更新你拉的新分支上的代码，将其他人更新的代码合并过来，保证你拉的新分支代码与最新的 master 分支代码一致（以免因为丢失最新修改的代码而造成线上事故）之后再将你的新拉的分支代码合并到 master 分支上面。

**以 feature-1.1.0 分支和 master 分支为例，将 feature-1.1.0 分支合并到 master 分支中。**

1、首先，切换到 feature-1.1.0 分支，将 master 分支合并到 feature-1.1.0 分支中。单击 “Local Branches” 下的 master，选择 “Merge into Current”，这里的 Current 是指目标（feature-1.1.0）分支 ，也就是将 master 分支合并到 feature-1.1.0 分支中，合并成功之后可以看到提示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/01e8529b8c21439b975460d9d15a9151.png)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/0aae89f6295844b7acffdd930dd91f6c.png)  

2、然后，切换到 master 分支下，将 feature-1.1.0 分支合并到 master 分支。

![在这里插入图片描述](https://img-blog.csdnimg.cn/d8ef594460d5495987827993a3543cb7.png)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/9f90b635d99d488fb89ca0d10d2c8a45.png)  

3、最后，我们可以看到在 “Local Branches” 下的 master 分支有个绿色的小箭头，这是因为我们刚才的分支合并在本地仓库操作的，并没有将远程的 feature-1.1.0 分支合并到 master 分支中。因此，我们还需要将 master 分支推送到远程仓库，点击 Push 或者使用快捷键 `Ctrl+Shift+K` 推送到远程仓库即可，推送成功之后绿色提示小箭头也会消失。

![在这里插入图片描述](https://img-blog.csdnimg.cn/c8666a0aa0cf40aab5b272c481154b70.png)

## 9. 几个常用快捷键

|快捷键|操作解释|
|---|---|
|`Ctrl+K`|commit changes(提交变化到本地仓库)|
|`Ctrl+Shift+K`|push commits to push(将本地仓库的变化同步推送到远程仓库)|
|`Ctrl+T`|update project(将远程仓库的代码合并更新到本地)|
|`Ctrl+Alt+Z`|Rollback(代码回滚)|























# 问题

## 1.如果push不上服务器
在控制台中:
git config --global http.proxy http://127.0.0.1:输入代理端口
git config --global https.proxy http://127.0.0.1:输入代理端口

![[Pasted image 20240514181828.png](img/Pasted%20image%2020240514181828.png)