# day04 redis应用

## 一、原生redis底层开发

> jedis使用，就和直接操作redis一样的命令。set，get，zadd，sadd，lpush等方法，底层都帮忙写好了。

服务端：理解ServerSocket 搭建服务端。

```java
package com.woniuxy.server;

import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @Author: 马宇航
 * @Todo: WebSocket开发一个伪装的Redis服务器
 * @DateTime: 25/03/04/星期二 10:33
 * @Component: 成都蜗牛学苑
 **/
public class MyJedisServer {
    public static void main(String[] args) throws IOException {
        // 1. 启动一个服务端的Socket, 监听6379端口
        ServerSocket serverSocket = new ServerSocket(6379);
        // 2. 等待客户端的连接, 相当于阻塞代码，会卡在这里，main线程
        while (true) {
            Socket sk = serverSocket.accept();
            InputStream inputStream = sk.getInputStream();
            // 3. 读取客户端发送过来的指令  临时存储缓冲区
            byte[] bytes = new byte[1024];
            // 4. 读取数据 到bytes数组中
            int len = inputStream.read(bytes);
            // 5. 把bytes数组中的数据转成字符串
            String str = new String(bytes, 0, len);
            System.out.println(str);
        }

    }
}
```

客户端：Socket 套接字，以及输入 输出流的关系，write和read之间怎么使用。

```java
package com.woniuxy.cli;

import java.net.Socket;

/**
 * @Author: 马宇航
 * @Todo: socket 来连接Redis服务器
 * @DateTime: 25/03/04/星期二 10:40
 * @Component: 成都蜗牛学苑
 **/
public class MyJedisClient {
    Socket sk;
    // 连接Redis服务器
    public MyJedisClient(String host, int port) throws Exception {
        sk = new Socket(host, port);
    }
    public MyJedisClient() {
    }
    //开发一个set方法
    /*
    *   redis的协议 resp：
    *   *3
    *   $3
    *   set
    *   $3
    *   key
    *   $5
    *   value
    */
    public void set(String key, String value) throws Exception {
        // 1. 把指令进行字符串拼接  这个性能好
        StringBuilder stringBuilder = new StringBuilder();
        // 2. 拼接*3 \r\n 表示退格换行
        stringBuilder.append("*3\r\n");
        // 3. 拼接$3 \r\n
        stringBuilder.append("$3").append("\r\n");
        // 4. 拼接set \r\n
        stringBuilder.append("set").append("\r\n");
        // 5. 拼接$num是多少？传入的key的长度 \r\n
        stringBuilder.append("$").append(key.length()).append("\r\n");
        stringBuilder.append(key).append("\r\n");
        // 6. 拼接$num是多少？传入的value的长度 \r\n
        stringBuilder.append("$").append(value.length()).append("\r\n");
        stringBuilder.append(value).append("\r\n");
        // 7. 把拼接好的字符串发送给Redis服务器
        sk.getOutputStream().write(stringBuilder.toString().getBytes());
        // 8. 刷新缓冲区
        sk.getOutputStream().flush();
    }
    //开发一个get方法
    /*
    *   redis的协议 resp：
    *   *2
    *   $3
    *   get
    *   $3
    *   key
    */
    public String get(String key) throws Exception {
        // 1. 把指令进行字符串拼接  这个性能好
        StringBuilder stringBuilder = new StringBuilder();
        // 2. 拼接*2 \r\n 表示退格换行
        stringBuilder.append("*2\r\n");
        // 3. 拼接$3 \r\n
        stringBuilder.append("$3").append("\r\n");
        // 4. 拼接get \r\n
        stringBuilder.append("get").append("\r\n");
        // 5. 拼接$num是多少？传入的key的长度 \r\n
        stringBuilder.append("$").append(key.length()).append("\r\n");
        stringBuilder.append(key).append("\r\n");
        // 6. 把拼接好的字符串发送给Redis服务器
        sk.getOutputStream().write(stringBuilder.toString().getBytes());
        // 7. 刷新缓冲区
        sk.getOutputStream().flush();
        // 8.停掉输出流 ，准备接受对方发过来的输入流
        sk.shutdownOutput();
        // 7.接受服务器返回的数据
        byte[] bytes = new byte[1024];
        // 8. 读取数据 到bytes数组中 read方法 输入流的读到内存中的byte数组
        int len = sk.getInputStream().read(bytes);
        return new String(bytes, 0, len);
    }
}
```

测试类：

```java
package com.woniuxy;


import com.woniuxy.cli.MyJedisClient;

public class MyJedisClientTest {
    public static void main(String[] args) throws Exception {
        // 正常情况
        MyJedisClient client = new MyJedisClient("192.168.80.6", 6379);
        client.set("class112","bbb");
        System.out.println(client.get("class112"));
    }
}
```

## 二、 sa-token集成

> 大家团队项目，并非用这个框架来开发，也可选使用这个。
>
> 这个项目开发，核心是为了大家理解 jwt，redis，了解登录和权限相关的内容。RBAC，基于角色的权限访问控制。
>
> 后面团队开发，我们直接使用ruoyi前后端分离版本。内部集成好了 权限，登录注册等相关的功能。
>
> 因为大家进入企业，基本的项目内容大多数都是成品，框架基础数据内容都是已经开发好了的，大家需要的是，阅读公司的框架基本原理，了解架构，直接开发应用业务。
>
> 后面写简历，也不要写 我开发了登录注册，jwt+redis实现的登录认证。



### 1. 简介

开发文档在：[https://sa-token.cc](https://sa-token.cc/)。

本文档将会尽力讲解每个功能的设计原因、应用场景，用心阅读文档，你学习到的将不止是 `Sa-Token` 框架本身，更是绝大多数场景下**权限设计**的最佳实践。

#### [Sa-Token 介绍](https://sa-token.cc/doc.html#/?id=sa-token-介绍)

**Sa-Token** 是一个轻量级 Java 权限认证框架，主要解决：**登录认证**、**权限认证**、**单点登录**、**OAuth2.0**、**分布式Session会话**、**微服务网关鉴权** 等一系列权限相关问题。

Sa-Token 旨在以简单、优雅的方式完成系统的权限认证部分，以登录认证为例，你只需要：

```java
// 会话登录，参数填登录人的账号id 
StpUtil.login(10001);
```

无需实现任何接口，无需创建任何配置文件，只需要这一句静态代码的调用，便可以完成会话登录认证。

如果一个接口需要登录后才能访问，我们只需调用以下代码：

```java
// 校验当前客户端是否已经登录，如果未登录则抛出 `NotLoginException` 异常
StpUtil.checkLogin();
```

在 Sa-Token 中，大多数功能都可以一行代码解决：

踢人下线：

```java
// 将账号id为 10077 的会话踢下线 
StpUtil.kickout(10077);
```

权限认证：

```java
// 注解鉴权：只有具备 `user:add` 权限的会话才可以进入方法
@SaCheckPermission("user:add")
public String insert(SysUser user) {
    // ... 
    return "用户增加";
}3456
```

路由拦截鉴权：

```java
// 根据路由划分模块，不同模块不同鉴权 
registry.addInterceptor(new SaInterceptor(handler -> {
    SaRouter.match("/user/**", r -> StpUtil.checkPermission("user"));
    SaRouter.match("/admin/**", r -> StpUtil.checkPermission("admin"));
    SaRouter.match("/goods/**", r -> StpUtil.checkPermission("goods"));
    SaRouter.match("/orders/**", r -> StpUtil.checkPermission("orders"));
    SaRouter.match("/notice/**", r -> StpUtil.checkPermission("notice"));
    // 更多模块... 
})).addPathPatterns("/**");
```

当你受够 Shiro、SpringSecurity 等框架的三拜九叩之后，你就会明白，相对于这些传统老牌框架，Sa-Token 的 API 设计是多么的简单、优雅！

> 过滤器，拦截器搞不明白，shiro是日本人开发的，他们的英语稀烂，里面的类名，又长又臭，Authorization，Authentiction，Realm等。
>
> ruoyi用的是SpringSecurity，所以，我后面会给大家讲ruoyi的源码，顺带了解下SpringSecurity。

#### [Sa-Token 功能一览](https://sa-token.cc/doc.html#/?id=sa-token-功能一览)

Sa-Token 目前主要五大功能模块：登录认证、权限认证、单点登录、OAuth2.0、微服务鉴权。

- **登录认证** —— 单端登录、多端登录、同端互斥登录、七天内免登录。
- **权限认证** —— 权限认证、角色认证、会话二级认证。
- **踢人下线** —— 根据账号id踢人下线、根据Token值踢人下线。
- **注解式鉴权** —— 优雅的将鉴权与业务代码分离。
- **路由拦截式鉴权** —— 根据路由拦截鉴权，可适配 restful 模式。
- **持久层扩展** —— 可集成 Redis，重启数据不丢失。
- **前后台分离** —— APP、小程序等不支持 Cookie 的终端也可以轻松鉴权。
- **Token风格定制** —— 内置六种 Token 风格，还可：自定义 Token 生成策略。
- **密码加密** —— 提供基础加密算法，可快速 MD5、SHA1、SHA256、AES 加密。
- **全局侦听器** —— 在用户登陆、注销、被踢下线等关键性操作时进行一些AOP操作。
- **全局过滤器** —— 方便的处理跨域，全局设置安全响应头等操作。
- **单点登录** —— SSO，内置三种单点登录模式：同域、跨域、同Redis、跨Redis、前后端分离等架构都可以搞定。
- **OAuth2.0认证** —— 轻松搭建 OAuth2.0 服务，支持openid模式 。
- **独立Redis** —— 将权限缓存与业务缓存分离。
- **jwt集成** —— 提供三种模式的 jwt 集成方案，提供 token 扩展参数能力。
- **自动续签** —— 提供两种Token过期策略，灵活搭配使用，还可自动续签。
- **开箱即用** —— 提供SpringMVC、WebFlux、Solon 等常见框架集成包，开箱即用。
- **最新技术栈** —— 适配最新技术栈：支持 SpringBoot 3.x，jdk 17。
- **sa-token没有和MQ集成**——我们来集成MQ，来实现一些业务。

功能结构图：

![sa-token-js](https://color-test.oss-cn-qingdao.aliyuncs.com/sa-token/x/sa-token-js4.png)



如果 Sa-Token 帮助到了您，希望您可以为其点上一个 `star`： [码云](https://gitee.com/dromara/sa-token)、 [GitHub](https://github.com/dromara/sa-token)

### 2. 开始使用

创建springboot项目，选择3.3.9版本的springboot，但是我们不用自己的springboot启动依赖包。

去掉自带的springboot依赖，引入sa-token的依赖即可。

```xml
		<!-- Sa-Token 权限认证，在线文档：https://sa-token.cc -->
		<dependency>
			<groupId>cn.dev33</groupId>
			<artifactId>sa-token-spring-boot3-starter</artifactId>
			<version>1.40.0</version>
		</dependency>
```

https://sa-token.cc/doc.html#/start/example 自己学会看文档，避免以后进企业看不懂文档。

直接启动springboot，只要能启动则配置完成。自己用apipost进行测试，只要能通过，则可以继续后一步，登录认证的开发（入门版）。



### 3. 权限认证自己去学

https://sa-token.cc/doc.html#/use/login-auth

CV更多的controller方法，感受登录后返回的信息。

![image-20250304115252374](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503041152500.png)



### 4. 集成redis

```xml
<!-- Sa-Token 整合 Redis （使用 jackson 序列化方式） -->
<dependency>
    <groupId>cn.dev33</groupId>
    <artifactId>sa-token-redis-jackson</artifactId>
    <version>1.40.0</version>
</dependency>
```

根据网上文档自己配置 yml

```yml
spring:
  data:
    # redis配置
    redis:
      # Redis数据库索引（默认为0）
      database: 1
      # Redis服务器地址
      host: 192.168.80.6
      # Redis服务器连接端口
      port: 6379
      # Redis服务器连接密码（默认为空）
      # password:
      # 连接超时时间
      timeout: 10s
      lettuce:
        pool:
          # 连接池最大连接数
          max-active: 200
          # 连接池最大阻塞等待时间（使用负值表示没有限制）
          max-wait: -1ms
          # 连接池中的最大空闲连接
          max-idle: 10
          # 连接池中的最小空闲连接
          min-idle: 0
```

自定义redis使用方案，直接注入 StringRedisTemplate，就可以使用。



或者看官方有没有集成好的RedisUtils，我们下午自己集成这个工具包也可以。

### 5. 自己了解

https://sa-token.cc/doc.html#/up/not-cookie



## 三、mybatis-plus来集成后端代码

https://baomidou.com/getting-started/ 官网。

> ORM框架，mybatis都只算一个半自动ORM框架。
>
> ORM：Object Relational Mapping，对象关系映射。  hibernate，jpa，mybatis-plus，全自动ORM框架，不需要写sql语句了。
>
> ​	实体类----OOP面相对象编程的方式----->数据库
>
> ```sql
> select * from user;
> ```
>
> ​	全自动ORM框架：
>
> ​	dao.findAll(); 这个方法，就等于上面的sql。
>
> ```sql
> select * from user where username = "admin" and password="123456";
> ```
>
> ​	dao.findOneByUsernameAndPassword("admin"，"123456"); 这个东西，原生是没有这个方法的，需要自定义，写接口即可，不需要写实现。

### 1. 引入依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-spring-boot3-starter</artifactId>
    <version>3.5.10.1</version>
</dependency>
<!-- 引入mysql驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.49</version>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <scope>provided</scope>
</dependency>
```



### 2. yml

```yml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql:///java112?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=GMT%2B8
    username: root
    password: 123456
```



### 3. sql语句执行生成

```sql
DROP TABLE IF EXISTS `user`;

CREATE TABLE `user`
(
    id BIGINT NOT NULL COMMENT '主键ID',
    username VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
		password VARCHAR(60) NULL DEFAULT NULL COMMENT '密码',
    age INT NULL DEFAULT NULL COMMENT '年龄',
    email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY (id)
);

INSERT INTO `user` (id, username,password, age, email) VALUES
(1, 'Jone','123456', 18, 'test1@baomidou.com'),
(2, 'Jack','123456', 20, 'test2@baomidou.com'),
(3, 'Tom', '123456',28, 'test3@baomidou.com'),
(4, 'Sandy', '123456',21, 'test4@baomidou.com'),
(5, 'Billie','123456', 24, 'test5@baomidou.com');
```

### 4. mapper层开发

```java
package com.woniuxy.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.woniuxy.entity.User;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;

/**
 * @Author: 马宇航
 * @Description: TODO
 * @DateTime: 25/03/04/星期二 14:31
 * @Component: 成都蜗牛学苑
 **/
@Mapper
public interface UserMapper extends BaseMapper<User> {
  
}

```



### 5. 测试类

```java
package com.woniuxy;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.woniuxy.entity.User;
import com.woniuxy.mapper.UserMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest
class SaTokenProjectApplicationTests {
    @Autowired
    UserMapper userMapper;

    @Test
    void contextLoads() {
       //1.查询所有信息
//     userMapper.selectList(new QueryWrapper<User>()).forEach(System.out::println);
       //2.根据账号或密码查询并排序
       QueryWrapper<User> queryWrapper = new QueryWrapper<>();
//     queryWrapper.eq("username", "Jone");
       queryWrapper.eq("password", "123456");
       queryWrapper.orderByDesc("id");
       List<User> users = userMapper.selectList(queryWrapper);
       System.out.println(users);
    }

}
```



### 6. 集成service

参考git地址 https://gitee.com/wasp_nest/java112-three-stage

### 7. Controller接口开发

参考git地址 https://gitee.com/wasp_nest/java112-three-stage





















