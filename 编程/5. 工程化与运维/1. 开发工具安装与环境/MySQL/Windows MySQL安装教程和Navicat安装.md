
# MySQL安装使用


## 1.下载MySQL

**Mysql官网下载地址[MySQL :: Download MySQL Installer (Archived Versions)](https://downloads.mysql.com/archives/installer/ "MySQL :: Download MySQL Installer (Archived Versions)")**

![](https://img-blog.csdnimg.cn/386a530f61e244f78428eff7765dc031.png)
## 2.解压

得到以下目录

![](https://img-blog.csdnimg.cn/ce441aad2003457b8b6efdfb413d72fb.png)

## 3.配置my.ini

```java
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
 
[mysqld]
#设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=D:\\mysql-soft\\mysql8               ----安装的路径
# 设置mysql数据库的数据的存放目录
datadir=D:\\mysql-soft\\mysql8\\data         -----数据存储的路径
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

配置完成后记得将my.ni放到你的MySQL安装包的目录里

![](https://img-blog.csdnimg.cn/6617f942d4ba460eb1312867fdd5bb0b.png)

## 4.执行命令

注：必须以管理员身份运行cmd

![](https://img-blog.csdnimg.cn/87cdf9d7c90047858845c07ccda5166b.png)

**然后进入你安装mysql中的bin目录：**

如：进入D盘，则输入d：

        然后cd +空格+ 具体的路径：cd mysql-8.0.17-winx64\bin 回车即可

![](https://img-blog.csdnimg.cn/644bed73a40b4122a0e5c750d24b6732.png)

### 1.[安装MySQL](https://so.csdn.net/so/search?q=%E5%AE%89%E8%A3%85MySQL&spm=1001.2101.3001.7020)服务：mysqld -install

![](https://img-blog.csdnimg.cn/d8be29ebce954c0b85524119a8b6ea47.png)

### 2.[初始化](https://so.csdn.net/so/search?q=%E5%88%9D%E5%A7%8B%E5%8C%96&spm=1001.2101.3001.7020)MySQL    mysqld --initialize --console

### 3.[启动MySQL](https://so.csdn.net/so/search?q=%E5%90%AF%E5%8A%A8MySQL&spm=1001.2101.3001.7020)服务    net start mysql

![](https://img-blog.csdnimg.cn/e4964fed63254fca9aceada5af01b9f6.png)

### 4.记录初始密码，利用初始密码登录    mysql -P 3306 -u root -p

![](https://img-blog.csdnimg.cn/bc57fd52eeb743cbb262c4309b125717.png)

### 5.改变MySQL链接密码

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';

6.退出数据库
mysql的命令是exit（此处不需要分号），关闭数据库的操作：先退出mysql目录，然后到其它任意的目录，输入命令：net stop mysql 

 到这里就差不多啦

接下来我们将安装一个MySQL的工具

5.安装Navicat
Navicat官网下载地址https://navicat.com.cn

![](https://img-blog.csdnimg.cn/f87159c767b148669eec66eb41955f5f.png)

 
 下载好之后解压就可以用啦
