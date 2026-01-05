# day06 GateWay认证服务

## 一、自定义断言

### 1. 学习目的

> 如何进步？以后工资如何翻倍？不要再写CRUD了。
>
> Spring里面有句话：**约定大于配置**。
>
> GateWay中的自定义断言，就可以I感受到约定大于配置这个说法。

### 2. 怎么学？

首先，以后写自定义断言的机会不多，但是自定义过滤器的机会是有的。同时，因为系统的断言够使用了。

![image-20250409102814933](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504091028994.png)



### 3. 如何找到目标类？

> 先找到系统提供的代码，通过这个代码来学习，就可以让我们自己开发专属的路由方案，和策略。
>
> 需求：
>
> 1. 查询1~5000的id的数据，访问 8091,5001~10000的id的数据，访问8092。



![image-20250409103447630](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504091034719.png)



> 还需要一点常识：
>
> 大家要知道Factory是什么东西？工厂设计模式。现实的工厂是做什么？生产制造东西？放Java，工厂就是生产（单例）对象的。
>
> 在找几个相同的类似代码，分析下源码，就能发现，他们都是继承了AbstractRoutePredicateFactory。
>
> 那么我们自己要自定义，也要继承这个。

```java
public class PathRoutePredicateFactory
       extends AbstractRoutePredicateFactory<PathRoutePredicateFactory.Config> {

    private static final String MATCH_OPTIONAL_TRAILING_SEPARATOR_KEY = "matchOptionalTrailingSeparator";
	//正则路径转换器，顾名思义
    private PathPatternParser pathPatternParser = new PathPatternParser();

    public PathRoutePredicateFactory() {
       super(Config.class);
    }
	//正则判断
    private static void traceMatch(String prefix, Object desired, Object actual,
          boolean match) {
       if (log.isTraceEnabled()) {
          String message = String.format("%s \"%s\" %s against value \"%s\"", prefix,
                desired, match ? "matches" : "does not match", actual);
          log.trace(message);
       }
    }

    public void setPathPatternParser(PathPatternParser pathPatternParser) {
       this.pathPatternParser = pathPatternParser;
    }
	//根据顺序截断写的字段的
    @Override
    public List<String> shortcutFieldOrder() {
       return Arrays.asList("patterns", MATCH_OPTIONAL_TRAILING_SEPARATOR_KEY);
    }
	
    @Override
    public ShortcutType shortcutType() {
       return ShortcutType.GATHER_LIST_TAIL_FLAG;
    }
	//apply正好就是我们的父类抽象类定义的方法，猜测这个就是核心代码（代码最多）
    //形参config对应的实参，我猜，就是自己内部的静态Config类
    @Override
    public Predicate<ServerWebExchange> apply(Config config) {
       final ArrayList<PathPattern> pathPatterns = new ArrayList<>();
       //
       synchronized (this.pathPatternParser) {
           //Separator 猜测 就是 逗号分隔后，再用正则去做点什么事情
          pathPatternParser.setMatchOptionalTrailingSeparator(
                config.isMatchOptionalTrailingSeparator());
           //path可能传多个，
          config.getPatterns().forEach(pattern -> {
             PathPattern pathPattern = this.pathPatternParser.parse(pattern);
              //往数组中，添加，你配置yml中的Path= 后面的值
             pathPatterns.add(pathPattern);
          });
       }
       return new GatewayPredicate() {
           //如果返回true，就放行
          @Override
          public boolean test(ServerWebExchange exchange) {
             PathContainer path = parsePath(
                   exchange.getRequest().getURI().getRawPath());

             PathPattern match = null;
              //pathPatterns 就是上面封装好了的 我们的配置 ["/product/**","/product2/**"]
             for (int i = 0; i < pathPatterns.size(); i++) {
                PathPattern pathPattern = pathPatterns.get(i);
                if (pathPattern.matches(path)) {
                   match = pathPattern;
                   break;
                }
             }
			//匹配成功返回True
             if (match != null) {
                traceMatch("Pattern", match.getPatternString(), path, true);
                PathMatchInfo pathMatchInfo = match.matchAndExtract(path);
                putUriTemplateVariables(exchange, pathMatchInfo.getUriVariables());
                return true;
             }
             else {
                traceMatch("Pattern", config.getPatterns(), path, false);
                return false;
             }
          }

          @Override
          public String toString() {
             return String.format("Paths: %s, match trailing slash: %b",
                   config.getPatterns(), config.isMatchOptionalTrailingSeparator());
          }
       };
    }
	//内部类 static 猜，获取配置中的数据的，配置数据的语法规范
    @Validated
    public static class Config {

       private List<String> patterns = new ArrayList<>();
		//Separator  分隔，猜  逗号分隔 , Path = /product/**,/product1/**
       private boolean matchOptionalTrailingSeparator = true;

       @Deprecated
       public String getPattern() {
          if (!CollectionUtils.isEmpty(this.patterns)) {
             return patterns.get(0);
          }
          return null;
       }

       @Deprecated
       public Config setPattern(String pattern) {
          this.patterns = new ArrayList<>();
          this.patterns.add(pattern);
          return this;
       }

       public List<String> getPatterns() {
          return patterns;
       }

       public Config setPatterns(List<String> patterns) {
          this.patterns = patterns;
          return this;
       }

       public boolean isMatchOptionalTrailingSeparator() {
          return matchOptionalTrailingSeparator;
       }

       public Config setMatchOptionalTrailingSeparator(
             boolean matchOptionalTrailingSeparator) {
          this.matchOptionalTrailingSeparator = matchOptionalTrailingSeparator;
          return this;
       }

       @Override
       public String toString() {
          return new ToStringCreator(this).append("patterns", patterns)
                .append("matchOptionalTrailingSeparator",
                      matchOptionalTrailingSeparator)
                .toString();
       }
    }
}
```

### 4. 自定义断言怎么做

#### 1. 第一步，先模仿，写出来不报错再说

```java
package com.woniuxy.config;

import org.springframework.cloud.gateway.handler.predicate.AbstractRoutePredicateFactory;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;

import java.util.function.Predicate;

/**
 * @Author: 马宇航
 * @Todo: 断言工具类，通过Product的库存，来过滤route，查询1~5000的库存的数据，访问 8091,5001~10000的库存的数据，访问8092。
 * @DateTime: 25/04/09/星期三 10:39
 * @Component: 成都蜗牛学苑
 **/
@Component //和@Bean的方式是一样的
public class ProductIdRoutePredicateFactory extends AbstractRoutePredicateFactory<ProductIdRoutePredicateFactory.Config> {
    public ProductIdRoutePredicateFactory(Class<Config> configClass) {
        super(configClass);
    }
    @Override
    public Predicate<ServerWebExchange> apply(Config config) {
        return null;
    }
    //面试题：static的问题！
    public static class Config {
    }
}
```

#### 2. 写业务，首先考虑yml怎么传参？

```yml
spring:
	cloud:
        gateway:
          routes:  # 路由配置 数组配置，使用-前缀
            - id: myh-order # 路由id 对方应用服务id
              uri: lb://myh-order # 路由地址 lb 负载均衡 冒号后面是服务id
              predicates: # 路由条件 断言
                - Path=/order/**
            - id: myh-product # 路由id
              uri: http://localhost:8091 # 路由地址
              predicates: # 路由条件
                - ProductId=1,5000
            - id: myh-product # 路由id
              uri: http://localhost:8092 # 路由地址
              predicates: # 路由条件
                - ProductId=5001,10000
            - id: myh-auth # 路由id
              uri: lb://myh-auth # 路由地址
              predicates: # 路由条件
                - Path=/auth/**
```

发现我们想要实现的配置需求，有且仅需要2个参数即可，那么就不适合Path的这个写法，推荐用BetweenRoutePredicateFactory，来模仿着写。

![image-20250409110839518](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504091108615.png)



### 3. 自定义断言代码

```java
package com.woniuxy.config;

import org.springframework.cloud.gateway.handler.predicate.AbstractRoutePredicateFactory;
import org.springframework.stereotype.Component;
import org.springframework.util.Assert;
import org.springframework.util.MultiValueMap;
import org.springframework.web.server.ServerWebExchange;

import java.util.Arrays;
import java.util.List;
import java.util.concurrent.atomic.AtomicBoolean;
import java.util.function.Predicate;

/**
 * @Author: 马宇航
 * @Todo: 断言工具类，通过Product的id，来过滤route，查询1~5000的id的数据，访问 8091,5001~10000的id的数据，访问8092。
 * @DateTime: 25/04/09/星期三 10:39
 * @Component: 成都蜗牛学苑
 **/
@Component
public class ProductIdRoutePredicateFactory extends AbstractRoutePredicateFactory<ProductIdRoutePredicateFactory.Config> {
    public static final String MINSTOCK1_KEY = "minStock";

    public static final String MINSTOCK2_KEY = "maxStock";

    public ProductIdRoutePredicateFactory() {
        super(Config.class);
    }

    @Override
    public List<String> shortcutFieldOrder() {
        return Arrays.asList(MINSTOCK1_KEY, MINSTOCK2_KEY);
    }

    @Override
    public Predicate<ServerWebExchange> apply(Config config) {
        //断言，前面的数量必须比后面的数量小
        Assert.isTrue(config.getMinStock()<config.getMaxStock(),
                config.getMinStock() + "必须小于" + config.getMaxStock());
        //由于大家不太会用Lamda表达式，所以直接用, 复制括号内容，(形参)->{}
        return (ServerWebExchange exchange)->{
            //可能搜索的时候的参数是一个范围 /product/list?stock=1000
            MultiValueMap<String, String> queryParams = exchange.getRequest().getQueryParams();
            List<String> stock = queryParams.get("stock");
            //lambda表达式 num 就是url传入的参数
            //原子开头，就是利用CAS来做的 线程安全的操作类
            AtomicBoolean flag = new AtomicBoolean(true);
            stock.forEach(num->{
                int i = Integer.parseInt(num);
                if (i>=config.getMinStock() && i<=config.getMaxStock()){
                    flag.set(true);
                }else {
                    flag.set(false);
                }
            });
            return flag.get();
        };
    }
    //面试题：static的问题！
    public static class Config {
        //这个字段肯定就是用来处理yml中的第一个最小数量参数
        private Integer minStock;
        //这个字段肯定就是用来处理yml中的第二个最大数量参数
        private Integer maxStock;

        public Integer getMinStock() {
            return minStock;
        }

        public Config setMinStock(Integer minStock) {
            this.minStock = minStock;
            return this;
        }

        public Integer getMaxStock() {
            return maxStock;
        }

        public Config setMaxStock(Integer maxStock) {
            this.maxStock = maxStock;
            return this;
        }
    }
}
```



![image-20250409115318919](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504091153049.png)



## 二、自定义过滤器

### 1. 过滤器类型

Spring Cloud Gateway 的 Filter 从作用范围可分为另外两种GatewayFilter 与 GlobalFilter。

| 类型          | 作用                                     |
| ------------- | ---------------------------------------- |
| GatewayFilter | 应用到**单个路由**或者一个分组的路由上。 |
| GlobalFilter  | 应用到所有的路由上。                     |

##### 1.1 局部过滤器

局部过滤器（GatewayFilter），是针对单个路由的过滤器。可以对访问的URL过滤，进行切面处理。在Spring Cloud Gateway中通过GatewayFilter的形式内置了很多不同类型的局部过滤器。这里简单将Spring Cloud Gateway内置的所有过滤器工厂整理成了一张表格，虽然不是很详细，但能作为速览使用。如下：

| 过滤器工厂                  | 作用                                                         | 参数                                                         |
| --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **AddRequestHeader**        | 为原始请求添加Header                                         | Header的名称及值                                             |
| **AddRequestParameter**     | 为原始请求添加请求参数                                       | 参数名称及值                                                 |
| **AddResponseHeader**       | 为原始响应添加Header                                         | Header的名称及值                                             |
| DedupeResponseHeader        | 剔除响应头中重复的值                                         | 需要去重的Header名称及去重策路                               |
| CircuitBreaker              | 为路由引入resilience4j的断路器保护，类似sentinel             | 自定义熔断器的名称                                           |
| FallbackHeaders             | 为fallbackUri的请求头中添加具体的异常信息                    | Header的名称                                                 |
| **prefixPath**              | 为原始请求路径添加前缀                                       | 前缀路径                                                     |
| PreserveHostHeader          | 为请求添加一个preserveHostHeader=true的属性，路由过滤器会检查该属性以决定是否要发送原始的Host | 无                                                           |
| RequestRateLimiter          | 用于对请求限流，限流算法为令牌桶                             | KeyResolver、rateLimiter、statusCode、denyEmptyKey、emptyKeyStatus |
| **RedirectTo**              | http状态码及重定向的URL                                      | 将原始请求重定向到指定的URL                                  |
| RemoveHopByHopHeadersFilter | 为原始请求册除IETF组织规定的一系列Header                     | 默认就会启用，可以通过配置指定仅出除比Header                 |
| RemoveRequestHeader         | 为原始请求删除某个Header                                     | Header名称                                                   |
| RemoveResponseHeader        | 为原始响应册除某个Header                                     | Header名称                                                   |
| **RewritePath**             | 重写原始的请求路径                                           | 原始路径正则表达式以及重写后路径的正则表达式                 |
| RewriteResponseHeader       | 重写原始响应中的某个Header                                   | Header名称，值的正则表达式，重写后的值                       |
| SaveSession                 | 在转发请求之前，强制执行websession : :save操作               | 无                                                           |
| secureHeaders               | 为原始响应添加一系列起安全作用的响应头                       | 无，支持修改这些安全响应头的值                               |
| SetPath                     | 修改原始的请求路径                                           | 修改后的路径                                                 |
| SetResponseHeader           | 修改原始响应中某个Header的值                                 | Header名称，修改后的值                                       |
| SetStatus                   | 修改原始响应的状态码                                         | HTTP状态码，可以是数字，也可以是字符串                       |
| StripPrefix                 | 用于截断原始请求的路径                                       | 使用数字表示要截断的路径的数量                               |
| Retry                       | 针对不同的响应进行重试                                       | retries、statuses、methods、series                           |
| RequestSize                 | 设置允许接收最大请求包的大小。如果请求包大小超过设置的值，则返回413 Payload TooLarge | 请求包大小，单位为字节，默认值为5M                           |
| ModifyRequestBody           | 在转发请求之前修改原始请求体内容                             | 修改后的请求体内容                                           |
| ModifyResponseBody          | 修改原始响应体的内容                                         | 修改后的响应体内容                                           |
| TokenRelay                  | Oauth2第三方认证使用的过滤器                                 |                                                              |

每个过滤器工厂都对应一个实现类，并且这些类的名称必须以 GatewayFilterFactory 结尾，这是Spring Cloud Gateway的一个约定，

例如 **AddRequestParameter** 对应的实现类为**AddRequestParameterGatewayFilterFactory** 。

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_parameter_route
        uri: https://example.org
        filters:
        - AddRequestParameter=red, blue  #局部过滤器
```

对应的类的代码：

```java
public class AddRequestParameterGatewayFilterFactory
		extends AbstractNameValueGatewayFilterFactory {

	@Override
	public GatewayFilter apply(NameValueConfig config) {
		return new GatewayFilter() {
            //exchange 不解释 GatewayFilterChain 过滤器链
            
			@Override
			public Mono<Void> filter(ServerWebExchange exchange,
					GatewayFilterChain chain) {
				URI uri = exchange.getRequest().getURI();
				StringBuilder query = new StringBuilder();
				String originalQuery = uri.getRawQuery();
				//http://localhost:10000/product?id=1
				if (StringUtils.hasText(originalQuery)) {
					query.append(originalQuery);
                    //?id=1&
					if (originalQuery.charAt(originalQuery.length() - 1) != '&') {
						query.append('&');
					}
				}

				String value = ServerWebExchangeUtils.expand(exchange, config.getValue());
				// TODO urlencode?
                //?id=1&name=value
				query.append(config.getName());
				query.append('=');
				query.append(value);

				try {
					URI newUri = UriComponentsBuilder.fromUri(uri)
							.replaceQuery(query.toString()).build(true).toUri();
					//建造者模式， 可以一直 xx.x().xxx().xxx().build()
					ServerHttpRequest request = exchange.getRequest().mutate().uri(newUri)
							.build();
					//chain.filter 链路放行，继续往下一个过滤器去执行。
					return chain.filter(exchange.mutate().request(request).build());
				}
				catch (RuntimeException ex) {
					throw new IllegalStateException(
							"Invalid URI query: \"" + query.toString() + "\"");
				}
			}

			@Override
			public String toString() {
				return filterToStringCreator(AddRequestParameterGatewayFilterFactory.this)
						.append(config.getName(), config.getValue()).toString();
			}
		};
	}

}

```

> 不写局部过滤器，以后参考自定义断言的约定大于配置的写法，模仿着官方提供的局部过滤器，自己开发自己的自定义局部过滤器。
>
> 因为是局部过滤器，所有不需要order，因为不是全局过滤器一样有很长的链路。



和自定义断言类似！

判断请求头里面呢，有没有Authorization：token，如果有，则登录放行，允许反问其他服务，如果没有，则跳转Auth服务的登录业务。

#### 1.2 全局过滤器

[全局过滤器：Spring Cloud Gateway global-filter](https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/#global-filters)



全局过滤器（GlobalFilter）作用于所有路由，Spring Cloud Gateway 定义了Global Filter接口，用户可以自定义实现自己的Global Filter。通过全局过滤器可以**实现对权限的统一校验**，**安全性验证**等功能，并且全局过滤器也是程序员使用比较多的过滤器。Spring Cloud Gateway内部也是通过一系列的内置全局过滤器对整个路由转发进行处理如下：

![image-20210718163636616](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/xiangwei/20210718163636.png)

`自定义全局过滤器：实现GlobalFilter和Ordered接口`



当请求与路由匹配时，筛选web处理程序会添加`GlobalFilter`和所有路由特定的实例`GatewayFilter`过滤链。这个组合的筛选器链按`org.springframework.core.Ordered`接口，可以通过实现`getOrder()`方法。



由于Spring Cloud Gateway区分过滤器逻辑执行的“pre”和“post”阶段(参见[它是如何工作的](https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/#gateway-how-it-works))，具有最高优先级的滤波器在“前”阶段是第一个，在“后”阶段是最后一个。

除了上面的代码，剩下一个过滤器链路的顺序，Ordered接口重写getOrder()来配置的。数字越小越靠前。



下面的自定义过滤器的写法(官方demo)：

```java
@Component
public class CustomGlobalFilter implements GlobalFilter, Ordered {

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("custom global filter");
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        return -1;
    }
}
```



## 三、Auth认证服务自己开发

> 全局过滤器，在业务逻辑下写。
>
> 认证模块，需要登录认证，redis，jwt。
>
> 补充内容：单独演示，自己重新建一个maven项目
>
> ```xml
> <dependency>
>   <groupId>io.jsonwebtoken</groupId>
>   <artifactId>jjwt</artifactId>
>   <version>0.9.1</version>
> </dependency>
> ```
>
> 生产jwt的代码：
>
> ```java
> package com.woniuxy;
> 
> import io.jsonwebtoken.JwtBuilder;
> import io.jsonwebtoken.Jwts;
> import io.jsonwebtoken.SignatureAlgorithm;
> 
> import java.util.Date;
> import java.util.HashMap;
> import java.util.Map;
> 
> /**
>  * @Author: 马宇航
>  * @Todo: jwt生产
>  * @DateTime: 25/04/09/星期三 14:27
>  * @Component: 成都蜗牛学苑
>  **/
> public class JWTGenerator {
>     public static void main(String[] args) {
>         Map map = new HashMap<String, String>();
>         map.put("name","马宇航");
>         map.put("age","18");
>         map.put("sex","男");
>         map.put("password","不要放");
>         String token = Jwts.builder().setId("ids")
>                 .setSubject("主体信息，了解")
>                 .setIssuedAt(new Date())
>                 .setClaims(map) //相当于明文信息
>                 .signWith(SignatureAlgorithm.HS256, "woniuxy")
>                 .setExpiration(new Date(System.currentTimeMillis() + 1000 * 1000))
>                 .compact();
>         System.out.println(token);
>     }
> }
> ```
>
> 解析代码：
>
> ```java
> package com.woniuxy;
> 
> import io.jsonwebtoken.Jwt;
> import io.jsonwebtoken.JwtParser;
> import io.jsonwebtoken.Jwts;
> 
> /**
>  * @Author: 马宇航
>  * @Todo: 解析JWT
>  * @DateTime: 25/04/09/星期三 14:31
>  * @Component: 成都蜗牛学苑
>  **/
> public class JWTParase {
>     public static void main(String[] args) {
>         String token = "eyJhbGciOiJIUzI1NiJ9.eyJwYXNzd29yZCI6IuS4jeimgeaUviIsInNleCI6IueUtyIsIm5hbWUiOiLpqazlrofoiKoiLCJleHAiOjE3NDQxODE0OTMsImFnZSI6IjE4In0.M8mh22CPe_8vCvIsFjbi1Lg7HaYFlcYIVcHsfM_AK48";
>         JwtParser jwtParser = Jwts
>                 .parser()
>                 .setSigningKey("woniuxy");
>         Jwt jwt = jwtParser.parse(token);
>         System.out.println(jwt.getBody().toString());
>     }
> }
> ```
>
> {password=不要放, sex=男, name=马宇航, exp=1744181493, age=18}
>
> ![image-20250409144151473](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504091441669.png)
>
> 核心点setClaims 这个里面，**相当于明文，所以大家一定不要把敏感信息放里面。**
>
> ```cmd
> eyJhbGciOiJIUzI1NiJ9   #header： 加密方式 的Bash64加密  HS256的算法加密
> .eyJwYXNzd29yZCI6IuS4jeimgeaUviIsInNleCI6IueUtyIsIm5hbWUiOiLpqazlrofoiKoiLCJleHAiOjE3NDQxODE0OTMsImFnZSI6IjE4In0 #body: 自定义的claim里面的内容，就会存这里，claim内容越多，这个body就越长 Bash64(body)
> .M8mh22CPe_8vCvIsFjbi1Lg7HaYFlcYIVcHsfM_AK48 #签名：Bash64(Bash64(header)+Bash64(body))
> ```

### 1. commons里面开发工具类

> JWT生成工具类，因为Auth模块还有Gateway都要用JWT工具类。



引入依赖：

```xml
<!-- 提供 Auth和GateWay的全局过滤器使用-->
        <dependency>
            <groupId>com.auth0</groupId>
            <artifactId>java-jwt</artifactId>
            <version>3.18.2</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
```

#### 1.开发JWT工具类

```java
package com.woniuxy.utils;

import com.auth0.jwt.JWT;
import com.auth0.jwt.JWTVerifier;
import com.auth0.jwt.algorithms.Algorithm;
import com.auth0.jwt.exceptions.JWTDecodeException;
import com.auth0.jwt.exceptions.JWTVerificationException;
import com.auth0.jwt.exceptions.TokenExpiredException;
import com.auth0.jwt.interfaces.DecodedJWT;

import java.util.Date;

/**
 * @Author: 马宇航
 * @Todo: JWT工具类，生成token，验证token，获得token中的信息无需secret解密也能获得，AI生成工具，百度都有源码
 * @DateTime: 25/04/09/星期三 15:12
 * @Component: 成都蜗牛学苑
 **/
public class JWTUtils {
    //内部定义枚举类
    public enum TokenStatusEnum{
        TOKEN_EXPIRE,
        TOKEN_ERROR,//被人篡改过token
        TOKEN_SUCCESS;
    }
    //统一秘钥定义
    private static final String SECURY_KEY="wonixuy123123";
    //过期时间 正常的jwt的过期时间
    private static final Long TOKEN_EXPIRE_TIME= 10 * 60 * 1000L; //10MIN -->企业中是30min
    //双token的，刷新token 时间会比较长，面试容易被问到的点
    public static final Long REFRESH_TOKEN_EXPIRE_TIME=30 * 60 * 1000L; //30MIN -->企业中 60min
    //加密算法固定
    private static Algorithm algorithm = Algorithm.HMAC256(SECURY_KEY);
    /**
     * 生成token
     * ChangeLog : 1. 创建 (25/01/08/0008 09:51 [马宇航]);
     * @param username  登录成功的用户名
     * @return java.lang.String
     */
    public static String createToken(String username){
        Date now = new Date();
        return JWT.create()
                .withIssuer("mayuhang") //签发人
                .withIssuedAt(now) //签发时间
                .withClaim("username",username) //payload 自定义的用户数据 明文
                .withExpiresAt(new Date(now.getTime() + TOKEN_EXPIRE_TIME)) //过期时间
                .sign(algorithm);
    }
    /**
     * token验证
     * ChangeLog : 1. 创建 (25/01/08/0008 09:57 [马宇航]);
     * @param token
     * @return com.woniuxy.utils.JWTUtils.TokenStatusEnum
     */
    public static TokenStatusEnum verify(String token){
        try {
            JWTVerifier verifier = JWT.require(algorithm)
                    .withIssuer("mayuhang")
                    .build();
            verifier.verify(token);
            return TokenStatusEnum.TOKEN_SUCCESS;
        }catch (TokenExpiredException e){
            //过期异常会在之前先进行校验
            return TokenStatusEnum.TOKEN_EXPIRE;
        }catch (JWTVerificationException e){
            //这个异常 就直接是 token 验证不通过的异常
            return TokenStatusEnum.TOKEN_ERROR;
        }
    }
    /**
     * Description : 获得token中的信息无需secret解密也能获得  <br/>
     * ChangeLog : 1. 创建 (2021/5/3 22:01 [mayuhang]);
     * @param token
     * @return token中包含的用户名
     **/
    public static String getUserName(String token){
        try {
            DecodedJWT jwt = JWT.decode(token);
            return jwt.getClaim("username").asString();
        }catch (JWTDecodeException e){
            return TokenStatusEnum.TOKEN_ERROR.name();
        }
    }
}
```

#### 2.开发redis工具类

```java
package com.woniuxy.utils;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;

import java.util.Map;
import java.util.concurrent.TimeUnit;

/**
 * @Author: 马宇航
 * @Todo: redis工具类
 * @DateTime: 25/04/09/星期三 15:35
 * @Component: 成都蜗牛学苑
 **/
@Component
public class RedisUtils {
    @Autowired
    StringRedisTemplate stringRedisTemplate;
    /**
     * 设置键值对
     * @param key 键
     * @param value 值
     */
    public void set(String key, String value) {
        stringRedisTemplate.opsForValue().set(key, value);
    }

    /**
     * 设置带过期时间的键值对
     * @param key 键
     * @param value 值
     * @param timeout 过期时间
     * @param unit 时间单位
     */
    public void set(String key, String value, long timeout, TimeUnit unit) {
        stringRedisTemplate.opsForValue().set(key, value, timeout, unit);
    }

    /**
     * 根据键获取值
     * @param key 键
     * @return 值
     */
    public String get(String key) {
        return stringRedisTemplate.opsForValue().get(key);
    }

    /**
     * 删除键
     * @param key 键
     * @return 是否删除成功
     */
    public boolean delete(String key) {
        return stringRedisTemplate.delete(key);
    }

    /**
     * 判断键是否存在
     * @param key 键
     * @return 是否存在
     */
    public boolean hasKey(String key) {
        return stringRedisTemplate.hasKey(key);
    }

    /**
     * 为键设置过期时间
     * @param key 键
     * @param timeout 过期时间
     * @param unit 时间单位
     * @return 是否设置成功
     */
    public boolean expire(String key, long timeout, TimeUnit unit) {
        return stringRedisTemplate.expire(key, timeout, unit);
    }

    /**
     * 获取键的剩余过期时间
     * @param key 键
     * @param unit 时间单位
     * @return 剩余过期时间
     */
    public Long getExpire(String key, TimeUnit unit) {
        return stringRedisTemplate.getExpire(key, unit);
    }

    /**
     * 设置 Hash 类型的字段值
     * @param key 键
     * @param hashKey Hash 字段
     * @param value 值
     */
    public void hset(String key, String hashKey, String value) {
        stringRedisTemplate.opsForHash().put(key, hashKey, value);
    }

    /**
     * 批量设置 Hash 类型的字段值
     * @param key 键
     * @param map 包含多个 Hash 字段和值的 Map
     */
    public void hsetAll(String key, Map<String, Object> map) {
        stringRedisTemplate.opsForHash().putAll(key, map);
    }

    /**
     * 获取 Hash 类型的字段值
     * @param key 键
     * @param hashKey Hash 字段
     * @return 值
     */
    public String hget(String key, String hashKey) {
        return (String) stringRedisTemplate.opsForHash().get(key, hashKey);
    }

    /**
     * 获取整个 Hash 类型的数据
     * @param key 键
     * @return 包含所有 Hash 字段和值的 Map
     */
    public Map<Object, Object> hgetAll(String key) {
        return stringRedisTemplate.opsForHash().entries(key);
    }

    /**
     * 删除 Hash 类型的字段
     * @param key 键
     * @param hashKeys 要删除的 Hash 字段数组
     * @return 删除的字段数量
     */
    public Long hdel(String key, Object... hashKeys) {
        return stringRedisTemplate.opsForHash().delete(key, hashKeys);
    }

    /**
     * 判断 Hash 类型中是否存在某个字段
     * @param key 键
     * @param hashKey Hash 字段
     * @return 是否存在
     */
    public boolean hHasKey(String key, String hashKey) {
        return stringRedisTemplate.opsForHash().hasKey(key, hashKey);
    }
    //单独给某个键设置过期时间
    public void setExpire(String key, long timeout, TimeUnit unit) {
        stringRedisTemplate.expire(key, timeout, unit);
    }
}
```

### 2. auth模块里面开发登录业务

service和dao自己写，我下面的 代码，不符合逻辑，不要再controller层写业务。

```java
package com.woniuxy.controller;

import com.woniuxy.entity.User;
import com.woniuxy.entity.utils.ResponseMyEntity;
import com.woniuxy.service.AuthService;
import com.woniuxy.utils.JWTUtils;
import com.woniuxy.utils.RedisUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
import java.util.concurrent.TimeUnit;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/09/星期三 15:21
 * @Component: 成都蜗牛学苑
 **/
@RestController
@RequestMapping("/auth")
public class AuthController {
    @Autowired
    AuthService authService;
    @Autowired
    RedisUtils redisUtils;
    @PostMapping("/login")
    public ResponseMyEntity login(@RequestBody User user) {
        //不要直接传账号密码去匹配，通常只传账号
        User account = authService.login(user.getUsername());
        if (account == null) {
            return new ResponseMyEntity(500, "账号或密码错误");
        }else if (account.getPassword().equals(user.getPassword())){
            //token本身的过期时间 30min
            String token = JWTUtils.createToken(account.getUsername());
            //token使用双token，不要把长token返回给前端，因为token并非安全
            //refreshToken自身没有过期效果 所以只能通过redis来控制过期时间 60min
            String refreshToken = UUID.randomUUID().toString();
            /*
                1.用户长时间不操作，30min到60min之间，token过期，但是refreshToken还在，
                2.所以，重新自动生产新的token，而不要用户重新登录
                3.但是用户如果超过了60min，refreshToken过期，用户需要重新登录
                4.所以，refreshToken的过期时间要比token的过期时间长
                5.所以，refreshToken是一个key，token是一个value的一部分

                思考：为什么token的过期时间要设置的很短？
                核心还是安全问题，token不安全。不能让一个token一直存在
             */
            //把token和用户信息，都存起来，key是refreshToken ，用什么类型？
            Map<String, Object> value= new HashMap<>();
            redisUtils.hsetAll(refreshToken, value);
            //给某个key设置过期时间
            redisUtils.expire(refreshToken, 60, TimeUnit.MINUTES);
            return new ResponseMyEntity(refreshToken);
        }else
            return new ResponseMyEntity(500, "账号或密码错误");
    }
}
```

## 四、GateWay中的配置

### 1. yml引入route-->Auth

```yml
- id: myh-auth # 路由id
  uri: lb://myh-auth # 路由地址
  predicates: # 路由条件
    - Path=/auth/**
```

### 2. 开发全局过滤器

所有登录请求，都需要在请求头里面，找Authorization！

```java
package com.woniuxy.config;

import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.Ordered;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

/**
 * @Author: 马宇航
 * @Todo: 全局过滤器
 * @DateTime: 25/04/09/星期三 16:43
 * @Component: 成都蜗牛学苑
 **/
@Component
public class AuthGlobalFilter implements GlobalFilter,Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        System.out.println("我们的全局过滤器生效！"+exchange.getRequest().getURI());
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        return Ordered.HIGHEST_PRECEDENCE;
    }
}
```

以上代码，已经可以生效了。

> 业务需求，校验token是否存在，以及是否过期。



完整代码如下：

```java
package com.woniuxy.config;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.woniuxy.entity.utils.ResponseMyEntity;
import com.woniuxy.utils.JWTUtils;
import com.woniuxy.utils.RedisUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.Ordered;
import org.springframework.core.io.buffer.DataBuffer;
import org.springframework.http.HttpStatus;
import org.springframework.http.server.reactive.ServerHttpResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

import java.util.Map;
import java.util.concurrent.TimeUnit;

/**
 * @Author: 马宇航
 * @Todo: 全局过滤器
 * @DateTime: 25/04/09/星期三 16:43
 * @Component: 成都蜗牛学苑
 **/
@Component
public class AuthGlobalFilter implements GlobalFilter,Ordered {
    @Autowired
    RedisUtils redisUtils;
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        System.out.println("我们的全局过滤器生效！"+exchange.getRequest().getURI());
        //判断是否是登录请求，如果是登录请求，直接放行
        if (exchange.getRequest().getURI().getPath().contains("/auth/login")){
            return chain.filter(exchange);
        }
        //判断是否是注册请求，如果是注册请求，直接放行
        if (exchange.getRequest().getURI().getPath().contains("/auth/register")){
            return chain.filter(exchange);
        }
        //不是上诉请求，就需要校验token
        String refreshToken = exchange.getRequest().getHeaders().getFirst("Authorization");
        //判断是否有refreshToken
        if (redisUtils.hasKey(refreshToken)) {
            //取出里面的内容
            Map<String, Object> content = redisUtils.hgetAll(refreshToken);
            String token = (String) content.get("token");
            //判断token是否过期
            JWTUtils.TokenStatusEnum verify = JWTUtils.verify(token);
            //只有token过期在30min~60min之间，才需要重新生成token
            if (verify == JWTUtils.TokenStatusEnum.TOKEN_EXPIRE) {
                //token过期，但是refreshToken还在，重新生成token
                String newToken = JWTUtils.createToken((String) content.get("account"));
                //更新redis中的token
                content.put("token", newToken);
                //把新的token重新更新进去
                redisUtils.hsetAll(refreshToken, content);
                //给某个key设置过期时间
                redisUtils.expire(refreshToken, 60, TimeUnit.MINUTES);
                //重新更新了 新token后，要放行
                return chain.filter(exchange);
            }
            else if (verify == JWTUtils.TokenStatusEnum.TOKEN_ERROR) {
                //定义一个自定义Flux流式响应的方法
               return returnUnauthorized(exchange);
            } else if (verify == JWTUtils.TokenStatusEnum.TOKEN_SUCCESS) {
                //校验通过业务
                return chain.filter(exchange);
            }else{
                //token 完全不存在
                return returnUnauthorized(exchange);
            }
        }else {
            return returnUnauthorized(exchange);
        }
    }

    private Mono<Void> returnUnauthorized(ServerWebExchange exchange) {
        //获取响应
        ServerHttpResponse response = exchange.getResponse();
        ResponseMyEntity responseMyEntity = new ResponseMyEntity(HttpStatus.UNAUTHORIZED.value(), "token过期");
        //想要在WebFlux返回，不能直接用，用我们的字节流方案，自己处理自定义响应类型
        try {
            //转换为字节流
            byte[] bytes = new ObjectMapper().writeValueAsBytes(responseMyEntity);
            DataBuffer wrap = response.bufferFactory().wrap(bytes);
            //响应Mono方案
            return response.writeWith(Mono.just(wrap));
        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }

    }

    @Override
    public int getOrder() {
        return Ordered.HIGHEST_PRECEDENCE;
    }
}
```

## 五、commons中开发权限注解

> 因为所有模块的controller都要用到权限注解。操作权限。Perm   role  user 三个表 RBAC概念。
>
> 通常redis里面存储的数据，是会包含用户的角色权限信息。
>
> 开发一个自定义注解，



```java
package com.woniuxy.anno;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * @Author: 马宇航
 * @Description: 自定义权限注解
 * @DateTime: 25/04/09/星期三 17:33
 * @Component: 成都蜗牛学苑
 **/
//表示，该注解，可以加到方法上和类上
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME) //运行时生效
public @interface PermAccess {
    //权限的标记字段 order::add
    String value() default "";
}
```

上诉代码要生效，需要用到一个拦截器。

```java
package com.woniuxy.interceptor;

import com.woniuxy.anno.PermAccess;
import org.hibernate.bytecode.enhance.spi.interceptor.AbstractInterceptor;
import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.lang.reflect.Method;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/09/星期三 17:36
 * @Component: 成都蜗牛学苑
 **/
public class PermInterceptor extends HandlerInterceptorAdapter {
    /**
     * 进入controller之前有效果
     * ChangeLog : 1. 创建 (25/04/09/星期三 17:37 [马宇航]);
     * @param request
     * @param response
     * @param handler controller对象，目标进入的controller对象，
     * @return boolean true就会放行
    */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println(handler);
        //直接获取方法上的注解
        if (handler instanceof HandlerMethod) {
            Method method = ((HandlerMethod) handler).getMethod();
            //判断方法上是否有PermAccess注解
            if (method.isAnnotationPresent(PermAccess.class)) {
                //如果存在，则进行值的获取，来校验，用户权限和注解配置的权限是否匹配
                PermAccess permAccess = method.getAnnotation(PermAccess.class);
                String value = permAccess.value();
                //"order::add" 是写死的，但是实际上，是根据refreshToken去redis中获取 用户的信息，拿到用户的权限列表
                if(value.equals("order::add")){
                    System.out.println("权限校验通过");
                    return true;
                }
            }
        }
        return false;
    }
    /**
     * controller执行之后有效果
     * ChangeLog : 1. 创建 (25/04/09/星期三 17:37 [马宇航]);
     * @param request
     * @param response
     * @param handler
     * @param modelAndView
     * @return void
    */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        super.postHandle(request, response, handler, modelAndView);
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        super.afterCompletion(request, response, handler, ex);
    }
}
```

配置类：

```java
package com.woniuxy.interceptor;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

/**
 * @Author: 马宇航
 * @Todo: 拦截器配置类
 * @DateTime: 25/04/09/星期三 17:40
 * @Component: 成都蜗牛学苑
 **/
@Configuration
public class SpringMVCInterceptorConfiguration implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new PermInterceptor())
                .addPathPatterns("/**")
                .excludePathPatterns("/login")
                .order(1); //拦截器的执行顺序
    }
}
```

上述代码，都是在commons中开发的，就会导致GateWay因为这个拦截器配置，无法启动，大家解决下这个问题。


AOP的切面，切入点表达式，做一个根据表达式去扫描我们的注解。

```java
//排除拦截器的配置类，因为我们的拦截器是在springmvc中配置的，而不是在springboot中配置的 没生效啊
//所以我们要排除拦截器的配置类，让springboot自己去配置拦截
@SpringBootApplication
@ComponentScan(excludeFilters = @ComponentScan.Filter(
        type = FilterType.ASPECTJ,
        pattern = "com.woniuxy.interceptor.*" //不要扫描这个包路径下的类
))
public class GatewayAPP {
    public static void main(String[] args) {
        SpringApplication.run(GatewayAPP.class, args); // 移除重复调用
    }
}
```