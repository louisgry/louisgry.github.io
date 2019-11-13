---
title: 《剑指Java》Ch6-JVM
date: 2019-11-11 19:32:04
categories: Daily
tags: Java虚拟机
description: 类加载（平台无关性、Class Loader、loadClass和forName）、内存模型（线程独占、线程共享）、面试题：JVM参数、堆栈、intern()
---

### Ch6-JVM
- 6-1：谈谈你对Java的理解
    - 从语言特性入手
        - 平台无关性
        - GC
        - 并发
        - 面向对象
        - 类库
        - 异常处理
- 6-2：平台无关性如何实现
    - Once compile, Run anywhere：一次编译到处运行
    ![](/images/19-11-11/1.png)
    - `javac`：编译java文件
    - `javap`：反编译
        ```
        javap -c com.imooc.Code
        ```
    - 为什么JVM不直接将源码解析成机器码
        - 免去重复的准备工作，如各种检查
        - 兼容性：可以由其他语言生成字节码

- 6-3：JVM如何加载.class文件
    - Java虚拟机（内存模型+GC）
        - 由四部分组成：Class Loader(类加载器)、Execution Engine(执行引擎)、Native Interface(本地接口)、Runtime Data Area(运行时数据区域)
    - JVM如何加载.class文件
        - JVM由四部分组成，JVM主要通过Class Loader把符合格式要求的.class文件加载到内存中，再通过Execution Engine解析文件里的字节码，并提交给操作系统去执行
    ![](/images/19-11-11/2.png)
- 6-4：什么是反射
    - 面试问反射 -> 背反射的概念（没用） -> 写一个反射的例子（应在理解后记忆）
    - 反射：**将Java类中的全部成份映射为一个个Java对象**
    ```java
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
- 6-5：谈谈ClassLoader
    - 类从编译到执行的过程
    - Class Loader：从系统外部获得class二进制数据流，然后交个JVM进行操作
    - Class Loader种类：
        - BootStrapClassLoader
        - ExtClassLoader
        - AppClassLoader
        - CustomClassLoader
- 6-6：ClassLoader的双亲委派机制
    - 双亲委派机制
        - 定义：在加载一个类时，先将这个类交给父级加载器加载，如果父级加载器无法加载，再由自己加载
        - 作用：避免多份同样字节码加载
    - native：本地库或非Java代码
- 6-7：loadClass和forName的区别
    - 类的加载方式
        - 隐式加载：new
        - 显式加载：loadClass、forName
    - loadClass和forName的区别
        - forName会进行类的初始化
    ```java
    ClassLoader cl = Robot.class.getClassLoader();
    Class r = Class.forName("com.imooc.interview.reflect.Robot");
    ```
- 6-8~9：Java内存模型之线程独占
    - 内存概述
        - 内存寻址：逻辑地址到物理地址的转化
        - 地址空间：内核空间、用户空间 
    - JVM内存空间结构模型：Runtime Data Area
        - 线程私有：程序计数器、虚拟机栈、本地方法栈（Native）
        - 线程共享：MetaSpace、Java堆（常量池）
    - Java虚拟机栈
        - 栈帧：局部变量表（locals）、操作数栈（stack）
        - 演示：`add(1,2)的过程`
            - `iload`：入栈
            - `istore`：出栈
    - 递归为什么会引发java.lang.StackOverflowError异常？
        - 递归过深，栈帧数超过虚拟栈深度
    - 虚拟机栈过多会引发java.lang.OutOfMemoryError异常 
- 6-10：Java内存模型之线程共享
    - 元空间（MetaSpace）与永久代（PermGen）区别
        - 元空间使用本地内存，永久代使用JVM内存
    - 永久代的不足
        - 字符串常量池存在永久代中，容易出现性能问题和内存溢出
        - 类和方法的信息大小难以确定，永久代大小的指定困难
        - 永久代回收效率偏低，给GC带来不必要的复杂性
        - 永久代是HotSpot特有，不方便与其他JVM的集成 
    - Java堆
        - 对象实例的分配区域
        - GC管理的主要区域（新生代、老年代）
- 6-11~12：JVM常考面试题
    - JVM三大性能调优参数-Xms -Xmx -Xss的含义？
        - -Xms：堆初始值，-Xmx：堆最大值，-Xss虚拟机栈大小
    - Java内存模型中堆和栈的区别（内存分配策略）
        - 静态存储：编译时确定每个数据目标在运行时存储空间需求
        - 栈式存储：数据区需求编译时不确定，运行时模块入口前确定
        - 堆式存储：编译时或运行时模块入口都无法确定，动态分配
    - Java内存模型中堆和栈的区别（二者的联系）
        - 引用对象、数组时，栈里定义变量保存堆中目标的首地址
    ![](/images/19-11-11/3.jpg)
    - Java内存模型中堆和栈的区别？
        - 五个方面：管理方式、空间大小、碎片相关、分配方式、效率
        - https://www.zhihu.com/question/29833675/answer/82661572
    	![](/images/19-11-11/4.png)
    - 元空间、堆、线程独占之间的联系（内存角度）
        - 元空间（Class）-> Java堆（Object）-> 线程独占（程序计数器、虚拟机栈、本地方法栈）
    	![](/images/19-11-11/5.jpg)
    - 不同JDK版本之间的intern()方法区别（JDK6 vs. JDK7+）
        - intern()方法：把字符串对象放到常量池中 
        - 以下代码：jdk6全是false，jdk7+是false和true
        ```java
        String s = new String("a");
        s.intern()
        String s2 = "a";
        System.out.println(s == s2);

        String s3 = new String("a") + new String("a");
        s.intern()
        String s4 = "aa";
        System.out.println(s3 == s4);
        ```