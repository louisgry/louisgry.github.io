---
title: 《玩转Spring Data》
date: 2019-08-14 15:41:34
categories: Daily
tags: Spring
description: 使用传统方式访问数据库、Spring Data JPA
---

https://github.com/louisgry/Programming/tree/master/springdata
<!-- more -->
- Ch1：课程介绍
    - Spring Data：简化数据库的访问方式
    - 包含多个子项目：Spring Data JPA、MongoDB、Redis、Solr
- Ch2：使用传统方式访问数据库
    - 学习新技术之前，先学习为什么要用这个技术
    - 传统方式：JDBC、Spring JdbcTemplate
    - JDBC：`Connection、Statement、ResultSet`、Test Case（单元测试）
    - Maven：GAV（GroupId、ArtifactId、Version）
    - 使用JDBC
        1. 新建Maven项目，配置db.properties
        2. 创建数据库和表（spring_data库：student表）
        3. 开发JDBCUtil工具类：util下编写JDBCUtil类
        4. 建立对象模型和DAO层开发：StudentDAO、StudentDAOImpl
    - 使用Spring JdbcTemplate
        1. Maven依赖
        2. 配置beans.xml（DataSource & JdbcTemplate注入）
        3. 编写接口实现类StudentDAOSpringJdbcImpl
        4. Test Case：DataSourceTest、StudentDAOSpringJdbcImplTest
    - 弊端分析
        - DAO很多代码
        - DAOImpl很多重复代码
        - 开发分页功能还需重新封装
- Ch3：Spring Data快速入门
    - 使用Spring Data
        1. Maven依赖（spring-data-jpa、hibernate-entitymanager）
        2. 配置beans-new.xml（一共5个步骤）
        3. domain中添加Employee实体类
        4. 编写SpringDataTest测试类（testEntityManagerFactory方法）
        5. repository中添加EmployeeRepository接口
        6. 编写EmployeeRepositoryTest测试类（testFindByName方法）
    - Spring Data优点
        - 只需要写Interface，不用写实现类（findByName）
- Ch4：Spring Data JPA进阶
    - Repository接口讲解
        - Repository接口（标记接口，里面是空的）是Spring Data的核心接口，不提供任何方法
        - 1.继承：`public interface Repository<T, ID extends Serializable>{}`
        - 2.注解：`@RepositoryDefinition(domainClass=Employee.class, idClass=Integer.class)`
    - Repository子接口
        - CrudRepository
        - PageAndSortingRepository
        - JpaRepository
    - Repository查询方法定义规则和使用
        - 命名规则：findByNameInOrAgeLessThan
    - Query注解
        - 方式一：
        ```java
        @Query("select o from Employee o where o.name=?1 and o.age=?2")
        ```
        - 方式二：
        ```java
        @Query("select o from Employee o where o.name=:name and o.age=:age")
        public List<Employee> queryParams2(@Param("name")String name, @Param("age")Integer age);
        ```
        - 方式三：
        ```java
        @Query(nativeQuery = true, value = "select count(1) from employee")
        ```
    - 更新操作整合事务使用
        - `@Modifying`注解
        - 事务一般是在Service层
        - `@Transactional`添加事务
- Ch5：Spring Data JPA高级
    - CrudRepository
        - repository下新建EmployeeCrudRepository接口
        - service下编写save方法
        - test下测试save方法 
    - PageAndSortingRepository
        - repository下新建EmployeePageAndSortingRepository接口
        - test下测试testPage方法 
        - test下测试testPageAndSort方法 
    - JpaRepository
        - repository下新建EmployeeJpaRepositoryRepository接口
        - test下测试testFindOne方法 
    - JpaSpecificationExecutor
        - repository下新建EmployeeJpaSpecificationExecutorRepository接口（继承JpaRepository和JpaSpecificationExecutor）
        - test下测试testQuery方法（age>50，id降序，分页） 
- Ch6：课程总结
    - SpringData支持关系型和非关系型数据库
    - 传统方式访问数据库
    - Spring Data入门及高阶
