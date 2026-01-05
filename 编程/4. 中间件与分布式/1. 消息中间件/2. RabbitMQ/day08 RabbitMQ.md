# day08 RabbitMQ

## 一、权限RBAC

> 权限有哪些？
>
> 1. 白名单、黑名单，允许用户在不登录的时候访问。
> 2. 菜单权限：最基本的项目的要求，也是大家以后上班接触最多的，但是不太需要你去开发了。
> 3. 按钮权限：更为细致的权限，若依里面就有按钮权限，**是否能看见**新增删除按钮，以及是否**后端能直接执行**删除命令 通常都会在后端设置权限限制的注解。
> 4. 数据权限：在saas系统，或者paas系统里面非常常见。
>    1. saas和paas：基础设施即服务（IaaS）；2. 平台即服务（PaaS）；3. 软件即服务（SaaS）
>    2. 服务器数据库都是厂商的，你们公司只是有使用权。但是便宜。
>    3. 必须要考虑数据权限。（租户：每个使用软件的客户。）
>    4. 若依的数据权限，会根据用户名和部门来进行过滤。每一行数据新增的时候会给创建人和创建人的部门字段。
>    5. ![image-20250310104821608](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101048666.png)
>    6. gitlab 支持本地私有化部署，功能和github和gitee一样。



## 二、RBAC

### 1. 概念

**基于角色的权限访问控制**(Role-Based Access Control)作为传统访问控制(自主访问，强制访问)的有前景的代替受到广泛的关注。在RBAC中，权限与角色相关联，用户通过成为适当角色的成员而得到这些角色的权限。

1. 角色：一个统称，admin，public，超级管理员，部门负责人。
2. 权限：有菜单的权限，数据操作的权限，数据权限。



### 2. 无角色方案

用户，权限。用户和权限直接关联。

![image-20250310112328574](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101123622.png)

会有什么问题呢？

> 1. 每个人都要单独维护
>
> 2. 权限统一删除修改，会很麻烦 ids（1,2,3,4,5,7,6）
>
> 3. sql语句有点麻烦: 会涉及到子查询，算是最简单的sql语句。
>
>    ```sql
>    select * from perm where 权限id in (select 权限字段ids from user where userid=1);
>    ```

### 3. 有角色表（RBAC）

> 最简单的RBAC ，仅仅只需要5张表即可。

![image-20250310112936433](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101129507.png)

![image-20210425225959659](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/20210425225959.png)

这种方案，就可以快速的，实现员工的权限分配。

### 4. 菜单冗余按钮权限（若依的方案）

> 去掉权限表，把权限和菜单结合在了一起。但是菜单中过于冗余（冗余把**功能按钮权限**也要加入到菜单中）了。

### ![image-20250310113226634](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101132714.png)5. 权限菜单分离方案

> 这个方案的问题就在于，sql语句，会查询多次，不仅要考虑菜单的权限，还要去权限表查询按钮权限。

![image-20250310113609895](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101136996.png)

### 6. 租户方案（saas系统，数据权限方案）

> 每一个注册了我的saas系统的公司，都会给他分配一个租户id。数据新增的时候，都要插入这个租户id。
>
> 根据租户id来区分数据属于哪个公司的。





### 7 分库方案（paas系统，每个租户，拥有自己的库）

> mysql数据库，分成了多个不同的库，每个公司使用不同的数据库。成本提高，数据安全提高。



![image-20250310114101667](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101141737.png)

## 三、常见的认证机制

session，cookie机制，token机制等，有点不明所以。

### 1. HTTP Basic Auth（原始最早的认证方案）

每次请求，都带上账号密码，没有带cookie信息。不安全的方案。生产环境下，现在看不见这种方案了。



### 2. Cookie 认证

常见于前后端不分离的系统，前端代码，和后台代码共用一个服务容器（tomcat），他们的端口ip都一样。前端会用cookie在存储登录的用户信息，但是后端存储的名字叫做session。

> 浏览器请求，都是无状态请求（服务器其实不知道浏览器发来请求的这个人是谁。），所以，浏览器的所有请求，都要在请求头里面带上cookie信息。 加密的数据，除了服务端，其他用户根本没法解析这个信息。

![image-20250310114626521](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101146595.png)



后端，会拿着cookie的id: 123456asdfjlkasjfb值，存储持久化到服务器的本地磁盘上：session:123456asdfjlkasjfb 文件，里面就是你当前登录的用户信息{"id":1,"username":"admin",role:……}。序列化和反序列化的问题。



### 3. OAuth 第三方认证协议

> 常见现在使用的多的就是 Oauth2 第三方认证方案，微信，qq，微博登录。
>
> Oauth就是一种协议，**通信之下必有协议**。
>
> 协议简单来说就是双方约定的一种固定语法，不然听不懂。就是给网络使用的，https协议，tcp协议，udp协议等等。
>
> 中文协议，才能确保我们之间能够正常通信。



基于不同的使用场景，**OAuth2.0**设计了四种模式：

1. 授权码（Authorization Code）：OAuth2.0 标准授权步骤，Server 端向 Client 端下放 `Code` 码，Client 端再用 `Code` 码换取授权 `Access-Token`。
2. 隐藏式（Implicit）：无法使用授权码模式时的备用选择，Server 端使用 URL 重定向方式直接将 `Access-Token` 下放到 Client 端页面。
3. 密码式（Password）：Client 端直接拿着用户的账号密码换取授权 `Access-Token`。
4. 客户端凭证（Client Credentials）：Server 端针对 Client 级别的 Token，代表应用自身的资源授权。

![https://oss.dev33.cn/sa-token/doc/oauth2/sa-oauth2-setup.png](https://oss.dev33.cn/sa-token/doc/oauth2/sa-oauth2-setup.png)

> 流程解释：
>
> 1. 客户端ProcessON 向第三方服务端 微信，发起请求，请求获得授权码（弹出一个微信登录二维码，你用手机扫码一下，手机上要点击授权。）
>
>    ![image-20250310115539109](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101155305.png)
>
> 2. 客户端就会收到微信服务器发过去的授权码。
>
> 3. 使用这个授权码去微信的服务器尝试登录，登录后，微信就会给你的客户端发送一个 token密钥。
>
> 4. 客户端判断登录成功后，就可以使用这个token，去微信的**资源服务器**拿到相应的资源（微信名字，微信头像，电话号码等，这些东西都是 微信那边限制好了的，并不能拿到所有信息。）。
>
> 5. 客户端拿到微信的基本个人信息之后，直接帮你去走注册流程（要注册到自己的数据库）。
>
> 6. 下一次扫码登录，就不会有注册流程了，直接拿着微信的认证用户名，判断本地数据库有没有注册过，有直接登录成功。



这里有一个特殊要求，不是所有的客户端，都可以随便去第三方系统中，获得授权码的。你的服务器必须先去第三方服务器上，认证。



[OAuth2.0 第三方开放平台完整开发流程参考](https://sa-token.cc/doc.html#/oauth2/readme?id=oauth20-第三方开放平台完整开发流程参考)

1. oauth2-server 平台端
   1. 搭建 oauth2-server 数据后台管理端（后台人员对底层数据增删改查维护的地方）。
   2. 搭建 oauth2-server 数据前台申请端（给第三方公司提供一个申请注册 client 的地方）。
   3. 搭建 oauth2-server 授权端 以及其接口文档（让第三方公司拿到 access_token）。
   4. 搭建 oauth2-server 资源端 以及其接口文档（让第三方公司通过 access_token 拿到对应的资源数据）。
   5. 以上四端可以为一个项目，也可以为四个独立的项目。
2. oauth2-client 第三方公司端
   1. 第三方公司登录 oauth-server 数据前台申请端，申请注册应用，拿到 `clientId`、`clientSecret` 等数据。
   2. 根据自己的业务选择对应的 scope 申请签约，等待平台端审核通过。
   3. 在自己系统通过 `clientId`、`clientSecret` 等参数对接 oauth2-server 授权端，拿到 `access_token`。
   4. 通过 `access_token` 调用 oauth2-server 资源端接口，拿到对应资源数据。
3. 用户端操作
   1. 打开第三方公司开发的网站或APP等程序。
   2. 一般有个“通过xx第三方登录”的按钮，点它。
   3. 跳转到了 oauth2-server 端的网站，在此网站用 oauth2-server 的账号开始登录。
   4. 登录完成，继续跳转到授权页，点击确认授权。
   5. 授权完成，oauth2-server 端生成一个 code 码，重定向回 oauth2-client 的网站，把 code 参数挂到对应的 url 上。
   6. oauth2-client 从 url 中读取 code 参数，提交到 oauth2-client 的后端。
   7. oauth2-client 后端拿着 `code`、`clientId`、`clientSecret` 等信息调用 oauth2-server 授权端 的接口，得到 `access_token`。
   8. 继续拿着 `access_token` 调用 oauth2-server 资源端获取此用户对应的数据。
   9. 一般最终目的拿到一个 openid 值，oauth2-client 根据 openid 进行登录。生成自己的会话 token ，返回到数据到前端。
   10. 前端拿到自己 oauth2-client 生成的会话 token ，完成登录。开始进行业务操作。



### 4. Token Auth

现在最常用的方案，也是前后端分离、小程序、app等的认证方案。

> 下面的内容，本质上，还是可以用cookie来进行认证，只不过，只能手动的处理。后端手动每次返回 sessionid，前后手动存储cookieid，每次请求手动把cookieid拼接到请求头中。

[何为无 Cookie 模式?](https://sa-token.cc/doc.html#/up/not-cookie?id=何为无-cookie-模式)

无 Cookie 模式：特指不支持 Cookie 功能的终端，通俗来讲就是我们常说的 —— **前后端分离模式**。

常规 Web 端鉴权方法，一般由 `Cookie模式` 完成，而 Cookie 有两个特性：

1. 可由后端控制写入。
2. 每次请求自动提交。

这就使得我们在前端代码中，无需任何特殊操作，就能完成鉴权的全部流程（因为整个流程都是后端控制完成的）
而**在app、小程序等前后端分离场景**中，一般是没有 **Cookie 这一功能**的，此时大多数人都会一脸懵逼，咋进行鉴权啊？

见招拆招，其实答案很简单：

- 不能后端控制写入了，就前端自己写入。（难点在**后端如何将 Token 传递到前端**）
- 每次请求不能自动提交了，那就手动提交。（难点在**前端如何将 Token 传递到后端**，同时**后端将其读取出来**）



所以，我们的token认证机制就出来了。

把以前的sessionid，变成了token 字符串。



最常用的token方案，JWT （**Json  Web Token**）。

**eyJhbGciOiJIUzI1NiJ9**`.`eyJqdGkiOiI4ODgiLCJzdWIiOiLnrqHnkIblkZgiLCJpYXQiOjE2MTk2MzIwMzJ9`.`**5de6yHqZn7SMBduX9Xxu04w4WfREjyRLp5e9iqzSnjE**

jwt的三个部分：

1. 第一个部header ，加密的算法内容 { "alg": "HS256"}
2. 第二个部分 payload 载荷，相当于明文，可以解密解析出来里面存放的内容，同时也是我们用户自定义存储区域类似一个MAP{ "jti": "888", "sub": "管理员", "iat": 1619632032}
3. 第三个部分 签名，它是用第一个部分base64加密后+载荷的base64加密后，再次用base64加密后，得到的一个值。



jwt令牌，可以支持自己**过期失效**的，本质上用redis的ttl也能实现。

双token机制，长短token机制，利用jwt过期，和redis的ttl过期，来实现的一种jwt隐藏的功能。

> 后端不再返回这个长 的jwt令牌，因为jwt可以反向解密的。



## 四、RabbitMQ

### 1. 什么是MQ

消息队列：Message Queue ，一般队列有 FIFO的特性，我们要处理的数据，通常可以放在mq中。

线程之间和进程之间的通信有什么方法？

1. 线程之间的通信，使用静态全局变量。
2. 多个线程操作同一个本地文件，A线程操作的东西，B线程是知道。
3. 多线程直接操作redis中的共同数据。



进程之间怎么通信：

1. 还可以使用静态全局变量么？不行，变量的各自进程内部的东西。
2. 本地文件，来实现进程之间的通信呢？微信保存一个文件，qq去读取。
3. 都去操作同一个redis的共同数据，可以。
4. 可以使用MQ（redis也可以实现，list是不是可以用来做 队列），来实现，进程之间的通信。



,MQ可以帮助我们实现多个进程之间的通信，所以同一个进程之间的多个线程，也可以使用MQ来实现相互之间的通信。



### 2. 什么是RabbitMQ

RabbitMQ是⼀个由`erlang`开发的基于AMQP（Advanced Message Queue ）协议的**开源**实现。⽤于在分布式系统中存储转发消息，在易⽤性、扩展性、⾼可⽤性等⽅⾯都⾮常的优秀。是当前最主流的消息中间件之⼀。

尽管消息流经RabbitMQ，但它们只能存储在队列中。⼀个队列只受主机内存和磁盘限制的约束，它本质上是⼀个很⼤的消息缓冲区。许多⽣产者可以发送进⼊⼀个队列的消息，并且许多消费者可以尝试从⼀个队列接收数据。实质上是⽣产者——消费者关系。



主⻚地址:https://www.rabbitmq.com/



### 3. 特点

RabbitMQ 最初起源于⾦融系统，⽤于在分布式系统中存储转发消息，在易⽤性、扩展性、⾼可⽤性等⽅⾯表现不俗。

1. 可靠性（Reliability）

   RabbitMQ 使⽤⼀些机制来保证可靠性，如**持久化**、**传输确认**、**发布确认**。

2. 灵活的路由（Flexible Routing）

   在消息进⼊队列之前，通过 Exchange 来路由消息的。对于典型的路由功能，RabbitMQ 已经提供了⼀些内置的 Exchange 来实现。针对更复杂的路由功能，可以将多个 Exchange 绑定在⼀起，也通过插件机制实现⾃⼰的Exchange。

3. **消息集群**（Clustering）

   多个 RabbitMQ 服务器可以组成⼀个集群，形成一个逻辑 **Broker**。**集群**的目的也是为了可靠性。

4. **高可用**（Highly Available Queues）

   队列可以在集群中的机器上进⾏镜像，使得在部分节点出问题的情况下队列仍然可⽤。

5. 多语言客户端（Many Clients）

   RabbitMQ ⼏乎⽀持所有常⽤语⾔，⽐如 Java、.NET、Ruby 等等。所有 MQ 产品从模型抽象上来说都是⼀样的过程： 消费者（consumer）订阅某个队列。⽣产者（producer）创建消息，然后发布到队列（queue）中，最后将消息发送到监听的消费者。

   > 跨系统的通信方案。A银行（java）--->B银行（Net），可以实现通信。

6. 管理界面(Management UI)

   提供了易用的用户界面，使得用户可以监控和管理消息 Broker 的许多方面。 

7. 跟踪机制(Tracing)

   如果消息异常，RabbitMQ 提供了**消息的跟踪机制**，使用者可以找出发生了什么。

8. 插件机制(Plugin System)

   提供了许多插件，来从多方面进行扩展，也可以编辑自己的插件。常用的 **延迟插件**。

### 4. 使用场景

消息队列在实际应⽤中包括如下四个场景：

1. **应用解耦**：多应⽤间通过消息队列对同⼀消息进⾏处理，避免调⽤接⼝失败导致整个过程失败；
   1. 你在注册的时候，要发送一个邮件和短信。单线程不解耦，注册成功的前提是，邮件和短信都发送成功。

2. **异步处理**：多应⽤对消息队列中同⼀消息进⾏处理，应⽤间并发处理消息，相⽐串⾏处理，减少处理时间；
   1. 异步：同时进行，互不影响。同时听歌和唱歌。作用于不同事物上。
   2. 同步：协同步骤，一个一个来。一边吃饭一边唱歌。  作用与同一个事物上。

3. **限流削峰**：广泛应⽤于**秒杀或抢购**活动中，避免流量过⼤导致应⽤系统挂掉的情况；
4. **消息驱动的系统**：系统分为消息队列、消息⽣产者、消息消费者，⽣产者负责产⽣消息，消费者(可能有多个)负责对消息进⾏处理；



> **应⽤耦合**



具体场景：

四阶段，全套都是为了应用解耦。



注册的时候，同时发送邮件通知和短信通知。

解耦后，就不会因为这些通知发送失败，导致你的注册失败。



> **异步处理**

具体场景：⽤户为了使⽤某个应⽤，进⾏注册，系统需要发送注册邮件并验证短信。对这两个操作的处理⽅

式有两种：串⾏及并⾏。

1. 串⾏⽅式：新注册信息⽣成后，先发送注册邮件，再发送验证短信；

在这种⽅式下，需要最终发送验证短信后再返回给客户端。

2. 并⾏处理：新注册信息写⼊后，由发短信和发邮件并⾏处理；

在这种⽅式下，发短信和发邮件 需处理完成后再返回给客户端。

这种结果所需要花的时间⽐第⼀种要少.

3. 消息队列并在写⼊消息队列后⽴即返回成功给客户端，则总的响应时间依赖于写⼊消息队列的时间，⽽写⼊消息

队列的时间本身是可以很快的，基本可以忽略不计，因此总的处理时间相⽐串⾏提⾼了2倍，相⽐并⾏

提⾼了⼀倍；

> **限流削峰**

具体场景：购物⽹站开展秒杀活动，⼀般由于瞬时访问量过⼤，服务器接收过⼤，会导致流量暴增，相关系

统⽆法处理请求甚⾄崩溃。⽽加⼊消息队列后，系统可以从消息队列中取数据，相当于消息队列做了⼀次缓

冲。

流程:

1. 请求先⼊消息队列，⽽不是由业务处理系统直接处理，做了⼀次缓冲,极⼤地减少了业务处理系统的压

⼒；

2. 队列⻓度可以做限制，事实上，秒杀时，后⼊队列的⽤户⽆法秒杀到商品，这些请求可以直接被抛弃，

返回活动已结束或商品已售完信息；

> **消息驱动的系统**

具体场景：⽤户新上传了⼀批照⽚， ⼈脸识别系统需要对这个⽤户的所有照⽚进⾏聚类，聚类完成后由对账

系统重新⽣成⽤户的⼈脸索引(加快查询)。这三个⼦系统间由消息队列连接起来，前⼀个阶段的处理结果放

⼊队列中，后⼀个阶段从队列中获取消息继续处理。

特点:

1. 避免了直接调⽤下⼀个系统导致当前系统失败；

2. 每个⼦系统对于消息的处理⽅式可以更为灵活，可以选择收到消息时就处理，可以选择定时处理，也可

以划分时间段按不同处理速度处理；**消息队列的⼯作模式**。

消息队列包括两种模式，点对点模式（point to point， queue）和发布/订阅模式（publish/subscribe，

topic）。

> **点对点**

点对点模式下包括三个⻆⾊：

消息队列

发送者 (⽣产者)

接收者（消费者）

⽣产者将消息发送到队列中,接受者从队列⾥⾯获取消息进⾏消费,⼀旦被消息被消费后,队列⾥⾯就不会在存

在当前消息.

特点:

1. 每个消息只有⼀个接收者（Consumer）(即⼀旦被消费，消息就不再在消息队列中)；

2. 发送者和接收者间没有依赖性，发送者发送消息之后，不管有没有接收者在运⾏，都不会影响到发送者

下次发送消息；

3. 接收者在成功接收消息之后需向队列应答成功，以便消息队列删除当前接收的消息；

> 发布订阅模式

发布/订阅模式下包括三个⻆⾊：

⻆⾊主题（Topic）

发布者(Publisher)

订阅者(Subscriber)发布者将消息发送到Topic,系统将这些消息传递给多个订阅者。

特点: 

1. 每个消息可以有多个订阅者； 
2. 发布者和订阅者之间有时间上的依赖性。针对某个主题（Topic）的订阅者，它必须创建⼀个订阅者之后，才能消费发布者的消息。 
3. 为了消费消息，订阅者需要提前订阅该⻆⾊主题，并保持在线运⾏；

## 五、RabbitMQ的安装

RabbitMQ分为linux安装和window安装,先在window系统上安装来⽤,后续在linux上玩⼉。

### 1. Erlang和MQ的安装 win版

> 安装Erlang：otp_win64_25.0.2.exe

⾸先，您需要安装⽀持的 Windows 版Erlang。下载并运⾏Erlang for Windows 安装程序

下载地址为：https://erlang.org/download/otp_versions_tree.html

下载并运⾏，完后了配置环境变量，测试**Erlang**安装；除了文件路径调整，其他直接默认下一步。

1. 在系统变量中创建ERLANG_HOME，值为erlang的安装位置

   ![image-20220622233434611](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206222334655.png)

2. 在系统的path变量中添加erlang的bin目录路径%ERLANG_HOME%\bin

   ![image-20220622233638336](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206222336391.png)

3. 测试Erlang安装效果

   ![image-20220622234141114](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206222341146.png)

> 安装RabbitMQ: rabbitmq-server-3.10.5.exe

下载地址http://www.rabbitmq.com/install-windows.html

设置环境变量:激活rabbitmq_management启动:

1. 在系统变量上创建RABBITMQ_SERVER

   ![image-20220622234816788](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206222348824.png)

2. 在path变量后添加rabbitmq的sbin路径%RABBITMQ_SERVER%\sbin

   ![image-20220622234933360](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206222349410.png)

3. 测试rabbitMQ安装情况(重新打开cmd)

   ![image-20220622235142700](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206222351764.png)

   如果显示以上内容表示rabbitmq还没有启动，需要安装插件。

   输入rabbitmq-plugins.bat enable rabbitmq_management安装，显示以下内容表示安装成功

   ![image-20220622235402984](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206222354029.png)

4. 运行MQ：输入rabbitmq-server.bat启动rabbit，显示以下内容表示启动成功。

   ![image-20220622235852549](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206222358618.png)

5. 访问测试:

   测试地址 http://localhost:15672/

   默认的⽤户名：guest

   默认的密码为：guest

   登录成功后页面如下：

   ![image-20220623000109997](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206230001149.png)

### 2.Linux版安装

#### 2.1 安装erlang

 erlang下载地址：http://erlang.org/download/

下载otp_src_18.3.tar.gz，上传该

在linux服务器上，可以在usr目录中看到该文件：

![image-20220702121741175](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021217421.png)

切换到该目录。解压otp_src_19.3.tar.gz

	cd /usr/
	tar zxvf otp_src_18.3.tar.gz

进入otp_src_18.3目录

	cd otp_src_18.3

接下来我们要安装各种erlang所需要的依赖库（这一步骤可以提前，不是非要固定在这个位置的）:

	yum -y install make gcc gcc-c++ kernel-devel m4 ncurses-devel openssl-devel unixODBC-devel 

然后，运行otp_src_18.3目录下的configure脚本，来进行配置安装目录，这里我们预备安装到/opt/erlang目录下：

	./configure --prefix=/opt/erlang
	
	--prefix参数指定了安装路径

/usr/otp_src_18.3/下执行下列命令，安装Erlang

	make			// 编译
	make install	// 安装

修改vim /etc/profile配置文件，在该文件的最下方添加下面的环境变量配置即可：

	ERLANG_HOME=/opt/erlang
	export PATH=$PATH:$ERLANG_HOME/bin
	export ERLANG_HOME

最后执行如下命令让配置文件生效：

	source /etc/profile

可以输入erl命令来验证Erlang是否安装成功，如果出现类似以下的提示即表示安装成功：

![image-20220702131354785](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021313020.png)

#### 2.2 安装MQ

下载RabbitMQ3.6.12[点击下载](https://github.com/rabbitmq/rabbitmq-server/releases/tag/rabbitmq_v3_6_12)，请用点魔法。

![image-20220702134216009](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021342079.png)

将下载好的压缩包，上传到服务器/usr文件夹中中：

![image-20220702134711081](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021347343.png)

在将rabbitmq上传给linux后，再将rabbitmq安装到与erlang同一个目录（/opt）下面：并修改文件名为rabbitmq

	tar vxf /usr/rabbitmq-server-generic-unix-3.6.12.tar.xz -C /opt
	mv /opt/rabbitmq_server-3.6.12/ /opt/rabbitmq

同样修改vim /etc/profile文件，添加下面的环境变量：

	export PATH=$PATH:/opt/rabbitmq/sbin
	export RABBITMQ_HOME=/opt/rabbitmq

此时的/etc/profile文件的末尾内容为：

![image-20220702133148731](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021331878.png)

之后执行以下命令让配置文件生效：

	source /etc/profile

#### 2.3 运行MQ

在修改了/etr/profile后，可以任意打开一个shell窗口，输入以下命令以运行RabbtiMQ服务：

	rabbitmq-server -detached

> 在rabbitmq-server命令后面添加一个"-detached"参数是为了能够让RabbitMQ服务以守护进程的方式在后台运行
> 这样就不会因为当前Shell窗口的关闭而影响服务。

如果不加-detached，则是以前台启动，该窗口关闭则退出。

![image-20220702140431314](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021404439.png)



运行以下命令查看RabbitMQ是否正常启动：	

	rabbitmqctl status

如果RabbitMQ正常启动，会输出以下信息：

![image-20220702140558618](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021405074.png)

> 如果此处打开速度极慢，则需要设置hosts文件：
>
> 原因是因为你本机名称如果不是localhost，速度就会很慢。
>
> ![image-20220702140755654](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021407692.png)
>
> vim /etc/hosts
>
> 添加127.0.0.1 localhost

停止命令

```
rabbitmqctl stop
```

#### 2.4 运行MQ控制台

> RabbitMQ的默认账户"guest"，是有限制的！默认情况下，访问RabbitMQ服务的用户名和密码都是"guest"，这个账户有限制，默认只能通过本地网络（如localhost）访问，远程网络访问受限，所以在实现生产和消费消息之前，需要另外添加一个用户，并设置相应的访问权限。

添加新用户，用户名为"root"，密码为"root123":

	rabbitmqctl add_user root root123

为root用户设置所有权限，可配置，可读，可写

	rabbitmqctl set_permissions -p / root ".*" ".*" ".*"

设置root用户为管理员角色：

	rabbitmqctl set_user_tags root administrator

启动控制台：

 ```
rabbitmq-plugins enable rabbitmq_management
 ```

![image-20220702142008269](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021420398.png)

开启5672和15672防火墙：

```
firewall-cmd --add-port=5672/tcp --permanent
firewall-cmd --add-port=15672/tcp --permanent
firewall-cmd --reload
```

![image-20220702142302634](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021423843.png)

访问ip:15672，出现下图即可。

![image-20220702142434519](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202207021424595.png)

使用root账号和root123密码，即可登陆进去。



> 安装的问题，exe安装包，会在服务中启动一个rabbitmq的服务，默认开机自启动。
>
> 部分同学的计算机名字是中文，就会导致服务启动失败，自己换成linux安装。

## 六、基础名词



![image-20250310155034625](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101550755.png)



名词解释：

P，product，生产者。

C，consumer，消费者。

ConnectionFactory（连接管理器）		应用程序与Rabbit之间建立连接的管理器，程序代码中使用；

Channel（信道）						消息推送使用的通道；TCP通道。

VirtualHost（brock MQ集群）			集群环境的统称，一个虚拟系统，里面有多个不同的MQ。

Exchange（交换器 X）					用于接受、分配消息；

Queue（队列）						用于存储生产者的消息；

RoutingKey（路由键）				用于把生成者的数据分配到**交换器**上；

BindingKey（绑定键）				用于把交换器的**消息绑定到队列**上；



### 1. queue

就是一个正常的允许持久化的一个对象，可以设置为临时的队列，以及自动删除。



### 2. exchange的类型

MQ中的交换机，有点像现实的交换机，它就是用来分发不同的消息，进入不同的队列。

Exchange常用有4种类型：

**direct**(默认)，fanout, topic, 和headers，不同类型的Exchange转发消息的策略有所区别：

1. **Direct** 直接交换器，⼯作⽅式类似于单播，Exchange会将消息发送**完全匹配ROUTING_KEY的Queue**
2. fanout 广播是式交换器，不管消息的ROUTING_KEY设置为什么，Exchange都会将消息转发给所有绑定的Queue。
3. **topic** 主题交换器，⼯作⽅式类似于组播，Exchange会将消息转发和ROUTING*KEY*匹配模式相同的所有队列，⽐如，*ROUTING*KEY为user.stock的Message会转发给绑定匹配模式为 *.stock,user.stock， * . * 和#.user.stock.#的队列。（ * 表是匹配⼀个任意词组，#表示匹配0个或多个词组）
4. headers 消息体的header匹配（ignore忽略）  这个不用学 根据请求头中的数据，来进行分流。

![image-20250310155825554](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101558666.png)



### 3. 工作模式

1. 简单模式

   ![image-20250310160057959](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101600089.png)

2. 工作模式

   ![image-20250310160106646](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101601769.png)

3. 发布订阅模式

   ![image-20250310160121361](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101601489.png)

4. 路由模式

   ![image-20250310160138186](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101601298.png)

5. 主题模式

   ![image-20250310160153796](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101601943.png)

## 七、简单原生模式开发

### 1. 在ruoyi项目中新增一个子模块

引入依赖

```xml
<dependency>
    <groupId>com.rabbitmq</groupId>
    <artifactId>amqp-client</artifactId>
</dependency>
```

### 2. 生产者，连接到本地

```java
package com.woniuxy.nativedemo;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Author: 马宇航
 * @Todo: 创建一个原生的 可以利用 mq的 生产者
 * @DateTime: 25/03/10/星期一 16:08
 * @Component: 成都蜗牛学苑
 * <img style="width:500px" src="https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101550755.png"/>
 **/
public class NativeProduct {
    public static void main(String[] args) throws IOException, TimeoutException {
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("127.0.0.1");
        // 端口号 和浏览器的ui后台15672 不一样  后面yml中配置，就写5672
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("java112");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin123");
        //连接工厂是创建 connection ，需要信道 channel
        Connection connection = connectionFactory.newConnection();
        // 信道创建
        Channel channel = connection.createChannel();
        // 声明一个队列，目前没有考虑 交换机，但是实际上已经配置了交换机
        //5个参数 队列名称，是否持久化，是否独占，是否自动删除，其他参数
        channel.queueDeclare("hello",false,false,false,null);
        // 发送消息
        String msg = "Hello World";
        // 参数： 1. 交换机名称 默认是空串交换机 2.路由key默认使用队列名字  3.其他参数 4.消息体
        channel.basicPublish("","hello",null,msg.getBytes());
        // 消息体是字节数组
        System.out.println("消息发送成功");
        // 关闭连接
        channel.close();
        connection.close();
    }
}
```



### 3. 消费者 持续异步监听缴费

```java
package com.woniuxy.nativedemo;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Author: 马宇航
 * @Todo: MQ原生消费者
 * @DateTime: 25/03/10/星期一 16:21
 * @Component: 成都蜗牛学苑
 **/
public class NativeConsumer {
    public static void main(String[] args) throws IOException, TimeoutException {
        //快点直接生成全部消费者内容，基于我的生产者
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("127.0.0.1");
        // 端口号 和浏览器的ui后台15672 不一样  后面yml中配置，就写5672
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("java112");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin123");
        //连接工厂是创建 connection ，需要信道 channel
        Connection connection = connectionFactory.newConnection();
        // 信道创建
        Channel channel = connection.createChannel();
        // 接收消息
        // 参数： 1.队列名称 2.是否自动确认 4.消费者标签 5.其他参数  为什么消息确认要用匿名内部类？
        // 因为是消费异步处理，所以需要回调函数，所以需要匿名内部类
        channel.basicConsume("hello", false,
                (String consumerTag, Delivery message) -> {
                // 消息体
                byte[] body = message.getBody();
                System.out.println("消息接收成功：" + new String(body));
                // 手动确认
                channel.basicAck(message.getEnvelope().getDeliveryTag(), false);
            },
                (String consumerTag)->{
            System.out.println(consumerTag);
        });
        // 自动确认
        // 消息体是字节数组
        System.out.println("消息接收成功");
    }
}
```

![image-20250310163646506](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101636684.png)

## 八、springboot整合开发

### 1. 引入依赖

```xml
<!--        </dependency>-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
        </dependency>
```

![image-20250310173056634](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101730953.png)

admin关联--order --->mq模块

### 2. yml

```yml
spring:
    rabbitmq:
      host: localhost
      port: 5672
      username: admin
      password: admin123
      virtual-host: java112
      listener:
        simple:
          # 消费者每次从队列中拉取的消息数量
          prefetch: 1
          # 消费者每次确认的消息数量
      connection-timeout: 10000
      # 自动消息返回
      publisher-returns: true
      template:
        mandatory: true
```

### 3. 配置类

![image-20250310172354561](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101723844.png)

```java
package com.woniuxy.config;

import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Author: 马宇航
 * @Todo: 工作模式配置类，配置 交换机，队列，绑定关系
 * @DateTime: 25/03/10/星期一 17:22
 * @Component: 成都蜗牛学苑
 * <img src="https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503101723844.png"/>
 **/
@Configuration
public class WorkQueueConfiguration {
    /**
     * 工作模式 队列 队列名字叫做workQueue
     * @return
     */
    @Bean
    public Queue workQueue() {
        //参数解释： 队列名字，是否持久化，是否独占，是否自动删除
        return new Queue("workQueue", true, false, false);
    }
}

```

### 4. 开发生产者（myh-order 订单模块生产者）

#### 1. controller

> 实体类使用 order表即可。

```java
order模块的 controller层，先处理新增方法。
    @PostMapping("/")
    public AjaxResult addOrder(@RequestBody MyhOrder order) {
        if (order == null) {
            return AjaxResult.error("订单信息不能为空");
        }
        if (order.getId() == null){
            //新增
            orderService.save(order);
        }else {
            //修改
            orderService.updateById(order);
        }
        return AjaxResult.success();
    }
```

#### 2. service

```java
@Autowired
RabbitTemplate rabbitTemplate;

@Override
public int save(MyhOrder order) {
    int insert = orderMapper.insert(order);
    if (insert > 0) {
        // 发送消息 routingkey 可以直接绑定空串交换机 然后队列名就成为了routingkey，后面不这么用
        rabbitTemplate.convertAndSend("workQueue", order);
    }
    return insert;
}
```



### 5. 开发一个消费者单独的服务

> 单独创建一个springboot项目，引入springmvc的依赖和amqp的依赖，都是启动器。
>
> 目的是为了让大家感受到，消费者，是2台**不同的进程**，**不同端口**，**轮流消费队列中的数据**。

![image-20250311144206638](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503111442824.png)

#### 1.  引入依赖

```xml
<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-amqp</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.amqp</groupId>
			<artifactId>spring-rabbit-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
	</dependencies>
```

#### 2.  yml配置

> 由于需要多端口启动。

```yml
server:
  port: ${SERVER_PORT} #就是允许配置vm启动参数，-DSERVER_PORT=8089 然后来启动2个不同的端口。
spring:
  rabbitmq:
    username: admin
    password: admin123
    host: localhost
    port: 5672
    listener:
      simple:
        # 消费者每次从队列中拉取的消息数量
        prefetch: 1
        # 消费者手动确认的消息数量
        acknowledge-mode: manual
    connection-timeout: 10000
    virtual-host: java112
  application:
    name: mq-listener
```

涉及到mq反序列化对象的报错，需要配置一个环境变量。

![image-20250311144816187](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503111448402.png)

#### 3.  直接开发一个监听器

> 额外还需要引入若依中的order实体类。

```java
package com.woniuxy.listener;

import com.woniuxy.entity.MyhOrder;
import org.springframework.amqp.core.AcknowledgeMode;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

/**
 * @Author: 马宇航
 * @Todo: 监听器，监听work工作模式的queue消息
 * @DateTime: 25/03/11/星期二 14:07
 * @Component: 成都蜗牛学苑
 **/
@Component//这个和@Service，@Configuration是一样的，仅仅只是给开发的人看的
public class WorkListener {
    //去读取当前的yml配置文件，拿到该服务的端口。
    @Value("${server.port}")
    int port;
    //ackMode="AUTO"   属性是临时的，后面我们用代码逻辑来 ack确认消息。unack情况，异常，或者处理不了。
    @RabbitListener(queues={"workQueue"},ackMode="AUTO")
    public void orderConsumption(MyhOrder order){
        //仅仅只是，模拟下业务，后面正常还要出发支付模块，后面要讲mq死信队列，15min过期。
        System.out.println("服务端口是："+port+";拿到了数据:"+order);
    }
}
```
