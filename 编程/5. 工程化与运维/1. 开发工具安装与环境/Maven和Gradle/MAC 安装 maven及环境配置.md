# 1. 下载安装包。

> 从官网（[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)）下载 Maven 并解压。
> 
> 选择  apache-maven-3.9.1-bin.tar.gz
> 
> ![](https://img2023.cnblogs.com/blog/667853/202303/667853-20230329084816468-1506288472.png)
> 
>> # 2. 配置 Maven 环境变量。
> 
> ① 打开终端 ，
> 
> 如果使用的shell 是 bash    输入vim ~/.bash_profile
> 
> 如果使用的shell 是 zsh     输入  vim ~/.zshrc
> 
> ② 输入i进入编辑模式，然后输入 Maven 的环境变量：
> 
> `export M2_HOME=/Users/chinhae/Documents/JAVA/Maven/apache-maven-3.9.9` 
> **注意:这里填写的是你解压出来maven的文件夹路径**
> `export PATH=$PATH:$M2_HOME/bin`
> 
> 通过echo $JAVA_HOME查看是否配置过 JAVA_HOME ，如果 JAVA_HOME 没有配置，还需要导入 JAVA_HOME 环境变量：
> 
> export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home
> 
> ③ 编辑完后，按esc退出编辑，并输入:wq保存并退出。
> 
> ④ 输入source ~/.bash_profile   或者   source ~/.zshrc  使环境变量生效。
> 
> ⑤ 输入m查看 Maven 否安装成功。
> 
>   

> # 3.  进行setting.xml文件的配置，需要aliyun的镜像文件下载地址和下载依赖文件的仓库位置
> 
> 打开你下载的Maven：
> 
> apache-maven-3.6.1 ➡️ conf ➡️ settings.xml然后进行编辑
> 
> ## 3.1  配置私有仓库位置
> ![](https://img2023.cnblogs.com/blog/667853/202303/667853-20230329104527388-457534791.png)

替换成自己的位置 /Users/arctic.kong/Documents/dev_software/apache-maven-3.8.8/local_repo

![](https://img2023.cnblogs.com/blog/667853/202303/667853-20230329104649863-359588840.png)

## .2 配置阿里云镜像
```
<mirror>  
<id>maven-default-http-blocker</id>  
<mirrorOf>external:http:*</mirrorOf>  
<name>Pseudo repository to mirror external repositories initially using HTTP.</name>  
<url>http://0.0.0.0/</url>  
<blocked>true</blocked>  
</mirror>
```


用以下内容替换上面代码  
```
<mirror>  
<id>alimaven</id>  
<mirrorOf>central</mirrorOf>  
<name>aliyun maven</name>  
<url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
</mirror>  
```

# 4、准备工作已经完成，下面就可以去Idea中进行使用了

打开idea的项目后进行setting设置

![](https://img2023.cnblogs.com/blog/667853/202303/667853-20230329105349754-1868470253.png)

Maven home path是你下载的maven的路径

User setting file是Maven的setting.xml文件的路径

Local repository是你自己创建的放置依赖文件的仓库（最好创建在和maven同级别下）

注意⚠️：Override的✅一定要勾上要不然没有办法选择

最后点击OK即可使用