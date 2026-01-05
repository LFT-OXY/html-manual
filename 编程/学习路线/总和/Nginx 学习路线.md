
> Nginx 求职高频面试题：[开始刷题](https://www.mianshiya.com/bank/1824363578297618433)

## 介绍

不少同学可能只‌在部署项目时接触过 Ngin⁢x ，甚至有的同学没有接触过‌ Nginx 。

要知道 Nginx‌ 作为一个高性能的 web 服务器⁢和反向代理服务器，无论是前端还是后‌端同学都有必要对其有一定程度的了解﻿。                ⁡                

## 学习路线

### 1、基础入门

- Nginx 是什么：了解 Nginx 的定义，特点（轻量级、高性能的反向代理 Web 服务器）及其应用场景。
- 特点：
    - 轻量级：占用资源少，适合低资源服务器使用。
    - 高性能：异步非阻塞的事件驱动模型，使其能够高效处理数万并发连接。
    - 稳定性强：在高并发场景下仍能保持稳定。
    - 灵活性强：支持自定义模块和脚本扩展。
- Nginx 作用：
    - 作为 Web 服务器提供静态内容服务
    - 作为反向代理和负载均衡器
    - 作为 API 网关
- 安装：参考官方文档进行安装 [Nginx安装指南](https://nginx.org/en/docs/install.html)。
- Nginx 基本命令
    - 启动：`nginx`
    - 停止：`nginx -s stop`
    - 重启：`nginx -s reload`
    - 检查配置：`nginx -t`（确保修改后的配置文件语法正确）
    - 查看状态：通过 `systemctl status nginx` 或 `nginx -V` 查看当前版本和编译参数。
- 配置文件
    - 全局配置块
    - HTTP 块
    - Server 块
    - Location 块
- 静态文件服务：如何配置 Nginx 提供静态文件（HTML、CSS、JavaScript等）。

### 2、常用基础配置

- 反向代理：配置 Nginx 作为反向代理，将请求转发到后端服务器。
- 负载均衡：设置简单的负载均衡策略（轮询、权重等）。健康检查配置，确保服务正常运行时才参与负载均衡。
- 虚拟主机：配置多个虚拟主机，支持不同域名。
- HTTPS 配置：使用 SSL/TLS 为网站启用 HTTPS。
- 动静分离：比如 静态资源由 Nginx 直接处理，动态资源交由后端处理。
- 防盗链：检查 HTTP 请求头中的 Referer 字段。

### 3、高级操作

#### 可视化管理工具

- Nginx Amplify：由 Nginx 官方推出的一款轻量级监控和可视化管理工具。
- Nginx Proxy Manager：开源的 Web UI 管理工具，专门用于通过 Nginx 管理反向代理、SSL 配置、重定向等。
- NginxConfig：生成 Nginx 配置文件的开源、可视化的 Web 工具。
- GoAccess：日志可视化。
- Nginx UI：[https://nginxui.com/zh_CN/guide/about.html](https://nginxui.com/zh_CN/guide/about.html)
- 其它工具：比如宝塔 Linux 服务器管理面板。

#### 高级配置

- 缓存配置：使用 Nginx 的缓存机制提高性能。
- 重定向和重写：实现 URL 重定向和 URL 重写规则。
- 自定义日志：配置 Nginx 日志格式，进行请求日志的管理。
- 访问控制与认证：配置IP白名单、黑名单和基本认证。
- 扩容：纵向扩容（增加单台服务器的硬件资源）和 横向扩容（增加服务器数量）。
- worker 进程优雅关闭：nginx -s quit
- keepalive：在单个 TCP 连接上复用多个 HTTP 请求，减少连接建立和关闭的开销。
- UDP 协议的反向代理。
- Gzip、Brotli 压缩。
- 自定义错误页面。
- WebSocket 支持。
- HTTP/2 支持。

#### 模块扩展

- 模块使用：学习常用模块的配置，如`rewrite`、`gzip`等。
- 高可用性配置：结合 Keepalived 实现 Nginx 高可用。
- 开发一个模块：[https://nginx.org/en/docs/dev/development_guide.html](https://nginx.org/en/docs/dev/development_guide.html)
- OpenResty 平台：通过 OpenResty 的 lua 模块执行脚本等。

### 4、原理扩展

- 事件驱动模型：理解 Nginx 的事件处理机制和异步架构。
    - 异步非阻塞 IO：避免了传统阻塞式服务器在处理大量请求时的性能瓶颈，在处理请求时不会因为某个 I/O 操作（如等待文件读取或网络响应）而挂起整个进程。
    - 事件循环机制：基于多路复用技术（如 `epoll`、`kqueue`），通过单个线程处理多个连接。事件循环不断检测事件的状态（如连接是否可读、可写）。
    - 连接池与内存管理：连接池和缓冲区复用，确保在高并发情况下最大化资源利用，避免频繁的内存分配和回收。
- 模块化设计：学习 Nginx 的模块结构，如何开发和使用第三方模块。
    - 核心模块：这些模块提供基础的功能，如连接管理、请求解析等。
    - 标准模块：例如 `HTTP`、`Mail`、`Stream` 模块，提供了常见的 Web 和网络服务功能。
    - 第三方模块：通过插件的形式加载第三方模块，常见的第三方模块有 `njs` 模块、`ngx_http_upstream_hash_module` 模块等。
- 性能调优：分析 Nginx 性能瓶颈，并进行相应的优化。
    - CPU、内存的优化：调整 `worker_processes` 数量以充分利用多核 CPU；使用 `worker_cpu_affinity` 绑定 worker 进程到特定 CPU 核心，提高缓存命中率。
    - 连接数优化：使用 `worker_connections` 配置单个 worker 进程的最大连接数。合理配置连接数能有效处理大量并发请求。
    - 内存缓冲区调优：`client_body_buffer_size`、`client_max_body_size`、`proxy_buffers` 等指令可用于优化 Nginx 的内存使用，避免过度使用 swap 或内存溢出。
    - 异步 I/O 的配置：使用 `sendfile`、`tcp_nopush` 和 `tcp_nodelay` 这些指令来优化网络数据包的传输方式，减少系统调用和上下文切换，提升传输效率。
    - 进程管理：Nginx 的 master-worker 进程模型允许多个 worker 进程同时处理请求，合理配置进程数和 CPU 亲和性能显著提升并发处理能力。
    - 缓存优化：通过 `proxy_cache`、`fastcgi_cache` 机制进行缓存优化，加快静态资源响应速度，减少服务器负载。
- 处理 http 请求的 11 个流程
	- ![CleanShot 2025-10-26 at 12.34.17@2x.png|475](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/CleanShot%202025-10-26%20at%2012.34.17@2x.png)

- 负载均衡机制
    - 轮询：请求轮流分发到不同的后端服务器，这是一种最简单的均衡方式。
    - 最少连接：将请求发送给当前处理请求最少的服务器。
    - IP Hash：通过计算客户端 IP 地址的哈希值，始终将来自同一 IP 的请求发送到同一个后端服务器。
    - 权重：为不同服务器分配请求权重。
    - 健康检查：比如 nginx_upstream_check_module 模块。
- 限流机制
    - 请求速率限流：控制单位时间内的请求频率，防止大量请求瞬时涌入导致服务器过载。
    - 并发连接限流：限制同时建立的连接数，适合限制占用长时间连接资源的场景。
    - 限流原理：漏桶算法、令牌桶算法
- 进程间的通信
    - 信号
    - 共享内存
    - 套接字
- 源码

### 5、备战面试

- 常见面试问题：
    - [Nginx 原理面试题 - 面试鸭 - 程序员求职面试刷题神器](https://www.mianshiya.com/bank/1824363578297618433)
- 实践项目：搭建一个小型项目，将所学知识应用于实际环境中，例如构建一个简单的网站或 API 服务。