### 1.SpringApplication

修改启动打印日志信息

```yaml
spring:
  main:
    log-startup-info: true
```

#### 1.1 startup failure

> FailureAnalyzers   定制自己的失败分析器
>
> 配置文件开启debug模式
>
> java -jar myproject-0.0.1-SNAPSHOT.jar --debug

#### 1.2 Lazy Initialization

>**spring.main.lazy-initialization**=true

懒加载 ，会隐藏某些启动问题和jvm内存的问题，不建议使用。

#### 1.3. Customizing the Banner

>类路径下放一个banner.txt文件
>
>SpringApplication.setBanner(…) 也可以设置

#### 1.4. Customizing SpringApplication

```java
public static void main(String[] args) {
    SpringApplication app = new SpringApplication(MySpringConfiguration.class);
    app.setBannerMode(Banner.Mode.OFF);
    app.run(args);
}
```

通过自己创建的SpringApplication对象来定制。

#### 1.5. Fluent Builder API

```java
new SpringApplicationBuilder()
        .sources(Parent.class)
        .child(Application.class)
        .bannerMode(Banner.Mode.OFF)
        .run(args);
```

通过SpringApplicationBuilder构建SpringApplication并运行程序。

#### 1.6. Application Availability

> ApplicationAvailability 注册到自己的bean中使用，可以检查健康状况

##### 1.6.1. Liveness State

##### 1.6.2. Readiness State

> CommandLineRunner
>
> ApplicationRunner
>
> @PostConstruct

启动期间要执行的任务可以通过实现上面的接口通过回调的方式实现

##### 1.6.3. Managing the Application Availability State

> 应用的可用性状态管理  通过事件发布机制监听或者发布健康状态
>
> ApplicationAvailability

#### 1.7. Application Events and Listeners

1. 由于必须要在spring上下文加载之前注册监听器，不能使用@Bean的方式，通过下面的方式

   > SpringApplication.addListeners()
   >
   > SpringApplicationBuilder.listeners
   >
   > org.springframework.context.ApplicationListener=com.example.project.MyListener  **spring.factories文件中**

2. 事件发布顺序

   > 1. ApplicationStartingEvent   除了listeners and initializers的任何步骤之前
   > 2. ApplicationEnvironmentPreparedEvent  上下文创建之前，Environment准备好
   > 3. ApplicationContextInitializedEvent  上下文准备好，bean定义没加载
   > 4. ApplicationPreparedEvent     bean定义被加载
   > 5. ApplicationStartedEvent      refresh调用完成，*Runner接口的回调函数还没执行
   > 6. AvailabilityChangeEvent
   > 7. ApplicationReadyEvent   *Runner的回调函数执行完成
   > 8. AvailabilityChangeEvent
   > 9. ApplicationFailedEvent

3. 其他的事件发布

   > 上下文refresh过程中发布的
   >
   > webserver相关的

#### 1.8. Web Environment

> NONE				AnnotationConfigApplicationContext
>
> SERVLET			AnnotationConfigServletWebServerApplicationContext
>
> REACTIVE			AnnotationConfigReactiveWebServerApplicationContext

#### 1.9. Accessing Application Arguments

> ApplicationArguments  
>
> 可以通过自动注入获取ApplicationArguments  的对象，然后获取参数

#### 1.10. Using the ApplicationRunner or CommandLineRunner

**要使用@Component或者@Configuration注解后才能生效**

#### 1.11. Application Exit



#### 1.12. Admin Features

### 2. Externalized Configuration

> spring-boot有一套规则允许重写配置的属性值，具体的规则参照官网
>
> 常用的一般是命令行-->(profile)外部配置文件-->(profile)内部配置文件-->外部配置文件-->内部配置文件
>
> 支持通配符匹配多个文件

####  2.1. Configuring Random Values