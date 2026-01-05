Linux安装redis详细教程
一、删除redis
查看reids 是否在运行，如果在运行的话，先关闭 。

ps -ef|grep redis

如果redis 没有设置密码启动，直接使用该命令
redis-cli shutdown

如果redis 设置密码启动，想关闭redis服务器：./redis-cli -a 密码 shutdown
./bin/redis-cli -a redis@123 shutdown

删除掉解压后的文件目录和所有文件

rm -rf /usr/local/redis-5.0.7

二、安装redis
1. 下载并将redis压缩包解压到指定的目录
这里是将redis解压到/opt文件夹下，可以使用-C指定到解压的文件夹

wget http://download.redis.io/releases/redis-5.0.7.tar.gz
tar -zvxf redis-5.0.7.tar.gz -C /usr/local/

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ea72cb1123acf3d072aafaa208e2c7ae.png)

##### 2. 安装gcc

redis是c语言编写的，所以我们需要进入解压的目录下安装gcc

```
cd redis-5.0.7/
yum -y install gcc
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/07d207e2b3487d866585f9de464637b5.png)



**安装成功后输入 : gcc -v 查看版本**
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5af333b86706e4e5f96005a77f50368f.png)



## 2.安装

将下载好的安装包放入任意的目录下，我一般放入opt下

![img](https://i-blog.csdnimg.cn/direct/7c400f40b50a4406b5a77d2377b5c5d7.png)

通过tar -zxvf 文件名 进行解压

```
tar -zxvf redis-5.0.4.tar.gz
```



![img](https://i-blog.csdnimg.cn/direct/b9392ce9cc944044addbb6169e16745a.png)

![img](https://i-blog.csdnimg.cn/direct/5061401c36f3457897b74133dc13d02a.png)

解压完成之后，会出现一个蓝色的redis文件夹 ，个人习惯一般会将这个文件夹移动到/usr/local/redis （像nginx 等中间件都会移动到 /usr/local下便于查找）

命令:

将redis-5.0.4移动到/usr/local/redis下：

```Haskell
mv redis-5.0.4 /usr/local/redis
```

 进入 /usr/local/redis 并通过 ls 命令查看

```Haskell
cd /usr/local/redis
ls
```

![img](https://i-blog.csdnimg.cn/direct/40960d5e8b5f4082a4cafff0b3dac959.png)

##  2.5安装前置环境准备:

需要安装c++ 通过命令 yum -y install gcc-c++

```Haskell
yum -y install gcc-c++
```

## 3.编译

1、进入到/usr/local/redis/之后 输入命令 make 进行编译

![img](https://i-blog.csdnimg.cn/direct/e546c3563b214e938cd92a430714eb35.png)

2、编译成功之后执行：make PREFIX=/usr/local/redis install

```Haskell
make PREFIX=/usr/local/redis install
```

![img](https://i-blog.csdnimg.cn/direct/54d35d6bd39847a98007bcf804dd1409.png)

执行成功之后会出现两个文件 分别为 bin文件夹 和 redis.conf 文件

![img](https://i-blog.csdnimg.cn/direct/f8a5b78063b04d86aa674aa58fe96f68.png)

##  4.配置redis.conf

# 注解： 创建保存配置文件的文件夹

mkdir redisconf

# 注解：将redis.conf拷贝进去

cp redis.conf redisconf/

# 注解： 进入 redisconf

cd redisconf/

#注解： 使用vim 修改文件

vim redis.conf 

![img](https://i-blog.csdnimg.cn/direct/421257eea9524c0da59788208ab2448b.png)

 2.进入之后输入：“:set number” 将行号显示出来

![img](https://i-blog.csdnimg.cn/direct/f62dfcff783140f6a1c850eb78ff8e9b.png)

3. 此次主要是修改简单的配置文件，可以后台启动，并且外部能访问。（一般项目的redis也只配置这么多）

 3.1、将69行注释 目的：允许内网通过ip能访问

![img](https://i-blog.csdnimg.cn/direct/cf1e6c4501d94bf4b6abf9447c26af2e.png)

3.2、将88行yes 改为no 目的：允许外网访问 

![img](https://i-blog.csdnimg.cn/direct/76b3f6d683694dea8059d55584bdc710.png)

3.3、改端口号不必多说 如果你的6379端口号已经被占用可以改 

![img](https://i-blog.csdnimg.cn/direct/40e667e521fc420083904f4060bef25a.png)

3.4、将136行改为yes 目的： 后台启动

![img](https://i-blog.csdnimg.cn/direct/f201e4d557cc46c5ab176f4fe4bec8fd.png)



edis密码修改:requirepass
（设置 redis 连接密码，图例密码为redis@123）
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e8c01234d818b4cfe00b39144fcb1d85.png)

配置完成之后保存 



## 5.[启动redis](https://so.csdn.net/so/search?q=启动redis&spm=1001.2101.3001.7020)

 1.进入 /usr/local/redis/bin下

```Haskell
cd /usr/local/redis/bin
```

![img](https://i-blog.csdnimg.cn/direct/96f3f824e0f1431a8bc3e45aced8e07b.png)

2.通过配置文件启动 ：/usr/local/redis/bin/redis-server /usr/local/redis/redisconf/redis.conf 

/usr/local/redis/bin/redis-server /usr/local/redis/redisconf/redis.conf 

![img](https://i-blog.csdnimg.cn/direct/7cab41dc989e49058182d38dbd752301.png)

3.查看是否启动成功

![img](https://i-blog.csdnimg.cn/direct/14eb91a96e664d569768fce16b67ee07.png)





##### 10. 关闭redis服务

（1) 通过kill命令：（暴力关闭，容易丢失数据）

```
#查看redis的进程pid
ps -ef|grep redis 
kill -9 pid
123
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/dd9681872e5f86a51810df369a434e77.png)（2) 通过redis-cli命令关闭：（正常用这个方式关闭)

```
 redis-cli shutdown
 # 如果redis 设置密码启动，想关闭redis服务器：./redis-cli -a 密码 shutdown
./bin/redis-cli -a redis@123 shutdown
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a797280290cecdf7eb9571472597fb86.png)



##### 创建远程连接

1. 使用命令登录redis机器，测试set get命令

   redis-cli -p 6379 -a redis@123

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/69f24c5b4902965d3b4eb1641e78d69c.png)



. 新增连接到redis 服务器，输入名称、ip地址、端口、密码。测试连接
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d1a9b8474fc73adc69d6fd07ad71aba4.png)



3. 可以查看redis的数据

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3847c5ea6f95ef211e2bf64622d15eb5.png)