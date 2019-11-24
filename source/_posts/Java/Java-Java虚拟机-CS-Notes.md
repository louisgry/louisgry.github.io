---
title: Java：Java虚拟机-CS-Notes
date: 2019-11-23 19:32:04
categories: Java
tags: 
- Ebbinghaus
- Java虚拟机
description: 运行时数据区域：程序计数器、Java虚拟机堆、本地方法栈、Java堆、方法区、运行时常量池、直接内存<br>垃圾收集：判断一个对象是否可被回收、引用类型、垃圾收集算法、垃圾收集器<br>内存分配与回收策略：Minor GC 和 Full GC、内存分配与回收策略、Full GC的触发条件<br>类加载机制：类的生命周期、类加载过程、类初始化时机、类与类加载器、类加载器分类、双亲委派模型、自定义类加载器实现
---
> 《深入理解Java虚拟机》


# 一、运行时数据区域
## 程序计数器

## Java虚拟机堆

## 本地方法栈

## Java堆
    - Java堆
        - 对象实例的分配区域
        - GC管理的主要区域（新生代、老年代）
    - Java内存模型中堆和栈的区别？
        - 五个方面：管理方式、空间大小、碎片相关、分配方式、效率
        - https://www.zhihu.com/question/29833675/answer/82661572
    - 元空间、堆、线程独占之间的联系（内存角度）
        - 元空间（Class）-> Java堆（Object）-> 线程独占（程序计数器、虚拟机栈、本地方法栈）
## 方法区
     - 永久代的不足
        - 字符串常量池存在永久代中，容易出现性能问题和内存溢出
        - 类和方法的信息大小难以确定，永久代大小的指定困难
        - 永久代回收效率偏低，给GC带来不必要的复杂性
        - 永久代是HotSpot特有，不方便与其他JVM的集成 

## 运行时常量池

## 直接内存

# 二、垃圾收集
- 垃圾收集针对Java堆和方法区进行
- 程序计数器、虚拟机栈、本地方法栈属于线程私有的，只存在于线程的生命周期内，线程结束后就会消失，不需要进行垃圾回收

## 判断一个对象是否可被回收
### 1. 引用计数算法
- 为对象添加一个引用计数器，当对象增加一个引用时计数器加1，引用失效时计数器减1，回收引用计数为0的对象
- 缺点：循环引用（引用计数器永远不为0）
```java
public class ReferenceCountingGC {

    public Object instance = null;
    
    public static void main(String[] args) {
        // 循环引用，无法回收
        ReferenceCountingGC a = new ReferenceCountingGC();
        ReferenceCountingGC b = new ReferenceCountingGC();
        a.instance = b;
        b.instance = a;
        
        a = null;
        b = null;
    }
}
```

### 2. 可达性分析算法
- 以GC Roots为起始点进行搜索，可达的对象都是存活的，不可达的对象可被回收
- 可作为GC Roots的对象
    - 虚拟机栈中局部变量表中引用的对象
    - 本地方法栈JNI中引用的对象
    - 方法区中静态属性引用的对象
    - 方法区中常量引用的对象
    ![](/images/java/jvm/1.png)

### 3. 方法区的回收
- 永久代的垃圾收集主要回收两部分内容：废弃常量和无用的类
- 废弃常量
    - 与回收堆中的对象类似。如字符串"abc"进入常量池，却没有String对象引用，则被回收
- 无用的类
    - 该类所有的实例都已经被回收，此时Java堆中不存在该类的任何实例
    - 加载该类的ClassLoader已经被回收
    - 该类对应的Class对象没有在任何地方被引用，也就无法在任何地方通过反射访问该类的方法

### 4. finalize()
- 类似C++的析构函数，用于关闭外部资源
- 当一个对象可被回收时，如果需要执行该对象的finalize()方法，那么就有可能在该方法中让对象重新被引用，从而实现自救
- 自救只能进行一次，如果回收的对象之前调用了finalize()方法自救，后面回收时不会再调用该方法
- 问题：Object的finalize()方法是否与C++的析构函数作用相同？
    - 与C++的析构函数不同，析构函数调用确定，而finalize()不确定
    - finalize()方法：将未被引用的对象放置于F-Queue队列，方法随时可能会被终止，为对象创造一次重生的机会
    ```java
    public static Finalization finalization
    ```

## 引用类型
### 1. 强引用
### 2. 软引用
### 3. 弱引用
### 4. 虚引用
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

## 垃圾收集算法
### 1. 标记-清除算法
### 2. 标记-整理算法
### 3. 复制算法
### 4. 分代收集算法

## 垃圾收集器
### 1. Serial

### 2. ParNew

### 3. Parallel Scavenge

### 4. Serial Old

### 5. Parallel Old

### 6. CMS

### 7. G1 (Garbage First)
    - 适用于年轻代和老年代的收集器
        - G1收集器（复制+标记-整理算法）：Garbage First
            - 特点：并发和并行、分代收集、空间整合、可预测的停顿
            - 将整个Java堆内存划分成多个大小相等的Region
            - 年轻代和老年代不再物理隔离
        ![](/images/19-11-12/2.jpg)
        - 前沿GC：Epsilon GC、ZGC
        - 垃圾收集器之间的联系：
        ![](/images/19-11-12/3.jpg)


# 三、内存分配与回收策略
## Minor GC 和 Full GC


## 内存分配与回收策略
### 1. 对象优先在Eden分配

### 2. 大对象直接进入老年代

### 3. 长期存活的对象将进入老年代

### 4. 动态对象年龄判定

### 5. 空间分配担保

## Full GC的触发条件
### 1. 调用System.gc()

### 2. 老年代空间不足

### 3. 空间分配担保失败

### 4. JDK1.7及以前的永久代空间不足

### 5. Concurrent Mode Failure


# 四、类加载机制
- 反射
    - 面试问反射 -> 背反射的概念（没用） -> 写一个反射的例子（应在理解后记忆）
    - 反射：**将Java类中的全部成份映射为一个个Java对象**
    ```
    Class rc = Class.forName("com.imooc.interview.reflect.Robot");
    Robot r = (Robot) rc.newInstance();
    System.out.println("Class name: " + rc.getName());
    r.sayHi("hello Reflect");

    Method getHello = rc.getDeclaredMethod("throwHello", String.class);
    getHello.setAccessible(true);
    Object str = getHello.invoke(r, "Bob");
    System.out.println("getHello: " + str);

    Method sayHi = rc.getMethod("sayHi", String.class);
    sayHi.invoke(r, "welcome");

    Field name = rc.getDeclaredField("name");
    name.setAccessible(true);
    name.set(r, "Alice");
    sayHi.invoke(r, "welcome");
    ```
## 类的生命周期

## 类加载过程    
- 为什么JVM不直接将源码解析成机器码
    - 免去重复的准备工作，如各种检查
- JVM如何加载.class文件
    - Java虚拟机（内存模型+GC）
        - 由四部分组成：Class Loader(类加载器)、Execution Engine(执行引擎)、Native Interface(本地接口)、Runtime Data Area(运行时数据区域)
    - JVM如何加载.class文件
        - JVM由四部分组成，JVM主要通过Class Loader把符合格式要求的.class文件加载到内存中，再通过Execution Engine解析文件里的字节码，并提交给操作系统去执行
        - 兼容性：可以由其他语言生成字节码
        ![](/images/19-11-11/2.png)

## 类初始化时机

## 类与类加载器
- loadClass和forName的区别
    - 类的加载方式
        - 隐式加载：new
        - 显式加载：loadClass、forName
    - loadClass和forName的区别
        - forName会进行类的初始化
    ```
    ClassLoader cl = Robot.class.getClassLoader();
    Class r = Class.forName("com.imooc.interview.reflect.Robot");
    ```

## 类加载器分类
    - Class Loader种类：
        - BootStrapClassLoader
        - ExtClassLoader
        - AppClassLoader
        - CustomClassLoader
## 双亲委派模型
- 双亲委派机制
    - 定义：在加载一个类时，先将这个类交给父级加载器加载，如果父级加载器无法加载，再由自己加载
        - 作用：避免多份同样字节码加载
    - native：本地库或非Java代码

## 自定义类加载器实现