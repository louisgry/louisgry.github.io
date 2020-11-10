---
title: Java入门第二季-imooc
date: 2019-06-05 00:03:17
categories: Java
tags: Java基础
description: 类和对象、封装、继承、多态
---

### 类和对象
- 概念
    - 类是模子，确定对象将会拥有的属性和方法
- 构造方法
    - `new`关键字后面跟的就是**构造方法**（构造方法与类名相同，没有返回值）
    - 若自己定义有参的构造方法，系统不会自动创建无参的构造方法
    ```java
    public class Telphone{
        public Telphone(float newCpu){
            cpu = newCpu;
        }
    }
    ```
- static关键字
    - static静态变量
        - `static String name = "Java";`
        - 调用：`hello.name`（静态变量属于整个类所有）
    - static静态方法
        - 静态方法中可以直接调用静态变量，非静态变量需先实例化，非静态方法也需实例化。
        - 普通方法可以直接调用静态和非静态变量
- 静态初始化块
    - 初始化块(Initialization Block)：[Java：初始化块](https://blog.csdn.net/p106786860/article/details/18838435)
    ```java
    {
        num2 = 74;
    }
    ```
    - 静态初始化块：最先执行（且只加载一次，再次创建对象时不会被再次执行），然后是初始化块，最后是构造方法
    ```java
    static{
        num3 = 83;
    }
    ```
### 封装
- 概念
    - 将类的某些信息隐藏在类内部，不允许外部程序直接访问，而是通过该类提供的方法来实现对隐藏信息的操作和访问
    - 实现步骤：1.修改属性可见性；2.Getter和Setter方法
- Java中的包
    - 包的作用：1.管理Java文件；2.解决同名文件冲突
    - 定义包：`package com.imooc`（全小写）
    - 调用包：`import com.imooc.Telphone`
- 访问修饰符
    - private：本类
    - 默认：本类+同包
    - protected：本类+同包+子类
    - public：本类+同包+子类+其他
- this关键字
    - this关键字代表当前对象（this.属性、this.方法）
    ```java
    public void setScreen(float screen){
        this.screen = screen;
    }
    ```
- Java中的内部类
    - 四种内部类（Inner Class）：成员内部类、静态内部类、方法内部类、匿名内部类
    - 成员内部类：使用类名+this访问同名的外部类变量；使用外部类对象创建内部类对象
    ```java
    public class HelloWorld{
        private String name = "imooc";
        public class Inner {
            String name = "爱慕课";
            public void show() { 
                System.out.println("外部类中的name：" + HelloWorld.this.name);
                System.out.println("内部类中的name：" + name);
            }
        }
        public static void main(String[] args) {
            HelloWorld hello = new HelloWorld (); 
            Inner inn = hello.new Inner() ;
            inn.show();
        }
    }
    ```
    - 静态内部类（static修饰）
        - 不能直接访问外部类的非静态成员
        - 外部类静态成员与内部类成员名称相同时：可用类名.静态成员调用 
        - 创建对象不需要通过外部类对象
    ```java
    public class HelloWorld {
        private static int score = 84;
        public static class SInner {
            int score = 91;
            public void show() {
                System.out.println("访问外部类中的score：" + HelloWorld.score);
                System.out.println("访问内部类中的score：" + score);
            }
        }
        public static void main(String[] args) {
            SInner si = new SInner();
            si.show();
        }
    }
    ```
    - 方法内部类（定义在外部类的方法中）
    ```java
    public class HelloWorld {
        private String name = "爱慕课";
        public void show() { 
            class MInner {
                int score = 83;
                public int getScore() {
                    return score + 10;
                }
            }
            MInner mi = new MInner();
            int newScore = mi.getScore();
            System.out.println("姓名：" + name + "\n加分后的成绩：" + newScore);
        }
        public static void main(String[] args) {
            HelloWorld mo = new HelloWorld();
            mo.show();
        }
    }
    ```

### 继承
- 概念
    - 继承是类与类的一种关系（Java是单继承）
    - 子类拥有父类的所有属性和方法（private除外）
    - 继承的初始化顺序：先父类初始化（先属性，再构造方法）、再子类
- extends关键字
    - class 子类 extends 父类
    ```java
    class Dog extends Animal{
    }
    ```
- 方法重写
    - 要求：名称、参数、返回值都相同
- final关键字
    - final修饰：类、方法、属性
    - final+类：不能被继承
- super关键字
    - 在对象内部使用，代表父类对象（super.age，super.eat()）
    - 子类的构造过程中会调用父类的构造方法（显式调用：`super();`）
- Object类
    - Object类是所有类的父类（两个重要方法：toString()和equals()）
    - toString()：方法重写，实现输出对象的属性
    ```java
    @Override
    public String toString(){
        return "Dog [age="+age+"]";
    }
    System.out.println(dog); // Dog [age=10]
    ```
    - equals()：比较对象的引用是否指向同一块内存地址
        - hashCode()：返回对象的哈希码（对象地址字符串）
        - 方法重写：比较两个对象值是否相同
    ```java
    @Override
    public boolean equals(Object obj){
        if(this==obj)
            return true;
        if(obj==null)
            return false;
        if(getClass() != obj.getClass())
            return false;
        Dog other = (Dog) obj;
        if(age != other.age)
            return false;
        return true;
    }
    if(dog1.equals(dog2)) // ...
    ```
        - 类对象（类型：属性、方法） vs. 类的对象（值）
        

### 多态
- 概念
    - 对象的多种形态（继承是多态的基础）
    - 两种类型
        - 引用多态：父类的引用可以指向本类/子类的对象
        ```java
        Animal obj1 = new Animal();
        Animal obj2 = new Dog();
        // Dog obj3 = new Animal(); // 错误
        ```
        - 方法多态：创建本类对象时，调用本类方法；创建子类对象时，调用子类的方法
        ```java
        obj1.eat(); // 两个都是父类引用创建的对象，但是方法不同
        obj2.eat(); 
        ```
- 引用类型转换
    - 隐式/自动类型转换：小类型到大类型
    ```java
    Animal obj = dog;
    ```
    - 强制类型转换：大类型到小类型
    ```java
    Dog dog = (Dog) obj;
    ```
    - instanceof 避免类型转换的安全性问题
    ```java
    if(obj instanceof Cat){
        Cat cat = (Cat) obj; // obj此时指向Dog    
    }
    ```
- Java中的抽象类
    - 语法定义：`abstract`关键字修饰
    - 作用（应用场景）：限制规定子类必须实现某些方法，但不关注实现细节
    - 使用规则：
        - abstract定义抽象类；
        - 定义抽象方法只有声明；
        - 包含抽象方法的类是抽象类；
        - 抽象类中可以有普通方法，也可以没有抽象方法；
        - 抽象类不能直接创建，但可以定义引用变量
    - 实例
    ```java
    public class Abstract {
    public abstract class Telphone{
        public abstract void call();
    }

    public class CellPhone extends Telphone{
        @override
        public void call(){
            System.out.println("Call by cellphone");
        }
    }

    public class SmartPhone extends Telphone{
        @override
        public void call(){
            System.out.println("Call by smart phone");
        }
    }

    public static void main(String[] args){
        Telphone cp = new CellPhone();
        Telphone sp = new SmartPhone();
        cp.call();
        sp.call();
    }
    ```
- 接口
    - 接口可以理解为一种特殊的类，由全局常量和公共的抽象方法组成（接口定义了类的规范，是多继承的）
    - 定义：`interface`关键字，不是class（一般使用public修饰，以及abstract，接口名一般以I开头）
    ```java
    public abstract interface 接口名 [extends 父类1, 父类2...]
    ```
    - 使用接口：`implements`关键字
    ```java
    class 类名 extends 父类 implements 接口1,接口2...
    ```
    - 匿名内部类（没有名字的内部类）
    ```java
   IPlayGame ip3 = new IPlayGame(){
        public void playGame(){
            System.out.println("Play games");
        }
    };
    ip3.playGame();
    ```
    - 实例
    ```java
    public interface IPlayGame{
        public void playGame();
    }

    public class SmartPhone extends Telphone implements IPlayGame{
        public void call(){
            System.out.println("Call");
        }

        public void playGame(){
            System.out.println("Play games");
        }
    }

    public class PSP implements IPlayGame{
        public void playGame(){
            System.out.println("Play games");
        }
    }

    public static void main(String[] args){
        IPlayGame ip1 = new SmartPhone();
        IPlayGame ip2 = new PSP();
        ip1.playGame();
        ip2.playGame();
        IPlayGame ip3 = new IPlayGame(){
            public void playGame(){
                System.out.println("Play games");
            }
        };
        ip3.playGame();
    }
    ```
- 接口和抽象类有什么区别？[JavaGuide](https://snailclimb.top/JavaGuide/)
    1. 接口的方法默认是public，所有方法在接口中不能有实现，而抽象类可以有非抽象方法
    2. 接口中除了public static final修饰的变量，不能有其他变量，而抽象类可以
    3. 一个类可以实现多个接口，但只能实现一个抽象类
    4. 接口默认是public修饰，抽象方法只是不能被private修饰
    5. 从设计层面说，抽象是类的抽象（是一种模板设计），接口是对行为的抽象（是一种行为的规范）
- UML
    - 统一建模语言（Unified Modeling Language）
    - 常用UML图
        - 用例图（The Use Case Diagram）
        - 类图（The Class Diagram）
    - 建模工具：Viso、PowerDesigner
- Demo
```java
public class People{
    public void say(){
        System.out.println("Hello");
    }
}

public class Chinese extends People{
    public void say(){
        System.out.println("Chinese");
    }
}

public class British extends People{
    public void say(){
        System.out.println("English");
    }
}

public static void main(String[] args){
    People zh = new Chinese();
    People en = new British();
    zh.say();
    en.say();
}
```