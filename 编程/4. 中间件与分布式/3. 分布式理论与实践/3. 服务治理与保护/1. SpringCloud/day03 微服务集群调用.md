# day03 微服务的集群调用

## 一、作业有做过

### 1. 集群环境搭建

![image-20250403105613777](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504031056868.png)

复制3份服务，通过VM参数配置，-D表示数据参数，SERVER_PORT就是VM我们自定义的参数名字。

![image-20250403105013474](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504031050539.png)

对应yml文件：

```yml
server:
  port: ${SERVER_PORT:8081} #如果环境变量(jvm参数)有这个值，就用这个值，否则用8081
spring:
  application:
    name: myh-product  #这个名字后面注册中心要用
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql:///java112_cloud?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8&useSSL=false
    username: root
    password: 123456
  #  jpa配置
  jpa:
    hibernate:
      #      一旦实体类发生变化，则会自动更新这个数据库的表内容
      # DDL数据定义语言（建表建库） DCL(用户)  DQL（CRUD）  DML（数据权限操作）
      ddl-auto: update
    #我mysql用的是5.7的
    database-platform: org.hibernate.dialect.MySQL5Dialect
    show-sql: true #打印sql语句执行慢的东西
```

![image-20250403105221158](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504031052217.png)



最后一步操作，让Product服务，返回的消息中，携带上端口的信息。@Value

```java
//读取yml中的server.port配置的值
    @Value("${server.port}")
    private String port;

//扣减库存，根据传入pid的数量来扣减库存
@PutMapping("/{pid}/{num}")
public ResponseMyEntity update(@PathVariable Integer pid, @PathVariable Integer num) {
    int result = productService.update(pid,num);
    ResponseMyEntity responseMyEntity = new ResponseMyEntity(result);
    responseMyEntity.put("port",port);
    return responseMyEntity;
}
```



### 2. Order业务开发

> 需求：
>
> 每次生成订单，访问order的时候，让它轮流 访问Product的三个服务，实现轮训效果。
>
> 进阶需求：
>
> 如果有服务死机，掉线。则自动剔除这个服务。
>
> 如果有服务上线，比如多了一个8084，还能自动添加到这个里面？ 如果不使用第三方中间件或者中间服务，很难办到。





Order业务升级，打印之前的响应端口

```java
package com.woniuxy.service.impl;

import com.woniuxy.dao.OrderDao;
import com.woniuxy.entity.Order;
import com.woniuxy.entity.Product;
import com.woniuxy.entity.utils.ResponseMyEntity;
import com.woniuxy.service.OrderService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.http.ResponseEntity;
import org.springframework.http.client.ClientHttpResponse;
import org.springframework.stereotype.Service;
import org.springframework.web.client.ResponseExtractor;
import org.springframework.web.client.RestTemplate;

import java.io.*;
import java.net.MalformedURLException;
import java.net.URI;
import java.net.URL;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/02/星期三 11:58
 * @Component: 成都蜗牛学苑
 **/
@Service
public class OrderServiceImpl implements OrderService {
    @Autowired
    OrderDao orderDao;
    public List<Order> findAll() {
        return orderDao.findAll();
    }
    private static List<String> list = new ArrayList();
    static {
        list.add("http://localhost:8081/product/");
        list.add("http://localhost:8082/product/");
        list.add("http://localhost:8083/product/");
    }
    //定义一个全局的变量，来轮流循环取list下标的值
    private static int index = 0;
    public ResponseMyEntity createOrder(Order order) throws IOException, ClassNotFoundException {
        //每次该方法执行，index+1，取余list.size()，来保证index不会越界
        String url = list.get((index++) % list.size());
        //写业务，我们可以拿到的参数，第一个是用户uid，商品的pid，商品的购买数量
        //1.先通过商品的pid，查询商品的数量
        RestTemplate restTemplate = new RestTemplate();
        //通过系统的ResponseEntity，来接收返回值，我们的ResponseEntity
        Product product = restTemplate.getForEntity(url + order.getPid(),
                Product.class).getBody();
        //库存数量
        Integer stock = product.getStock();
        //2.判断库存数量是否足够
        int i = stock - order.getNumber();
        //其他方案，修改Product的PutMapping为PostMapping，才能拿到结果
//        ResponseMyEntity responseMyEntity = restTemplate.postForObject(url + order.getPid() + "/" + order.getNumber(), null, ResponseMyEntity.class);
        //硬着头皮方案，一定要Put请求拿它响应
        ResponseEntity responseEntity = restTemplate.execute(url + order.getPid() + "/"+order.getNumber(),
                HttpMethod.PUT,
                null,
                //不要自己去重写，http的流的反序列化容易出现问题，直接使用框架的
                restTemplate.responseEntityExtractor(ResponseMyEntity.class),
                 new HashMap<String,Object>());
        ResponseMyEntity res = (ResponseMyEntity) responseEntity.getBody();
        return res;
    }
}
```





### 3. 使用异常的方式来剔除访问不了的服务

```java
package com.woniuxy.service.impl;

import com.woniuxy.dao.OrderDao;
import com.woniuxy.entity.Order;
import com.woniuxy.entity.Product;
import com.woniuxy.entity.utils.ResponseMyEntity;
import com.woniuxy.service.OrderService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.http.ResponseEntity;
import org.springframework.http.client.ClientHttpResponse;
import org.springframework.stereotype.Service;
import org.springframework.web.client.ResponseExtractor;
import org.springframework.web.client.RestTemplate;

import java.io.*;
import java.net.MalformedURLException;
import java.net.URI;
import java.net.URL;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/02/星期三 11:58
 * @Component: 成都蜗牛学苑
 **/
@Service
public class OrderServiceImpl implements OrderService {
    @Autowired
    OrderDao orderDao;
    public List<Order> findAll() {
        return orderDao.findAll();
    }
    private static List<String> list = new ArrayList();
    static {
        list.add("http://localhost:8081/product/");
        list.add("http://localhost:8082/product/");
        list.add("http://localhost:8083/product/");
    }
    //定义一个全局的变量，来轮流循环取list下标的值
    private static int index = 0;
    public ResponseMyEntity createOrder(Order order) {
        //每次该方法执行，index+1，取余list.size()，来保证index不会越界
        String url = list.get((index++) % list.size());
        //写业务，我们可以拿到的参数，第一个是用户uid，商品的pid，商品的购买数量
        //1.先通过商品的pid，查询商品的数量
        RestTemplate restTemplate = new RestTemplate();
        ResponseMyEntity res = null;
        try {
            //通过系统的ResponseEntity，来接收返回值，我们的ResponseEntity
            Product product = restTemplate.getForEntity(url + order.getPid(),
                    Product.class).getBody();
            //库存数量
            Integer stock = product.getStock();
            //2.判断库存数量是否足够
            int i = stock - order.getNumber();
//        ResponseMyEntity responseMyEntity = restTemplate.postForObject(url + order.getPid() + "/" + order.getNumber(), null, ResponseMyEntity.class);
            ResponseEntity responseEntity = restTemplate.execute(url + order.getPid() + "/" + order.getNumber(),
                    HttpMethod.PUT,
                    null,
                    //不要自己去重写，http的流的反序列化容易出现问题，直接使用框架的
                    restTemplate.responseEntityExtractor(ResponseMyEntity.class),
                    new HashMap<String, Object>());
            res = (ResponseMyEntity) responseEntity.getBody();
        }catch (Exception e){
            //剔除有异常的数据 这里剔除
            list.remove(url);
            e.printStackTrace();
        }
        return res;
    }
}
```

### 4. 如何实现重新上线8083，自动加入到list？

暂时不说: 可以使用第三方的服务或者中间件来实现。





## 二、Nacos

英文全称**Dynamic Naming and Configuration Service**，Na为naming/nameServer即**注册中心**,co为configuration即**配置中心**，service是指该注册/配置中心都是以服务为核心。

Nacos注册中心分为server与client，server采用Java编写，为client提供注册发现服务与配置服务。而client（我们自己服务）可以用多语言实现，client与微服务嵌套在一起，nacos提供sdk和openApi，如果没有sdk也可以根据openApi手动写服务注册与发现和配置拉取的逻辑。

官网教程：



### 

![nacos_map](https://nacos.io/img/nacosMap.jpg)

- 特性大图：要从功能特性，非功能特性，全面介绍我们要解的问题域的特性诉求
- 架构大图：通过清晰架构，让您快速进入 Nacos 世界
- 业务大图：利用当前特性可以支持的业务场景，及其最佳实践
- 生态大图：系统梳理 Nacos 和主流技术生态的关系
- 优势大图：展示 Nacos 核心竞争力
- 战略大图：要从战略到战术层面讲 Nacos 的宏观优势

**易用：** 简单的数据模型，标准的 restfulAPI，易用的控制台，丰富的使用文档。
**稳定：** 99.9% 高可用，脱胎于历经阿里巴巴 10 年生产验证的内部产品，支持具有数百万服务的大规模场景，具备企业级 SLA 的开源产品。
**实时：** 数据变更毫秒级推送生效；1w 级，SLA 承诺 1w 实例上下线 1s，99.9% 推送完成；10w 级，SLA 承诺 1w 实例上下线 3s，99.9% 推送完成；100w 级别，SLA 承诺 1w 实例上下线 9s，99.9% 推送完成。
**规模：** 十万级服务/配置，百万级连接，具备强大扩展性。

## 三、Nacos 生态图

![image-20210726214737542](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206101559034.png)

如 Nacos 全景图所示，Nacos 无缝支持一些主流的开源生态，例如

- [Spring Cloud](https://nacos.io/en-us/docs/quick-start-spring-cloud.html)
- [Apache Dubbo and Dubbo Mesh](https://nacos.io/zh-cn/docs/use-nacos-with-dubbo.html)
- [Kubernetes and CNCF](https://nacos.io/zh-cn/docs/use-nacos-with-kubernetes.html)。

使用 Nacos 简化服务发现、配置管理、服务治理及管理的解决方案，让微服务的发现、管理、共享、组合更加容易。

关于如何在这些生态中使用 Nacos，请参考以下文档：

[Nacos与Spring Cloud一起使用](https://nacos.io/zh-cn/docs/use-nacos-with-springcloud.html)

[Nacos与Kubernetes一起使用](https://nacos.io/zh-cn/docs/use-nacos-with-kubernetes.html)

[Nacos与Dubbo一起使用](https://nacos.io/zh-cn/docs/use-nacos-with-dubbo.html)

[Nacos与gRPC一起使用](https://nacos.io/zh-cn/docs/roadmap.html)

[Nacos与Istio一起使用](https://nacos.io/zh-cn/docs/use-nacos-with-istio.html)



架构图：

![img](https://cdn.nlark.com/lark/0/2018/png/9687/1543984258587-3a2cc018-728f-414e-8186-216b896122c9.png#height=1184&id=EFIc9&originHeight=1184&originWidth=1608&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=1608)

## 四、安装运行

1. 修改配置文件，配置数据库名字，账号密码。

   ![image-20250103140301385](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202501031403464.png)

![image-20250103110915670](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202501031109782.png)

启动命令：/bin目录下

```cmd
startup.cmd -m standalone
```

## 五、nacos引入注册中心

### 1. 依赖引入

在commons模块下引入依赖：

```xml
<!--     nacos客户端 注册与发现 引入   -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
```

### 2. yml配置（每个单独的服务都要链接注册nacos中）

```yml
server:
  port: 8091
spring:
  application:
    name: myh-order  #这个名字后面注册中心要用
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql:///java112_cloud?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8&useSSL=false
    username: root
    password: 123456
  #  jpa配置
  jpa:
    hibernate:
      #      一旦实体类发生变化，则会自动更新这个数据库的表内容
      # DDL数据定义语言（建表建库） DCL(用户)  DQL（CRUD）  DML（数据权限操作）
      ddl-auto: update
    #我mysql用的是5.7的
    database-platform: org.hibernate.dialect.MySQL5Dialect
    show-sql: true #打印sql语句执行慢的东西
#下面的就是nacos的注册中心的配置
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
```

### 3. 注册后怎么用？

```java
package com.woniuxy;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/02/星期三 10:19
 * @Component: 成都蜗牛学苑
 **/
@SpringBootApplication
@EnableDiscoveryClient //开启服务发现
public class OrderApp {
//    注册RestTemplate成为Bean，才能在springboot项目下使用RestTemplate
    @Bean
    @LoadBalanced //开启负载均衡，这个配置，才是决定了下面的请求能否轮训的配置
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
    //配置 服务调用策略
    @Bean
    public IRule iRule() {
        //随机策略
        return new RandomRule();
    }
    
    public static void main(String[] args) {
        SpringApplication.run(OrderApp.class, args);
    }
}

//product服务也要加这个，都要
@SpringBootApplication
@EnableDiscoveryClient
public class ProductAPP {
}
```

service:

```java
@Autowired
RestTemplate restTemplate;

Product product = restTemplate.getForEntity("http://myh-product/product/" + order.getPid(),
                    Product.class).getBody();
```

Ribbon的负载均衡策略 可以用类的表：

| 策略名                    | 作用                                                         |
| ------------------------- | ------------------------------------------------------------ |
| RoundRobinRule            | 轮询                                                         |
| RandomRule                | 随机                                                         |
| AvailabilityFilteringRule | 过滤掉连接失败的服务节点，并且过滤掉高并发的服务节点，然后从健康的服务节点中，使用轮询策略选出一个节点返回。 |
| WeightedResponseTimeRule  | 该策略根据平均响应时间计算所有服务的权重，响应时间越快的服务权重就越大，权重越大被选中的几率就越高。刚启动时如果统计信息不足，则使用RoundRobinRule策略，等到统计信息足够多的时候，则会自动切换到WeightedResponseTimeRule策略！ |
| RetryRule                 | 首先使用轮询策略进行负载均衡，如果轮询失败，则在一段时间内进行重试，如果超过了指定的时间，则报错 |
| BestAvailableRule         | 该策略会先过滤掉由于多次出现访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务。 |

## 六、nacos引入配置中心

> 配置中心的好处，本地就很单纯，几乎看不见配置。

### 1. 引入依赖

```xml
<!-- nacos的配置中心 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

### 2. yml

首先，修改本地的application.yml ---->bootstrap.yml

```yml
spring:
#  application:
#    name: myh-order  #这个名字后面注册中心要用
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
      config:
        server-addr: localhost:8848
        prefix: myh-order  #类似 spring.application.name的名字，和配置中心的配置文件前缀 myh-order
        file-extension: yml  #文件扩展名  .yml
        group: DEFAULT_GROUP  #看情况，根据公司要求配置即可
#        namespace: public   # 看情况，根据公司要求配置即可
  # 多环境配置
  profiles:
    active: dev  # 第二部分配置 myh-order  -dev
```

### 3. 把本地之前的yml配置复制到 nacos中

#### 3.1 配置中心的DataId规则

![image-20250403151839024](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504031518215.png)



在 Nacos Spring Cloud 中，`dataId` 的完整格式如下：

```xml
${prefix}-${spring.profiles.active}.${file-extension}
```

- `prefix` 默认为 `spring.application.name` 的值，也可以通过配置项 `spring.cloud.nacos.config.prefix`来配置。
- `spring.profiles.active` 即为当前环境对应的 profile。 **注意：当 `spring.profiles.active` 为空时，对应的连接符 `-` 也将不存在，dataId 的拼接格式变成 `${prefix}.${file-extension}`**
- `file-exetension` 为配置内容的数据格式，可以通过配置项 `spring.cloud.nacos.config.file-extension` 来配置。目前只支持 `properties` 和 `yaml` 类型。







#### AI回答：

在Spring Boot项目中，`bootstrap.yml` 和 `application.yml` 是两种常用的配置文件，它们在加载时机和应用场景上有所不同：

#### 3. 加载时机

1. **bootstrap.yml**：
   - **加载时机**：`bootstrap.yml` 会在 `application.yml` 之前加载。
   - **用途**：主要用于配置一些需要在**应用上下文完全启动之前就加载**的配置，比如配置中心（如Spring Cloud Config）的连接信息。它的加载不依赖于Spring的上下文，因此可以用来配置那些需要在应用启动早期就初始化的组件。

2. **application.yml**：
   - **加载时机**：`application.yml` 在 `bootstrap.yml` 之后加载。
   - **用途**：用于配置应用的主要配置信息，如数据库连接、服务端口等。它的加载依赖于Spring的上下文，因此通常用于配置那些在应用启动过程中需要使用的配置。

#### 应用场景

- **bootstrap.yml**：
  - 适用于需要早期加载的配置，比如配置中心的连接信息、加密解密配置等。
  - 在微服务架构中，常用于从配置中心加载配置信息。

- **application.yml**：
  - 适用于应用的主要配置，如数据库连接信息、日志配置、应用端口等。
  - 是Spring Boot应用的标准配置文件。

#### 示例

**bootstrap.yml**：
```yaml
spring:
  application:
    name: my-service
  cloud:
    config:
      uri: http://config-server:8888
      fail-fast: true
```

**application.yml**：
```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: root
    password: password
```

#### 总结

- `bootstrap.yml` 优先加载，用于早期配置，特别是与配置中心相关的配置。
- `application.yml` 在 `bootstrap.yml` 之后加载，用于应用的主要配置。

理解它们的加载时机和用途，有助于更好地管理和组织Spring Boot项目的配置信息。



### 4. 实时生效！

> 正常情况，修改了nacos的配置文件，它要重启后生效。
>
> 我们可以添加一个注解，让这个配置实时生效！！
>
> @RefreshScope //动态刷新配置 不用重启  
>
> 内部的@Value就可以拿到实时改变的yml配置。

```java
@RefreshScope //动态刷新配置 不用重启
public class OrderController {
    @Autowired
    OrderService orderService;

    @Value("${product.myname}")
    private String myname;
    }	
```
