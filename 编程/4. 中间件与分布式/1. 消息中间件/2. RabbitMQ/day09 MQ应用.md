# day09 MQ应用

## 一、发布订阅模式下的广播模式

![image-20250311161828302](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503111618428.png)

### 1. myh-mq这个模块下 配置类

> 创建一个交换机，2个队列，并把交换机绑定上两个队列。

```java
package com.woniuxy.config;

import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.FanoutExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Author: 马宇航
 * @Todo: 用来做 发布订阅模式下的 广播模式
 * @DateTime: 25/03/11/星期二 16:19
 * @Component: 成都蜗牛学苑
 * <img src="https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503111618428.png"/>
 **/
@Configuration
public class PublishSubscribeConfig {
    //@Bean 就可以把这个方法的返回值，作为对象的创建，放到spring容器中，bean的名字就叫做 id=fanoutQueue1
    /*
        @Autowired 默认byType  这个就是DI依赖注入
        @Resource  默认byName  bean的id= fanoutQueue1
    */
    @Bean
    public Queue fanoutQueue1() {
        return new Queue("fanoutQueue1");
    }
    @Bean
    public Queue fanoutQueue2() {
        return new Queue("fanoutQueue2");
    }
    //创建交换机
    @Bean
    public FanoutExchange fanoutExchange() {
        return new FanoutExchange("fanoutExchange");
    }
    //需要绑定交换机和队列
    @Bean
    public Binding bindQueue1() {
        //参数解释： Builder后缀，就是用到的建造者模式，也可以一直点下去
        return BindingBuilder.bind(fanoutQueue1()).to(fanoutExchange());
    }
    @Bean
    public Binding bindQueue2() {
        //参数解释： Builder后缀，就是用到的建造者模式，也可以一直点下去
        return BindingBuilder.bind(fanoutQueue2()).to(fanoutExchange());
    }
}
```

### 2. 发布消息

order的save方法里面，第一个mq是进行后续的支付业务。第二个mq就用广播模式进行同时，比如邮件发送和短信发送。

![image-20250311163907683](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503111639743.png)

```java
@Override
public int save(MyhOrder order) {
    //订单保存
    int insert = orderMapper.insert(order);
    if (insert > 0) {
        // 发送消息 routingkey 可以直接绑定空串交换机 然后队列名就成为了routingkey，后面不这么用
        rabbitTemplate.convertAndSend("workQueue", order);
        // 发送消息 广播模式 routingkey是用来绑定交换机和对应的队列的，但是广播不需要绑定，
        rabbitTemplate.convertAndSend("fanoutExchange","", order.getOrderName()+"已经生成，请尽快支付。");
    }
    return insert;
}
```

### 3. 监听消息 mq-listener这个项目，独立springboot项目

```java
package com.woniuxy.listener;

import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @Author: 马宇航
 * @Todo: 广播的监听器
 * @DateTime: 25/03/11/星期二 16:29
 * @Component: 成都蜗牛学苑
 **/
@Component
public class FanoutListener {
    @RabbitListener(queues = "fanoutQueue1",ackMode = "AUTO")
    public void fanoutQueue1(String msg) {
        System.out.println(msg+",模拟邮件发送！");
    }
    @RabbitListener(queues = "fanoutQueue2",ackMode = "AUTO")
    public void fanoutQueue2(String msg) {
        System.out.println(msg+",模拟短信发送！");
    }
}
```

## 二、route路由 直连模式

![image-20250311164121480](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503111641558.png)

### 1. myh-mq配置类

根据上面的图来：

```java
package com.woniuxy.config;

import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.DirectExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/03/11/星期二 16:42
 * @Component: 成都蜗牛学苑
 **/
@Configuration
public class RouteConfig {
    @Bean
    public Queue routeQueue1() {
        return new Queue("routeQueue1", true, false, false);
    }
    @Bean
    public Queue routeQueue2() {
        return new Queue("routeQueue2", true, false, false);
    }
    //变成直连交换机  根据routingkey，直接把交换机连接对应的queue
    @Bean
    public DirectExchange directExchange() {
        return new DirectExchange("directExchange");
    }
    @Bean
    public Binding routeBinding1() {
        return BindingBuilder.bind(routeQueue1()).to(directExchange()).with("a");
    }
    //下面两个方法，都是给Queue2 绑定不同的routingkey
    @Bean
    public Binding routeBinding2() {
        return BindingBuilder.bind(routeQueue2()).to(directExchange()).with("a");
    }
    @Bean
    public Binding routeBinding3() {
        return BindingBuilder.bind(routeQueue2()).to(directExchange()).with("b");
    }
}
```

### 2. 发布消息

```java
@Override
public int save(MyhOrder order) {
    //订单保存
    int insert = orderMapper.insert(order);
    if (insert > 0) {
        // 发送消息 routingkey 可以直接绑定空串交换机 然后队列名就成为了routingkey，后面不这么用
        rabbitTemplate.convertAndSend("workQueue", order);
        // 发送消息 广播模式 routingkey是用来绑定交换机和对应的队列的，但是广播不需要绑定，
        if (order.getIsPay()==1) {
            //已经支付，则两个队列都通知
            rabbitTemplate.convertAndSend("directExchange","a", order.getOrderName()+"已经支付。");
        }else {
            //未支付，只通知队列2 通过routingkey来 表示 消息发给哪个队列
            rabbitTemplate.convertAndSend("directExchange", "b", order);
        }
    }
    return insert;
}
```

### 3. 开始监听

```java
package com.woniuxy.listener;

import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @Author: 马宇航
 * @Todo: 广播的监听器
 * @DateTime: 25/03/11/星期二 16:29
 * @Component: 成都蜗牛学苑
 **/
@Component
public class RouteListener {
    @RabbitListener(queues = "routeQueue1",ackMode = "AUTO")
    public void fanoutQueue1(String msg) {
        System.out.println(msg+",路由直连模式，模拟邮件发送！");
    }
    @RabbitListener(queues = "routeQueue2",ackMode = "AUTO")
    public void fanoutQueue2(String msg) {
        System.out.println(msg+",路由直连模式，模拟短信发送！");
    }
}
```
