# day07 ruoyi框架讲解

## 一、前端框架必备的知识点

### 1. 路由守卫（导航守卫）

ruoyi ：permission.js文件，配置了路由守卫。

```js
import router from './router'
import store from './store'
import { Message } from 'element-ui'
import NProgress from 'nprogress'
import 'nprogress/nprogress.css'
import { getToken } from '@/utils/auth'
import { isPathMatch } from '@/utils/validate'
import { isRelogin } from '@/utils/request'

NProgress.configure({ showSpinner: false })
//白名单列表
const whiteList = ['/login', '/register']
//白名单方法
const isWhiteList = (path) => {
  return whiteList.some(pattern => isPathMatch(pattern, path))
}
//路由 拦截器，路由跳转之前会进这个方法。
router.beforeEach((to, from, next) => {
  NProgress.start()
  if (getToken()) {
    to.meta.title && store.dispatch('settings/setTitle', to.meta.title)
    /* has token*/
    if (to.path === '/login') {
        //跳转到 哪里
      next({ path: '/' })
      NProgress.done()
    } else if (isWhiteList(to.path)) {
        //直接放行
      next()
    } else {
        //store  vuex里面的东西，登录的信息在store里面
      if (store.getters.roles.length === 0) {
        isRelogin.show = true
        // 判断当前用户是否已拉取完user_info信息
        store.dispatch('GetInfo').then(() => {
          isRelogin.show = false
          store.dispatch('GenerateRoutes').then(accessRoutes => {
            // 根据roles权限生成可访问的路由表
            router.addRoutes(accessRoutes) // 动态添加可访问路由表
            next({ ...to, replace: true }) // hack方法 确保addRoutes已完成
          })
        }).catch(err => {
            store.dispatch('LogOut').then(() => {
              Message.error(err)
              next({ path: '/' })
            })
          })
      } else {
        next()
      }
    }
  } else {
    // 没有token
    if (isWhiteList(to.path)) {
      // 在免登录白名单，直接进入
      next()
    } else {
      next(`/login?redirect=${encodeURIComponent(to.fullPath)}`) // 否则全部重定向到登录页
      NProgress.done()
    }
  }
})
//路由后的拦截器，可以页面跳转后 来一点业务。
router.afterEach(() => {
  NProgress.done()
})

```

### 2. axios的拦截器

以后企业的前端代码，只要是vue的，都会把所有请求封装成 api/xxx/xx.js

本质都是用的axios，只不过很多企业会自己封装这个axios，-->src\utils\request.js

```js
import axios from 'axios'
import store from '@/store'
import { getToken } from '@/utils/auth'

//之前在main.js配置过 统一的ip，前后端分离请求头都是 json格式发送 {"key":"value"}
axios.defaults.headers['Content-Type'] = 'application/json;charset=utf-8'
// 创建axios实例
const service = axios.create({
  // axios中请求配置有baseURL选项，表示请求URL公共部分
  baseURL: process.env.VUE_APP_BASE_API,
  // 超时
  timeout: 10000
})

// request拦截器  请求拦截器：所有请求发出去之前，都会进入这里
service.interceptors.request.use(config => {
  // 是否需要设置 token  isToken 需不需要带token，
  const isToken = (config.headers || {}).isToken === false
  // 是否需要防止数据重复提交  手抖 新增2条数据
  const isRepeatSubmit = (config.headers || {}).repeatSubmit === false
  //需要token的话
  if (getToken() && !isToken) {
      //在所有的请求发出去之前，请求头里面 添加一个key ：Authorization:Bearer xxxx.xxxx.xxxsdf13toekn
    config.headers['Authorization'] = 'Bearer ' + getToken() // 让每个请求携带自定义token 请根据实际情况自行修改
  }
  // get请求映射params参数 {"username":"admin","password":"123456"}
  if (config.method === 'get' && config.params) {
    let url = config.url + '?' + tansParams(config.params);
    url = url.slice(0, -1);
    config.params = {};
    config.url = url;
  }
    //处理post和put请求，前端的json对象的序列化
  if (!isRepeatSubmit && (config.method === 'post' || config.method === 'put')) {
    const requestObj = {
      url: config.url,
        //JSON.stringify(config.data) 就是把json对象转成json字符串（序列化后的内容）
      data: typeof config.data === 'object' ? JSON.stringify(config.data) : config.data,
      time: new Date().getTime()
    }
    const requestSize = Object.keys(JSON.stringify(requestObj)).length; // 请求数据大小
    const limitSize = 5 * 1024 * 1024; // 限制存放数据5M
    if (requestSize >= limitSize) {
      console.warn(`[${config.url}]: ` + '请求数据大小超出允许的5M限制，无法进行防重复提交验证。')
      return config;
    }
    const sessionObj = cache.session.getJSON('sessionObj')
    if (sessionObj === undefined || sessionObj === null || sessionObj === '') {
      cache.session.setJSON('sessionObj', requestObj)
    } else {
        //前端 重复提交的处理代码
      const s_url = sessionObj.url;                  // 请求地址
      const s_data = sessionObj.data;                // 请求数据
      const s_time = sessionObj.time;                // 请求时间
      const interval = 1000;                         // 间隔时间(ms)，小于此时间视为重复提交
      if (s_data === requestObj.data && requestObj.time - s_time < interval && s_url === requestObj.url) {
        const message = '数据正在处理，请勿重复提交';
        console.warn(`[${s_url}]: ` + message)
        return Promise.reject(new Error(message))
      } else {
        cache.session.setJSON('sessionObj', requestObj)
      }
    }
  }
  return config
}, error => {
    console.log(error)
    //Promise, 异步阻塞
    Promise.reject(error)
})

// 响应拦截器： 核心的，this.$axios.post().then(正常响应结果).catch(错误响应结果);
service.interceptors.response.use(res => {
    // 未设置状态码则默认成功状态
    const code = res.data.code || 200;
    // 获取错误信息
    const msg = errorCode[code] || res.data.msg || errorCode['default']
    // 二进制数据则直接返回
    if (res.request.responseType ===  'blob' || res.request.responseType ===  'arraybuffer') {
        //我们页面的方法的this.$axios.get().then(res=>{})  
        //上面的res == 下面的 res.data, 后面使用的时候注意，log打印出来看
      return res.data
    }
    // res.data.code == 200  code是自己封装的响应码
    if (code === 401) { //401 登录过期 token过期
      if (!isRelogin.show) {
        isRelogin.show = true;
        MessageBox.confirm('登录状态已过期，您可以继续留在该页面，或者重新登录', '系统提示', { confirmButtonText: '重新登录', cancelButtonText: '取消', type: 'warning' }).then(() => {
          isRelogin.show = false;
            //vuex里面的登录状态 调用退出方法
          store.dispatch('LogOut').then(() => {
              //前端的 重定向
            location.href = '/index';
          })
      }).catch(() => { //消息盒子的catch
        isRelogin.show = false;
      });
    }
      return Promise.reject('无效的会话，或者会话已过期，请重新登录。')
    } else if (code === 500) { //500就是后端报错
        //消息盒子
      Message({ message: msg, type: 'error' })
      return Promise.reject(new Error(msg))
    } else if (code === 601) {
        //
      Message({ message: msg, type: 'warning' })
      return Promise.reject('error')
    } else if (code !== 200) {
        //非正常请求处理方案
      Notification.error({ title: msg })
      return Promise.reject('error')
    } else {
      return res.data
    }
  },
  //系统 请求响应码，不是我们自己控制的 this.$axios.post().then(正常响应结果).catch(错误响应结果);               //error就是处理的catch(错误响应结果); 里面的内容
  error => {
    console.log('err' + error)
    let { message } = error;
    if (message == "Network Error") {
      message = "后端接口连接异常";
    } else if (message.includes("timeout")) {
      message = "系统接口请求超时";
    } else if (message.includes("Request failed with status code")) {
      message = "系统接口" + message.substr(message.length - 3) + "异常";
    }
    Message({ message: message, type: 'error', duration: 5 * 1000 })
    return Promise.reject(error)
  }
)

export default service
```

## 二、企业中拿到一个项目我们要做什么事情

我们这里就用若依的项目，来作为示范。

### 1. 项目架构了解

是前后端分离的，还是不分离的？

分离  --->    前端什么框架：vue，react，uniapp

​	   	后端是什么框架：springboot + maven|grade + git|svn + 数据库是什么+ 数据库连接工具（jpa+mytabis, mybatis plus，公司自己基于mybatis封装的框架）

不分离 --->  后端是ssm？还是springboot？



jdk版本，也要确认好。

权限框架还是要了解下，是 shiro 还是 SpringSecurity，还是sa-token。

### 2. 先跑起来看

先跑起来再说，就算里面什么都不认识。



### 3. 看系统的pom，依赖了那些jar包

![image-20250307105828105](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071058261.png)

### 4. 项目模块文件夹 连猜带蒙

就根据模块名字，猜里面可能是什么功能和业务。

![image-20250307110401410](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071104520.png)

如果某个模块，你猜不出里面的内容，就打开分析。

### 5. 看看前端页面

![image-20250307111509974](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071115077.png)

第二个 vue.config.js

![image-20250307111832176](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071118308.png)

第三个：package.json

![image-20250307112044840](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071120946.png)

第四个：env.dev..

![image-20250307112210973](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071122076.png)

## 三、若依后台自己的模块创建

### 1. 第一个创建子模块

> 项目的根目录下，创建子模块。

![image-20250307114018106](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071140196.png)

maven子模块，不要选择springboot模块，微服务四阶段再选springboot。

![image-20250307114205679](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071142772.png)

检查，父pom里面有没有下面的内容：

![image-20250307114328578](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071143681.png)

### 2. 自己的模块pom引入对应的依赖

```xml
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.ruoyi</groupId>
            <artifactId>ruoyi-common</artifactId>
        </dependency>
    </dependencies>
```

### 3. 把我们的模块pom引入到ruoyi-admin的pom里面

![image-20250307120526085](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071205207.png)

### 4. 创建一个ruoyi的目录下的配置类

![image-20250307120413397](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071204531.png)

> 我们的controller其实是没有权限直接访问的：
>
> 如果我们的模块是允许匿名访问，比如小程序中的很多api接口都是可以匿名访问的：
>
> 去修改下面的路径的配置：
>
> src/main/java/com/ruoyi/framework/config/SecurityConfig.java
>
> ![image-20250307120814188](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071208335.png)



### 5. 拦截器和过滤器知道不？常见面试题

拦截器：Spring特有的，Intercepter，springmvc框架特有的东西，脱离spring就没法用了。

过滤器：filter，所有的web项目都可以用的东西。



访问顺序--->过滤器链 --->拦截器-->后端接口--->数据库 ---回-> 接口--->拦截器--->过滤器。

SpringSecurte框架底层 就是一堆 过滤器链。

登录需要认证，前端需要携带 请求头 Authorization: xxx jwt，后端靠过滤器来拦截。





> 讲一个登录的示例：
>
> ![image-20250307121525572](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071215710.png)

## 四、ruoyi的登录流程

### 1. 前端代码的页面跳转分析

> 1. 通过登录 url地址，找到 route.js，找到对应的页面代码 login.vue
>
> 2. 登录按钮，找到的对应方法
>
>    ![image-20250307141058597](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071410738.png)
>
> 3. store/index.js 
>
>    ![image-20250307141239106](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071412237.png)
>
> 4. 进入到 user这模块中
>
>    ![image-20250307141335380](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071413516.png)
>
> 5. Login方法，就是登录的前端接口位子，@ src/api/login.js
>
>    发起的请求 /login

### 2. 通过前端的path去找对应的后端代码

> 这个方法用的非常多，不然找不到前端接口对应的后端代码。

双击shift或者直接点击下图的放大镜：

![image-20250307141710240](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071417341.png)

搜索/login

![image-20250307141817439](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071418538.png)

对应后端的controller接口：

![image-20250307142046867](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071420990.png)



接下来就是令牌的生成：loginService.login()

```java
public String login(String username, String password, String code, String uuid)
    {
        // 验证码校验
        validateCaptcha(username, code, uuid);
        // 登录前置校验：类似布隆过滤器，减少数据库无效访问。 ip黑名单限制（网络安全防御的）。
        loginPreCheck(username, password);
        // 用户验证
        Authentication authentication = null;
        try
        {
            //UsernamePasswordAuthenticationToken类就是SpringSecurte框架自带的登录验证的类
            //sa-token 帮我们封装好了。
            UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(username, password);
            //上面的UsernamePasswordAuthenticationToken是SS的内置实体类，存放账号密码的
            //Holder 可以理解为 容器，就是用来存放东西的。
            AuthenticationContextHolder.setContext(authenticationToken);
            // 该方法会去调用UserDetailsServiceImpl.loadUserByUsername
            authentication = authenticationManager.authenticate(authenticationToken);
        }
        finally
        {
            AuthenticationContextHolder.clearContext();
        }
        AsyncManager.me().execute(AsyncFactory.recordLogininfor(username, Constants.LOGIN_SUCCESS, MessageUtils.message("user.login.success")));
        LoginUser loginUser = (LoginUser) authentication.getPrincipal();
        recordLoginInfo(loginUser.getUserId());
        // 生成token
        return tokenService.createToken(loginUser);
    }
```

上图最后会调用这个，UserDetailsServiceImpl.loadUserByUsername

![image-20250307143246505](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071432643.png)

校验密码：这个里面做了错误次数统计，并使用了redis的ttl特性，来记录和删除。

![image-20250307143602158](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071436336.png)

最后一步 ，createLoginUser方法，封装部门id，菜单权限，到一个LoginUser实体类中。

![image-20250307143903993](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071439143.png)



最后方法直接完成，回到最初

![image-20250307144339361](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071443522.png)

最后，记录登录日志，同时生成token。

```java
public String createToken(LoginUser loginUser)
    {
     //生成一个uuid：短token 不是jwt
        String token = IdUtils.fastUUID();
        loginUser.setToken(token);
    	//记录登录的 ip，os，浏览器信息
        setUserAgent(loginUser);
       //生成刷新token：默认用的双token认证机制
        refreshToken(loginUser);--->{
            //登录时间
            loginUser.setLoginTime(System.currentTimeMillis());
            //登录后 默认失效时间
            loginUser.setExpireTime(loginUser.getLoginTime() + expireTime * MILLIS_MINUTE);
       		 // 这个是uuid的拼接
            String userKey = getTokenKey(loginUser.getToken());
             // 根据uuid将loginUser缓存  uuid当做key，登录用户当做 value 缓存起来
            redisCache.setCacheObject(userKey, loginUser, expireTime, TimeUnit.MINUTES);
        }
        Map<String, Object> claims = new HashMap<>();
        claims.put(Constants.LOGIN_USER_KEY, token);
        claims.put(Constants.JWT_USERNAME, loginUser.getUsername());
   		 //这一步才是在创建jwt
        return createToken(claims);--->{
            //xsfjlaksjdf.skdfjlasf.sjdlfk
              String token = Jwts.builder()
                .setClaims(claims)
                .signWith(SignatureAlgorithm.HS512, secret).compact();
        		return token;
        }
    }
```

登录后，前端是使用cookie进行 jwt的缓存：

![image-20250307150557920](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071505065.png)



![image-20250307150640385](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202503071506556.png)

## 五、基于ruoyi框架开发前后端代码

> 自行完成。













