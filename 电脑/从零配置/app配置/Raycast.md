
# 1. 设置快捷键为 cmd + 空格
![image.png|300](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419132520901.png)

# 2. 窗口移动和窗口切换多个显示器
![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419132641198.png)


![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419132657881.png)

# 3 . 剪贴板历史快捷键
 ![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419132742549.png)

# 4 . 窗口切换
![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419133148036.png)

# 5 .  Quicklink: 快速启动链接

## 1. 通过别名快速查找+访问常用网址

使用方法如下：

1. 首先通过`Create quicklink`创建一个`Quicklink`
![image.png|550](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419133759612.png)

2. 再将需要常访问的链接保存到**Link**属性中
3. 然后设置一个别名：**Name**
4. 点击`Save quicklink`完成
5. 再次访问时就不需要专门到打开浏览器里去点击书签，直接在Raycast中搜索别名检索，然后按下Enter键就可在浏览器中打开链接

![image.png|475](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419143020095.png)


`Quicklink` 还支持设置打开链接的浏览器，不改动就使用默认浏览器

## 2. 对访问链接设置动态参数

有时我们可能需要访问同个网页的不同分支页面，这些页面的URL基本相同只是部分参数有区别，这种情况下再去创建多个`Quicklink`就显得有些没必要，我们可以使用Quicklink的参数功能在每次访问时手动输入参数来动态地更新链接

比如，现在我们要访问一个后台日志的网站，它通过URL中的env参数分别区分不同环境

- [https://juejin-log?env=prod](https://link.juejin.cn?target=https%3A%2F%2Fjuejin-log%3Fenv%3Dprod "https://juejin-log?env=prod")
- [https://juejin-log?env=grey](https://link.juejin.cn?target=https%3A%2F%2Fjuejin-log%3Fenv%3Dgrey "https://juejin-log?env=grey")
- [https://juejin-log?env=test](https://link.juejin.cn?target=https%3A%2F%2Fjuejin-log%3Fenv%3Dtest "https://juejin-log?env=test")

那么我们创建`Quicklink`时可以只创建一个对象，在link属性中通过花括号插入env参数即可

- `Quicklink`的动态参数还支持设置默认值:`{argument：defaultVal}`

![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419143944926.png)

![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419133824446.png)

![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419134259623.png)

# 6 . 进入商店下载插件
![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419140919168.png)


- Port Manager
![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419141104017.png)

使用方式: 输入 kill ,就可以直接删除端口号

-  Kill Process: 一键关闭进程
显示后台所有运行的进程列表，包括进程CPU和内存的占用比例，**按Enter键即可Kill进程**，不需要再点点点去找任务管理器了

![image.png|475](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419144621868.png)


- Terminal Finder
![image.png|550](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419141302177.png)

使用方式: 输入 find, 可以跳转终端此文件夹
![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419141346201.png)

也可以反着跳转
![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419141408482.png)


- 安装 github
![image.png|500](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419142115290.png)


- 取色器
![image.png|283](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419143636383.png)
