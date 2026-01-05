## Docker自定义构建镜像

可能绝大多数情况下，Docker的镜像都是来自网络，但是也有可能我们自己有会去构建一些我们自己的镜像

#### 怎么做？

在构建Docker镜像时，需要使用一个文件，文件的名固定为：Dockerfile

#### Dockerfile4部分

Dockerfile 分为四部分：基础镜像信息、维护者信息、镜像操作指令和容器启动时执行指令

##### 基础镜像信息

```java
#设置基础镜像
FROM centos
```

##### 维护者信息

```java
MAINTAINER 565599455@qq.com
```

##### 镜像操作指令

```java
#复制压缩包到镜像中，并进行解压
ADD ./jdk-8u11-linux-x64.tar.gz /usr/local/java
ADD ./apache-tomcat-8.5.20.tar.gz /usr/local/tomcat
#set environment variable
ENV JAVA_HOME /usr/local/java/jdk1.8.0_11
ENV JRE_HOME $JAVA_HOME/jre  
ENV CLASSPATH .:$JAVA_HOME/lib:$JRE_HOME/lib  
ENV PATH $PATH:$JAVA_HOME/bin
#EXPOSE 映射端口
EXPOSE 8080
```

##### 容器启动时执行指令

```java
#ENTRYPOINT 配置容器启动时，需要执行的文件
ENTRYPOINT /usr/local/tomcat/apache-tomcat-8.5.20/bin/startup.sh && tail -F /usr/local/tomcat/apache-tomcat-8.5.20/logs/catalina.out
```

#### Dockerfile的详细命令介绍

**FROM :** 指定基础镜像，要在哪个镜像建立

FROM centos

**MAINTAINER：**指定维护者信息

MAINTAINER 565599455@qq.com

**RUN：**在镜像中要执行的Linux命令

**ADD:** 相当于 COPY，但是比 COPY 功能更强大(如果是压缩包，会自动解压)

**COPY ：**复制本地主机的 （为 Dockerfile 所在目录的相对路径）到容器中的

**ENV：**定义环境变量

**WORKDIR：**指定当前工作目录，相当于 cd ，为后续的 RUN 、 CMD 、 ENTRYPOINT 指令配置工作目录。

**EXPOSE：**指定容器要打开的端口

**VOLUME：**挂载目录，创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。

格式为VOLUME ["/data"]

**ENTRYPOINT** 指定容器在运行时，需要执行的文件，或者需要命令的操作的文件

**CMD** 指定容器在运行时，需要运行的命令

#### Dockerfile的示例

##### 1、构建Tomcat

```java
#设置基础镜像
FROM centos
MAINTAINER 565599455@qq.com
#复制压缩包到镜像中，并进行解压
ADD ./jdk-8u11-linux-x64.tar.gz /usr/local/java
ADD ./apache-tomcat-8.5.20.tar.gz /usr/local/tomcat
#set environment variable
ENV JAVA_HOME /usr/local/java/jdk1.8.0_11
ENV JRE_HOME $JAVA_HOME/jre  
ENV CLASSPATH .:$JAVA_HOME/lib:$JRE_HOME/lib  
ENV PATH $PATH:$JAVA_HOME/bin
#EXPOSE 映射端口
EXPOSE 8080
#ENTRYPOINT 配置容器启动时，需要执行的文件
ENTRYPOINT /usr/local/tomcat/apache-tomcat-8.5.20/bin/startup.sh && tail -F /usr/local/tomcat/apache-tomcat-8.5.20/logs/catalina.out
```



##### 2、构建Nginx

```java
# Base images 基础镜像
FROM centos

#MAINTAINER 维护者信息
MAINTAINER tianfeiyu 

#ENV 设置环境变量
ENV PATH /usr/local/nginx/sbin:$PATH

#ADD  文件放在当前目录下，拷过去会自动解压
ADD nginx-1.8.0.tar.gz /usr/local/  
ADD epel-release-latest-7.noarch.rpm /usr/local/  

#RUN 执行以下命令 
RUN rpm -ivh /usr/local/epel-release-latest-7.noarch.rpm
RUN yum install -y wget lftp gcc gcc-c++ make openssl-devel pcre-devel pcre && yum clean all
RUN useradd -s /sbin/nologin -M www

#WORKDIR 相当于cd
WORKDIR /usr/local/nginx-1.8.0 

RUN ./configure --prefix=/usr/local/nginx --user=www --group=www --with-http_ssl_module --with-pcre && make && make install

RUN echo "daemon off;" >> /etc/nginx.conf

#EXPOSE 映射端口
EXPOSE 80

#CMD 运行以下命令
CMD ["nginx"]
```

#### 构建自己的Tomcat

1、在Linux中选择一个目录/root/docker/，将JDK以及Tomcat的压缩包上传至该目录

![image-20201225101252064](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201225101252.png)

2、创建Dockerfile文件

![image-20201225101331244](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201225101331.png)

3、编写Dockerfile内容

```java
#设置基础镜像
FROM centos
MAINTAINER 565599455@qq.com
#复制压缩包到镜像中，并进行解压
ADD ./jdk-8u271-linux-x64.tar.gz /usr/local/java
ADD ./apache-tomcat-8.5.61.tar.gz /usr/local/tomcat
#set environment variable
ENV JAVA_HOME /usr/local/java/jdk1.8.0_271
ENV JRE_HOME $JAVA_HOME/jre  
ENV CLASSPATH .:$JAVA_HOME/lib:$JRE_HOME/lib  
ENV PATH $PATH:$JAVA_HOME/bin
#EXPOSE 映射端口
EXPOSE 8080
#ENTRYPOINT 配置容器启动时，需要执行的文件
ENTRYPOINT /usr/local/tomcat/apache-tomcat-8.5.61/bin/startup.sh && tail -F /usr/local/tomcat/apache-tomcat-8.5.61/logs/catalina.out
```

4、执行该文件，构建一个Tomcat镜像出来

Dockerfile在哪个目录下，就在哪个目录执行如下命令：

```java
docker build -t woniu/tomcat:latest --rm=true .
```

> docker build 构建一个镜像
>
> woniuxy/tomcat 镜像的名称
>
> Latest 镜像的版本 eg:1.0  2.0  latest最新版本
>
> --rm=true . 我也不知道，这是个啥鬼东西，反正就是不能省略（注意：最后还有一个   .）

5、运行容器

```java
docker run -d -p 8080:8080 --name woniutomcat woniu/tomcat:latest
```

在浏览器，即可查看效果！

Dockerfile的内容，到此结束！！！