# day10 git

## 一、Git历史

同生活中的许多伟大事件一样，Git 诞生于一个极富纷争大举创新的年代。Linux 内核开源项目有着为数众广的参与者。绝大多数的 Linux 内核维护工作都花在了提交补丁和保存归档的繁琐事务上（1991－2002年间）。到 2002 年，整个项目组开始启用分布式版本控制系统 BitKeeper 来管理和维护代码。

到 2005 年的时候，开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了免费使用 BitKeeper 的权力。这就迫使 Linux 开源社区（特别是 Linux的缔造者 Linus Torvalds ）不得不吸取教训，只有开发一套属于自己的版本控制系统才不至于重蹈覆辙。他们对新的系统订了若干目标：

• 速度

• 简单的设计

• 对非线性开发模式的强力支持（允许上千个并行开发的分支）

• 完全分布式

• 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

![img](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510001544.jpg) 

## 二、Git与SVN对比

### 2.1 Svn

SVN是`集中式`版本控制系统，版本库是集中放在中央服务器的，而干活的时候，用的都是自己的电脑，所以首先要从中央服务器哪里得到最新的版本，然后干活，干完后，需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工作，如果在局域网还可以，带宽够大，速度够快，如果在互联网下，如果网速慢的话，就郁闷了。

下图就是标准的集中式版本控制工具管理方式：

![img](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510001540.png)

集中管理方式在一定程度上看到其他开发人员在干什么，而管理员也可以很轻松掌握每个人的开发权限。

但是相较于其优点而言，集中式版本控制工具缺点很明显：

l 服务器单点故障

l 容错性差

### 2.2  Git

Git是分布式版本控制系统，那么它就没有中央服务器的，每个人的电脑就是一个完整的版本库，这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。既然每个人的电脑都有一个完整的版本库，那多个人如何协作呢？比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

下图就是分布式版本控制工具管理方式：

 

![img](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510001725.jpg) 

## 三、git的工作流程

<img src="https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121524229.png" alt="image-20250312152428159" style="zoom:50%;" />

名词解释：

1. 远程仓库：gitee和github以及gitlab，云上的数据，都称之为远程仓库。
2. 工作区：平时，日常敲代码的地方。
3. **暂存区**：比如新增了一个文件，或者多个，通常会提示你需要add一下。
4. 本地仓库：本地文件夹下有一个.git的隐藏文件，这个就是本地仓库内容。



下图，是本地管理的一个逻辑图。**commit后，暂存区 stage 就没有数据了**。

![image-20250312162054393](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121620494.png)







### 1. 本地git管理《新增文件后流程》

1. 本地的一个空文件夹，首先必须现在本地创建一个版本库。然后这个文件夹里面就会出现一个.git的隐藏文件。

   ![image-20250312153216853](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121532894.png)

2. 创建一个文件：1.txt，然后这个文件，默认是在 工作区。

3. 在1.txt上面右键，就会出现一个 add的选项。点击后，1.txt就在 暂存区。

   ![image-20250312153549218](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121535260.png)

4. 再次右键点击1.txt就会出现，git提交的选项，提交就是commit，默认提交到本地的**master分支**上。

   ![image-20250312153631102](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121536137.png)

5. 提交时候需要注意和填写的内容如下图。已经提交到本地仓库的master分支里面，作为了第一个本地版本。

   ![image-20250312154044766](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121540825.png)

### 2. 本地git管理《修改文件后的流程》

1. txt文件中，编辑内容后，其实我们写的东西都是在**暂存区**。请问，后面怎么操作？不需要add了，只需要commit即可。

2. commit，然后重复上面第一步里面的第4和5小点的动作即可。

   ![image-20250312154913666](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121549723.png)

### 3. 本地git管理《误删文件后的流程》

> 三个方案恢复备份：
>
> 1. 右键还原
> 2. 日志还原commit了的版本
> 3. diff比较不同，当前的工作区和master分支的head 指向的最新分支之间的差异，然后手动判断还原。

1. 第一个方案，直接还原，在本地仓库中，右键选择 还原。

   ![image-20250312162354476](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121623540.png)

2. 查看日志，发现下图显示 工作树变更，工作树其实就是  工作区 workspace发生了变化。

   ![image-20250312162706033](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121627111.png)

3. 可以选择任意之前commit的版本，然后恢复到当前那个版本下。

![image-20250312163035006](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121630083.png)



4. diff 比较不同

   ![image-20250312163408354](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121634398.png)



## 四、和远程仓库对接

> 下节课，把大家拉到我的仓库中来，随便玩，问题越多越好。

> 问题的原因就在于，本地有一个本地仓库，远端对不上，就会报错。如下图：

![image-20250312170659513](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121706575.png)

第一次合并的时候，可以使用命令来执行。

```cmd
git.exe pull --progress -v --no-rebase --allow-unrelated-histories "origin" master
```

查看日志，合并相关历史后，接下来是 push 还是commit？下图可知，本地的版本和远端的版本不一致，要高一个版本。直接push。

> push要把，本地的master，推送到远程的origin/master上。然后，下图的红色和黄色的标记，就会合并为一个。表示本地和远程仓库的版本一致。

![image-20250312171520293](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121715366.png)

push后，远程仓库如下图：

![image-20250312171941277](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121719372.png)

本地的日志：

![image-20250312172034400](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121720467.png)





### 1. 出现问题的情况

> 本地文件有修改，但是没有commit，直接pull拉取代码，会出现下图报错。
>
> Please commit your changes or stash them before you merge.
>
> 在合并之前，请提交您的更改或暂存它们。
>
> stash是暂存的意思。

![image-20250312173048275](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121730332.png)

如果说，我本地只commit了，但是没有pull拉过代码，直接想要push，如果远程仓库没有改变说可以推送成功的（常见于单机自己玩）。

在你直接push的时候，会提示你，先git pull 再push。

![image-20250312173633576](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121736641.png)

就是merge的作用，叫做冲突的解决！

![image-20250312173728964](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121737022.png)

出现版本冲突，很常见。



![image-20250312173906642](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121739701.png)

查看远端，发现，它改了我的文件，这个是非常常见的现象，以后公司里面，一定会有这个问题。



![image-20250312174215717](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121742803.png)

点击解决。

推荐手动cv。

![image-20250312174909800](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121749952.png)



下一个方案，如果是idea中的代码出现类似这种冲突。

冲突的文件，然后选择merge

![image-20250312175332153](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121753253.png)



![image-20250312175417255](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121754322.png)





![image-20250312175529127](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121755198.png)



推荐在idea中merge，它操作人性化一点。



## 五、简单版通用git使用方式!

### 步骤:

### 1.打开gitee官网.登录

![image-20210510142335960](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510142336.png)





### 2.新建仓库

![image-20210510142535714](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510142535.png)

### 3.先添加仓库成员

不添加成员的话, 小组成员只能下载 ,不能提交代码!

在项目首页, 点击管理!

![image-20210510142754214](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510142754.png)

![image-20210510142912951](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510142913.png)

### 4.可以拉项目下来

首先 现在网页上 点击克隆

![image-20210510143216609](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510143216.png)

右键点击 git 克隆菜单, 弹入这个框:

![image-20210510143151220](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510143151.png)

### 5.修改拉下来的代码 提交上去

提交, 使用push(推送)按钮

### 6.需要注意

每次push之前, 必须pull 拉取代码, 如果有冲突, 就需要解决冲突(要么以云端的代码为主, 要么以自己的代码为主),  或者合并冲突(自己手动 合并两套代码)

### 7.简单版名词定义

pull  拉取云端最新代码(别人已经推送成功的代码)

拉取下来后,  **解决冲突!**

**合并好**的代码后,  先commit提交,  再push到云端上!

## 六、远程仓库(进阶版)

建库  ssh

### 7.1 生成/添加SSH公钥

[SSH Key](https://gitee.com/help/labels/19) [SSH 公钥](https://gitee.com/help/labels/29)

Gitee 提供了基于SSH协议的Git服务，在使用SSH协议访问仓库仓库之前，需要先配置好账户/仓库的SSH公钥。

解决了一个, 每次提交需要输入账号密码的情况!

你可以按如下命令来生成 sshkey:

```
ssh-keygen -t rsa -C "xxxxx@xxxxx.com"  
# Generating public/private rsa key pair...
```

> 注意：这里的 `xxxxx@xxxxx.com` 只是生成的 sshkey 的名称，并不约束或要求具体命名为某个邮箱。
> 现网的大部分教程均讲解的使用邮箱生成，其一开始的初衷仅仅是为了便于辨识所以使用了邮箱。

按照提示完成三次回车，即可生成 ssh key。

(linux这么玩)通过查看 `~/.ssh/id_rsa.pub` 文件内容，获取到你的 public key

```
cat ~/.ssh/id_rsa.pub
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc....
```

![image-20210510172810189](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510172810.png)

复制生成后的 ssh key，通过仓库主页 **「管理」->「部署公钥管理」->「添加部署公钥」** 

添加后，在终端（Terminal）中输入

```
ssh -T git@gitee.com
```

首次使用需要确认并添加主机到本机SSH可信列表。若返回 `Hi XXX! You've successfully authenticated, but Gitee.com does not provide shell access.` 内容，则证明添加成功。

![](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210510172820.png)

添加成功后，就可以使用SSH协议对仓库进行操作了。

### 7.2仓库公钥和可部署公钥

为了便于用户在多个项目仓库下使用一套公钥，免于重复部署和管理的繁琐，Gitee 推出了「可部署公钥」功能，支持在一个仓库空间下使用当前账户名下/参与的另一个仓库空间的部署公钥，实现公钥共用。

演示了只有

## 七、本地莫名其妙文件直接推送git方案(错误解决办法)

出现该错误的原因主要是因为远程库有README这个文件，而本地库没有该文件。



`出现fatal: refusing to merge unrelated histories这个错误时，`

运行： git pull origin master –allow-unrelated-histories



`再次错误：fatal: Couldn't find remote ref –allow-unrelated-histories`



则运行：git pull --rebase origin master

然后再运行：git push origin master