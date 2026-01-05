macOS上可以安装多个版本的jdk，方法如下：

### 1.下载jdk

在Oracle官网上下载不同版本的jdk： 

https://www.oracle.com/java/technologies/downloads/#java17

![在这里插入图片描述](https://img-blog.csdnimg.cn/78893b077c884ca3a6032e1f128a9268.png)

## 方案一

### 1.查看本机所有的jdk
```java
/usr/libexec/java_home -V
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/0ac793c7a9bc4f88b335d1d7bd28326c.png)

### 3. 配置环境变量

![在这里插入图片描述](https://img-blog.csdnimg.cn/3ac0794bde7545bb8d4900f806a1fa0b.png)



在终端执行命令`echo $SHELL`获得终端类型：

- `/bin/zsh` => `zsh` => `.zshrc`
- `/bin/bash` => `bash` => `.bash_profile`

```bash
#修改默认Shell为zsh
chsh -s /bin/zsh 
```

 如果看到的是`/bin/zsh`

```bash
#配置JAVA_HOME
sudo echo 'export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk-1.8.jdk/Contents/Home'>> ~/.zshrc
sudo echo 'export JAVA_17_HOME=/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home'>> ~/.zshrc
sudo echo 'export JAVA_21_HOME=/Library/Java/JavaVirtualMachines/jdk-21.jdk/Contents/Home'>> ~/.zshrc
sudo echo 'export PATH="$JAVA_HOME:$PATH"'>> ~/.zshrc
source ~/.zshrc
```

如果看到的是`/bin/bash`

```bash
#配置JAVA_HOME
sudo echo 'export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-1.8.jdk/Contents/Home'>> ~/.bash_profile
sudo echo 'export PATH="$JAVA_HOME:$PATH"'>> ~/.bash_profile
source ~/.bash_profile
```


`vim ~/.bash_profile`
`vim ~/.zshrc`
打开bash_profile文件。可以在终端vim ~/.bash_profile打开，也可以打开访达shift+cmd+G然后输入/Users/mac/.bash_profile（本机bash_profile的路径）

```java
# jdk
export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk-1.8.jdk/Contents/Home
export JAVA_17_HOME=/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home

export JAVA_HOME=$JAVA_8_HOME
JRE_HOME=$JAVA_HOME/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
export JRE_HOME
export PATH
export CLASSPATH

alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
alias jdk17="export JAVA_HOME=$JAVA_17_HOME"
```

执行命令

`source ~/.zshrc`

### 4.切换jdk

我们定义了别名：jdk8和jdk11和jdk17。

![在这里插入图片描述](https://img-blog.csdnimg.cn/8085acd0261b4c6d95f6870e0d47fd28.png)


方案二：用jenv
参考文章见

https://blog.csdn.net/weixin_46019681/article/details/126923331

参考文章
Mac 多版本jdk安装与切换
https://zhuanlan.zhihu.com/p/567928991

Mac安装多个版本JDK并实现切换【保姆级笔记超简单】
https://blog.csdn.net/weixin_46019681/article/details/126923331
