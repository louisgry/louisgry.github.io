---
title: Nginx负载均衡-系统设计
date: 2020-1-12 21:51:01
categories: 系统设计
tags: 
- 技术要点
description: what -> why -> how
---

## What
什么是负载均衡？

### 正向代理与反向代理
- 正向代理隐蔽客户端，反向代理隐蔽服务器（代理对象不同，一是客户端一是服务器）
- 正向代理：e.g.科学上网，使用代理(Proxy)服务器请求
- 反向代理：e.g.访问网站，不知道具体是哪台服务器

### 负载均衡
- 负载均衡服务器是一个中间件，将服务请求按实际情况分配到不同的服务器
- 负载均衡是通过反向代理来实现的，通过负载均衡可以有效调节服务器的压力

### 常用负载均衡方式
- [轮询](#轮询)（默认）
    - 每个请求按时间顺序逐一分配到不同的服务器，如果宕机会自动剔除
- [weight](#weight)
    - 指定轮询的几率，weight权重和访问比率成正比，权重越高几率越大，常用于服务器性能不均衡场景
- [ip_hash](#ip_hash)
    - 每个请求按访问ip的hash结果进行分配，每个用户固定访问一个服务器，可以解决Session问题
- [fair](#fair)（第三方）
    - 按后端服务器的响应时间来分配请求，响应时间短的优先分配
- [url_hash](#url_hash)（第三方）
    - 按访问url的hash结果来分配请求，使每个url定向到同一服务器，服务器为缓存时比较有效

## Why
为什么需要负载均衡？
- 当一台服务器的单位时间内的访问量越大时，服务器压力就越大，大到超过自身承受能力时，服务器就会崩溃
- 为了避免服务器崩溃，让用户有更好的体验，我们通过负载均衡的方式来分担服务器压力


## How
如何应用负载均衡？

### 轮询
```
upstream backserver {
    server 192.168.0.14;
    server 192.168.0.15;
}
```

### weight
```
upstream backserver {
    server 192.168.0.14 weight=3;
    server 192.168.0.15 weight=7;
}
```

### ip_hash
```
upstream backserver {
    ip_hash;
    server 192.168.0.14:88;
    server 192.168.0.15:80;
}
```

### fair
```
upstream backserver {
    server server1;
    server server2;
    fair;
}
```

### url_hash
```
upstream backserver {
    server squid1:3128;
    server squid2:3128;
    hash $request_uri;
    hash_method crc32;
}
```
