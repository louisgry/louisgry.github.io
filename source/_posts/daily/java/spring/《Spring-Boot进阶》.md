---
title: 《Spring Boot进阶》
date: 2019-08-18 00:13:03
categories: Daily
tags: Spring
description: 表单验证、AOP处理请求、统一异常处理、单元测试
---
https://github.com/louisgry/Programming/tree/master/springboot
<!-- more -->
- Ch1：课程介绍
    - Web进阶
        1. `@Valid`表单验证
        2. AOP处理请求
        3. 统一异常处理
        4. 单元测试
- Ch2：表单验证
    - Girl对象： `@min`
    ```java
    @Min(value = 18, message = "未成年少女禁止进入")
    private Integer age;
    ```
    - GirlController：`@Valid`、传对象Girl girl
    ```java
    public Girl girlAdd(@Valid Girl girl, BindingResult bindingResult){
        if(bindingResult.hasErrors()){
            System.out.println(bindingResult.getFieldError().getDefaultMessage());}
    ```
- Ch2：AOP处理请求
    - AOP是一种编程范式
        - 与语言无关，是一种编程思想，还有OOP和面向过程
        - 不只是Java有，还有C#等也有AOP
        - 切面：相同的功能封装成为一个通用的模块（将通用逻辑从业务中分离出来）
        - spring不会初始化controller的构造方法
    - aspect
        - 添加依赖`spring-boot-starter-aop`
        - 新建HttpAspect类：`@Aspect`、`@Component`
        ```java
        @Pointcut("execution(public * com.imooc.springboot.controller.GirlController.*(..))")
        public void log() {}
        
        @Before("log()")
        public void doBefore(JoinPoint joinPoint){}
        ```
- Ch2：日志框架
    - `org.slf4j.Logger`
    ```java
    private final static Logger logger = LoggerFactory.getLogger(HttpAspect.class);
    ```
- Ch2：统一异常处理
    - exception
        - 新建GirlException类：`extends RuntimeException`（Spring只对RuntimeException作事务回滚）
    - handle
        - 新建ExceptionHandle类：`@ControllerAdvice`、`@ExceptionHandler`、`@ResponseBody`
    - enums
        - ResultEnum枚举类：构造方法和Getter方法
- Ch3：单元测试
    - 对controller做单元测试：`@AutoConfigureMockMvc`
    ```java
    @Autowired
    private MockMvc mockMvc;

    @Test
    public void girlList() throws Exception {
        // 测试status
        mockMvc.perform(MockMvcRequestBuilders.get("/girls"))
                .andExpect(MockMvcResultMatchers.status().isOk());
        //测试content
        mockMvc.perform(MockMvcRequestBuilders.get("/girls"))
                .andExpect(MockMvcResultMatchers.content().string("abc"));
    }
    ```
    - Maven打包时跳过单元测试：`mvc clean package -Dmaven.test.skip=true`