---
title: 分布式Session共享-系统设计
date: 2020-1-20 21:51:01
categories: 系统设计
tags: 
- 技术要点
description: what -> why -> how
---

## What
分布式Session是什么？

### Cookie与Session
- Cookie：存在客户端，由服务端发送给客户端的信息，保存在HTTP响应头里
- Session：存在服务端，是服务器用来保存用户操作的一系列会话信息，如存储用户登录信息等，以保持登录状态

### Session的创建
- 创建Session
    - 浏览器第一次访问服务器会在服务器生成一个Session，服务器会分配一个唯一的会话标识（JSessionID）
- Session的实现方式
    - 使用Cookie实现：将会话标识JSessionID保存在Cookie中，以后每次请求客户端都会带上JSessionID告诉服务端请求哪个会话
    - 使用URL回写实现：服务器发给浏览器的链接都携带JSessionID参数，当Cookie被禁用时使用URL回写
- 使用Java创建Session
    ```java
    HttpServletRequest request = ((ServletRequestAttributes) 
     RequestContextHolder.getRequestAttributes()).getRequest();
    HttpSession session = request.getSession();
    ```


### 分布式Session
- 单机服务器时Session只在一个机器上，但是在集群中，Session分布在不同服务器上
- 当用户请求被分发到不同的服务器上时，就会出现读取不到Session数据的情况，Session共享问题就会出现了

### 分布式Session共享方案
- Session复制：将一台服务器上的Session数据复制到其他服务器
    - 适用：集群中机器数量较少，网络流量较小
    - 优点：实现简单、配置较少、某台机器宕机不影响
    - 缺点：广播式复制有一定的延迟、网络开销大、集群规模大时内存会不够
- Session绑定：利用hash算法，如nginx的ip_hash，使得同一ip的请求分发到同一服务器上
    - 适用：集群中机器数量适中，对稳定性要求不高的场景
    - 优点：实现简单、配置方便、没有额外网络开销
    - 缺点：某台机器宕机用户Session会丢失、容易造成单点故障，不符合高可用要求
- Cookie记录：Session记录在客户端，将Session放在请求中发送给服务器，服务器修改Session响应给客户端，客户端即Cookie
    - 优点：Cookie简单易用，可用性高，大部分要记录的Session信息比较小
    - 缺点：受Cookie大小限制，能记录的信息有限；每次请求响应都通过Cookie，若用户关闭Cookie访问会受到影响
- Session服务器：利用独立部署的Session服务器统一管理Session，读写Session都需访问Session服务器（Redis、Memcached、MySQL）
    - 适用：集群中机器数量较多、网络环境复杂
    - 优点：Session服务器可以解决以上的问题，可靠性好
    - 缺点：实现复杂、稳定性依赖于缓存的稳定，Session信息放入缓存时要有合理的策略写入

## Why
为什么需要分布式Session共享？

### Session一致性问题
- 只要用户不重启浏览器，每次HTTP短连接请求，理论上服务器都能定位到Session，保持会话

### 分布式Session共享
- 为了解决Session一致性问题

## How
怎么应用分布式Session技术？

### Spring Session+Redis实现分布式Session共享