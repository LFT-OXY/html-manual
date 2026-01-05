在本机Mac OS上安装JDK以及配置环境，本篇是最详细的教程，细化每个步骤过程，保姆级的教程！

目录

一、下载JDK安装包

二、安装

三、查询安装路径

四、配置环境变量

五、使配置生效

————————————————————正文分割线——————————————————

一、下载JDK安装包
到oracle官网下载适合的JDK安装包，选择Mac OS对应的版本。


![](https://img-blog.csdnimg.cn/4386f2cf33a44a2184193b7791fda6ea.png)

下滑页面，点击macOS找到我们系统对应的版本

如果是M系列芯片的选择 Arm 64 DMG installer 对应的链接点击即可自动跳转下载

如果是intel系列的选择 X64 DMG installer 对应的链接点击即可自动跳转下载

### **二、安装**

下载完成后双击进行安装，跟着提示依次点击下一步即可完成安装。

安装完成后我们测试一下是否正确完成安装

打开终端terminal，输入：java -version

![](https://img-blog.csdnimg.cn/8672f351b6354ee2bbb4de1af1b49eb6.png)

 显示以上信息即代表已经正确完成安装！

### **三、查询安装路径**

安装完成后我们需要查看所在的路径

在终端terminal输入命令：`/usr/libexec/java_home -V`

![](https://img-blog.csdnimg.cn/0dd58ecee27d4f9e939944e1bd2f14de.png)

显示以上信息，最后一行即为路径

复制备用，后面需要用到。

### **四、配置环境变量**

1.在终端terminal，输入：ls -a

此命令的作用是显示当前目录下的所有文件及文件夹

![](https://img-blog.csdnimg.cn/10cc987243b34eeb835e497da1fdf8d0.png)

2.查看一下是否有.bash_profile文件，此文件用于配置环境的（如果有此文件则跳过第3步）

3.若没有则需要新建此文件

在终端terminal输入命令：`touch .bash_profile`

此命令就是创建一个文件

4.新建了上述文件之后使用命令打开文件

在终端terminal输入命令：`open -e .bash_profile`

此命令是打开文件

![](https://img-blog.csdnimg.cn/29e00f70ed674da9b623edf91d23e045.png)

新建的文件打开后是一个空白的

5.在空白的文件里输入以下的代码：
```java
JAVA_HOME=这里输入您前面复制的路径
PATH=$JAVA_HOME/bin:$PATH:.
CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
export JAVA_HOME
export PATH
export CLASSPATH
```


![](https://img-blog.csdnimg.cn/874c1c148dc644a2a218563239840151.png)

 如图并保存即可！

（跳过第三步的，不需要新建文件的小伙伴，打开.bash_profile文件后可能会发现里面并不是空白的，是有了一些代码的。不要慌张，无视那些代码，换行后直接粘贴并保存我们的代码即可。）


五、使配置生效
1.重新打开终端terminal输入命令：

`source .bash_profile`

（此命令的作用是读取和执行.bash_profile文件，使我们刚才的配置生效）

2.接着在终端terminal输入命令

`echo $JAVA_HOME`

（此命令的作用是显示刚才配置的信息）

![](https://img-blog.csdnimg.cn/4ae2f0c2b3b8457a804786e5d869caf5.png)

如图显示即代表成功！