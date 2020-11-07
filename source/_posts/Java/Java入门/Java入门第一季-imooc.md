---
title: Java入门第一季-imooc
date: 2019-05-30 15:32:45
categories: Java
tags: Java基础
description: Java初体验、Java基础
---

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
