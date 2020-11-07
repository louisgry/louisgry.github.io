---
title: Spring Boot-imooc
date: 2019-08-15 15:41:34
categories: 系统设计
tags: 
- Spring
description: Spring Boot入门、进阶、Spring Boot 2.0
---

## SpringBoot入门
https://github.com/louisgry/Programming/tree/master/Spring/springboot
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


## SpringBoot进阶
https://github.com/louisgry/Programming/tree/master/Spring/springboot
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

## SpringBoot2.0
- Ch1：
    - Spring Boot 2.0 新特性
        - 编程语言：Java8+、Kotlin
        - 底层框架：Spring Framework  5.0x
        - 全新特性：Web Flux
    - Web Flux
        - 函数式编程：Java 8 Lambda
        - 响应式编程：Reactive Streams
        - 异步编程：Servlet 3.1或Asyc NIO
- Ch2：Spring Boot 项目
    - 场景说明
        - 定义用户模型，属性：用户Id、名称
        - 客户端发送POST请求，创建用户（Web MVC）
        - 客户端发送GET请求，获取所有用户（Web Flux）
    - Web Flux
        - 项目：`Reactive Web`
        - 依赖：`spring-boot-starter-webflux`
        - Console：PID、Netty、RequestMappingHandlerMapping
    - domian
        - 新建User类
        - `int id;`
        - `String name;`
        - `toString()`
    - repository
        - UserRepository
        - `@Repository`
        ```java
        // 采用内存型的存储方式 -> Map
        private final ConcurrentMap<Integer, User> repository = new ConcurrentHashMap<>();
        // Id 自增长
        private final static AtomicInteger idGenerator = new AtomicInteger();
        // 保存用户对象
        public boolean save(User user){
            Integer id = idGenerator.incrementAndGet(); // 从1开始
            user.setId(id);
            return repository.put(id, user) == null;
        }
        ```
    - controller
        - 新建UserController：`@RestController`
        ```java
        private final UserRepository userRepository;
        @Autowired
        public UserController(UserRepository userRepository){
            this.userRepository = userRepository;
        }

        @PostMapping("/person/save")
        public User save(@RequestParam String name){
            User user = new User();
            user.setName(name);
            if(userRepository.save(user)){
                // 格式化输出System.out.printf
                System.out.printf("用户对象：%s 保存成功！\n", user);
            }
            return user;
        }
        ```
- Ch2：Web Flux
    - Reactive
        - Reactor是异步NIO，是Reactive的一种实现
    - 两个概念
        - Flux：0-N个对象集合
        - Mono：0-1个对象集合，类似Optional避免对象为空（但是flux和mono异步处理）
        - Flux和Mono都是Publisher
    - config
        - 新建RouterFunctionConfiguratioin：`@Configuration`
        - RouterFunction、ServerResponse
        - Servlet
            - 请求接口：ServletRequest 或 HttpServletRequest
            - 响应接口：ServletResponse 或 HttpServletResponse
        - Spring 5.0 重新定义了接口
            - 请求接口：ServerRequest
            - 响应接口：ServerResponse
            - 即可支持Servlet规范，也支持自定义规范：如Netty Web Server
        - 注入方式
            - setter注入
            - 注解注入
            - 构造器注入
        ```java
        @Bean
        @Autowired
        public RouterFunction<ServerResponse> personFindAll(UserRepository userRepository){
            Collection<User> users = userRepository.findAll();
            return RouterFunctions.route(RequestPredicates.GET("/persion/list-webflux"),
                    request -> {
                        // 返回所有的用户对象
                        Mono<ServerResponse> responseMono = null;
                        Flux<User> userFlux = Flux.fromIterable(users);
                        return ServerResponse.ok().body(userFlux, User.class);
                    });
        }
        ```
- Ch3：多模块
    - 项目下新建module
    - 子模块的pom的`parent`修改为父模块的
        - 模型层：model（domain：User）
        - 持久层：persistence（repository：UserRepository）
        - 表示层：view（controller：UserController）
    - 依赖关系：web -> persistence -> model
- Ch4：Spring Boot三大特性
    - 自动装配
    - 嵌入式容器
    - DevOps


