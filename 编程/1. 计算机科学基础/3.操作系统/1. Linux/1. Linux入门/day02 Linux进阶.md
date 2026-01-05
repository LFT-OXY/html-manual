
## 一、Linux命令详情

### 1. 命令的分类

1. 内部命令：Linux集成一个shell解释器壳子（/bin/bash壳子），内部的内置命令，系统启动就加载好了这些内部命令。
2. 外部命令：shell解释器自带的命令之外的东西，安装的第三方的东西，包括自己写的shell脚本。

> 1. linux是区分大小写，同时，linux是基于文件的一个操作系统，所有的操作，包括网络连接，都是会在内部创建出一个文件。
>
> /dev/tcp/192.168.12.22/8000。
>
> 2. linux是不看文件后缀的！xxx.sh 没有后缀，它也是一个shell脚本。

### 2. 命令格式

```cmd
命令字 选项 参数
```

- 命令字：命令的名字，在系统里面是能够定位到对应的文件的。

  - ```cmd
    [root@localhost ~]# which cd
    /usr/bin/cd
    ```

- 选项：

  - 单字符选项：ls -al 就是 ls -a -l，单字符选项是可以写一起。
  - 多字符选项：--all，就是ls -a 的长字符形式，长字符不能写一起。  ls --all --format=long

- 参数:

  - 对应选项后的值：

  - ```cmd
    mv -bv *.txt /home/office
    -bv 选项
    第一个参数：*.txt
    第二个参数：/home/office
    把第一个参数移动到第二个参数的文件夹下(如果office不是文件夹而是文件，则会重命名)
    ```

### 3. 帮助-用的最多的

- help方式： help mv  语法

- --help方式 ： -h, --help            显示此帮助消息并退出

  ```cmd
  [root@localhost ~]# yum -h
  已加载插件：fastestmirror
  Usage: yum [options] COMMAND
  解释Usage语法：[options]中括号表示可选， COMMAND必填
  List of Commands:
  
  check          检查 RPM 数据库问题
  check-update   检查是否有可用的软件包更新
  clean          删除缓存数据
  erase          从系统中移除一个或多个软件包
  groups         显示或使用、组信息
  help           显示用法提示
  history        显示或使用事务历史
  info           显示关于软件包或组的详细信息
  install        向系统中安装一个或多个软件包
  
  Options:
    -h, --help            显示此帮助消息并退出
    -t, --tolerant        忽略错误
    -C, --cacheonly       完全从系统缓存运行，不升级缓存
    -c [config file], --config=[config file]
                          配置文件路径
    -R [minutes], --randomwait=[minutes]
                          命令最长等待时间
    -d [debug level], --debuglevel=[debug level]
                          调试输出级别
    --showduplicates      在 list/search 命令下，显示源里重复的条目
  ```

- man：最详细的帮助手册，也有中文版补丁，自己ai百度去查。

  ```cmd
  # 中文版man手册
  yum update
  yum install man-pages-zh-CN
  ```

示例：

```cmd
[root@localhost ~]# cp --help
用法：cp [选项]... [-T] 源文件 目标文件
　或：cp [选项]... 源文件... 目录
　或：cp [选项]... -t 目录 源文件...
Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.


cp [选项]... -t 目录 源文件...
-t是cp的选项，目录是-t的参数，不包含后面的源文件... 源文件是 cp的参数
-t,  --target-directory=目录  将所有参数指定的源文件/目录
                                           复制至目标目录
                                           
cp [选项]... --target-directory=目录 源文件...
```



### 4. 管道符和重定向 重点

**管道符**： | 管道符支持多层，命令A | 命令B| 命令C

![image-20250228104442533](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202502281044625.png)

```cmd
ps -ef | grep tomcat
上面过滤 tomcat的进程
kill -9 15686
下面这个就是根据pid，来杀死进程。
```

![image-20250228104917763](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202502281049861.png)

0是stdin

1是stdout

2是stderr

& 混合结果 包含1和2



**重定向**: 管道符也是一种重定向，1重定向到0，重新把某个结果，定向到另外一个地方。

输出重定向，大于，> 。

追加重定向，大大于，>> 

```cmd
ls -al > ~/devshow.txt 
默认把 stdout 就是1 重定向到后面的文件当中。

1>   前面的标准stdout重定向到后面内容
2>   前面的标准stderr重定向到后面的内容

2> /dev/null 就让报错信息，直接消失。

echo abc >> 1.txt
echo efg >> 1.txt
[root@localhost ~]# cat 1.txt 
abc
edf
```

输入重定向，小于，< 。

## 二、查看和检索文件

### 1. 查看文件

- **cat**(重要)：直接一口气查看一堆文件。最好用。

- more：可以分页查看，b上一页，q是退出，空格是翻页。

- less：less /etc/ssh/sshd_config 不污染shell环境。

- head：默认查看文件头10行

- **tail**(重要的)：默认查看文件尾部10行，常用于日志文件查看，-f 持续查看

  ```cmd
  tail -f /var/log/messages 
  ```

### 2. 统计和检索文件内容

- wc：统计文件的字节数、字数、行数

  ```cmd
  -c # 统计字节数，或--bytes：显示Bytes数。
  -l # 统计行数，或--lines：显示列数。
  -w # 统计字数，或--words：显示字数。一个字被定义为由空白、跳格或换行字符分隔的字符串。
  ```

- grep 检索过滤文件内容

  ```cmd
  [root@localhost ~]# grep ftp /etc/passwd
  ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
  ```

  他支持正则表达式：

  ```cmd
  ^    # 锚定行的开始 如：'^grep'匹配所有以grep开头的行。    
  $    # 锚定行的结束 如：'grep$' 匹配所有以grep结尾的行。
  .    # 匹配一个非换行符的字符 如：'gr.p'匹配gr后接一个任意字符，然后是p。    
  *    # 匹配零个或多个先前字符 如：'*grep'匹配所有一个或多个空格后紧跟grep的行。    
  .*   # 一起用代表任意字符。   
  []   # 匹配一个指定范围内的字符，如'[Gg]rep'匹配Grep和grep。    
  [^]  # 匹配一个不在指定范围内的字符，如：'[^A-Z]rep' 匹配不包含 A-Z 中的字母开头，紧跟 rep 的行
  \(..\)  # 标记匹配字符，如'\(love\)'，love被标记为1。    
  \<      # 锚定单词的开始，如:'\<grep'匹配包含以grep开头的单词的行。    
  \>      # 锚定单词的结束，如'grep\>'匹配包含以grep结尾的单词的行。    
  x\{m\}  # 重复字符x，m次，如：'0\{5\}'匹配包含5个o的行。    
  x\{m,\}   # 重复字符x,至少m次，如：'o\{5,\}'匹配至少有5个o的行。    
  x\{m,n\}  # 重复字符x，至少m次，不多于n次，如：'o\{5,10\}'匹配5--10个o的行。   
  \w    # 匹配文字和数字字符，也就是[A-Za-z0-9]，如：'G\w*p'匹配以G后跟零个或多个文字或数字字符，然后是p。   
  \W    # \w的反置形式，匹配一个或多个非单词字符，如点号句号等。   
  \b    # 单词锁定符，如: '\bgrep\b'只匹配grep。  
  ```

> 进阶的：sed ，awk 难度太高，不用涉及。

## 三、备份和解压

### 1. gzip   gunzip命令

> 备份后的文件名字，默认是叫做 xxx.gz文件。

### 2. tar 重点命令

> 大家见的最多的压缩包：
>
> xxx.tar         纯用tar来压缩，没有使用-z选项。  -z选项就是用  gzip来进行压缩。
>
> xxx.tar.gz     +了-z选项后，压缩的文件名字， 多一个.gz。

```cmd
记住：
1. 压缩
tar -zcvf 用来压缩文件
2. 解压
tar -zxvf 用来解压文件  .tar.gz

解释:
-z 使用的是 gzip来压缩和解压，后缀 是否有 .gz
-c 压缩
-x 解压
-v verbers 详细过程信息展示，不加不影响使用
-f 表示
```

