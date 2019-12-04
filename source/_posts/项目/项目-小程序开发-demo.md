---
title: 项目：小程序开发-demo
date: 2019-06-27 11:40:43
categories: 项目
tags: 
- 小程序开发
description: 小程序开发：区域信息列表的增删改查
---
https://github.com/louisgry/campus-hobby-group

## 需求和技术
- 需求：区域信息列表的增删改查
- 技术：SpringBoot1.5.9、MyBatis1.3.1、MySQL8.0.17

## 数据库设计
- area表： area_id、area_name、priority、create_time、update_time
- 数据库存储引擎：InnoDB

## 项目开发
### Dao
- 配置
    - resources：`mybatis-config.xml`
    - config.dao：`DataSourceConfiguration`类，`@Configuration`
    - resources：application.yml配置mybatis
    - config.dao：`SessionFactoryConfiguration `类，`@Configuration`
- dao
    - dataobject：Area实体类
    - dao：AreaDao接口
- mapper
    - resources.mapper：`AreaDao.xml`
    - 编写sql：增删改查
- test
    - 点击AreaDao按ALT+ENTER，生成test（JUnit4）
    - 取消@Autowired AreaDao报错提示：Editor-Inspection-Spring-Spring Core-Code-Autowiring for Bean class->Severity设为Warning
    - lombok无效：修改为1.18.8 provided版本

### Service
- 配置
    - config.service：`TransactionManagementConfiguration`事务管理类，`@Configuration`
- service
    - service：AreaService接口
    - service.impl：接口实现，事务场景，`@Service`
- exception
    - enums：ResultEnum枚举类，`@Getter`
    - exception：AreaException异常类

### Controller
- controller
    - controller：AreaController
- handle
    - 统一异常处理类：`@ControllerAdvice、@ExceptionHandler、@ResponseBody`
- swagger
    - 根目录：XiaoquSwagger类
    - controller：`@ApiOperation`