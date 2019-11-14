---
title: Java：Java基础-imooc
date: 2019-11-13 20:32:45
categories: Java
tags: Java基础
description: 第一季（Java初体验、Java基础）、第二季（类和对象、封装、继承、多态）、第三季（异常、常用类、集合框架）
---

## 《Java入门第一季》
### Java初体验
<!-- more -->
- 简介
    - 诞生：1995（Sun）
    - 收购：2009（Oracle）
    - Java 8：2014
- 版本（J2EE，2005年Java6前）
    - Java SE（面向对象、API、JVM）
    - Java EE（JSP、EJB、服务）
    - Java ME（移动设备、游戏通信）
- 核心概念
    - JVM：Java Virtual Machine（源文件`.java` 编译-> 字节码文件`.class` 解释-> 程序）
    - JRE：Java Runtime Environment 运行时环境
    - JDK：Java Development Kit 开发工具包
- 安装
    - 第一步：装JDK（也就有了JRE和JVM，JDK>JRE>JVM）
    - 第二步：配置环境变量
        - JAVA_HOME：JDK路径 `jdk根目录`
        - PATH：JDK命令文件位置 `...\bin`
        - CLASSPATH：类库文件位置 `.; ...\lib`（.表示当前路径）
    - 第三步：验证（CMD:` java -version`）
- Hello world（记事本写 `javac HelloWorld.java` -> `java HelloWorld`）
    ```java
    public class HelloWorld{
        public static void main(String[] args){ // Small(main), Capital(String)
            System.out.println("Hello World!"); // "" not ''
        }
    }
    ```
- 使用IntellJ IDEA开发Java程序
    1. 创建Java项目
    2. 创建程序包（Package、Class）
    3. 编写Java源程序
    4. 运行Java程序

### Java基础
- 变量和常量
    - 标识符：可`$`开头，不能以数字开头
    - 数据类型：
        - 基本数据类型（数值型、字符型、布尔型）
        - 引用数据类型（类、接口、数组）
    ```java
    float x = 0.5f; // 要加f
    char a = '男'; // 单引号
    boolean b = true; // 关键字boolean，true开头小写
    //String是引用类型
    ```
- 运算符
    - 异或：`^`，与或非：`&& || !`
    - 条件运算符：`? :`
    - 整数除法（向下取整）：`40/9=4, 60/9=6`
    - 字符串比较：`a=="abc"`/`a.equals("abc")`
    - 多个条件：`switch(today){case '一': ... default: ....}`
- 流程控制语句
    - 计算整数的位数
    ```java
    public class HelloWorld{
        public static void main(String[] args){
            int num = 999;
            int count = 0;
            do{
                num /= 10;
                count ++;
            }while(num !=0 );        
            System.out.println("它是个"+count+"位的数！");
        }
    }
    ```
- 数组
    - 整数数组：`int[] scores = {1,2,3,4,5};`
    - 声明数组并分配空间：`String[] abc = new String[5];`
    - Arrays类
    ```java
    import java.util.Arrays;
    Arrays.sort(nums);
    Arrays.toString(nums);
    ```
    - foreach操作数组
    ```java
    for(int score : scores){
        System.out.println(score);
    }
    ```
    - 二维数组
    ```java
    int[][] nums = {{1,2},{3,4}};
    for(int i=0; i<nums.length; i++)
        for(int j=0; j<nums[i].length; j++){
            System.out.println(nums[i][j]);
    }
    ```
- 方法
    - 定义：`public void print()`
    - 调用：`HelloWorld hello = new HelloWorld(); hello.print();`
    - 重载：`public void print(String name)`
    - 随机数（Math类）：`(int)(Math.random()*100)`


## 《Java入门第二季》

### 类和对象
<!-- more -->
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

## 《Java入门第三季》

### 异常
- 概念
    - Throwable是父类，下有Error和Exception两个子类
    - Error：VirtualMachineError（虚拟机错误），ThreadDeath（线程死锁）
    - Exception：RuntimeException（非检查异常），CheckedException 检查异常
        - 非检查异常：NullPointerException, ArrayIndexOutOfBoundsException, ClassCatException, ArithmeticException
        - 检查异常：IOException, SQLException
- 异常处理
    - `try...catch`
    ```java
    try{
        System.out.println("输入年龄：");
        Scanner in = new Scanner(System.in);
        int age = in.nextInt();
        System.out.println(age);
    }catch(InputMismatchException e){
        System.out.println("请输入整数！");
    }
    ```
    - 多重catch块（先子类后父类）及`try...catch...finally`
    ```java
    try{
        Scanner in = new Scanner(System.in);
        int one = in.nextInt();
        int two = in.nextInt();
        System.out.println(one/two);
    }catch(InputMismatchException e){
        System.out.println("请输入整数！");
    }catch(ArithmeticException e){
        System.out.println("除数不能为零！");
    }catch(Exception e){
        e.printStackTrace();
        System.out.println("程序发生异常！");
    }finally{
        System.out.println("finally执行！");
    }
    ```
- 异常抛出
    - throw关键字：throws声明将要抛出何种类型的异常，throw抛出异常
    ```java
    public void compute() throws Exception{
        System.out.println(5/0);
    }
    
    public void compute2(){
        try{
            System.out.println(5/0);
        }catch(Exception e){
            System.out.println(e.getMessage());
        }
    }
    ```
- 自定义异常
    - class 自定义异常类 extends 异常类型
    ```java
    public class DrunkException extends Exception{
        public DrunkException(){
        }
        
        public DrunkException(String message){
            super(message);
        }
    }
    ```
- 异常链
    - 示例
    ```java
    public void test1() throws DrunkException{
        throw new DrunkException("喝酒不开车！");
    }

    public void test2(){
        try {
            test1();
        } catch (DrunkException e) {
            RuntimeException newExc = new RuntimeException("禁止开车");
            newExc.initCause(e);
            // RuntimeException newExc = new RuntimeException(e);
            throw newExc;

        }
    }

    public static void main(String[] args){
        exception hello = new exception();
        // 异常链
        try{
            test2();
        }catch(Exception e){
            e.printStackTrace();
        }
    }
    ```

### 常用类
- 字符串
    - 字符串的不变性
        - `String s1 = "imooc"`不等于`String s2 = new String("imooc")`
        - 每次new一个字符串会产生一个新的对象，即使内容相同对于==运算是fasle（比较内容用equals）
    - String类的常用方法
        - `length()，indexOf(), substring()`
        - 字符串索引不能用s[i]，要用`s.charAt(i)`
    - 文件名和邮箱格式验证
        - 练习
        ```java
        String fileName = "immoc.com.java";
        String email = "louis@imooc.com";
        int index = fileName.lastIndexOf('.'); // 最后一次出现"."的位置
        String prefix = fileName.substring(index+1); // 获取文件后缀名
        int index2 = email.indexOf('@');
        int index3 = email.indexOf('.');
        if(index != -1 && index !=0 && prefix.equals("java")){
            // "."存在，且不能在首位，文件后缀名是java
            System.out.println("文件名正确");
        }
        if(index2 != -1 && index2 < index3){
            // "@"存在，且在"."之前
            System.out.println("邮件格式正确");
        }
        ```
    - StringBuilder与StringBuffer类
        - 为什么：String类具有不可变性，频繁操作字符串时会产生很多额外的临时变量，可以用StringBuilder与StringBuffer类
        - 区别：StringBuffer是线程安全的，StringBuilder无，故性能较好
        ```java
        StringBuilder str1 = new StringBuilder("imooc");
        str1.append(" hello");
        str1.insert(5, 1);
        System.out.println(str1);
        ```

- 包装类
    - 作用：为了让基本数据类型也具备对象的特性
    - 举例：Integer，Float，Double
    - 自动装箱和拆箱机制（JDK1.5）
        - 装箱（把基本类型转换成包装类）
        ```java
        Integer x = new Integer(5); // 手动
        Integer y = 5; // 自动
        ```
        - 拆箱（包装类 -> 基本类型）
        ```java
        int m = x.intValue(); // 手动
        int n = x; // 自动 
        ```
- 基本类型与字符串的转换
    - 基本类型 -> 字符串：
        1. 包装类的toString()
        2. String类的valueOf()
        3. 空字符串""+基本类型
        ```java
        int a = 1;
        String str1 = Integer.toString(a);
        String str2 = String.valueOf(a);
        String str3 = ""+a;
        ```
    - 字符串 -> 基本类型：
        1. 包装类的parseXXX方法
        2. 包装类的valueOf()
        ```java
        int b = Integer.parseInt(str);
        int c = Integer.valueOf(str);
        ```
- Date类
    - 导入：`java.util.Date`
    - 格式化输出
    ```java
    Date d = new Date();
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    String today = sdf.format(d);
    ```
    - 文本转时间
    ```java
    String day = "2014年6月1日 21:05:36";
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
    Date d = sdf.parse(day);
    ```
- Calendar类
    - 示例
    ```java
    Calendar c = Calendar.getInstance();
    // MONTH, DAY_OF_MONTH, HOUR_OF_DAY, MINUTE, SECOND
    int year = c.get(Calendar.YEAR); 
    Date d = c.getTime();
    ```
- Math类
    - 取整：`round()，floor()，ceil()`
    - `random()`：返回[0,1)之间的随机浮点数
    ```java
    int num = (int) Math.random()*10;
    ```

### 集合框架
- 概念：Collection和Map
    - Collection：List (ArrayList)、Queue (LinkedList)、Set (HashSet)
    - Map：HashMap
    - 重要的接口和类：Collection接口、Map接口、Collections工具类、Comparable接口、Comparator接口
- List接口方法
    - 初始化：`List coursesList = new ArrayList()`
    - `add()`：`coursesList.add(new Course("1","Java")`
    - `get()`：`Course tmp = (Course) coursesList.get(0)`
    - `addAll()`：`coursesList.addAll(Arrays.asList({new Course("2","Python"), new Course("3","C++"}))`
    - `set()`：`coursesList.set(0, new Course("4","Algorithm"))`
    - `remove()`：`coursesList.remove(coursesList.get(0)) //.remove(0)`
- Set接口方法
    - 同List的方法
    - 初始化：`Set<Course> courses = new HashSet<Course>()`
    - `add()`：`student.courses.add(new Course("1","Java"))`
- Map接口方法
    - 初始化：`Map<String, Student> students = new HashMap<String, Student>()`
    - `put()`：`students.put(ID, newStu)`（同时可以修改）
    - `remove()`：`students.remove(id)`
    - keySet
        - `keySet()`
        ```java
        Set<String> keySet = students.keySet();
        System.out.println("size: "+students.size());

        for(String id: keySet){
            Student stu = students.get(id);
            if(stu != null){
                System.out.println("Student "+stu.name);
            }
        }
        ```
    - entrySet
        - `getKey()` 和`getValue()` 方法
        ```java
        Set<Entry<String, Student>> entrySet = students.entrySet();
        for(Entry<String, Student> entry: entrySet){
            System.out.println("key: "+entry.getKey() + ", value: "+entry.getValue());
        }
        ```
- 集合框架的查询
    - `contains()`：测试List是否存在某课程（但是不同对象就有不同值）
    ```java
    Course c = (Course) coursesList.get(0);
    System.out.println(coursesList.contains(c)); // true
    Course c2 = new Course(c.id, c.name);
    System.out.println(coursesList.contains(c2)); // false
    ```
    - 解决方法：重写`equals()`方法
    ```java
    public boolean equals(Object obj){
        if(this==obj)
            return true;
        if (obj==null)
            return false;
        if(!(obj instanceof Course))
            return false;
        Course c = (Course) obj;
        if(this.name == null){
            if(c.name == null)
                return true;
            else
                return false;
        }
        else{
            if(this.name.equals(c.name))
                return true;
            else
                return false;
        }
    }
    ```    
    - `contains()`：判断Set中课程是否存在
        - 存在跟List相同的问题，需重写`equals()`和`hashCode()`
        - 但是，有bug！写完还是输出false
    - `indexOf()`：查询位置
        - `coursesList.indexOf(c3)`
    - `contains()`：判断Map中课程是否存在
        - `students.containsKey(id)`
        - `students.containsValue(name)`
- Comparable 和 Comparator 接口
    - `Collections.sort()`：可对基本类型进行排序
    ```java
    List<Integer> integerList = new ArrayList<Integer>();
    List<String> stringList = new ArrayList<String>();
    Collections.sort(integerList);
    Collections.sort(stringList);
    // 问题：不能对Student这样的自定义类排序
    Collections.sort(studentList); // 报错
    ```
    - Comparable接口：默认比较规则（`compareTo()`）
    ```java
    public class Student implements Comparable<Student>{
        // 重写Student类的compareTo方法
        @Override
        public int compareTo(Student o) {
            return this.id.compareTo(o.id);
        }
    }
    // 调用
    Collections.sort(studentList);
    ```
    - Comparator接口：临时比较规则（`compare()`）
    ```java
    // 新建StudentComparator类
    public class StudentComparator implements Comparator<Student> {

        @Override
        public int compare(Student o1, Student o2) {
            return o1.name.compareTo(o2.name);
        }
    }
    // 调用
    Collections.sort(studentList, new StudentComparator());
    ```
