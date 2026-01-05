# day07 Docker补充

## 一、docker简介

![img](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101555121.jpeg)

Docker 是一种开源的虚拟进程技术，**基于GO语言**进行开发的。

Docker 可以让**开发者自己打包我们自己的应用或依赖包**到一个轻量级的，可移植的容器中去。然后，再一发布，那么就可以在所有的Linux上进行运行，一次构建，到处使用

Docker容器技术，一定是依托Linux操作系统存在！

镜像(集装箱)产生出来的程序，我们叫“容器”（打开使用的集装箱）。容器完成使用一种“沙箱”机制。

> 集装箱房子。麻雀虽小五脏俱全，开箱即用。

#### 沙箱的概念

沙箱是一种**虚拟进程技术**。并且对现有的Linux系统不会产生任何的影响。

1、搭建测试，生产环境

2、发布自己的应用程序

#### 使用Docker的原因

1、保证开发，测试，上线环境统一

2、更快速的交付和部署（不推荐用docker来部署项目，也不要用docker来安装mysql）

3、提供比虚拟机更为高效的虚拟技术

4、可以更轻松的迁移或者横向扩展

#### Docker 和Vmware虚拟机的区别

VM 是一个运行在宿主机之上的完整的操作系统，VM运行时 ，需要消耗宿主机大量系统资源，例如：CPU,内存，硬盘等一系列的资源。Docker容器与VM不一样，它只包含了应用程序以及各种依赖库。

正因为它占用系统资源非常少，所以它更加的轻量。它在启动时，可能只需要简单的一个命令就可以了。启动仅仅只需要几秒或几十秒钟就可以完成。对于宿主机来讲，承担VM可能5-10个就已经非常厉害了 ，但是Docker容器很轻松就承担几千个。而且网络配置相对而言也比较简单，主要以桥接方式为主。

<img src="https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101555709.png" alt="img" style="zoom:200%;" />

#### Docker的应用场景

<img src="https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101555440.jpeg" alt="img" style="zoom:200%;" />



开发，测试，生产环境上使用

#### Docker容器的3个概念

镜像：是一种特殊的文件系统，它包含了：程序，资源，配置等信息（可以想象为Java中的类）

容器：是镜像的实例，镜像是一种静态的体现，而容器是一种动态的**实例**（可以想象为Java中的对象）

仓库：专门用于放置**镜像**的地方，镜像可以从网络上下载，也可以自己去产生（和maven仓库一样）

## 二、Docker引擎的安装

安装Docker，首先需要先确定你的操作系统一定是Linux。并且的Linux的内核版本一定要 > 3.8以上

#### 检查Linux的内核版本

```java
uname -r
```

![image-20201224121732099](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101555183.png)

#### 更新yum 

```java
yum update
```

![image-20201224121912817](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101555308.png)

出现锁定的问题：https://jingyan.baidu.com/article/6079ad0eb5f6a628ff86dbe6.html

阿里云的contos7安装教程

https://help.aliyun.com/zh/ecs/use-cases/install-and-use-docker?spm=5176.21213303.J_qCOwPWspKEuWcmp8qiZNQ.19.27a52f3dTb8d0p&scm=20140722.S_help@@%E6%96%87%E6%A1%A3@@51853._.ID_help@@%E6%96%87%E6%A1%A3@@51853-RL_docker-LOC_llm-OR_ser-V_4-RE_new5-P0_3#4787944e3bwid



> 上面这个链接可以快捷跳转登录 个人镜像加速源

#### 安装Docker的依赖插件

```java
yum install -y yum-utils device-mapper-persistent-data lvm2
```

![image-20201224140255091](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101555670.png)

#### 配置国内Docker加速源

```java
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

![image-20201224140455828](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101555272.png)

#### 检查yum中关于Docker的版本

```java
yum list docker-ce --showduplicates | sort -r
```

#### 安装Docker



```cmd
过期了，不考虑用：yum install docker-1.13.1-162.git64e9980.el7.centos.x86_64

官方的下载方式
yum -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

![image-20201224140758481](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101556653.png)

依赖包，这里输入：y

#### 检查Docker版本

```java
docker -v
```

#### 设置Docker开机启动

```java
systemctl enable docker
```

#### 开启Docker引擎

```java
systemctl start|restart|stop docker
```

![image-20201224141306863](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101556770.png)

到此，安装结束！

## 三、配置国内源

镜像：可以来自于自己构建，也可以来自于网络（不要用阿里的镜像源，国内都不好用。）

Docker镜像原本来自国外，地址：https://hub.docker.com/

![image-20201224142001175](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101556089.png)

从国外服务器获取 镜像时，由于跨了国际，可能非常慢，此时就需要配置国内镜像加速源

##### 创建配置目录

```java
sudo mkdir -p /etc/docker
```

##### 配置加速器

> 大家自己去找一下 能用的镜像源 加速:
>
> bash <(curl -sSL https://gitee.com/xjxjin/scripts/raw/main/check_docker_registry.sh)

```java
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors":
   [
        "https://docker.xuanyuan.me",
        "https://docker.1ms.run",
        "https://dockers.xuanyuan.me",
        "https://docker.linkedbus.com",
        "https://atomhub.openatom.cn",
        "https://docker.m.daocloud.io"
   ]
}
EOF
```

![image-20201224143046045](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101556643.png)

##### 重新启动Docker服务

依次执行，下面的2条命令

```java
sudo systemctl daemon-reload
sudo systemctl restart docker
```

![image-20201224143220535](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207062336157.png)

到此，Docker容器使用之前的准备工作，就告一段落！



## 四、安装mysql

### 1. pull

> 友情提示，在公司是不让用docker去安装mysql的！

```cmd
docker pull mysql

#如果需要固定版本
docker pull mysql:5.7
```

### 2. images

用来查看当前下载好的镜像文件。

```cmd
docker images

[root@localhost yum.repos.d]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
mysql        latest    567107cb6971   5 weeks ago   797MB

```

### 3. run

> 把镜像文件启动一个容器出来。

```cmd
[root@localhost ~]# docker  run  --name  mysql  -p  3308:3306 --privileged=true  -e MYSQL_ROOT_PASSWORD=123456 -d 56a8c14e1404
ebe7265ad93f50f15ca8c879dbb54be9c0ea56455156cf969ce58d25ab946c09

[root@localhost yum.repos.d]# docker run  --name  mysql  -p  3308:3306  -d   567107cb6971
```

>解释下：
>
>1. --name 是指定容器的名字   ：用于后期快速重启启动停止容器的命令, docker restart|start|stop mysql
>2. -p 对外端口:容器内部端口， 外部访问3308，就会转发到容器内部的mysql3306端口。
>3. --privileged=true  表示容器内部的启动账号权限是 root权限，有管理员权限。
>4. -e 是这个容器自带的选项，MYSQL_ROOT_PASSWORD=123456，没有这个你run的时候是有提示的，需要你配置下密码。
>5. -d 指定镜像名字mysql:latest 或IMAGE ID 

### 4. ps

> docker ps -a 可以查看没有启动的 所有容器
>
> docker ps 查看正在运行的 up状态的 容器

```cmd
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE          COMMAND                   CREATED          STATUS          PORTS                                                  NAMES
ebe7265ad93f   56a8c14e1404   "docker-entrypoint.s…"   40 seconds ago   Up 39 seconds   33060/tcp, 0.0.0.0:3308->3306/tcp, :::3308->3306/tcp   mysql
```



```
[root@localhost ~]# docker  run  --name  mysql5.7  -p  3307:3306 --privileged=tru
e  -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```

### 5. Docker的前台模式运行MySQL

```JAVA
docker run --name mysql01 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=12345 -i -t --privileged=true mysql:5.7 /bin/bash
```

> -i  以前台运行模式进行启动docker容器
>
> -t 为容器重新分配一个伪输入终端，通常与 -i 同时使用
>
> /bin/bash 让容器启动完毕后，开启一个内部进程

![image-20201224152356635](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557168.png)

这种模式有个特点是：退出容器内部，Docker容器将会自动关闭

### 6. 前台模式和后台模式的区别

前台模式的特点：启动之后就立马进入到docker内部

后台模式的特点：启动之后，我们的程序依旧在物理上

**推荐使用：**后台模式启动

## 五、Docker容器的常见命令

#### 1、docker ps 查看当前正在运行的容器实例

这条命令的作用：显示当前正在运行的容器

![image-20201224153106238](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557035.png)

> **显示的内容：**容器的ID，镜像的名称，docker容器启动内部程序的命令，容器的创建时间，容器的当前状态以及存在的时间，端口映射关系，容器的名字

#### 2、docker ps -a 查看当前正在运行，以及曾经运行过的容器实例

这条命令的作用：显示当前正在运行的容器，以及已经停止的容器

![image-20201224153443726](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557715.png)



#### 3、docker stop [容器的名称/容器的ID] 停止某一个容器

这条命令的作用：停止某一个容器

![image-20201224153735397](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557635.png)



#### 4、docker rm  [容器的名称/容器的ID] 从docker环境中，移除某一个容器

这条命令的作用：将某一个停止的容器，从Docker环境中移除掉

![image-20201224153829032](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557788.png)



#### 5、docker logs [容器的名称/容器的ID] 查看docker的日志输出

这条命令的作用：查看Docker容器的日志输出

![image-20201224154013749](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557081.png)



#### 6、docker exec -it [容器的名称/容器的ID] /bin/bash 从宿主机进入到Docker容器内部

这条命令的作用：进入到Docker容器内部，实际上就是：将后台模式转换为前台模式

![image-20201224154304502](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557006.png)



#### 7、docker inspect [容器的名称/容器的ID] 查看容器的详细信息

这条命令的作用：查看容器的详细信息

![image-20201224154948932](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557731.png)



#### 8、查看容器的IP地址

如果要查看某一个容器的内部IP地址 ，请使用下面的命令

docker inspect --format='{{.NetworkSettings.IPAddress}}' [容器的名称/容器的ID]  

![image-20201224155050957](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557022.png)



#### 9、docker start [容器的名称/容器的ID]  启动容器

这个命令的作用：开启一个出于停止状态下的容器

![image-20201224155234979](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557788.png)



#### 10、docker restart [容器的名称/容器的ID]  重启容器

这个命令的作用：重新启动一个容器

![image-20201224155349566](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557506.png)



#### 11、docker rmi [镜像的名称:版本号/镜像的ID]  删除镜像

这个命令的作用：删除镜像

![image-20201224160004473](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101557190.png)

## 六、idea中远程链接docker

![image-20250110122241181](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202501101222333.png)



![image-20250110122208356](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202501101222471.png)

## 七、run的选项 -v 挂载

> mysql，redis，持久化数据，如果都存放在docker中，那么如果不小心删掉了容器，那么就没了。
>
> docker run -v 物理磁盘:容器文件

```cmd
[root@localhost ~]# docker  run  --name  mysql5.7 v1 -v /var/mysql:/var/lib/mysql  -p  3307:3306 --privileged=true  -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
#/var/mysql 物理机的一个存储位子
```



