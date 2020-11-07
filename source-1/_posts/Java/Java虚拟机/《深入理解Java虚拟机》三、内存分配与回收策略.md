---
title: 《深入理解Java虚拟机》三、内存分配与回收策略
date: 2019-11-22 16:24:04
categories: Java
tags: 
- Ebbinghaus
- Java虚拟机
description: "1. Minor GC 和 Full GC、2. 内存分配策略、3. Full GC的触发条件"
---
> 《深入理解Java虚拟机》



# 三、内存分配与回收策略
## Minor GC 和 Full GC
- Minor GC：回收新生代，新生代对象存活时间短，Minor GC执行频繁，速度也较快。Eden空间满时触发一次Minor GC
- Full GC：回收新生代和老年代，老年代对象存活时间长，Full GC执行较少，速度较慢

## 内存分配策略
### 1. 对象优先在Eden分配
- 大多数情况下，对象在新生代Eden区分配，当其空间不足时，发起Minor GC

### 2. 大对象直接进入老年代
- 大对象：指需要连续内存空间的对象，如很长的字符串或数组
- 经常出现大对象容易导致内存还有不少空间，就提前触发垃圾收集以获取足够的连续空间来分配给大对象
- `-XX:PretenureSizeThreshold`，大于此值的对象直接在老年代分配，避免在Eden和Survivor间大量内存复制

### 3. 长期存活的对象将进入老年代
- 为对象定义年龄计数器，对象在Eden出生并经过Minor GC依然存活，将移动至Survivor中，年龄增加1岁，增加到一定年龄则移动到老年代中
- `-XX:MaxTenuringThreshold`定义年龄的阈值

### 4. 动态对象年龄判定
- 除了达到年龄阈值外，如果在Survivor中相同年龄的所有对象大小的总和大于Survivor空间的一半，则年龄大于或等于该年龄的对象可以直接进入老年代

### 5. 空间分配担保
- 使用复制算法的Minor GC需要老年代的内存空间作担保
- 在Minor GC前，虚拟机会先检查老年代最大可用的连续空间是否大于新生代所有对象的总空间，成立则Minor GC是安全的。如果不成立，虚拟机会查看HandlePromotionFailure的值是否允许担保失败，如果允许继续检查老年代最大可用的连续空间是否大于历次晋升到老年代对象的平均大小，如果大于则进行Minor GC，否则进行Full GC

## Full GC的触发条件
### 1. 调用System.gc()
- 只是建议虚拟机执行Full GC，但不一定真正执行

### 2. 老年代空间不足
- 老年代不足：大对象、长期存活的对象进入老年代
- 可以通过`-Xmn`参数调大新生代的大小，让对象尽量在新生代被回收。还可以通过`-XX:MaxTenuringThreshold`调大晋升老年代的年龄，让对象在新生代存活久一点

### 3. 空间分配担保失败
- 使用复制算法的Minor GC需要老年代的内存空间作担保，如果失败会进行Full GC

### 4. JDK1.7及以前的永久代空间不足
- 当系统中要加载的类、反射的类和调用的方法较多时，永久代空间可能会不足，在未采用CMS时会执行Full GC，如果回收不了则抛出OOM异常

### 5. Concurrent Mode Failure
- 执行CMS GC的同时有对象要放入老年代，而此时老年代空间不足（如浮动垃圾导致），会报Concurrent Mode Failure错误，并触发Full GC

