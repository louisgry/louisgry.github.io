---
title: 数据库系统原理-CSNotes
date: 2019-12-28 23:32:46
categories: 数据库
tags: 
- 数据库原理
description: 事务、并发一致性问题、封锁、隔离级别、多版本并发控制、Next-Key Locks、关系数据库设计理论、ER图
---

# 一、数据库系统原理
## 事务
## 并发一致性问题
## 封锁
## 隔离级别
1. 未提交读（Read Uncommitted）：事务中的修改，即使没有提交，对其他事务也是可见的
2. 提交读（Read Committed）：一个事务所做的修改在提交之前对其他事务是不可见的
3. 可重复读（Repeatable Read）：保证在同一事务中多次读取同样数据的结果是一样的
4. 可串行化（Serializable）：强制事务串行执行，需要加锁实现

## 多版本并发控制
## Next-Key Locks
## 关系数据库设计理论
## ER图


