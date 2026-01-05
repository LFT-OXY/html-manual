# Linux安装Git

## 前言

服务器版本：CentOS7.8
git官网：https://git-scm.com/download/linux
有两种安装方法第一种，[yum安装](https://so.csdn.net/so/search?q=yum安装&spm=1001.2101.3001.7020)，直接输入一行命令就可以进行安装了

```bash
yum install git
```

![image-20250404151235491](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151235491.png)

![image-20250404151245407](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151245407.png)

![image-20250404151252405](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151252405.png)





输入 git --version查看Git是否安装完成以及查看其版本号
默认安装位置：`/usr/libexec/git-core`



![image-20250404151305535](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151305535.png)





但是推荐自定义安装，为什么？看看官网最新版本是多少了



![image-20250404151328514](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151328514.png)





而且还不能安装到我们指定的位置，所以推荐大家自定义安装



## 第一步，官网下载安装包



https://git-scm.com/download/linux



![image-20250404151358136](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151358136.png)



## 第二步，解压安装包

```bash
tar -zxvf git-2.34.1.tar.gz 
```

![image-20250404151742435](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151742435.png)



![image-20250404151748050](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151748050.png)



## 第三步，安装编译环境

```bash
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
```



![image-20250404151813227](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151813227.png)



安装上面编译环境的时候，yum自动帮你安装了git，这时候你需要先卸载这个旧版的git。
否则还是一个老版本



![image-20250404151830697](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151830697.png)



```bash
yum remove git
```



## 第四步，编译源码

进入刚刚解压的git文件夹中

```bash
make prefix=/usr/local/git all
```



![image-20250404151941257](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151941257.png)





prefix里面填你想要放的文件夹位置



![image-20250404151947176](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404151947176.png)



## 第五步，安装git

```bash
make prefix=/usr/local/git install
```



![image-20250404152015522](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404152015522.png)



这个时候一定要在git-2.34.1的文件夹下执行
执行之后就在这个文件夹里面出现了以下的情况



![image-20250404152024858](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404152024858.png)





## 第六步，配置环境变量

```bash
vim /etc/profile
```



![image-20250404152041672](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404152041672.png)



```bash
export GIT_HOME=/usr/local/git
export PATH=$GIT_HOME/bin:$PATH
```





刷新配置文件

```bash
source /etc/profile
```



最后输入git --version就会发现已经成功安装最新版本！

![image-20250404152101495](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250404152101495.png)