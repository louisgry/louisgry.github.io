---
title: 《深入理解Java虚拟机》四、类加载机制
date: 2019-11-24 16:24:04
categories: Java
tags: 
- Ebbinghaus
- Java虚拟机
description: 1. 类的生命周期、2. 类加载的过程、3. 类初始化时机、4. 类加载器
---
> 《深入理解Java虚拟机》



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
