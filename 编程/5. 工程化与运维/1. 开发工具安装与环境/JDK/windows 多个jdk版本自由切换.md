#### JDK安装

##### 1.点击[JDK官方下载地址](https://www.oracle.com/technetwork/java/javase/downloads/index.html "JDK官方下载地址")下载

![](https://i-blog.csdnimg.cn/blog_migrate/836db2a4b75917adfa44ba5102e0ca41.png)这里还能选择之前的版本下载
![](https://i-blog.csdnimg.cn/blog_migrate/ba925d386e50e9aec65e5d08e512decd.png)

2）根据电脑[操作系统](https://so.csdn.net/so/search?q=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F&spm=1001.2101.3001.7020)和位数选择对用的包。Windows的选Windows ，电脑为64位的选x64,32位选x86。安装下载不在赘诉，安装地址可改变，不要有中文。



![](https://i-blog.csdnimg.cn/blog_migrate/a1281ee49a82c6c32c52b13980635c1c.png)

![](https://i-blog.csdnimg.cn/blog_migrate/d1b9ca3130a542c240dbdacc35707ec3.png)
#### 2.环境配置

**1.在安装完之后，打开电脑设置->系统->关于->高级系统设置->高级->环境变量。**

**2.在系统变量中分别设置多个环境变量值，如下表：**

|环境变量名称|环境变量值|
|---|---|
|JAVA8_HOME|D:\JAVA\jdk1.8|
|JAVA17_HOME|D:\JAVA\jdk17|
|JAVA21_HOME|D:\JAVA\jdk21|
|JAVA_HOME|%JAVA17_HOME%|
说明：首先设置一个变量JAVA_HOME，后面JAVA8_HOME，JAVA17_HOME，JAVA21_HOME代表为三个版本取了三个变量名，接着分别是他们的路径。在切换版本时，只需要把第一个JAVA_HOME的变量值改为需要的变量。（环境变量名称按照下载的版本设置，值按安装的地址配置）

![](https://i-blog.csdnimg.cn/blog_migrate/5f1430911695345b8b7d7d8468f9f166.png)![](https://i-blog.csdnimg.cn/blog_migrate/25192b180a455f6deaafb275c3203703.png)![](https://i-blog.csdnimg.cn/blog_migrate/e060b471e1e92211a38c2f3666cc5b43.png)
 **3、新建系统变量，变量名称CLASSPATH，变量值：**

```crystal
.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar
```

![](https://i-blog.csdnimg.cn/blog_migrate/f0236e54f3ee575256c882a12d71a844.png)

**4、设置系统环境变量Path，加上变量值：%JAVA_HOME%\bin和%JAVA_HOME%\jre\bin**

![](https://i-blog.csdnimg.cn/blog_migrate/4a4528549cbde9accb59dce7b5f6f8b6.png)

**注意：这里的配置得放在Path的最前端，否则也可能测试不成功。**

##### **3.测试切换版本**

**(1)选择JDK17**
![](https://i-blog.csdnimg.cn/blog_migrate/c73388661240714d4b7cec438c8b5df0.png)

![](https://i-blog.csdnimg.cn/blog_migrate/29d30cb784c6d62fe34c38616665f004.png)

**（2）选择JDK8**
![](https://i-blog.csdnimg.cn/blog_migrate/30ebbaf538a5de519dd57150606b8c1f.png)

![](https://i-blog.csdnimg.cn/blog_migrate/8824f63f77896c857adc78432e43b3a1.png) 

**（3）选择JDK21**
![](https://i-blog.csdnimg.cn/blog_migrate/0b30aaa73820f07c609a070128c27982.png)

![](https://i-blog.csdnimg.cn/blog_migrate/123a92681405ba8bc13656da531b24fd.png)

##### 4.切换版本失败的原因

**1.将环境变量保存后重新启动cmd**

![](https://i-blog.csdnimg.cn/blog_migrate/6e60c1cb86c79a51781c3c0b403bfc21.png)

**2、删除javapath的Java文件**

如果切换不成功，这可能是安装jdk时，自动生成了优先级更高的配置项，使用以下方式解决：  
打开 `C:\ProgramData\Oracle\Java\javapath`。删除底下三个文件。
java.exe，javaw.exe，javaws.exe

3.删除注册表文件

从注册表（运行regedit）windows +R 输入regedit中 删除jdk的默认启动的版本。在HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft的Java Development Kit的CurrentVersion项的值和Java Runtime Environment的CurrentVersion项的值

![](https://i-blog.csdnimg.cn/blog_migrate/1e01e55287bdca8af1ddf26a79505084.png)

![](https://i-blog.csdnimg.cn/blog_migrate/97459f912460f16797f0caf3f6957883.png)
