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

> myconfig.name = ${random.uuid}

只在properties文件中生效

#### 2.2. Accessing Command Line Properties

> --debug=false
>
> 可以通过SpringApplication.setAddCommandLineProperties(false)关闭

#### 2.3. Application Property Files

> 1. /config 目录
> 2. 当前目录
> 3. 类路径下的/config目录
> 4. 类路径根目录

**从上到下查询，后面的可以替换前面的属性值，优先级高**

#### 2.4. Profile-specific Properties

#### 2.5. Placeholders in Properties

```properties
app.name=MyApp
app.description=${app.name} is a Spring Boot application
```

可以使用占位符，引用前面定义过的属性

#### 2.6. Encrypting Properties

#### 2.7. Using YAML Instead of Properties

```java
@ConfigurationProperties(prefix="my")
public class Config {

    private List<String> servers = new ArrayList<String>();

    public List<String> getServers() {
        return this.servers;
    }
}
```

yaml配置文件需要使用@ConfigurationProperties做结构化映射，不能使用@Value。

#### 2.8. Type-safe Configuration Properties

> @ConstructorBinding
>
> @ConfigurationProperties("acme")



> @Configuration(proxyBeanMethods = false) 
>
> @EnableConfigurationProperties(AcmeProperties.class)

通过@EnableConfigurationProperties(AcmeProperties.class)导入映射属性文件的javabean

### 3. Profiles

### 4. Logging

#### 4.1. Log Format

#### 4.2. Console Output

##### 4.2.1. Color-coded Output

#### 4.3. File Output

#### 4.4. Log Levels

#### 4.5. Log Groups

#### 4.6. Custom Log Configuration

#### 4.7. Logback Extensions

##### 4.7.1. Profile-specific Configuration

##### 4.7.2. Environment Properties

### 5. Internationalization

### 6. JSON

#### 6.1. Jackson

#### 6.2. Gson

#### 6.3. JSON-B

### 7. Developing Web Applications

#### 7.1. The “Spring Web MVC Framework”

#### 7.1.1. Spring MVC Auto-configuration

MVC自动配置包含的内容

> ContentNegotiatingViewResolver  ，BeanNameViewResolver
>
> 静态资源和webjar支持
>
> Converter，GenericConverter，Formatter
>
> HttpMessageConverters
>
> MessageCodesResolver
>
> index.html 静态html支持
>
> Favicon 图标支持
>
> ConfigurableWebBindingInitializer

如果需要额外定制自己的MVC属性，实现WebMvcConfigurer接口并使用@Configuration注解，不要@EnableWebMvc注解，会覆盖掉默认的配置。

> you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`.

保持默认配置的情况下定制组件

> If you want to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, and still keep the Spring Boot MVC customizations, you can declare a bean of type `WebMvcRegistrations` and use it to provide custom instances of those components.

#### 7.1.2. HttpMessageConverters

定制自己的HttpMessageConverter

```java
import org.springframework.boot.autoconfigure.http.HttpMessageConverters;
import org.springframework.context.annotation.*;
import org.springframework.http.converter.*;

@Configuration(proxyBeanMethods = false)
public class MyConfiguration {

    @Bean
    public HttpMessageConverters customConverters() {
        HttpMessageConverter<?> additional = ...
        HttpMessageConverter<?> another = ...
        return new HttpMessageConverters(additional, another);
    }

}
```

#### 7.1.3. Custom JSON Serializers and Deserializers

定制json序列化和反序列话

```java
import java.io.*;
import com.fasterxml.jackson.core.*;
import com.fasterxml.jackson.databind.*;
import org.springframework.boot.jackson.*;

@JsonComponent
public class Example {

    public static class Serializer extends JsonSerializer<SomeObject> {
        // ...
    }

    public static class Deserializer extends JsonDeserializer<SomeObject> {
        // ...
    }

}
```

#### 7.1.4. MessageCodesResolver

#### 7.1.5. Static Content





> --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 附加

### 1.主程序类

```java
@SpringBootApplication
```

组成：

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
```

#### @SpringBootConfiguration：

标记为springboot的配置类

#### @EnableAutoConfiguration：

组成：

```java
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
```

##### @AutoConfigurationPackage：

```java
@Import(AutoConfigurationPackages.Registrar.class)
```

注册要扫描的包

##### @Import(AutoConfigurationImportSelector.class)：

```java
public static final String FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories";
```

从上述文件中读取所有自动配置类的全类名。

#### @ComponentScan：

配置扫描包的位置，可以配置过滤规则

### 2.配置文件

> application.properties
>
> application.yaml

#### 2.1yaml基本语法

```yaml
spring:
  application:
    name: spring-boot-learn
```

key value键值对，冒号后面有空格，层级通过缩进控制，同缩进的层级相同，大小写敏感。

#### 2.2值的写法

##### 字面量

默认字符串不需要单引号或者双引号

双引号：不会转义字符串，\n输出就是换行

单引号：会转义字符串，\n输出还是\n

##### 对象 map

普通写法：

```yaml
user: 
  name: gaojian
  sex: nan
```

行内写法：

```yaml
person: {name: gaojian,sex: nan}
```

##### 数组

普通写法：

```yaml
pets: 
  - cat
  - dog
  - pig
```

行内写法：

```yaml
people: [man,women,boy,girl]
```

#### 2.3将yaml中配置属性映射到类的属性

```java
/**
 * 使用@ConfigurationProperties将yaml配置文件中的值映射到此类的属性上
 * prefix代表在yaml的前缀值
 * 必须使用@Component注册为Spring的组件
 * 默认是从全局配置文件获取值
 */
@Component
@ConfigurationProperties(prefix = "person")
```

```yaml
person:
  name: gaojian
  age: 20
  death: false
  maps:
    key1: value1
    key2: value2
  list:
    - demo
    - test
    - prod
  dog:
    name: mydog
    age: 12
```

```properties
person.name=gaojian
person.age=22
person.death=false
person.maps.key1=value1
person.maps.key2=value2
person.list=a,b,c
person.dog.name=ogs
person.dog.age=3
```

@ConfigurationProperties和@Value的对比

|                             | @ConfigurationProperties | @Value   |
| --------------------------- | ------------------------ | -------- |
| 普通注入功能                | 批量注入属性值           | 单个注入 |
| 松散绑定（-后的转大写）     | 支持                     | 不支持   |
| SpEL                        | 不支持                   | 支持     |
| JSR303校验                  | 支持                     | 不支持   |
| 复杂类型（比如直接映射map） | 支持                     | 不支持   |

JSR303校验使用方式：

> @Validated

#### 2.4从指定配置文件获取值（只支持properties文件）

```java
@Component
@PropertySource("classpath:user.properties")
@ConfigurationProperties(prefix = "user")
```

#### 2.5导入Spring配置文件

```java
@ImportResource
```

### 3.profile

#### 3.1多profile文件

激活方式1：

```yaml
spring:
  profiles:
    active: dev
```

#### 3.2yaml文档块

```yaml
---
server:
  port: 8081
spring:
  profiles: dev
```

#### 3.3命令行方式指定参数

#### 3.4虚拟机参数

#### 3.5默认配置文件加载优先级

> file:./config 
>
> file:./
>
> classpath:/config
>
> classpath:/

优先级由高到低，高优先级覆盖低优先级

#### 3.6改变默认配置文件位置

```yaml
spring:
  config:
    location: classpath:/location
```

#### 3.7加载优先级

![image-20200720211037641](.\images\image-20200720211037641.png)

### 4.日志

日志切换 日志配置，详情参照日志

### 5.web开发

#### 5.1.springboot对静态资源映射规则

1）webjars映射路径

/webjars/**  映射的路径 classpath:/META-INF/resources/webjars

2）/**映射的路径

```
"classpath:/META-INF/resources/",
"classpath:/resources/",
"classpath:/static/", 
"classpath:/public/",
"/"
```

3）欢迎页映射

```
"classpath:/META-INF/resources/",
"classpath:/resources/", 
"classpath:/static/", 
"classpath:/public/"
```

在上述位置的index.html