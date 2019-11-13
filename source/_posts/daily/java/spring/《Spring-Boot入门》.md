---
title: 《Spring Boot入门》
date: 2019-08-16 00:12:42
categories: Daily
tags: Spring
description: 项目配置、Controller、数据库操作、事务
---
https://github.com/louisgry/Programming/tree/master/springboot
<!-- more -->
- Ch1：课程引导
    - Spring Boot：编码、配置、部署、监控（作基础架构）
        - 1.化繁为简、简化配置
        - 2.是下一代框架
    - SpringBoot是SpringMVC的升级版
    - 微服务：SpringCloud是基于SpringBoot构建的
- Ch3：第一个Spring Boot应用
    - `Spring Boot 2.1.3`：sping-boot-starter-web
    - localhost:8080/hello
    - 本地使用maven运行服务
        - 方法一：项目目录下，`mvn spring-boot:run`
        - 方法二：先项目打包`mvn clean package`，然后启动java程序`java -jar target/springboot-0.0.1-SNAPSHOT.jar`
- Ch4：项目属性配置
    - 配置推荐使用application.yml
    ```
    server:
      port: 8081
      servlet:
        context-path: /springboot
    ```
    - 配置变量
    ```
    minMoney: 1
    description: "最小金额${minMoney}元"
    ```
    - 注解使用
        - `@Value`
        - `@Component、@ConfigurationProperties`
    - 使用对象配置
    ```
    limit:
      minMoney: 2
      description: "最小金额${limit.minMoney}元"
    ```
    - 多环境配置：划分配置为开发dev、生产pro
        - application-dev.yml、application-pro.yml
        - application.yml中配置使用
        ```
        spring:
          profiles:
            active: dev
        ```
        - 项目发布使用pro：`java -jar -Dspring.profiles.active=pro target/springboot-0.0.1-SNAPSHOT.jar`
- Ch5：Controller
    - `@Controller`：处理http请求
        - 返回数据需要使用模板：`spring-boot-starter-thymeleaf`
    - `@RestController`：Spring4之前返回原来json需要@ResponseBody配合@Controller
        - `@GetMapping`：返回json
        - `ctrl+p`：显示提示
        - `@PostMapping`：使用POST请求
        - 需要使用工具：PostMan    
    - `@RequestMapping`：配置url映射
        - `@RequestMapping`：既支持GET又支持POST（不推荐）
        - `@PathVariable`：say/100 （@GetMapping("/say/{id}")）
        - `@RequestParam`：say/?id=100
            - PostMan通过Body传参：`x-www-form-urlencoded`
- Ch6：数据库操作
    - JPA（Java Persistence API）：对象持久化标准，Hibernate实现
        - 配置文件application.yml：注意`username、url、serverTimezone、MySQL5Dialect`
    - RESTful API设计
        - GET  /luckymoneys/list 获取红包列表
        - POST /luckymoneys/send 创建一个红包（发红包）
        - GET  /luckymoneys/find 通过id查询红包
        - POST  /luckymoneys/receive  通过id更新红包（领红包）
    - dataobject
        - Luckymoney类：`@Entity`
    - repository
        - LuckymoneyRepository接口：`extends JpaRepository<Luckymoney, Integer>`
    - controller
        - LuckymoneyController类
        - `@RestController`：开发REST服务
        - `@Autowired`：自动装配
        - `@GetMapping`：返回json
        - error：found Optional（后面加`.orElse(null)`）
        ```java
    Optional<Luckymoney> optional = luckymoneyRepository.findById(id);
        if(optional.isPresent()){
            Luckymoney luckymoney = optional.get();
            luckymoney.setId(id);
            luckymoney.setConsumer(consumer);
            return luckymoneyRepository.save(luckymoney);
        }
        ```
    - url：`yml里的端口+context-path+RequestMapping+GetMapping路径` 
    - hibernate_sequence：用来存id，管理数据库的
- Ch7：事务
    - service
        - LuckymoneyService类：`@Service`
        - 事务：`@Transacational` （只有都成功才会插入到数据库中）
        - MySQL只有InnoDB引擎支持事务