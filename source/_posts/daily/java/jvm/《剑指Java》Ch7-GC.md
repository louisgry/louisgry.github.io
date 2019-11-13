---
title: 《剑指Java》Ch7-GC
date: 2019-11-12 19:32:04
categories: Daily
tags: Java虚拟机
description: 标记算法（引用计数、可达性分析）、清除算法（标记-清除、复制、标记-整理、分代收集算法）、新生代GC、老年代GC、适用于新生代和老年代的GC、面试题
---

### Ch7-GC
- 7-1：标记算法
    - 对象被判定为垃圾的标准：**没有被其他对象引用**
    - 判断对象是否为垃圾的算法
        - 引用计数算法（判断对象的引用数量）
            - 任何引用计数为0的对象实例可以被当作垃圾回收
            - 优点：执行效率高，程序执行受影响小
            - 缺点：无法检测出循环引用的情况，导致内存泄露
        - 可达性分析算法（判断对象的引用链是否可达）
            - 通过判断对象的引用链是否可达来决定对象是否可以被回收
            - 可以作为GC Root的对象：
                - 虚拟机栈中引用的对象（栈帧中的本地变量表）
                - 方法区中的常量引用的对象
                - 方法区中的类静态属性引用的对象
                - 本地方法栈中JNI（Java Native Interface，Native方法）的引用对象
                - 活跃线程的引用对象
- 7-2：回收算法
    - 标记-清除算法（Mark and Sweep）
        - 标记：从根集合进行扫描，对存活的对象进行标记
        - 清除：对堆内存从头到尾进行线性遍历，回收不可达对象内存
        - 缺点：容易造成碎片化
    - 复制算法（Copying）
        - 分为对象面和空闲面
        - 过程：对象在对象面上创建，存活的对象被从对象面复制到空闲面，将对象面所有对象内存清除
        - 优点：
            - 解决了碎片化问题
            - 顺序分配内存，简单高效
            - 适用于对象存活率低的场景（年轻代）
    - 标记-整理算法（Compacting）
        - 标记：从根集合进行扫描，对存活的对象进行标记
        - 清除：移动所有存活对象，按内存地址依次排列，然后将末端内存地址以后的内存全部回收
       - 优点：
            - 避免内存的不连续性
            - 不用设置两块内存的交换
            - 适用于对象存活率高的场景（老年代）
    - 分代收集算法（Generational Collector）
        - 按照对象生命周期的不同划分区域以采用不同的垃圾回收算法，以提高JVM回收效率
        - jdk7以前：年轻代+老年代+永久代（JDK8移除永久代）
        - 年轻代：尽可能快速地收集掉生命周期短的对象
            - Eden
            - survivor区：from、to
        ![](/images/19-11-12/1.jpg)
        - 老年代：存放生命周期较长的对象
        - GC分类：Minor GC（年轻代，复制算法）、Full GC（老年代）比较慢
            - 触发Full GC的条件：6个（主要：老年代、永久代空间不足、调用`System.gc()`）
        - 常用性能调优参数
            - `-XX:SurvivorRatio`：Eden和Survivor的比值，默认8:1
            - `-XX:NewRatio`：老年代和年轻代内存大小的比例
            - `-XX:MaxTenuringThreshold`：对象从年轻代晋升到老年代经过GC次数的最大阈值
- 7-3：新生代垃圾收集器
    - 两个概念
        - Stop-the-World：JVM由于要执行GC而停止应用程序的执行（JVM优化就是减少其发生的时间）
        - Safepoint：分析对象中对象引用关系不会发生变化的点（snapshot）
    - JVM运行模式（查看`java -version`）
        - Server
        - Client
    - 年轻代常见的垃圾收集器
        - Serial收集器（复制算法）：Client默认
            - 单线程收集，进行垃圾回收时，必须暂停所有工作线程
            - 优点：简单高效，Client下默认的年轻代收集器
        - ParNew收集器（复制算法）：多线程
            - 多线程收集，与Serial相似
            - 特点：单线程不如Serial，在多核下有优势
        - Parallel Scavenge收集器（复制算法）：Server默认
            - 系统吞吐量=运行用户代码时间/(运行用户代码时间+GC时间)
            - 比起关注用户线程的停顿时间，更关注系统吞吐量（适用于后台运行程序）
            - 优点：Server下默认的年轻代收集器
- 7-4：老年代垃圾收集器
    - 老年代常见的垃圾收集器
        - Serial Old收集器（标记-整理算法）：Client默认
            - 单线程收集，进行垃圾回收时，必须暂停所有工作线程
            - 优点：简单高效，Client下默认的老年代收集器
        - Parallel Old收集器（标记-整理算法）：吞吐量
            - 多线程，吞吐量优先
            - 与Parallel Scavenge年轻代收集器一起使用
        - CMS收集器（标记-清除算法）：Concurrent Mark Sweep，碎片化
            - 初始化标记：stop-the-world
            - 并发标记：并发追溯标记，程序不会停顿
            - 并发预清理：查找执行并发标记阶段从年轻代晋升到老年代的对象
            - 重新标记：暂停虚拟机，扫描CMS堆中的剩余对象
            - 并发清理：清理垃圾对象，程序不会暂停
            - 并发重置：重置CMS收集器的数据结构
    - 适用于年轻代和老年代的收集器
        - G1收集器（复制+标记-整理算法）：Garbage First
            - 特点：并发和并行、分代收集、空间整合、可预测的停顿
            - 将整个Java堆内存划分成多个大小相等的Region
            - 年轻代和老年代不再物理隔离
        ![](/images/19-11-12/2.jpg)
        - 前沿GC：Epsilon GC、ZGC
        - 垃圾收集器之间的联系：
        ![](/images/19-11-12/3.jpg)
        
- 7-5~6：GC常见面试题
    - Object的finalize()方法是否与C++的析构函数作用相同？
        - 与C++的析构函数不同，析构函数调用确定，而finalize()不确定
        - finalize()方法：将未被引用的对象放置于F-Queue队列，方法随时可能会被终止，为对象创造一次重生的机会
        ```
        public static Finalization finalization
        ```
    - Java中的强引用、软引用、弱引用、虚引用的作用是什么？
        - 强引用：抛出OOM也不会回收强引用对象，通过将对象设置为null来弱化引用，使其被回收
        ```
        String str = new String("abc");
        ```
        - 软引用：对象处在有用但非必须状态，内存空间不足时回收，可以用来实现高速缓存
        ```
        SoftReference<String> a = new SoftReference<String>(str);
        ```
        - 弱引用：非必须的对象，比软引用更弱，GC时会回收
        ```
        WealkReference<String> b = new WeakReference<String>(str);
        ```
        - 虚引用：不会决定对象的生命周期，任何时候都有可能被回收，必须和引用队列ReferenceQueue联合使用
        ```
        String str = new String("abc");
        ReferenceQueue queue = new ReferenceQueue();
        PhantomReference ref = new PhantomReference(str, queue);
        ```
        - 强引用、软引用、弱引用、虚引用区别：
        ![](/images/19-11-12/4.jpg)
        ![](/images/19-11-12/5.jpg)