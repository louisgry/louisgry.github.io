---
title: Java：Java基础-CS-Notes
date: 2019-11-22 20:32:45
categories: Java
tags: 
- Java基础
description: Java基础的其他知识：<br>
---

# Java基础的其他知识
### 配置Java环境变量
> [Java环境变量PATH和CLASSPATH](https://www.jianshu.com/p/d63b099cf283)
> [Java开发环境不再需要配置classpath！](https://juejin.im/post/5ce67fa1f265da1b6a346d16)

- PATH：`%JAVA_HOME%/jre/bin`
- CLASSPATH：`.;%JAVA_HOME%\lib\tools.jar;%JAVA_HOME%\lib\dt.jar`
	- `.`：表示当前目录
	- `tools.jar`：是系统用来编译类时用到的，即执行javac时
	- `dt.jar`：关于运行环境的类库，主要是swing包
- JAVA_HOME：jdk所在目录，如果上面两个使用绝对路径可以不用配置JAVA_HOME
- 注意：JDK1.5之后，不需要再配置CLASSPATH，只需要配置JAVA_HOME以及PATH即可

### rt.jar
- rt是runtime的缩写，rt.jar包含了开发常用的包，如java.lang、java.util、java.nio、java.sql等

## Java基础的相关问题
### 访问权限控制的权限等级排序
- public > protected > 默认（包访问权限）> private

### 面向对象的特征以及六大原则
1. 单一职责 
2. 开放封闭 
3. 里氏替换 
4. 接口分离 
5. 依赖倒置 
6. 迪米特原则

