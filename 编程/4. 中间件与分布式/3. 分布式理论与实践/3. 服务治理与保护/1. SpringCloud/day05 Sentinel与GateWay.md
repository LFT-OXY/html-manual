# day05 Sentinel

## 一、熔断效果（异常熔断）

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

## 二、限流手段（熔断的一种）

限流手段和熔断的手段的配置是一样的！

把熔断降级的类抽取出去，条件要求和fallback一致。

```java
package com.woniuxy.controller;

import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.woniuxy.entity.Order;
import com.woniuxy.entity.utils.ResponseMyEntity;

/**
 * @Author: 马宇航
 * @Todo: 熔断处理类
 * @DateTime: 25/04/08/星期二 11:09
 * @Component: 成都蜗牛学苑
 **/
public class OrderControllerBlockHandler {
    public static ResponseMyEntity blockMethod(Order order, BlockException e){
        return new ResponseMyEntity(200,e.getMessage()+"，服务器正在维护中...");
    }
    /**
     * 限流熔断效果
     * ChangeLog : 1. 创建 (25/04/08/星期二 11:10 [马宇航]);
     * @param order 正常的业务
     * @param e  熔断异常情况
     * @return com.woniuxy.entity.utils.ResponseMyEntity
    */
    public static ResponseMyEntity currentThrottlingMethod(Order order, BlockException e){
        return new ResponseMyEntity(500,e.getMessage()+"，请慢一点，太快了...");
    }
}
```

OrderController

```java
//TODO 订单的创建, 单独springboot，要 调用product的接口，修改库存
@PostMapping("/")
//设置fallback 降级方法
@SentinelResource(value = "createOrder",blockHandler = "currentThrottlingMethod",blockHandlerClass = OrderControllerBlockHandler.class)
public ResponseMyEntity createOrder(@RequestBody Order order) throws Exception{
    return orderService.createOrder(order);
}
```



配置createOrder的流控规则，流量控制。



![image-20250408111911034](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081119111.png)

限流配置如上图，效果如下：jmeter工具，10000个线程，跑2轮。

![image-20250408112344335](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081123474.png)

> 除了你配置的5个单机阈值的线程放行进入了系统，其他线程，直接熔断降级。
>
> 除了限流效果很拔群以外。
>
> 数据库问题也很大！

![image-20250408112612633](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081126696.png)



想要单机解决超卖问题，可以在product的服务中，添加锁。

```java
@Override
public synchronized int update(Integer pid, Integer num) {
    Product product = productDao.findById(pid).get();
    product.setStock(product.getStock() - num);
    Product save = productDao.save(product);
    if (save != null) {
        //1表示成功
        return 1;
    }else {
        return 0;
    }
}
```

但是，jmeter跑出来，耗时1m多钟。

![image-20250408113239873](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081132937.png)

而且，加锁，仅仅只支持单机解决。

> 后期在演示具体的锁不住的效果。后面在order服务上加锁，启动2台order集群。

## 三、授权（了解）

可以用于做黑白名单放行，单独的某个接口，我就不允许成都的人访问。



```java
package com.woniuxy.config;

import com.alibaba.csp.sentinel.adapter.spring.webmvc.callback.RequestOriginParser;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletRequest;

/**
 * @Author: 马宇航
 * @Todo: sentinel的授权配置类。
 * @DateTime: 25/04/08/星期二 14:28
 * @Component: 成都蜗牛学苑
 **/
@Component
public class SentinelConfig implements RequestOriginParser {

    @Override
    public String parseOrigin(HttpServletRequest httpServletRequest) {
        System.out.println(httpServletRequest.getRemoteAddr());
        return httpServletRequest.getRemoteAddr();
    }
}
```



![image-20250408144207675](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081442768.png)

效果就是

![image-20250408144229948](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081442029.png)

除了IP封禁，还可以做是否对方是浏览器访问，以及其他请求头信息，或者token信息，或者账号信息。



## 四、网关产品解析

### 1. 网关是什么？

> 网关要分为两种情况
>
> 1. 微服务中的网关，唯一入口。你访问taobao.com，你关心后端具体是哪一个IP或者的服务器？端口多少？
> 2. 网卡对应IP的网关：GateWay，相当于是 一个唯一出口。
>
> 目前微服务中遇到的问题是：
>
> 1. IP和端口会变，而且还很多。根本记不住
>
> 2. url资源地址是可以记住，不同的资源访问的ip和端口其实不一样。/product 和/order服务其实对应的服务ip端口不一样。
>
> 3. 如果说，需要登录认证？怎么做？无法在所有服务器中做Cookie和Session认证
>
>    1. 那么可以用redis+token来进行认证。
>
>       ![image-20250408152120609](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081521777.png)
>
> 4. 前后端分离跨域问题！请求方式或者ip地址或者端口不同，对于浏览器来说都是跨域。
>
>    1. 前端使用proxy代理服务器，来解决的跨域问题。那么写死的IP和端口。
>
>       ![image-20250408152351766](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081523856.png)
>
>    2. 那么有同学就觉得，那么就后端解决跨域。@CrossOrigin 注解要在所有的Controller上面添加，何况也不合适。代码入侵性太强。
>
>    ![image-20250408153133458](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081531573.png)

有了这个API网关后，好处：

1. 可以在网关这边做登录的filter过滤器，统一认证校验。
2. 监控和日志记录
3. 隐藏具体的内部微服务地址，类似Nginx反向代理服务器。

### 2. 常见网关产品

Kong

基于Nginx+Lua脚本开发，性能高，稳定，有多个可用的插件(限流、鉴权等等)可以开箱即用。问题：只支持Http协议；二次开发，自由扩展困难；提供管理API，缺乏更易用的管控、配置方式。

 

Nginx+lua实现

使用Nginx的反向代理和负载均衡可实现对api服务器的**负载均衡**及高可用

问题：自注册的问题和网关本身的扩展性



`Zuul`

Netflix开源，功能丰富，Zuul是Netflix开源的微服务网关，它可以和Eureka、Hystrix等组件配合使用。Zuul的核心功能就两个：路由、过滤。使用JAVA开发，易于二次开发；需要运行在web容器中，如Tomcat。



问题：缺乏管控，无法动态配置；依赖组件较多；处理Http请求依赖的是Web容器，性能不如Nginx；

由于Zuul本身的设计是基于“多线程阻塞IO”的，所以Zuul就显得很鸡肋，而Zuul 2.0直到2019年5月才发布，Spring Cloud早已推出了自己的 Spring Cloud Gateway。



简单来说，Zuul的前途不乐观，而Spring Cloud Gateway会有很好的发展。直接学习Gateway的。

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
        </dependency>
    </dependencies>
```

Traefik

Go语言开发；轻量易用；提供大多数的功能：服务路由，**负载均衡**等等；提供WebUI

问题：二进制文件部署，二次开发难度大；UI更多的是监控，缺乏配置、管理能力；

 

Traefik（发音为 "tra-FEEK"）是一个开源的反向代理和负载均衡器，特别适用于微服务架构和容器化环境，如 Docker、Kubernetes 等。在 Gateway 领域，Traefik 主要用于管理和路由进入应用程序的流量。

#### Traefik 的主要特点：

1. **简化配置**：Traefik 设计简洁，配置直观，通常通过配置文件或服务发现机制自动配置。
2. **服务发现**：能够自动发现服务配置，特别适合动态和自动化的容器环境。
3. **负载均衡**：支持多种负载均衡策略，确保流量均匀分配到后端服务。
4. **安全性**：支持 HTTPS、TLS、Let's Encrypt 等安全特性，保障通信安全。
5. **监控和指标**：提供丰富的监控和指标输出，方便运维人员实时监控服务状态。

#### 在 Gateway 领域的应用：

- **流量路由**：根据请求的域名、路径等信息，将流量路由到不同的后端服务。
- **服务熔断和限流**：保护后端服务不被过载，确保系统的稳定性。
- **金丝雀发布和蓝绿部署**：支持渐进式发布和无缝切换，降低发布风险。
- **API 网关**：可以作为 API 网关使用，管理 API 的访问和认证。

Traefik 因其轻量级和易用性，在微服务架构中非常受欢迎，常用于替代传统的反向代理服务器如 Nginx 和 HAProxy。



`Spring Cloud Gateway`

SpringCloud提供的网关服务 



## 五. GateWay简介

### 1. 名词概念 路由

路由管理，就是我们网关的核心，带着问题？网关是统一了我们的IP和端口，那么它怎么知道我要访问什么服务？

网关通常是有负载均衡效果，loadbalance  简写 lb，记住这个单词 lb。

![image-20250408161020973](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081610129.png)

SpringCloud Gateway 作为 Spring Cloud 生态系统中的网关，目标是替代 Zuul，基于 Spring Framework 5、Project Reactor 和 Spring Boot 2.0 构建。它采用了响应式编程模型，利用了 Reactor 的**非阻塞 I/O 和异步特性 合并AIO**。这种架构使得 SpringCloud Gateway 能够**高效地处理大量并发请求**。

> 延伸出一个**面试题**：NIO,BIO（blocking IO）,AIO 是什么意思？
>
> 阻塞是什么？常见BlockingQueue，阻塞队列。阻塞是CPU不执行这个线程任务。
>
> 上厕所，我们其实是非阻塞状态，我们会着急着原地转圈圈---->CPU空转！ 线程在执行，但是无法做任务。
>
> 非阻塞 IO：CPU空转，消耗CPU性能，提高效率。
>
> 阻塞IO：CPU交出执行权，等待，唤醒线程。冻结在这里，CPU不管他。
>
> 

在Java编程中，NIO、BIO和AIO是三种不同的I/O（输入/输出）处理模型，它们各自有不同的特点和适用场景。

#### 1. BIO（Blocking I/O，阻塞I/O）
- **特点**：
  - **阻塞式**：在读取或写入数据时，线程会被阻塞，直到操作完成。
  - **同步**：操作是同步进行的，即一个线程在执行I/O操作时，其他操作必须等待。
  - **简单易用**：适用于简单的、低并发的应用。

- **适用场景**：
  
  - 单线程或少量并发请求的应用。
  - 传统C/S架构的应用。
  
- **示例**：
  ```java
  Socket socket = new Socket("localhost", 8080);
  InputStream input = socket.getInputStream();
  int data = input.read(); // 这里的read()操作会阻塞
  ```

#### 2. NIO（Non-blocking I/O，非阻塞I/O）
- **特点**：
  - **非阻塞式**：在读取或写入数据时，线程不会被阻塞，可以立即返回并处理其他任务。
  - **同步**：尽管是非阻塞的，但操作仍然是同步的。
  - **基于Selector**：使用Selector（选择器）来管理多个通道（Channel），实现多路复用。
  - **缓冲区（Buffer）**：使用缓冲区来存储数据，提高了数据处理效率。

- **适用场景**：
  
  - 高并发、多连接的应用。
  - 如网络服务器、聊天服务器等。
  
- **示例**：
  ```java
  Selector selector = Selector.open();
  SocketChannel channel = SocketChannel.open();
  channel.configureBlocking(false);
  channel.register(selector, SelectionKey.OP_READ);
  while (true) {
      selector.select(); // 非阻塞地等待就绪的通道
      Set<SelectionKey> keys = selector.selectedKeys();
      for (SelectionKey key : keys) {
          if (key.isReadable()) {
              // 处理读取操作
          }
      }
  }
  ```

#### 3. AIO（Asynchronous I/O，异步I/O）
- **特点**：
  - **异步式**：在读取或写入数据时，线程不会被阻塞，操作完成后会通过回调函数通知。
  - **非阻塞**：操作是完全非阻塞的。
  - **基于Future和CompletionHandler**：使用Future和CompletionHandler来管理异步操作的结果。

- **适用场景**：
  
  - 需要高性能、高并发的应用。
  - 如文件服务器、高性能网络服务器等。
  
- **示例**：
  
  ```java
  AsynchronousServerSocketChannel serverChannel = AsynchronousServerSocketChannel.open();
  serverChannel.bind(new InetSocketAddress(8080));
  serverChannel.accept(null, new CompletionHandler<AsynchronousSocketChannel, Object>() {
      @Override
      public void completed(AsynchronousSocketChannel clientChannel, Object attachment) {
          // 处理新的连接
          serverChannel.accept(null, this); // 继续接受其他连接
      }
  
      @Override
      public void failed(Throwable exc, Object attachment) {
          // 处理失败情况
      }
  });
  ```

#### 总结
- **BIO**：简单易用，适用于低并发场景。
- **NIO**：通过非阻塞和Selector实现高并发，适用于中等并发场景。
- **AIO**：通过异步操作实现高性能，适用于高并发场景。

选择哪种I/O模型取决于具体的应用需求和性能要求。

而为了提升网关的性能，SpringCloud Gateway是基于WebFlux框架实现的，而WebFlux框架底层则使用了高性能的Reactor模式通信框架Netty。



Spring Cloud Gateway 的目标，不仅提供统一的路由方式，并且基于 Filter 链的方式提供了网关基本的功能，例如：安全，监控/指标，和限流。





### 2. 特征

SpringCloud官方，对SpringCloud Gateway 特征介绍如下：

（1）基于 Spring Framework 5，Project Reactor 和 Spring Boot 2.0

（2）集成 Hystrix 断路器，也可以集成**sentinel** 熔断器。

（3）集成 Spring Cloud DiscoveryClient (注册与发现），也能集成**Nacos**。

（4）**Predicates**（`断言`：很重要） 和 **Filters** 作用于特定路由，易于编写的 Predicates 和 Filters  开发这个断言和过滤器

（5）具备一些网关的高级功能：**动态路由、限流(sentinel也有,但是更多是单一某个接口限制)、路径重写、认证(授权效果)**。

从以上的特征来说，和Zuul的特征差别不大。SpringCloud Gateway和Zuul主要的区别，还是在底层的**通信框架**上区别。

GateWay不是用的SpringMVC, 用的是WebFlux,流式响应 AI中常见Flux响应.

#### 2.1 专业术语

yml文件中核心配置的三个参数:



![image-20220605221517596](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206052215655.png)

1. `Predicates`（断言）：

   这是一个 Java 8 的 Predicate，可以使用它来匹配来自 **HTTP 请求的任何内容**，例如 headers (里面通常有**Authorization**)或参数。断言的输入类型是一个 **ServerWebExchange**(类似MQ中讲的交换机)。`断言`是路由处理的**第一个环节**，它是路由的匹配规则，它决定了一个网络请求是否可以匹配给当前路由来处理。之所以它是一个集合的原因是我们可以给一个路由添加多个断言，当每个断言都匹配成功以后才算过了**路由的第一关**。

2. `Filter`（过滤器）：扩展出的面试题，SpringSecurte框架底层原理。

   > 什么是过滤器和拦截器??常见面试题.
   >
   > 过滤器是什么? 常用于 所有的Web服务,都可以用。
   >
   > 拦截器是？SpringMVC中特有的，Interceptor，离开这个环境就没了。
   >
   > 先后顺序，请求--->filter--->Interceptor--->后端接口--->service-->dao--->mysql--->dao--->service-->controller--->Interceptor--->filter--->client
   >
   > 4000工资怎么回答：
   >
   > filter --->pre --->后端--->post，
   >
   > interceptor 包含 3个东西，PreHandler --->后端--> PostHander--->前端页面 ---> afterCompletion

   和Zuul的过滤器在概念上类似，可以使用它**拦截和修改请求**，并且对上游的响应，进行二次处理。过滤器为org.springframework.cloud.gateway.filter.GatewayFilter类的实例。

   如果请求通过了前面的断言匹配，那就表示它被当前路由正式接手了，接下来这个请求就要经过一系列的过滤器集合。过滤器的功能就是八仙过海各显神通了，可以对当前请求做一系列的操作，比如说权限验证，或者将其他非业务性校验的规则提到网关过滤器这一层。在过滤器这一层依然可以通过修改Response里的Status Code达到中断效果，比如对鉴权失败的访问请求设置Status Code为403之后中断操作。

3. Route（路由）：

   网关配置的基本组成模块，和Zuul的路由配置模块类似。一个Route模块由一个 ID，一个目标 URI，一组断言和一组过滤器定义。如果断言为真，则路由匹配，目标URI会被访问。

4. URI:

   如果请求顺利通过过滤器的处理，接下来就到了最后一步，那就是转发请求。URI是统一资源标识符，它可以是一个具体的网址，也可以是IP+端口的组合，或者是Nacos中注册的服务名称

https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/



![image-20250107111433264](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202501071114467.png)

#### 2.2 工作流程

客户端向 Spring Cloud Gateway 发出请求。然后在 Gateway Handler Mapping 中找到与请求相匹配的路由，将其发送到 Gateway Web Handler。Handler 再通过指定的过滤器链来将请求发送到我们实际的服务执行业务逻辑，然后返回。过滤器之间用虚线分开是因为过滤器可能会在发送代理请求之前（“pre”）或之后（“post”）执行业务逻辑。

![Spring Cloud Gateway Diagram](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206031822000.png)

| 类型 | 作用                                                         |
| ---- | ------------------------------------------------------------ |
| pre  | 这种过滤器在请求**被路由之前**调用。我们可利用这种过滤器实现身份验证、在集群中选择请求的微服务、记录调试信息等。 |
| post | 这种过滤器在路由**到微服务以后执行**。这种过滤器可用来为响应添加标准的HTTP Header、收集统计信息和指标、将响应从微服务发送给客户端等。 |

client-->pre--->后端业务--->post

Filter在“pre”类型过滤器中可以做参数校验、权限校验、流量监控、⽇志输出、协议转换等，在“post”类型的过滤器中可以做响应内容、响应头的修改、⽇志的输出、流量监控等。

## 六、快速搭建网关

### 1. 创建myh-gateway ： 10000

不写了

### 2. 引入依赖

GateWay的依赖是单独引入的，其他模块不用！

```xml
    <dependencies>
        <dependency>
            <groupId>com.woniuxy</groupId>
            <artifactId>myh-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
<!--        引入GateWay依赖: 后面可能会报错-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
    </dependencies>
```

![image-20250408171706131](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081717290.png)

### 3.引入yml

使用nacos，复制一份，自己改，myh-gateway-dev.yml，大体配置都和之前一样。

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
        prefix: myh-gateway  #类似 spring.application.name的名字，和配置中心的配置文件前缀 myh-order
        file-extension: yml  #文件扩展名  .yml
  # 多环境配置 dev开发环境，test测试环境 prod 生产环境
  profiles:
    active: dev  # 第二部分配置 myh-order-dev
```



开发启动类，启动就报错

![image-20250408172128317](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504081721470.png)

为什么产生？怎么解决？移除web的依赖。因为GateWay使用了WebFlux就不能使用WebMVC。

```xml
<dependency>
    <groupId>com.woniuxy</groupId>
    <artifactId>myh-commons</artifactId>
    <version>1.0-SNAPSHOT</version>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </exclusion>
    </exclusions>
</dependenc
```

继续补充GateWay的yml配置，为了方便，暂时在本地写，不要再nacos中写，nacos最后写完，测试没问题再把配置写上面。

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
        prefix: myh-gateway  #类似 spring.application.name的名字，和配置中心的配置文件前缀 myh-order
        file-extension: yml  #文件扩展名  .yml
    gateway:
      routes:  # 路由配置 数组配置，使用-前缀
        - id: myh-order # 路由id 对方应用服务id
          uri: lb://myh-order # 路由地址 lb 负载均衡 冒号后面是服务id
          predicates: # 路由条件 断言
            - Path=/order/**
        - id: myh-product # 路由id
          uri: lb://myh-product # 路由地址
          predicates: # 路由条件
            - Path=/product/**,/product2/**
        - id: myh-auth # 路由id
          uri: lb://myh-auth # 路由地址
          predicates: # 路由条件
            - Path=/auth/**,/register/**
  # 多环境配置 dev开发环境，test测试环境 prod 生产环境
  profiles:
    active: dev  # 第二部分配置 myh-order-dev
```

