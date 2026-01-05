# day10 MQ高级应用

## 一、死信队列

### 1. 名词解释

**DLX**

> - Death Letter Exchange 死信交换机（死信邮箱）
> - 它本身就是个普通的交换机，只不过，当普通交换机无法把消息发送到队列的时候，（3种情况），就会把这个消息发送给死信队列（死信队列其实就是个普通队列）
> - 允许通过参数来控制正常交换机和队列拥有死信队列效果.

**死信**

>当发出去的消息，遇到下列三种情况，就会成为死信消息：（面试题）
>
>1. 当消息被（监听器消费者）拒绝 nack，同时requeue=false，重入队列，false表示**不重回原始队列**。
>2. 消息有TTL过期时间，超过这个时间没有消费
>3. 消息队列达到最大长度，放不下了，后面的消息就变成死信。

**DLQ**

> - Death Letter Queue 死信队列
>
> 死信出现，就会由死信交换机发送到这个死信队列当中。
>
> 切记！！！没有死信交换机和死信队列这两个东西，只是叫这个名字，但是他们本身就是**正常的普通交换机和普通队列**。
>
> 

![image-20250312112850180](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121128289.png)

### 2. 生产者 config配置

环境：

> 正常交换机：
>
> - ttlQueue队列，但是要配置ttl过期时间
> - normal5Queue，设置长度为5
> - ![image-20250312113630805](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121136951.png)
>
> 提供一个死信交换机（普通交换机）：
>
> 提供它连接的所谓的死信对嘞（普通队列）

```java
package com.woniuxy.config;

import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.DirectExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.HashMap;
import java.util.Map;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/03/12/星期三 11:37
 * @Component: 成都蜗牛学苑
 **/
@Configuration
public class DeathConfig {
    //************************************正常队列和正常交换机***********************************************
    @Bean
    public Queue ttlQueue() {
        Map<String, Object> args = new HashMap<String, Object>();
        //设置队列中的消息过期时间
        args.put("x-message-ttl",5000);
        //设置队列的死信交换机
        setDeadLetterExchange(args);
        //参数解释： 队列名字，是否持久化，是否独占，是否自动删除
        return new Queue("ttlQueue",true,false,false,args);
    }
    @Bean
    public Queue normal5Queue() {
        Map<String, Object> args = new HashMap<String, Object>();
        //设置队列中的消息过期时间
        args.put("x-max-length",5);
        //设置队列的死信交换机
        setDeadLetterExchange(args);
        //参数解释： 队列名字，是否持久化，是否独占，是否自动删除
        return new Queue("normal5Queue",true,false,false,args);
    }
    //正常业务交换机
    @Bean
    public DirectExchange nomalExchange() {
        return new DirectExchange("nomalExchange",true,false);
    }
   
    //bingding ttl的过期的
    @Bean
    public Binding ttlBinding() {
        return BindingBuilder.bind(ttlQueue()).to(nomalExchange()).with("ttlRouting");
    }
    //bingding 绑定队列有限制的
    @Bean
    public Binding normal5Binding() {
        return BindingBuilder.bind(normal5Queue()).to(nomalExchange()).with("normalRouting");
    }

    //************************************正常队列和正常交换机***********************************************
     //设置队列的死信交换机与队列之间的绑定关系，因为死信队列不止一个。
    private void setDeadLetterExchange(Map<String, Object> args) {
        //绑定死信交换机名字
        args.put("x-dead-letter-exchange", "deathExchange");
        //绑定死信routingkey
        args.put("x-dead-letter-routing-key", "deathRouting");
    }
    
    //************************************死信队列和死信交换机***********************************************
    //死信交换机：和正常交换机一样
    @Bean
    public DirectExchange deathExchange() {
        return new DirectExchange("deathExchange",true,false);
    }
    @Bean
    public Queue deathQueue() {
        //参数解释： 队列名字，是否持久化，是否独占，是否自动删除
        return new Queue("deathQueue",true,false,false);
    }
    @Bean
    public Binding deathBinding() {
        return BindingBuilder.bind(deathQueue()).to(deathExchange()).with("deathRouting");
    }
        //************************************死信队列和死信交换机***********************************************
}

```



参数：

| 参数名                      | 作用                                                         |
| --------------------------- | ------------------------------------------------------------ |
| x-message-tti(Time-To-Live) | 发送到队列的消息在丟弃之前可以存活多长时间(毫秒)             |
| x-max-length                | 限制队列最大长度（新增后挤出最早的），单位个数               |
| x-expires                   | 队列没有访问超时时，自动删除（包含没有消费的消息），单位毫秒 |
| x-max-length-bytes          | 限制队列最大容量                                             |
| x-dead-letter-exchange      | 死信交换机，将删除/过期的数据，放入指定交换机                |
| x-dead-letter-routing-key   | 死信路由，将删除/过期的数据，放入指定routingKey              |
| x-max-priority              | 队列优先级                                                   |
| x-queue-mode                | 对列模式，默认lazy（将数据放入磁盘，消费时放入内存）         |
| x-queue-master-locator      | 镜像队列                                                     |

### 3. 消息发送

```java
public int save(MyhOrder order) {
    //订单保存
    int insert = orderMapper.insert(order);
    if (insert > 0) {
        // 发送消息 到正常交换机
        order.setOrderName("ttlRouting，自动不消费过期，进入死信队列");
        rabbitTemplate.convertAndSend("nomalExchange","ttlRouting", order);
        order.setOrderName("normalRouting，超过了5个数据，就会进入死信队列");
        rabbitTemplate.convertAndSend("nomalExchange","normalRouting", order);
    }
}
```

### 4. 监听器

目前只开发一个监听死信队列的消费者

```java
package com.woniuxy.listener;

import com.woniuxy.entity.MyhOrder;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @Author: 马宇航
 * @Todo: 死信消费者
 * @DateTime: 25/03/12/星期三 11:57
 * @Component: 成都蜗牛学苑
 **/
@Component
public class DeathListener {
    @RabbitListener(queues="deathQueue",ackMode="AUTO")
    public void deathQueueListener(MyhOrder order) {
        System.out.println("死信的消费："+order);
    }
}
```



> 还缺一个死信情况！
>
> nack情况，下午讲。



### 5. 额外补充

> 可以不用，了解下。死信可以搞定，这个备用交换机就简单了。

![image-20250312140952987](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121409064.png)

![image-20250312141300920](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503121413035.png)

> 后续在四阶段或者就业阶段，我会给大家深入讲解MQ。其中还包括延迟队列，插件。