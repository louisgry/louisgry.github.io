---
title: 《Redis入门》
date: 2019-7-20 21:47:07
categories: 数据库
tags: 
- Redis
description: NoSQL、Redis、Jedis、Redis数据结构（string、hash、list、set、zset）、Redis特性（多数据库、事务）、持久化（RDB、AOF）
---

### 《Redis入门》
- NoSQL
    - NoSQL（Not Only SQL）：非关系型数据库
    - NoSQL的兴起：高并发读写、海量数据的高效存储和访问、高可拓展性和高可用性
    - NoSQL数据库的四大分类
        - 键值对存储：Redis
        - 列存储：HBase
        - 文档数据库：Mongodb
        - 图数据库：Neo4j
    - NoSQL的特点：易扩展、灵活的数据模型、大数据量高性能、高可用
- Redis概述
    - Redis的由来：2009年推出，用C语言开发
    - Redis是高性能键值对数据库，支持丰富的键值数据类型
        - 字符串类型
        - 列表类型
        - 有序集合类型
        - 散列类型
        - 集合类型
    - Redis的应用场景
        - 缓存
        - 任务队列
        - 网站访问统计
        - 数据过期处理
        - 应用排行榜
        - 分布式集群架构中的Session分离
- Redis安装与使用
    - Windows安装：GitHub下载 https://github.com/microsoftarchive/redis/releases
    ```
    redis-server.exe redis.windows.conf
    redis-cli
    ```
- Jedis入门
    - Jedis：是Redis官方首选的Java客户端开发包
    - Spring Boot下使用Redis：
    - 引入依赖
    ```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    ```
    - 单实例测试（服务器上需要打开6379端口：`vim /etc/sysconfig/iptables`）
    ```
    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    @Test
    public void redisTest(){
        // 保存字符串
        stringRedisTemplate.opsForValue().set("aaa", "111");
        Assert.assertEquals("111", stringRedisTemplate.opsForValue().get("aaa"));
    }
    ```
    - Redis存储对象：需要实现RedisSerializer<T>接口来对传入对象进行序列化和反序列化
    - 连接池方式
- Redis的数据结构
    - 字符串（string）
        - 赋值：`set name imooc`
        - 查看：`get name`
        - 先查看再赋值：`set name bytedance` ==> imooc
        - 删除：`del name`
        - 添加：`append num 5` ==> "55" 
        - 递增
            - `incr num`（如果没有该变量，设置其初值为0，再加1）==> 1
            - `incrby num 5` ==> 6
        - 递减
            - `decr num`（如果没有该变量，设置其初值为0，再加-1）==> -1
            - `decrby num 5` ==> 6
    - 哈希（hash）
        - 是String Key和String Value的map容器
        - 赋值
            - `hset myhash name louis`、`hset myhash age 22`
            -  `hmset myhash name louis age 22` 
        - 查看
            - `hget myhash name`、`hget myhash age`
            - `hmget myhash name age`
            - `hgetall myhash` ==> name, louis, age, 22
        - 删除
            - `hdel myhash name age`、`hdel myhash name`
            - `del myhash` ==> (empty list or set)
        - 增加
            - `hincrby myhash age 1`（无hincr、无hdecr、无hdecrby）
        - 判断存在： `hexists myhash age` ==> 1
        - map的长度：`hlen myhash` ==> 2
        - 所有的key：`hkeys myhash` 
        - 所有的value：`hvals myhash` 
    - 字符串列表（list）
        - 底层结构：ArrayList数组、LinkedList双向链表
        - 添加
            - 左端添加：`lpush list 1 2 3 ` ==> 3 2 1 
            - 右端添加：`rpush mylist 1 2 3` ==> 1 2 3
            - 存在时才添加：`lpushx list x`
            - 按index添加：`lset list 3 x`（在index=3的地方添加x）
            - 按elem插入：`linsert list before 3 x`（在第一个3前面插入x，`after`）
        - 查看
            - `lrange list 0 -1`
        - 弹出
            - 左端弹出：`lpop list` ==> 3
            - 右端弹出：`rpop mylist` ==> 3
        - 删除
            - 从左删除：`lrem list 2 3`（从前往后删除2个3）
            - 从右删除：`lrem list -2 3`（从后往前删除2个3）
            - 全部删除：`lrem list 0 3`（删除所有的3）
        - 返回长度：`llen list`
        - 组合
            - 从source取出添加到target：`rpoplpush mylist list`（mylist的最后弹出插入到list中的第一个）
            - 消费者：`rpop`
            - 生产者：`lpush`
    - 字符串集合（set）
        - set不允许添加重复的元素
        - 添加：`sadd set 1 2 3`
        - 查看：`smembers set`
        - 删除：`srem set 1 2`
        - 判断存在：`sismember set a` ==> 1
        - 差集：`sdiff set set1` ==> c 3
        - 交集：`sinter set set1` ==> a b 1 2
        - 并集：`sunion set set1` ==> a b c 1 2 3
        - 长度：`scard set` ==> 6
        - 随机：`srandmember set` ==> b
        - 存储
            - `sdiffstore set2 set set1`
            - `sinterstore set2 set set1`
            - `sunionstore set2 set set1`
    - 有序字符串集合（sorted-set / zset）
        - 添加：`zadd sort 1 a 2 b 3 c`
        - 修改：`zadd sort 2 a` ==> 0
        - 查看：`zscore sort a` ==>2
        - 长度：`zcard sort`
        - 删除：`zrem sort a b`
            - 排名范围删除：`zremrangebyrank sort 0 4`
            - 分数范围删除：`zremrangebyscore sort 80 100`
        - 排序：`zrange sort 0 -1 [withscores]`
            - 降序：`zrevrange sort 0 -1 [withscores]`
            - 范围：`zrangebyscore sort 0 100 [withscores] [limit 0 2]`
        - 自增
            - `zincrby sort 1 a`
        - 计数
            - `zcount sort 80 100` 
- Redis的通用命令
    - 查看所有的key：`keys *`
    - 通配符：`keys my*`（my开头的key）
    - 重命名：`rename name newname`
    - 设置过期时间：`expire name 1`（1秒后过期）
    - 查看过期时间：`ttl name`
    - 类型：`type name`
    - 全部清空：`flushall`
- Redis的特性
    - Redis的多数据库
        - 默认0号数据库
        - 使用1号数据库：`select 1`
        - 移动key到1号数据库：`move name 1`
    - Redis的事务
        - 事务中所有命令都会被串行化，保证事务中的命令被原子化执行
        - 开启事务：`multi`
        - 提交事务：`exec`
        - 回滚事务：`discard`
- Redis的持久化
    - Redis的高性能（所有的数据都存在内存中），持久化：数据从内存到硬盘的过程
    - RDB方式（Redis DataBase，全量持久化）
    - AOF方式（Append Only File，增量持久化） 
        - 从appendonly.aop文件中删除历史命令