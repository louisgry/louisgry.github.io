---
title: 数据库：Redis-剑指Java
date: 2019-10-20 20:47:07
categories: 数据库
tags: 
- Ebbinghaus
- Redis
description: Redis入门（NoSQL、Redis、Jedis、Redis数据结构、Redis特性、持久化）、Redis进阶（从海量数据中查询某一固定前缀的key、分布式锁、异步队列、持久化原理、主从同步、集群）
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
        - 先查看再赋值：`getset name bytedance` ==> imooc
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

### 《Redis进阶》
- 4-1：Redis简介
    - 主流应用架构
        - 穿透：缓存层查询不到数据，穿透到存储层查询
        - 熔断：当存储层宕机时，请求只访问缓存层
        ![](/images/basic/redis/1.jpg)
    - 缓存中间件：Memcache、Redis
        - Memcache代码层次类型Hash，支持简单数据类型
        - Memcache缺点：不支持数据持久化存储、不支持主从同步、不支持分片 (Sharding)
        - Redis数据类型丰富
        - Redis优点：支持数据持久化存储、支持主从同步、支持分片
    - Redis为什么这么快
        - 10W+ QPS（Query Per Second，每秒内查询次数）
        - Redis完全基于内存，绝大部分请求是内存操作，执行效率高
        - Redis数据结构简单，对数据操作也简单，没有表及关系，性能比关系型数据库高很多
        - 采用单线程，单线程配合I/O多路复用，非阻塞IO，来处理高并发请求
    - 多路I/O复用模型
        - FD（File Descriptor，文件描述符）
            - 一个打开的文件通过唯一的描述符进行引用，该描述符是打开文件的元数据到文件本身的映射
        - Blocking I/O（BIO，传统的阻塞I/O模型）
            - [BIO](https://juejin.im/post/5dc37d895188256d85228de6)通信方式在单线程服务器下一次只能处理一个请求，在处理完毕之前一直阻塞，因此不适用于高并发的情况
        - Select系统调用
            - I/O多路复用模型允许同时等待多个套接字描述符是否就绪
            - [select](https://blog.csdn.net/lihao21/article/details/66097377)允许进程指示内核等待多个事件中的任何一个发生，并只有在一个或多个事件发生或经历一段指定的时间后才唤醒它
        - 多路复用函数（epoll/kqueue/evport/select）
            - Redis优先选择时间复杂度为O(1)的多路复用函数作为底层实现
            - 以时间复杂度O(n)的select作为保底
            - Redis基于react设计模式监听I/O事件 
- 4-2：常用数据类型
    - String：最基本的数据类型，是二进制安全的
        - 保存字符串对象的结构：`struct sdshdr { int len; int free; char buf[] }`
    - Hash：String元素组成的字典，适合用于存储对象
    - List：安装String元素插入顺序排列的列表
        - `lpush`+`lpop`：栈
    - Set：String元素组成的无序集合，通过哈希表实现，不允许重复
        - 集合操作：可以很容易实现微博的共同关注功能
    - Sorted Set：通过分数来为集合中的成员进行从小到大的排序
    - HyperLogLog（计数）、Geo（地理位置）
    - Redis的底层数据类型基础
        - 简单动态字符串、链表、字典、跳跃表、整数集合、压缩列表、对象
- 4-3：从海量key中查询某一固定前缀的key
    - 思维要严谨：如果没加海量，也要问面试官数据的规模，不同规模有不同的方案
    - `keys k1*`：KEYS pattern，查找所有符合给定pattern的key
        - keys的问题：keys一次性返回所匹配的key、key数量过大会导致卡顿
        - `dbsize`：查key的规模
    - `scan cursor`：基于游标的迭代器，增量式迭代
        - 需要基于上一次的游标延续之前的迭代过程
        - 以0作为游标开始一次新的迭代，知道命令返回游标0，完成一次遍历
        - 不保证每次执行都返回某个给定数量的元素，count参数大致约束，支持模糊查询
        ```
        # SCAN cursor [MATCH pattern] [COUTN count]
        scan 0 match k1* count 10
        scan 11534336 match k1* count 10
        // Jedis：使用HashMap对cursor进行去重
        ```
    - 批量生成redis测试数据
    ```
    1.Linux Bash下面执行
      for((i=1;i<=20000000;i++)); do echo "set k$i v$i" >> /tmp/redisTest.txt ;done;
      生成2千万条redis批量设置kv的语句(key=kn,value=vn)写入到/tmp目录下的redisTest.txt文件中
    2.用vim去掉行尾的^M符号，使用方式如下：：
      vim /tmp/redisTest.txt
        :set fileformat=dos #设置文件的格式，通过这句话去掉每行结尾的^M符号
        ::wq #保存退出
    3.通过redis提供的管道--pipe形式，去跑redis，传入文件的指令批量灌数据，需要花10分钟左右
      cat /tmp/redisTest.txt | 路径/redis-5.0.0/src/redis-cli -h 主机ip -p 端口号 --pipe
    ```
- 4-4：如何通过Redis实现分布式锁
    - 分布式锁：控制分布式系统中访问共同资源的实现，互斥保证一致性
        - 需要解决的问题：互斥性、安全性(锁只能被持有方删除)、死锁、容错(部分节点即Redis宕机时还可以获取释放锁)
    - `SETNX key value`：如果key不存在，则创建并赋值
        - 时间复杂度O(1)，返回1成功，返回0失败
        ```
        setnx locknx test # ==> 1
        setnx locknx task # ==> 0
        ```
    - `EXPIRE key second`：解决setnx长期有效的问题
        - 设置key的生存时间，当key过期时，会被自动删除
        ```
        expire locknx 2 # 两秒后过期
        ```
        - Jedis中使用SETNX和EXPIRE：但二者结合，失去了原子性
        ```
        RedisService redisService = SpringUtils.getBean(RedisService.class);
        long status = redisService.setnx(key, "1");
        if(status == 1) {
            redisService.expire(key, expire);
            // pass
        }
        ```
    - Redis2.6.12后：SET组合了setnx和expire
        - SET key value [EX seconds] [PX milliseconds] [NX|XX]
        - PX：设置过期时间为毫秒
        - NX：当键不存在时，才对键进行设置操作
        - XX：当键存在时，才对键进行设置操作
        ```
        set lock 12345 ex 10 nx
        ```
    - 大量的key同时过期的注意事项：执行随机过期时间
- 4-5：如何实现异步队列
    - List作队列，rpush生产消息，lpop消费消息
        - 缺点：lpop不会等待队列里有值，直接消费
        - 弥补：通过在应用层引入sleep机制去调用lpop重试
    - `BLPOP key timeout`：阻塞直到队列有消息或超时
        - 一端执行blpop后，会监听另一端是否生产消息
        ```
        blpop list 30 # rpush后 ==> aaa
        rpush list aaa
        ```
    - pub/sub（主题订阅者模式）：一对多消费队列
        - 发送者pub发送消息，订阅者sub接收消息
        - 订阅者sub可以订阅任意数量的频道
        - 缺点：消息发布无状态，无法保证可达（需用Kafka）
        ```
        subscribe mytopic
        subscribe mytopic2
        publish mytopic hello
        publish mytopic2 good
        ```
- 4-6：持久化RDB
    - RDB（快照）持久化：保存某个时间点的全量数据快照
        - SAVE：阻塞Redis主进程，直到RDB文件被创建完毕
            - 配置中添加`save ""`来禁用RDB
        - BGSAVE：Fork (派生)出一个子进程来创建RDB文件，不阻塞服务器进程
            - 系统调用fork()：创建进程，实现了[Copy on Write](https://juejin.im/post/5bd96bcaf265da396b72f855) (写时复制)
    - 自动化触发RDB持久化
        - 根据redis.conf配置里的SAVE m n定时触发（用的是BGSAVE）
        - 主从复制时，主节点自动触发
        - 执行Debug Reload
        - 执行Shutdown且没有开启AOF持久化
    - RDB持久化的缺点
        - 内存数据全量同步，数据量大会由于I/O而严重影响性能
        - 可能会因为Redis宕机而丢失从当前至最近一次快照期间的数据
- 4-7：持久化AOF
    - Append-Only-File：保存写状态
        - 保存除查询以外的所有指令
        - 以append的形式追加保存到aof文件中（保存增量数据）
        - 修改配置为yes `appendonly yes`来开启AOF
    - 日志重写（aofrewrite）：解决aof文件大小不断变大的问题（原理：Copy on Write）
        - 调用fork()，创建一个子进程
        - 子进程把新的aof写到一个临时文件里，不依赖原来的aof文件
        - 主进程持续把新的变动写到内存的buff和原来的aof中
        - 主进程获取子进程重写aof的完成信号，往新的aof同步增量变动
        - 使用新的aof文件替换掉旧的aof文件
    - RDB和AOF的对比
        - RDB：全量数据快照，文件小，恢复快；但无法保存最近一次快照之后的数据
        - AOF：可读性高，适合保存增量数据，不易丢失；但文件体积大，恢复时间长
    - RDB-AOF混合持久化方式（新版默认）
        - 先RDB (BGSAVE) 做全量数据持久化，再AOF做增量持久化
- 4-8：Pipeline及主从同步
    - Pipeline类似Linux的管道
        - Redis基于请求/相应模型，单个请求处理需要一一应答
        - Pipeline批量执行指令，节省多次IO往返的时间（前提：指令没有依赖相关性）
    - Redis的同步机制（全量、增量同步）
        - 全同步过程
            - Slave发送sync命令到Master
            - Master启动一个后台进程，将Redis中的数据快照保存到文件中（BGSAVE）
            - Master将保存数据快照期间接收到的写命令缓存起来
            - Master完成写文件操作后，将该文件发送给Slave
            - 使用新的aof文件替换旧的aof文件
            - Master将这期间收集的增量写命令发送到Slave端
        - 增量同步过程
            - Master接收到用户的操作指令，判断是否需要传播到Slave
            - 将操作记录追加到aof文件
            - 将操作传播到其他Slave：1.对其主从库，2.向相应缓存写入指令
            - 将缓存中的数据发送到Slave
    - Redis Sentinel（哨兵）：解决主从同步Master宕机后主从切换问题
        - 监控：检查主从服务器是否运行正常
        - 提醒：通过API向管理员或者其他应用程序发送故障通知
        - 自动故障迁移：主从切换
    - 流言协议Gossip：Redis哨兵采用的通信协议
        - 在杂乱无章中寻求一致（区块链的通信协议）
        - 每个节点都随机的与对方通信，最终所有节点的状态达成一致
        - 种子节点定期随机向其他节点发送节点列表以及需要传播的消息
        - 不保证信息一定会传递给所有节点，但最终会趋于一致
- 4-9：Redis集群
    - Redis的数据分布存储
        - Redis Cluster采用无中心节点，每个节点之间使用Gossip流言协议通信
        - 分片：按照某种规则划分数据，将key分散存储在多个节点上
    - 在集群中寻找某一个key在哪台服务器上
        - 常规按照哈希划分无法实现节点的动态增减
        - 一致性哈希算法的概念
            - 对2^32取模，将哈希值空间组织成虚拟的圆环
            - 将数据key使用相同的哈希函数计算出哈希值
        - 一致性哈希算法的理解
            - 好处：如果某一个node宕机，按环的顺时针存到下一个node
            - 问题：node少时，会有Hash环的数据倾斜问题
            - 解决：引入虚拟节点解决数据倾斜问题