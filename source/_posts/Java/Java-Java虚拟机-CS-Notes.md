---
title: Java：Java虚拟机-CS-Notes
date: 2019-11-26 16:24:04
categories: Java
tags: 
- Ebbinghaus
- Java虚拟机
description: 一、运行时数据区域：1. 程序计数器、2. Java虚拟机栈、3. 本地方法栈、4. Java堆、5. 方法区、6. 运行时常量池、7. 直接内存<br>二、垃圾收集：1. 判断一个对象是否可被回收、2. 引用类型、3. 垃圾收集算法、4. 垃圾收集器<br>三、内存分配与回收策略：1. Minor GC 和 Full GC、2. 内存分配策略、3. Full GC的触发条件<br>四、类加载机制：1. 类的生命周期、2. 类加载的过程、3. 类初始化时机、4. 类加载器<br>五、Java虚拟机的其他知识：1. JVM组成结构、2. 虚拟机性能监控与故障处理工具、3. Java虚拟机的相关问题
---
> 《深入理解Java虚拟机》


# 一、运行时数据区域
- Java虚拟机由四部分组成：ClassLoader(类加载器)、Execution Engine(执行引擎)、Native Interface(本地接口)、Runtime Data Area(运行时数据区域)
- 运行时数据区域：JVM在执行Java程序的过程中会把它所管理的内存划分为若干个不同的数据区域
    ![](/images/java/jvm/1.png)

## 程序计数器
- 程序计数器（Program Counter Register）是一块较小的内存空间，记录正在执行的虚拟机字节码指令的地址（如果正在执行的是本地方法则为空）

## Java虚拟机栈
- Java虚拟机栈描述的是Java方法执行的内存模型：每个方法在执行的同时会创建一个栈帧，用于存储局部变量表、操作数栈、常量池引用等信息
- 从方法调用直至执行完成的过程，对应着一个栈帧在Java虚拟机栈中入栈和出栈的过程
- 可以通过`-Xss`参数来指定每个线程的Java虚拟机栈内存大小，JDK1.4默认256K，JDK1.5+默认1M
```java
java -Xss2M HackTheJava
```
- 该区域可能抛出以下异常
    - 当线程请求的栈深度超过最大值，会抛出StackOverflowError异常
    - 栈进行动态扩展时，如果无法申请到足够内存，会抛出OutOfMemoryError异常
    ![](/images/java/jvm/2.png)

## 本地方法栈
- 本地方法栈：与虚拟机栈类似，区别是本地方法栈为本地方法服务
- 本地方法一般是使用其他语言编写的，并且被编译为基于本机硬件和操作系统的程序，对这些方法需特别处理
    - JNI：Java本地接口（Java Native Interface）是一种编程框架，使得Java虚拟机中的Java程序可以调用本地库，也可以被其他程序调用
    ![](/images/java/jvm/3.png)

## Java堆
- Java堆：所有的对象都在堆中分配内存，是垃圾收集的主要区域（GC堆）
- 主要采用分代收集算法，其主要思想是针对不同类型的对象采取不同的垃圾回收算法
    - 新生代（Young Generation）
    - 老年代（Old Generation）
- 堆不需要连续内存，并且可以动态增加内存，增加失败会抛出OutOfMemoryError异常
- 可以通过`-Xms`和`-Xmx`参数来指定一个程序的堆内存大小，第一个参数设置初始值，第二个设置最大值
```java
java -Xms1M -Xmx2M HackTheJava
```

## 方法区
- 方法区：用于存放已被加载的类信息、常量、静态变量、即时编译器（JIT编译器）编译后的代码等数据
- 和堆一样不需要连续的内存，并且可以动态扩展，扩展失败也会抛出OutOfMemoryError异常
- 对方法区的垃圾回收主要目标是：常量池的回收、对类的卸载，但是比较难实现
- 永久代
    - HotSpot虚拟机把方法区当作永久代进行垃圾回收，但很难确定永久代的大小，因为有很多因素的影响，并且每次Full GC后永久代的大小都会改变，所以经常抛出OOM异常
    - 为了更容易管理方法区，从JDK1.8开始，移除永久代，并把方法区移至元空间，它位于本地内存中，而不是虚拟机内存中
- 方法区是一个JVM规范，永久代与元空间都是方法区的实现方式。JDK1.8后，原来永久代的数据被分到了堆和元空间中。元空间存储类的元信息，堆存储静态变量和常量池

## 运行时常量池
- 运行时常量池：是方法区的一部分。Class文件中的常量池会在类加载后被放入运行时常量池
- 除了在编译期生成的常量，也允许动态生成，如String类的intern()方法

## 直接内存
- JDK1.4中引入了NIO（New Input/Output）类，它可以使用Native函数库直接分配堆外内存，然后通过Java堆里的DirectByteBuffer对象作为这块内存的引用进行操作
- 好处：能在某些场景中显著提高性能，因为避免了在堆内存和堆外内存来回拷贝数据


# 二、垃圾收集
- 垃圾收集针对Java堆和方法区进行。程序计数器、虚拟机栈、本地方法栈属于线程私有的，只存在于线程的生命周期内，线程结束后就会消失，不需要进行垃圾回收

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
- Java虚拟机使用该算法来判断对象是否可被回收
- 可作为GC Roots的对象
    - 虚拟机栈中局部变量表中引用的对象
    - 本地方法栈JNI中引用的对象
    - 方法区中静态属性引用的对象
    - 方法区中常量引用的对象
    ![](/images/java/jvm/4.png)

### 3. 方法区的回收
- 永久代的垃圾收集主要回收两部分内容：废弃常量和无用的类（常量池的回收和对类的卸载）
- 废弃常量：与回收堆中的对象类似。如字符串"abc"进入常量池，却没有String对象引用，则被回收
- 为了避免内存溢出，在大量使用反射和动态代理的场景都需要虚拟机具备类卸载功能
- 无用的类：需同时满足以下3个条件
    - 该类所有的实例都已经被回收，此时Java堆中不存在该类的任何实例
    - 加载该类的ClassLoader已经被回收
    - 该类对应的Class对象没有在任何地方被引用，也就无法在任何地方通过反射访问该类的方法

### 4. finalize()
- 类似C++的析构函数，用于关闭外部资源
- 当一个对象可被回收时，如果需要执行该对象的finalize()方法，那么就有可能在该方法中让对象重新被引用，从而实现自救
- 自救只能进行一次，如果回收的对象之前调用了finalize()方法自救，后面回收时不会再调用该方法


## 引用类型
- 判断对象是否可被回收都与引用有关

### 1. 强引用
- 强引用：被强引用关联的对象不会被回收，可以通过将对象设置为null来弱化引用，使其被回收
```java
String str = new String("abc");
```

### 2. 软引用
- 软引用：被软引用关联的对象只有在内存不够的情况下才会被回收
```java
SoftReference<String> a = new SoftReference<String>(str);
```

### 3. 弱引用
- 弱引用：被弱引用关联的对象一定会被回收，即只能存活到下一次垃圾回收发生之前
```java
WeakReference<String> b = new WeakReference<String>(str);
```

### 4. 虚引用
- 虚引用：一个对象是否有虚引用，不会对其生存时间造成影响，也无法通过虚引用得到一个对象
- 目的：能在这个对象被回收前收到一个系统通知
```java
ReferenceQueue queue = new ReferenceQueue();
String str = new String("abc");
PhantomReference ref = new PhantomReference(str, queue);
```

## 垃圾收集算法
### 1. 标记-清除算法
- 标记-清除（Mark-Sweep），分为标记和清除两个阶段：首先标记出所有需要回收的对象，在标记完成后统一回收被标记的对象，标记过程使用标记方法判定
- 回收对象就是把对象分块，连接到被称为“空间链表”的单向链表，之后进行分配时只需要遍历这个空闲链表，就可以找到分块
- 缺点
    - 效率问题：标记和清除过程效率不高
    - 空间问题：会产生大量不连续的内存碎片，导致无法给大对象分配内存

### 2. 标记-整理算法
- 标记-整理（Mark-Compact），让所有存活的对象都向一端移动，然后直接清理掉端边界意外的内存
- 优点：不会产生内存碎片
- 缺点：需要移动大量对象，处理效率比较低

### 3. 复制算法
- 复制（Copying），将内存划分为大小相等的两块，每次只使用其中一块，当这一块内存用完了就将还存活的对象复制到另一块上面，然后把使用过的内存空间进行清理
- 现代虚拟机用复制算法回收新生代，但不是划分为相等的两块，而是Eden:Survivor默认为8:1，每次只使用Eden和其中一块Survivor（From、To是变化的），回收时将Eden和From中还存活的对象复制到To区，然后清理Eden和From区
- 缺点：只使用了内存的一半，需要额外的空间进行分配担保。且对象存活率高的场景就要进行较多的复制，效率会变低，所以不适用于老年代
    ![](/images/java/jvm/5.png)

### 4. 分代收集算法
- 分代收集（Generational Collection），现代虚拟机所采用，它根据对象存活周期的不同将内存划分为块，不同块采用不同的收集算法
- 新生代：使用复制算法
- 老年代：使用标记-清除算法，或标记-整理算法

## 垃圾收集器
- HotSpot虚拟机中的7个垃圾收集器，连线表示可以配合使用
    - 单线程与多线程：ParNew、Parallel Scavenge、Parallel Old是多线程的
    - 串行与并行：垃圾收集器与用户程序是交替执行或同时执行，只有CMS和G1是并行的
    ![](/images/java/jvm/6.png)

### 1. Serial
- Serial的意思是串行，即以串行的方式执行，Serial是单线程的收集器，只会使用一个线程进行垃圾收集
- Client下默认的新生代收集器，因为该场景下内存一般不会很大
- 优点：简单高效，在单CPU下，由于没有线程交互的开销，因此有最高的单线程收集效率

### 2. ParNew
- ParNew是Serial收集器的多线程版本
- Server下默认的新生代收集器，除了性能外，主要是因为除了Serial，只有ParNew能与CMS收集器配合使用

### 3. Parallel Scavenge
- Parallel Scavenge也是多线程收集器，其收集目标是吞吐量优先（适用于后台运行程序），不同于其他的目标是尽可能缩短垃圾收集时用户线程的停顿时间。缩短停顿时间是以牺牲吞吐量和新生代空间换取的：新生代空间变小，垃圾回收变频繁，导致吞吐量下降
- 系统吞吐量=运行用户程序时间/(运行用户程序时间+GC时间)
- 可以通过开关参数`-XX:+UseAdaptiveSizePolicy`打开GC自适应的调节策略（GC Ergonomics），就不用手动设置新生代大小、Eden和Survivor比例、晋升老年代对象年龄等参数

### 4. Serial Old
- Serial Old是Serial收集器的老年代版本，Client下的老年代收集器
- 如果用在Server场景下，有两大用途
    - 在JDK1.5及以前，与Parallel Scavenge配合使用
    - 作为CMS收集器的后备预案，在并发收集发生Concurrent Mode Failure时使用

### 5. Parallel Old
- Parallel Old是Parallel Scavenge的老年代版本
- 在注重吞吐量和CPU资源敏感的场景，可以优先考虑Parallel Scavenge和Parallel Old收集器

### 6. CMS
- CMS（Concurrent Mark Sweep），基于标记-清除算法，是一种以获取最短回收停顿时间为目标，适用于服务端应用（重视服务的响应速度，希望系统停顿时间最短，有更好的用户体验）。使用CMS来收集老年代时，新生代的收集只能选Serial或ParNew
    - 初始标记：仅仅只是标记一下GC Roots能直接关联到的对象，速度很快，需要停顿（Stop The World）
    - 并发标记：进行GC Roots Tracing的过程，在整个回收过程中耗时最长，不需要停顿
    - 重新标记：为了修正并发标记期间用户程序继续运作而导致标记产生变动的那一部分对象的标记记录，需用停顿（Stop The World）
    - 并发清除：不需要停顿
- 优点：并发收集、低停顿
- 缺点
    - 吞吐量低：低停顿时间是以牺牲吞吐量为代价的，导致CPU利用率不高
    - 无法处理浮动垃圾，可能会出现Concurrent Mode Failure。浮动垃圾是指并发清除阶段由于用户线程继续运行而产生的垃圾，这部分垃圾只能到下一次GC才被回收。由于浮动垃圾的存在，因此需要预留出一部分内存，此时CMS不能像其他收集器那样等待老年代快满了才回收。如果预留的内存不够存放，就会出下Concurrent Mode Failure，这时会触发Full GC，使用Serial old临时替换CMS，停顿时间变长
    - 空间问题：标记-清除算法导致的空间碎片，会出现老年代空间剩余，但无法找到足够大的连续空间来分配当前对象，要提前触发一次Full GC

### 7. G1
- G1（Garbage First），是一款面向服务端应用的垃圾收集器，在多CPU和大内存场景下有很好的性能
- G1可以直接对新生代和老年代一起回收，它把堆划分成多个大小相等的独立区域（Region），新生代和老年代不再物理隔离
- 每个Region都有一个Remembered Set，用来记录该Region对象的引用对象所在的Region，通过使用该Set在可达性分析时可以避免全堆扫描
    - 初始标记：仅仅只是标记一下GC Roots能直接关联到的对象
    - 并发标记：从GC Root开始对堆中对象进行可达性分析，找出存活的对象，耗时长，但可并行
    - 最终标记：为了修正并发标记期间用户程序继续运作而导致标记产生变动的那一部分对象的标记记录，虚拟机将对象变化记录在Remembered Set Logs里面，并合并数据到Set中，需要停顿，但可并行
    - 筛选回收：首先对Region中的回收价值和成本进行排序，根据用户所期望的GC停顿时间来制定回收计划，可并行
- 特点
    - 空间整合：整体是标记-整理算法，局部（两Region之间）是复制算法，不会有内存碎片
    - 可预测的停顿：能让使用者明确指定在一个长度为M毫秒的时间内，消耗在GC上的时间不超过N毫秒
    ![](/images/java/jvm/7.png)



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


# 四、类加载机制
- 类是在运行期间第一次使用时动态加载的，而不是一次性全部加载，因为它会占用很多的内存

## 类的生命周期
- 类的生命周期有7个阶段
    - **加载（Loading）**
    - **验证（Verification）**
    - **准备（Preparation）**
    - **解析（Resolution）**
    - **初始化（Initialization）**
    - 使用（Using）
    - 卸载（Unloading）
    ![](/images/java/jvm/8.png)

## 类加载的过程
- 类加载的5阶段：加载、验证、准备、解析、初始化

### 1.加载
- 加载是类加载（Class Loading）过程的一个阶段，注意不要混淆
- 加载过程完成以下三件事
    - 通过类的完全限定名称获取定义该类的二进制字节流
    - 将该字节流表示的静态存储结构转换为方法区的运行时存储结构
    - 在内存中生成一个代表该类的Class对象，作为方法区中该类各种数据的访问入口
- 其中二进制字节流可以从以下方式中获取
    - 从ZIP包读取，成为JAR、EAR、WAR格式的基础
    - 从网络中获取，最经典的应用是Applet
    - 运行时计算生成，例如动态代理技术，java.lang.reflect.Proxy使用ProxyGenerator.generateProxyClass的代理类的二进制字节流
    - 由其他文件生成，例如由JSP文件生成对应的Class类

### 2.验证
- 验证：确保Class文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全

### 3.准备
- 准备阶段为类变量分配内存并设置初始值，使用方法区的内存（类变量是被static修饰的变量）
- 不包括实例变量，实例变量将会在对象实例化时随着对象一起分配在Java堆中
- 类变量初始值一般为0，如以下变量初始化为0而不是123
```java
public static int i = 123;
```

### 4.解析
- 解析：将常量池的符号引用替换为直接引用的过程。为了支持Java的动态绑定，解析可在初始化阶段之后在开始
    - 符号引用：以一组符号来描述所引用的目标，如CONSTANT_Class_info
    - 直接引用：可以是直接指向目标的指针、相对偏移量、一个能间接定位到目标的句柄

### 5.初始化
- 初始化阶段才真正开始执行类中定义的Java程序代码。初始化阶段是虚拟机执行类构造器`<client>()`方法的过程。在准备阶段，类变量根据系统要求赋初值，而在初始化阶段，根据程序制定的主观计划去初始化类变量和其他资源
- `<client>()`是由编译器自动收集类中所有类变量的赋值动作和静态语句块中的语句合并的，编译器收集的顺序由语句在源文件中出现的顺序决定
    - 静态语句块只能访问到定义在它之前的类变量，不能访问定义在它之后的类变量
    - 父类的`<client>()`方法先执行，故父类中定义的静态语句块先于子类。
    - 接口中不可以使用静态语句块，但有类变量初始化的赋值操作，因此接口也会生成`<client>()`方法。但接口不需要先执行父接口的`<client>()`方法，只有当父接口中定义的变量被使用时才会初始化。同样，接口的实现类在初始化时也不会执行接口的`<client>()`方法
    - 虚拟机会保证一个类的`<client>()`方法在多线程环境下被正确的加锁和同步，如果多个线程同时初始化一个类，只会有一个线程执行该类的`<client>()`
    ```java
    public class Initial {
        static {
            i = 0;                  // 编译正常通过
            System.out.println(i);  // Error：非法向前引用
        }
        static int i = 1;
    }
    ```

## 类初始化时机
### 1. 主动引用
- 虚拟机没有强制约束何时加载，但以下五种情况必须对类进行初始化（加载、验证、准备在此之前开始）
    - 遇到new、getstatic、putstatic、invokestatic这四条字节码指令时，如果类没有进行过初始化，则需要先触发其初始化（场景：使用new关键字实例化对象时、读取或设置一个类的静态字段时、调用一个类的静态方法时）
    - 使用java.lang.reflect包的方法对类进行反射调用时，如果类没有进行初始化，则需要先触发其初始化
    - 当初始化一个类的时候，如果其父类还没有进行过初始化，则需要先触发其父类的初始化
    - 当虚拟机启动时，用户需要指定一个要执行的主类（包含main()方法的那个类），虚拟机会先初始化这个主类
    - 当使用JDK1.7的动态语言支持时，如果一个java.lang.invoke.MethodHandle实例最后的解析结果为REF_getStatic、REF_putStatic、REF_invokeStatic的方法句柄，并且这个方法句柄所对应的类没有进行过初始化，则需要先触发其初始化

### 2. 被动引用
- 除以上五种主动引用外，所有引用类的方式都不会触发初始化，称为被动引用。常见例子有
    - 通过子类引用父类的静态字段，不会导致子类初始化
    - 通过数组定义来引用类，不会触发该类的初始化。该过程会对数组类进行初始化（数组类是由虚拟机自动生成、直接继承自Object的子类，包含数组的属性和方法）
    - 常量在编译阶段会存入调用类的常量池中，本质上并没有直接引用到定义常量的类，不会触发定义常量类的初始化

## 类加载器
- 类加载器不仅限于类加载阶段。对于任意一个类，都需要由加载它的类加载器和这个类本身一同确立其在Java虚拟机中的唯一性
- 也就是说，两个类相等，需要类本身相等，还要使用同一个类加载器进行加载，因为每一个类加载器都拥有一个独立的类名称空间。这里的相等，包括类的Class对象的equals()、isAssignableFrom()、isInstance()方法的返回结果为true，也包括使用instanceof关键字做对象所属关系判定为true

### 类加载器分类
- 从Java虚拟机的角度，只有两种类加载器
    - 启动类加载器（Bootstrap ClassLoader）：使用C++实现，是虚拟机自身的一部分
    - 所有其他类的加载器：使用Java实现，独立于虚拟机，继承自抽象类java.lang.ClassLoader
- 从Java开发人员的角度，有以下三种加载器
    - 启动类加载器（Bootstrap ClassLoader）：负责将存放在`<JRE_HOME>\lib`目录中的，或被-Xbootclasspath参数所指定的路径中的，并且是虚拟机识别的（仅按照文件名识别，如rt.jar，名字不符合的类库即使放在lib目录中也不会被加载）类库加载到虚拟机内存中。启动类加载器无法被Java程序直接引用，用户在编写自定义类加载器时，如果需要把加载请求委派给启动类加载器，直接使用null代替即可
    - 扩展类加载器（Extension ClassLoader）：由ExtClassLoader实现，它负责将`<JAVA_HOME>\lib\ext`或者被java.ext.dir系统变量所指定路径中的类库加载到内存中，开发者可以直接使用扩展类加载器
    - 应用程序类加载器（Application ClassLoader）：由AppClassLoader实现，由于这个类加载器是ClassLoader中的getSystemClassLoader()方法的返回值，因此一般称为系统类加载器。它负责加载用户类路径CLASSPATH上所指定的类库，开发者可以直接使用这个类加载器，如果应用程序中没有自定义过自己的类加载器，一般情况下默认就是Application ClassLoader

### 双亲委派模型
- 双亲委派模型（Parents Delegation Model）：要求除了顶层的启动类加载器外，其他的类加载器都要有自己的父类加载器。父子关系一般通过组合关系实现，而不是继承
    ![](/images/java/jvm/9.png)

### 1. 工作过程
- 一个类加载器首先将类加载请求转发到父类加载器，只有当父类加载器无法完成时才尝试自己加载

### 2. 好处
- 使得Java类随着它的类加载器一起具有一种带有优先级的层次关系，从而使得基础类得到统一
- 例如，java.lang.Object放在rt.jar中，如果另写一个Object放到CLASSPATH中，程序编译通过。因为双亲委派模型，在rt.jar中的Object优先级更高，其使用的是启动类加载器，而CLASSPATH中的Object使用应用程序类加载器

### 3. 实现
- `loadClass()`方法运行过程：先检查类是否已经加载过，如果没有则让父类加载器去加载。当父类加载器加载失败时抛出ClassNotFoundException，此时尝试自己加载
```java
public abstract class ClassLoader {
    // The parent class loader for delegation
    private final ClassLoader parent;

    public Class<?> loadClass(String name) throws ClassNotFoundException {
        return loadClass(name, false);
    }

    protected Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {
        synchronized (getClassLoadingLock(name)) {
            // First, check if the class has already been loaded
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                try {
                    if (parent != null) {
                        c = parent.loadClass(name, false);
                    } else {
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }

                if (c == null) {
                    // If still not found, then invoke findClass in order
                    // to find the class.
                    c = findClass(name);
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
    }

    protected Class<?> findClass(String name) throws ClassNotFoundException {
        throw new ClassNotFoundException(name);
    }
}
```

### 自定义类加载器实现
- 应用程序是由三种类加载器相互配合从而实现类加载，此外还可以自定义类加载器
- 例如，自定义类加载器FileSystemClassLoader，继承自ClassLoader，用于加载文件系统上的类。它首先根据类的全名在文件系统上查找类的字节代码文件（.class文件），然后读取该文件内容，最后通过defineClass()方法把字节码转换成Class类的实例。自定义类加载器不需要重写loadClass()方法，但需要重写findClass()方法。
```java
public class FileSystemClassLoader extends ClassLoader {
    private String rootDir;

    public FileSystemClassLoader(String rootDir) {
        this.rootDir = rootDir;
    }

    protected Class<?> findClass(String name) throws ClassNotFoundException {
        byte[] classData = getClassData(name);
        if (classData == null) {
            throw new ClassNotFoundException();
        } else {
            return defineClass(name, classData, 0, classData.length);
        }
    }

    private byte[] getClassData(String className) {
        String path = classNameToPath(className);
        try {
            InputStream ins = new FileInputStream(path);
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            int bufferSize = 4096;
            byte[] buffer = new byte[bufferSize];
            int bytesNumRead;
            while ((bytesNumRead = ins.read(buffer)) != -1) {
                baos.write(buffer, 0, bytesNumRead);
            }
            return baos.toByteArray();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

    private String classNameToPath(String className) {
        return rootDir + File.separatorChar
        + className.replace('.', File.separatorChar) + ".class";
    }
}
```

# 五、Java虚拟机的其他知识
## JVM组成结构
- JVM基本结构，有别于JVM内存结构（也有别于JVM内存模型）
    - ClassLoader(类加载器)：根据特定格式，加载class文件到内存
    - Execution Engine(执行引擎)：对命令进行解析
    - Native Interface(本地接口)：融合不同语言的原生库为Java所用
    - Runtime Data Area(运行时数据区域)：JVM内存结构

## 虚拟机性能监控与故障处理工具
### 1. JDK的命令行工具
- `jps`: 虚拟机进程状态工具，显示指定系统内所有的HotSpot虚拟机进程
- `jstat`: 虚拟机统计信息监视工具，用于收集HotSpot虚拟机各方面的允许数据
- `jinfo`: Java配置信息工具，显示虚拟机配置信息
- `jmap`: Java内存映像工具，生成虚拟机的内存转储快照（heapdump文件）
- `jhat`: 虚拟机堆转储快照分析工具，用于分析heapdump文件
- `jstack`: Java堆栈跟踪工具，显示虚拟机的线程快照

### 2. JDK的可视化工具
- JConsole: Java监视与管理控制台
- Visual VM: 多合一故障处理工具

## Java虚拟机的相关问题
### JVM如何加载.class文件？
- JVM由四部分组成，JVM主要通过ClassLoader把符合格式要求的.class文件加载到内存中，再通过Execution Engine解析文件里的字节码，并提交给操作系统去执行

### loadClass()和forName()的区别？
- 类的加载方式
    - 隐式加载：new
    - 显式加载：loadClass、forName
- loadClass和forName的区别：forName()会进行类的初始化
```java
ClassLoader cl = Robot.class.getClassLoader();
Class r = Class.forName("com.imooc.interview.reflect.Robot");
```

### 为什么JVM不直接将源码解析成机器码
- 免去重复的准备工作，如各种检查
- 兼容性：可以由其他语言生成字节码

### 递归为什么会引发java.lang.StackOverflowError异常
- 递归过深，栈帧数超过虚拟栈深度。同时，虚拟机栈过多会引发java.lang.OutOfMemoryError异常

### 元空间（MetaSpace）与永久代（PermGen）区别
- 元空间使用本地内存，永久代使用JVM内存
- 永久代的不足
    - 字符串常量池存在永久代中，容易出现性能问题和内存溢出
    - 类和方法的信息大小难以确定，永久代大小的指定困难
    - 永久代回收效率偏低，给GC带来不必要的复杂性
    - 永久代是HotSpot特有，不方便与其他JVM的集成 

### JVM三大性能调优参数-Xms -Xmx -Xss的含义
- -Xms：堆初始值，-Xmx：堆最大值，-Xss虚拟机栈大小

### Java内存模型中堆和栈的区别
> [知乎：Java虚拟机的堆、栈、堆栈如何去理解？](https://www.zhihu.com/question/29833675/answer/82661572)

- 二者的联系：引用对象、数组时，栈里定义变量保存堆中目标的首地址
- 五个方面的区别：管理方式、空间大小、碎片相关、分配方式、效率
    - 管理方式：栈自动释放，堆需要GC
    - 空间大小：栈比堆小
    - 碎片相关：栈产生的碎片远小于堆
    - 分配方式：栈支持静态和动态分配，堆仅支持动态分配
    - 效率：栈的效率比堆高

### 元空间、堆、线程独占之间的联系
- 内存角度：元空间（Class）-> Java堆（Object）-> 线程独占（程序计数器、虚拟机栈、本地方法栈）

### 不同JDK版本之间的intern()方法区别（JDK6 vs. JDK7+）
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

### 常用性能调优参数有哪些
- `-XX:SurvivorRatio`：Eden和Survivor的比值，默认8:1
- `-XX:NewRatio`：老年代和年轻代内存大小的比例
- `-XX:MaxTenuringThreshold`：对象从年轻代晋升到老年代经过GC次数的最大阈值

### Object的finalize()方法是否与C++的析构函数作用相同
- 与C++的析构函数不同，析构函数调用确定，而finalize()不确定
- finalize()方法：将未被引用的对象放置于F-Queue队列，方法随时可能会被终止，为对象创造一次重生的机会
```java
public static Finalization finalization
```

### 你了解的前沿GC有哪些
- Epsilon GC：JDK11，A NoOp Garbage Collector（没有操作的垃圾收集器）
- ZGC：JDK11，与标记对象的传统算法相比，ZGC在指针上做标记，在访问指针时加入Load Barrier（读屏障），比如当对象正被GC移动，指针上的颜色就会不对，这个屏障就会先把指针更新为有效地址再返回，也就是，永远只有单个对象读取时有概率被减速，而不存在为了保持应用与GC一致而粗暴整体的Stop The World