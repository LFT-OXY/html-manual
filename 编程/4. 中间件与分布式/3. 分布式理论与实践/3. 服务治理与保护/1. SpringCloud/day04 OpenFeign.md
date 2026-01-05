# day04 OpenFeign

## 一、引入契机

### 1. 之前是有问题的

RestTemplate+Ribbon（负载均衡的配置）,有什么问题呢？

1. 耦合太高，所有的Service都要引入这个代码
2. 还需要手动去写url地址。
3. 要获取响应，以及不同的请求方案，写起来，真的麻烦。

正式的项目，不会这么使用的！使用类似本地controller调用Service的这种方案，去做远程的请求访问。

### 2. 解决方案

> 有了OpenFeign，我可以像调用本地Controller一样，去使用微服务远程调用。
>
> OpenFeign底层其实还是RestTemplate。
>
> OpenFeign相比以前的Feign，多了一个SpringMVC的注解使用。

![img](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206011754646.jpeg)

### 3. 引入OpenFeign

```xml
<!--    引入OpenFeign    -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
```

还需要在启动类上开启这个OpenFeign。

```
@EnableFeignClients //开启feign
```

### 4. OpenFeign接口开发

AOP是什么？面相切面编程。它的底层实现用的是动态代理！两种实现方案？

1. JDK动态代理：利用接口实现的方案，来做的
2. CGLib动态代理：子类扩展父类的方案实现的。

动态代理的特点：利用反射，在代码运行的时候，动态生成具体的实现类。

![手写AOP代理类思路](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071033165.jpg)

>OpenFeign写的是接口，不用写实现类，底层，就是利用的动态代理的方案，来实现的具体业务。类似JPA和mybatis-plus的Mapper层。
>
>下面的代码，直接CVProduct服务的controller方法过来即可。

```JAVA
package com.woniuxy.feign;

import com.woniuxy.entity.Product;
import com.woniuxy.entity.utils.ResponseMyEntity;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.*;

/**
 * @Author: 马宇航
 * @Todo: 利用OpenFeign来调用微服务，order--->product服务
 * @DateTime: 25/04/07/星期一 10:34
 * @Component: 成都蜗牛学苑
 **/
@FeignClient(value = "myh-product")
@RequestMapping("/product")  //指定调用的微服务的接口 restTemple("http://myh-product/product")
public interface ProductAPI {
    //下面的代码，都是直接复制对方的代码中的内容。
    //现在我自己写实现：restTemple.get("http://myh-product/product"+"/"+pid)
    @GetMapping(value = {"/{pid}"})
    public Product findAll(@PathVariable("pid") Integer pid);
    @PostMapping("/")
    public ResponseMyEntity add(@RequestBody Product product);
    //这里有坑，在OpenFeign中，@PathVariable 不能省略 参数值！
    @PutMapping("/{pid}/{num}")
    public ResponseMyEntity update(@PathVariable("pid") Integer pid, @PathVariable("num") Integer num);
}
```

Order服务的Service层注入上面的代码：

```java
package com.woniuxy.service.impl;

import com.woniuxy.entity.Order;
import com.woniuxy.entity.Product;
import com.woniuxy.entity.utils.ResponseMyEntity;
import com.woniuxy.feign.ProductAPI;
import com.woniuxy.service.OrderService;
import org.apache.http.HttpStatus;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.util.Collections;
import java.util.List;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/07/星期一 10:41
 * @Component: 成都蜗牛学苑
 **/
@Service
public class OrderServiceOpenFeignImpl implements OrderService {
    @Autowired
    ProductAPI productAPI;

    @Override
    public List<Order> findAll() {
        return Collections.emptyList();
    }

    @Override
    public ResponseMyEntity createOrder(Order order) throws IOException, ClassNotFoundException {
        Product product = productAPI.findAll(order.getPid());
        Integer stock = product.getStock();
        //2.判断库存数量是否足够
        int i = stock - order.getNumber();
        if (i < 0) {
            return new ResponseMyEntity(201,"库存不足");
        }
        ResponseMyEntity update = productAPI.update(order.getPid(), order.getNumber());
        return update;
    }
}
```

### 5. API接口处理更多参数类型

> 公司内部RestFul风格用的多一点，但是偶尔还是会有普通的传参方式。List类型，Map类型传参，Get,?号拼接参数类型传参。
>
> 引入一个新的注解，@SpringQueryMap 注解可以解析我们的Map或者是对象类型传参。



#### 5.1 多参数GET查询 Map参数

在product的controller开发 一个接口，根据价格差，查询商品信息。

```java
//非RestFul风格
/**
 * 查询一个价格范围内的商品，/findAll?startPrice=100&endPrice=200
 * ChangeLog : 1. 创建 (25/04/07/星期一 11:51 [马宇航]);
 * @param startPrice
 * @param endPrice
 * @return com.woniuxy.entity.utils.ResponseMyEntity
*/
@GetMapping("/findAll")
public ResponseMyEntity findAll(@RequestParam double startPrice,@RequestParam double endPrice) {
    List<Product> products = productService.findByPrice(startPrice,endPrice);
    return new ResponseMyEntity(products);
}
```

service,dao

```java
  @Override
    public List<Product> findByPrice(double startPrice, double endPrice) {
        //where price between ?1 and ?2
        return productDao.findAllByPriceBetween(startPrice,endPrice);
    }
```



order服务的API接口层：ProductAPI.java

**方案一：** 写的东西有点多，还必须把@RequestParam的参数必填。

```java
@GetMapping("/findAll")
public ResponseMyEntity findAll(@RequestParam("startPrice") double startPrice, @RequestParam("endPrice") double endPrice);

```

**方案二：**

```java
// 方案一
//    @GetMapping("/findAll")
//    public ResponseMyEntity findAll(@RequestParam("startPrice") double startPrice, @RequestParam("endPrice") double endPrice);
//方案二
    @GetMapping("/findAll")
    public ResponseMyEntity findAll2(@SpringQueryMap Map<String,Double> map);
```

测试类：

```java
package com.woniuxy;

import com.woniuxy.entity.utils.ResponseMyEntity;
import com.woniuxy.feign.ProductAPI;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.HashMap;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/07/星期一 11:54
 * @Component: 成都蜗牛学苑
 **/
@SpringBootTest
public class TestOpenFeignAPI {
    @Autowired
    ProductAPI productAPI;
    @Test
    public void test() {
        HashMap<String, Double> stringDoubleHashMap = new HashMap<>();
        stringDoubleHashMap.put("startPrice",100.0);
        stringDoubleHashMap.put("endPrice",2000.0);
        ResponseMyEntity all = productAPI.findAll2(stringDoubleHashMap);
        System.out.println(all);
    }
}
```

#### 5.2 多参数GET查询 对象参数

> 传入Product的属性，去搜索对应的数据出来。

```java
    /**
     * 根据实体类Product的属性来查询商品信息
     *  /findOne?name=xxx&price=xxx&stock=xxx
     * ChangeLog : 1. 创建 (25/04/07/星期一 12:06 [马宇航]);
     * @param name 模糊查询
     * @param price
     * @param stock
     * @return com.woniuxy.entity.Product
    */
@GetMapping("/findOne")
public ResponseMyEntity findProduct(@RequestParam String name,@RequestParam double price,@RequestParam Integer stock) {
    List<Product> all = productService.findOne(name, price, stock);
    return new ResponseMyEntity(all);
}
```

service和dao

```java
@Override
public List<Product> findOne(String name, double price, Integer stock) {
    // where name like xxx or price = xxx or stock = xxx
    List<Product> all  = productDao.findByNameLikeOrPriceOrStock(name,price,stock);
    return all;
}
```

order服务的API接口层：

```java
//方法一
//@GetMapping("/findOne")
//    public ResponseMyEntity findProduct(@RequestParam("name") String name,
                                        @RequestParam("price") double price,
                                        @RequestParam("stock") Integer stock);
//方法二
@GetMapping("/findOne")
    public ResponseMyEntity findProduct(@SpringQueryMap Product Product);
```

测试代码：

```java
@Test
public void test() {
    Product product = new Product();
    product.setName("%1%");
    product.setPrice(1000.0);
    product.setStock(1000);
    ResponseMyEntity all = productAPI.findProduct(product);
    System.out.println(all);
}
```

### 6. feign的额外配置

```yml
feign:
  client:
    config:
      myh-product: #配置远程调用的服务的名字
        connectTimeout: 2000 #连接超时时间
        readTimeout: 2000 #读取超时时间
        loggerLevel: full  #日志级别 full 详细日志，basic 基本日志，none 无日志
logging:
  level:
    root: info #指定所有接口的日志级别
    com.woniuxy.feign.ProductAPI: debug #指定某个接口的日志级别
```

超时重试配置： 通常比yml中的超时报错时间要长一点。

```java
@Bean
public Retryer retryRule() {
    //100毫秒后重试，最大重试时间为3000毫秒，重试次数为3次
    return new Retryer.Default(100l,3000l,3);
}
```



![image-20250407144202291](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071442498.png)

## 二、Sentinel 哨兵

> redis集群+sentinel哨兵，这里是有见过这个词。
>
> 目前学的，SpringCLoudAlibaba的sentinel。



https://github.com/alibaba/Sentinel/releases



中文官网的地址：[introduction | Sentinel中文网](https://sentinelguard.io/zh-cn/docs/introduction.html)



随着微服务的流行，服务和服务之间的稳定性变得越来越重要。Sentinel 是面向分布式、多语言异构化服务架构的流量治理组件，主要以**流量为切入点，从流量路由（route请求地址的跳转）、流量控制（controller接口的控制）、流量整形（MQ类似，排列整齐）、熔断降级（熔断：电路保险丝类似。降级：你处理不了，交给别人处理。）、系统自适应过载保护（第一次2s，挂了，第二次变成4s，还不行，升级为8s。或者本来服务器已经挂了，但是不一会又恢复了，又要让业务恢复正常，不需要人为操作，自动会尝试恢复）、热点流量防护（自己配置某些接口是热点接口，某一个服务的某一个接口，单独对他进行流量的防护配置。）**等多个维度来帮助开发者保障微服务的稳定性。

### Sentinel 的历史

- 2012 年，Sentinel 诞生，主要功能为入口流量控制。
- 2013-2017 年，Sentinel 在阿里巴巴集团内部迅速发展，成为基础技术模块，覆盖了所有的核心场景。Sentinel 也因此积累了大量的流量归整场景以及生产实践。
- 2018 年，Sentinel 开源，并持续演进。
- 2019 年，Sentinel 朝着多语言扩展的方向不断探索，推出 [C++ 原生版本](https://github.com/alibaba/sentinel-cpp)，同时针对 Service Mesh 场景也推出了 [Envoy 集群流量控制支持](https://github.com/alibaba/Sentinel/tree/master/sentinel-cluster/sentinel-cluster-server-envoy-rls)，以解决 Service Mesh 架构下多语言限流的问题。
- 2020 年，推出 [Sentinel Go 版本](https://github.com/alibaba/sentinel-golang)，继续朝着**云原生方向**演进。
- 2021 年，Sentinel 正在朝着 2.0 云原生**高可用决策中心组件**进行演进；同时推出了 [Sentinel Rust 原生版本](https://github.com/sentinel-group/sentinel-rust)。同时我们也在 Rust 社区进行了 Envoy WASM extension 及 eBPF extension 等场景探索。
- 2022 年，Sentinel 品牌升级为流量治理，领域涵盖流量路由/调度、流量染色、流控降级、过载保护/实例摘除等；同时社区将流量治理相关标准抽出到 [OpenSergo 标准](https://opensergo.io/zh-cn/)中，Sentinel 作为流量治理标准实现。

>开发版本，有C++和GO版本。本身用java实现的，因为我们下载的是jar包。
>
>
>
>`云原生是什么？`
>
>云原生（Cloud Native）是一种构建和运行应用程序的方法，它充分利用了**云计算（云服务器的流量性能计算）**的优势。云原生方向主要涉及以下几个方面：
>
>1. **微服务架构（Microservices）**：
>   - 应用程序被分解为多个小型、独立的服务，每个服务负责特定的功能，服务之间通过轻量级通信机制（如HTTP RESTful API）进行交互。
>
>2. **容器化（Containerization）**：
>   - 使用容器（如Docker）来打包应用程序及其依赖项，确保应用程序在不同环境中的一致性运行。
>
>3. **动态编排（Dynamic Orchestration）**：
>   - 使用编排工具（如Kubernetes）来管理容器的生命周期，包括部署、扩展、负载均衡和自我修复等。
>
>4. **持续交付（Continuous Delivery）**：CICD
>   - 通过自动化测试和部署流程，实现应用程序的快速迭代和持续交付，确保新功能可以迅速且安全地部署到生产环境。
>
>5. **DevOps文化**：
>   - 强调开发（Dev）和运维（Ops）团队的紧密合作，通过自动化工具和文化变革，提高开发效率和运维质量。
>
>6. **服务网格（Service Mesh）**：
>   - 在微服务架构中，使用服务网格（如Istio）来管理服务间的通信，提供流量管理、安全、监控等功能。
>
>7. **声明式基础设施（Infrastructure as Code, IaC）**：
>   - 使用代码来定义和管理基础设施，使得基础设施的部署和管理可以自动化和版本控制。
>
>8. **可观察性（Observability）**：
>   - 通过**日志、监控、追踪(链路追踪)**等手段，确保系统的状态和性能可以被实时观察和分析。
>
>云原生方向的目的是提高应用程序的**敏捷性、可扩展性和可靠性**，使其能够更好地适应云计算环境的需求。通过采用云原生技术，企业可以更快地交付高质量的软件，提升业务竞争力。





### Sentinel 基本概念

### 资源

资源是 Sentinel 的关键概念。它可以是 Java 应用程序中的任何内容，例如，由应用程序提供的服务，或由应用程序调用的其它应用提供的服务，甚至可以是一段代码。在接下来的文档中，我们都会用资源来描述代码块。

只要通过 Sentinel API 定义的代码，就是资源，能够被 Sentinel 保护起来。大部分情况下，可以使用**方法签名，URL，甚至服务名称**作为资源名来标示资源。

### 规则

围绕资源的实时状态设定的规则，可以包括流量控制规则、熔断降级规则以及系统保护规则。所有规则可以动态实时调整。

Sentinel 功能和设计理念，QPS（每秒点击量query 查询量），TPS(每秒事务执行数量）规则设置，面试闲聊的内容。

### 流量控制

流量控制在网络传输中是一个常用的概念，它用于调整网络包的发送数据。然而，从系统稳定性角度考虑，在处理请求的速度上，也有非常多的讲究。任意时间到来的请求往往是随机不可控的，而系统的处理能力是有限的。我们需要根据系统的处理能力对流量进行控制。Sentinel 作为一个调配器，可以根据需要把**随机的请求调整成合适的形状**，如下图所示：

![arch](https://sentinelguard.io/docs/zh-cn/img/sentinel-flow-overview.jpg)

流量控制有以下几个角度:

- 资源的调用关系，例如资源的**调用链路**，**资源和资源**之间的关系；
- 运行指标，例如 QPS、线程池、系统负载等；
- 控制的效果，例如**直接限流、冷启动、排队**等。

Sentinel 的设计理念是让您自由选择控制的角度，并进行灵活组合，从而达到想要的效果。

### 熔断降级

#### 什么是熔断降级

除了流量控制以外，降低调用链路中的不稳定资源也是 Sentinel 的使命之一。由于调用关系的复杂性，如果调用链路中的**某个资源（Service D）出现了不稳定，最终会导致请求发生堆积**。这个问题和 [Hystrix](https://github.com/Netflix/Hystrix/wiki#what-problem-does-hystrix-solve) 里面描述的问题是一样的。

![image](https://user-images.githubusercontent.com/9434884/62410811-cd871680-b61d-11e9-9df7-3ee41c618644.png)

Sentinel 和 Hystrix 的原则是一致的: 当调用链路中某个资源出现不稳定，例如，表现为 **timeout**，**异常比例升高**的时候，则对这个资源的调用进行限制，并让**请求快速fast-fall失败**，避免影响到其它的资源，最终**产生雪崩(服务雪崩）**的效果。

#### 熔断降级设计理念

在限制的手段上，Sentinel 和 Hystrix 采取了**完全不一样**的方法。**面试回答！**

Hystrix 通过[线程池](https://github.com/Netflix/Hystrix/wiki/How-it-Works#benefits-of-thread-pools)的方式，来对依赖(在我们的概念中对应资源)进行了**资源池隔离**。这样做的好处是**资源和资源之间做到了最彻底的隔离**。缺点是除了增加了**线程切换（上下文切换）**的成本，还需要预先给各个资源做线程池大小的分配。



Sentinel 对这个问题采取了两种手段:

- 通过**并发线程数**进行限制（线程数量是动态的）

和资源池隔离的方法不同，Sentinel 通过限制资源并发线程的数量，来减少不稳定资源对其它资源的影响。这样不但没有线程切换的损耗，也**不需要您预先分配线程池**的大小。当某个资源出现不稳定的情况下，例如响应时间变长，对资源的直接影响就是会造成线程数的逐步堆积。当线程数在特定资源上堆积到一定的数量之后，对该资源的新请求就会被拒绝。堆积的线程完成任务后才开始继续接收请求。

- 通过响应时间对资源进行降级

除了对并发线程数进行控制以外，Sentinel 还可以通过响应时间来**快速降级不稳定的资源**。当依赖的资源出现响应时间过长后，所有对该资源的访问都会被**直接拒绝**，直到过了指定的时间窗口之后才重新恢复。

### 系统负载保护

Sentinel 同时提供[系统维度的自适应保护能力](https://sentinelguard.io/zh-cn/docs/system-adaptive-protection.html)。防止雪崩，是系统防护中重要的一环。当系统负载较高的时候，如果还持续让请求进入，可能会导致系统崩溃，无法响应。在集群环境下，网络负载均衡会把本应这台机器承载的流量转发到其它的机器上去。如果这个时候其它的机器也处在一个边缘状态的时候，这个增加的流量就会导致这台机器也崩溃，最后导致整个集群不可用。

针对这个情况，Sentinel 提供了对应的保护机制，让系统的入口流量和系统的负载达到一个平衡，保证系统在能力范围之内处理最多的请求。

### Sentinel 是如何工作的

Sentinel 的主要工作机制如下：

- 对主流框架提供适配或者显示的 **API**（controller层接口），来定义需要保护的资源，并提供设施对资源进行实时统计和调用链路分析。
- 根据预设的规则，结合对资源的实时统计信息，对流量进行控制。同时，Sentinel 提供开放的接口，方便您定义及改变规则。
- Sentinel 提供实时的**监控系统**，方便您快速了解目前系统的状态。

### 流控降级与容错标准

Sentinel 社区正在将流量治理相关标准抽出到 [OpenSergo spec](https://opensergo.io/zh-cn/) 中，Sentinel 作为流量治理标准实现。有关 Sentinel 流控降级与容错 spec 的最新进展，请参考 [opensergo-specification](https://github.com/opensergo/opensergo-specification/blob/main/specification/zh-Hans/fault-tolerance.md)，也欢迎社区一起来完善标准与实现。



## 三、使用Sentinel

### 1. 启动sentinel的服务

java -jar sentinel-dashboard-1.8.8

启动后，账号密码默认都是sentinel。

![image-20250407160255364](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071602511.png)





### 2. commons模块下引入依赖

```xml
```

### 3. order模块的nacos配置中心里面

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
   # sentinel配置，spring.cloud.sentinel
  cloud:
    sentinel:
      transport:
        dashboard: localhost:8080  #sentinel控制台地址
  #  jpa配置
  jpa:
    hibernate:
      #      一旦实体类发生变化，则会自动更新这个数据库的表内容
      # DDL数据定义语言（建表建库） DCL(用户)  DQL（CRUD）  DML（数据权限操作）
      ddl-auto: update
    #我mysql用的是5.7的
    database-platform: org.hibernate.dialect.MySQL5Dialect
    show-sql: true #打印sql语句执行慢的东西
product:
  myname: maoniuxy
feign:
  client:
    config:
      myh-product: #配置远程调用的服务的名字
        connectTimeout: 2000 #连接超时时间
        readTimeout: 2000 #读取超时时间
        loggerLevel: full
logging:
  level:
    root: info #指定所有接口的日志级别
    com.woniuxy.feign.ProductAPI: debug #指定某个接口的日志级别
```

### 4. 不需要配置其他注解

> 只需要引入依赖，和yml配置即可，添加jmeter测试功能。

![image-20250407162041451](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071620620.png)

![image-20250407162112159](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071621297.png)



sentinel监控，看到的QPS数量：2000

![image-20250407162257817](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071622007.png)



## 四、搭建测试环境

> 有问题的环境，product中，sleep了2s。同时OpenFeign开启了重试。
>
> ![image-20250407164018696](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071640828.png)

![image-20250407163820789](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071638950.png)



测试的结果是，有大量的线程，在服务中，等待，或者空转CPU--->100%CPU执行。

![image-20250407164418602](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071644908.png)

![image-20250407164443501](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071644717.png)

## 五、降级处理（性能几乎没有提升）

order服务controller，中，配置注解：写在同一个类中。

```java
//TODO 订单的创建, 单独springboot，要 调用product的接口，修改库存
@PostMapping("/")
//设置fallback 降级方法
@SentinelResource(value = "createOrder",fallback = "fallback")
public ResponseMyEntity createOrder(@RequestBody Order order) throws Exception{
    return orderService.createOrder(order);
}

public ResponseMyEntity fallback(@RequestBody Order order,Throwable e){
    return new ResponseMyEntity(200,e.getMessage()+"服务器正在维护中...");
}
```

> 降级方案：
>
> 1. @SentinelResource(value = "createOrder",fallback = "fallback") 后面的fallback方法，比如在同一个类里面
>
>    1. 如果降级方法，不是在同一个类中，还需要配置fallbackClass
>
>    2. ```java
>       //TODO 订单的创建, 单独springboot，要 调用product的接口，修改库存
>           @PostMapping("/")
>           //设置fallback 降级方法
>           @SentinelResource(value = "createOrder",fallback = "fallback",fallbackClass = OrderControllerFallBack.class )
>           public ResponseMyEntity createOrder(@RequestBody Order order) throws Exception{
>               return orderService.createOrder(order);
>           }
>       ```
>
>    3. 降级处理类中的对应方法，必须是静态方法，否则没法解析
>
>       ```java
>       package com.woniuxy.controller;
>       
>       import com.woniuxy.entity.Order;
>       import com.woniuxy.entity.utils.ResponseMyEntity;
>       import org.springframework.web.bind.annotation.RequestBody;
>       
>       /**
>        * @Author: 马宇航
>        * @Todo: OrderController的降级类
>        * @DateTime: 25/04/07/星期一 17:04
>        * @Component: 成都蜗牛学苑
>        **/
>       public class OrderControllerFallBack {
>           public static ResponseMyEntity fallback(@RequestBody Order order, Throwable e){
>               return new ResponseMyEntity(200,e.getMessage()+"服务器正在维护中...");
>           }
>       }
>       ```
>
>       
>
> 2. 参数类型除了异常，必须和原始业务保持一致
>
> 3. 返回值类型也必须和正常业务方法保持一致



降级的时候，其实对于性能帮助不大，为什么？

> 降级方法，仅仅只是一种异常的处理方案，让我们的响应更加人性化。



## 六、熔断处理

先配置熔断处理方案：

![image-20250407172108780](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071721974.png)

熔断的规则：

![image-20250407172123668](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071721803.png)

1. RT是什么？read-time，最大响应时间，如果超过这个时间就叫做 慢调用！
2. 阈值：可以和最小请求数做计算。

![image-20250407172544711](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504071725874.png)



```java
//TODO 订单的创建, 单独springboot，要 调用product的接口，修改库存
@PostMapping("/")
//设置fallback 降级方法
@SentinelResource(value = "createOrder",blockHandler = "blockMethod")
public ResponseMyEntity createOrder(@RequestBody Order order) throws Exception{
    return orderService.createOrder(order);
}
//熔断后,所有请求,直接访问这个方法,
public ResponseMyEntity blockMethod(Order order, BlockException e){
    return new ResponseMyEntity(200,e.getMessage()+"服务器正在维护中...");
}
```

> 基本上性能是稳定了，可以快速失败（熔断的效果），熔断时长，不接受正常进入到createOrder的请求，直接响应200，进入到我们的熔断的方法中。





product服务的测试代码： 这个代码就会抛出异常！

```java
//都是给 其他服务使用的。
static int i =0;
@GetMapping(value = {"/{pid}"})
public Product findAll(@PathVariable Integer pid) {
    if ((i++) % 4== 0) {
        throw new RuntimeException("模拟异常");
    }
    return productService.findById(pid);
}
```
