### 1.引导类

（1）@SpringBootApplication

​		标明此应用为springboot应用

（2）组成

 * @SpringBootConfiguration

   将引导类声明为配置类

* @EnableAutoConfiguration

  开启自动配置功能

* @ComponentScan

  组件扫描位置配置

### 2.测试类

@SpringBootTest

表明启动测试功能时添加spring boot功能，加载spring容器等功能。

@WebMvcTest

web测试需要的注解

代码示例：**详细内容后期详细了解**

方式一：

```java
package tacos;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;
//静态导入
import static org.hamcrest.Matchers.containsString;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.view;

@WebMvcTest(HomeController.class)
public class HomeControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testHomePage() throws Exception {
        mockMvc.perform(get("/"))//设置请求路径
                .andExpect(status().isOk())//预期结果是200
                .andExpect(view().name("home"))//预期返回试图名称home
                .andExpect(content().string(containsString("Welcome to...")));//预期返回的页面包含字符串Welcome to...
    }
}
```

方式二：

```java
import com.dareway.websocket.server.WebsocketServerApplication;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT,classes = WebsocketServerApplication.class)
@AutoConfigureMockMvc
public class WebClientTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    public void testOnlineCount() throws Exception {
        mockMvc.perform(get("/onlineClient/showAll")
                .header("appId","digitalbox")
                .header("appToken","3a9e82cf29124d30ac2be28b0b58c7e5")
                .header("clientId","user"))
                .andExpect(status().is(200));
    }
}
```

**WebSocket单元测试问题**

```java
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
//websocket需要加入web环境，但是正常情况下测试不会
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class WebsocketServerApplicationTests {

    @Test
    void contextLoads() {

    }

}
```



### 3.Devtools配置

yaml配置：

```yaml
spring:
  devtools:
    restart:
      enabled: true
```

idea配置：

（1）勾选项目自动build

![image-20200709233730482](.\images\image-20200709233730482.png)

（2）ctrl + shift + alt + /,选择Registry,勾上 Compiler autoMake allow when app running

![image-20200709233948708](.\images\image-20200709233948708.png)

（3）修改之后使用快捷键

​	Ctrl + S 保存 Alt + F9 重新编译

（4）**thymeleaf的自动重启参考雷锋杨教程**

### 4.spring官方文档下载地址

http://repo.springsource.org/libs-release-local/org/springframework/spring