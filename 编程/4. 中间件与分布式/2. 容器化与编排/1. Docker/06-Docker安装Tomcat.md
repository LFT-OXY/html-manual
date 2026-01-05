## Docker安装Tomcat

在https://hub.daocloud.io/网站上，输入tomcat进行镜像查找

#### 下载镜像

```java
docker pull daocloud.io/library/tomcat:8.5.15-jre8
```

![image-20201224173926740](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224173926.png)

#### 检验下载

![image-20201224174018545](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224174018.png)

#### 启动Tomcat

```java
docker run -d -p 8080:8080 --name mytomcat daocloud.io/library/tomcat:8.5.15-jre8
```

![image-20201224174215596](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224174215.png)

#### 准备war包

自己干，参见文档07部分的内容

#### 上传war包

rz      上传命令

sz 文件名  下载命令

![image-20201224174521235](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224174521.png)

#### 挂载到Tomcat容器中

```java
docker run -d -p 8080:8080 --name mytomcat --privileged=true -v /root/tomcat/webapps:/usr/local/tomcat/webapps daocloud.io/library/tomcat:8.5.15-jre8
```

![image-20201224175440221](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224175440.png)

注意：--privileged=true 该选项一定要加

