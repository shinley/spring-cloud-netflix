# 服务发现：Eureka客户端

服务发现是基于微服务架构的关键原则之一。 尝试配置每个客户端或某种形式的约定可能非常困难，而且非常脆弱。 Eureka是Netflix提供的，可以实现服务发现服务器端和客户端的组件。 可以将服务器配置和部署为高可用性，每个服务器将注册服务的状态复制到其他服务器。

### 如何添加Eureka客户端依赖

要在项目中包含Eureka Client，可以添加group Id为`org.springframework.cloud`和artifact Id 为`spring-cloud-starter-eureka`的起步依赖。 有关使用当前Spring Cloud发布版本设置构建系统的详细信息，请参阅“Spring Cloud Project”页面。

### 注册到Eureka {#registering-with-eureka}

当一个客户端注册到Eureka时， 它需要提供自已一些元数据信息，比如主机和端口，还有健康检查的URL， 主页等等。Eureka从每个服务实例接收心跳消息。如果在配置的时间段内接收心跳消息失败，这个实例就会从注册中心被正常的移除。

Eureka客户端的例子：

```
@Configuration
@ComponentScan
@EnableAutoConfiguration
@EnableEurekaClient
@RestController
public class Application {

    @RequestMapping("/")
    public String home() {
        return "Hello world";
    }

    public static void main(String[] args) {
        new SpringApplicationBuilder(Application.class).web(true).run(args);
    }
}
```

\(以上例子，完全是一个标准的Spring boot应用程序\) 在这个例子中我们使用@EnableEurekaClient注解，但只有Eureka可以使用`@EnableDiscoveryClient`注解。 还需要一些配置来定位Eureka服务器。 例如：

application.yml

```
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
```

where "defaultZone" is a magic string fallback value that provides the service URL for any client that doesn’t express a preference \(i.e. it’s a useful default\).

（**以上不知如何翻译，所以保留原文**）

从环境（Environment）中获取的默认应用程序名称（服务ID）、虚拟主机和非安全端口分别是`${spring.application.name}, ${spring.application.name}`和`${server.port}`

@EnableEurekaClient使应用程序即是一个Eureka实例（它可以自已注册自已） 又一个客户端，（它可以从注册中心查询定位其它服务）。 它的实例的行为由`eureka.instance.*`配值驱动。但是如果你保证你的程序中有一个`spring.application.name`

（它默认是Euraka的service ID， 或者VIP），那么默认的配值已经很好了。

查看[EurekaInstanceConfigBean](https://github.com/spring-cloud/spring-cloud-netflix/tree/master/spring-cloud-netflix-eureka-client/src/main/java/org/springframework/cloud/netflix/eureka/EurekaInstanceConfigBean.java) 和 [EurekaClientConfigBean](https://github.com/spring-cloud/spring-cloud-netflix/tree/master/spring-cloud-netflix-eureka-client/src/main/java/org/springframework/cloud/netflix/eureka/EurekaClientConfigBean.java) 以获得更多可配置的选项的详细信息

### 认证Eureka服务

如果其中一个`eureka.client.serviceUrl.defaultZone`URL中嵌入了凭证（如：[http://user:password@localhost:8761/eureka），则HTTP基本身份验证将自动添加到您的eureka客户端。对于更复杂的需求，您可以创建一个类型为\`DiscoveryClientOptionalArgs\`的@Bean，并将\`ClientFilter\`实例注入到其中，所有这些都将应用于从客户端到服务器的调用。](http://user:password@localhost:8761/eureka），则HTTP基本身份验证将自动添加到您的eureka客户端。对于更复杂的需求，您可以创建一个类型为`DiscoveryClientOptionalArgs`的@Bean，并将`ClientFilter`实例注入到其中，所有这些都将应用于从客户端到服务器的调用。)

> **注意**：
>
> 由于Eureka的限制，不可能支持每个服务器的基本身份验证凭据，因此只有第一个被找到的被使用。

## 状态页和健康指标

Eureka实例的状态页面和运行状况指示器，默认情况下分别为“/ info”和“/ health”，它们是Spring Boot Actuator应用程序中有用端点的默认位置。 如果您使用非默认上下文路径或servlet路径（例如`server.servletPath = /foo`）或管理端点路径（例如`management.contextPath = /admin`），则需要更改这些，即使是执行程序应用程序。 例：

application.yml

```
eureka:
  instance:
    statusPageUrlPath: ${management.context-path}/info
    healthCheckUrlPath: ${management.context-path}/health
```

这些链接显示在客户端的元数据中，并在某些情况下用于决定是否将请求发送到应用程序，因此正确的配置它们是非常有用的。

### 注册安全的应用程序

如果你的应用程序想要通过HTTPS调用，您可以在`EurekaInstanceConfig`配置中，通过`eureka.instanc.[nonSecurePortEnabled，securePortEnabled] = [false，true]`设置两个标志。 这将使Eureka发布的实例信息明确使用安全通信机制。 Spring Cloud `DiscoveryClient`将始终返回一个https://...; 以这种方式配置的服务的URI，以及Eureka（本机）实例信息将具有安全的健康检查URL。

由于Eureka在内部工作的方式，它仍然会发布非安全状态和主页URL，除非你明确地覆盖。 也可以使用占位符来配置eureka实例网址，例如

application.yml

```
eureka:
  instance:
    statusPageUrl: https://${eureka.hostname}/info
    healthCheckUrl: https://${eureka.hostname}/health
    homePageUrl: https://${eureka.hostname}/
```

（注意`${eureka.hostname}`是仅在最新版本的Eureka中可用的本地占位符，你也可以使用Spring占位符实现同样的功能，例如使用

`${eureka.instance.hostName}）`。

> **注意：**
>
> 如果应用程序在代理后面运行，并且SSL终止在代理中（例如，如果你在Cloud Foundry或其他平台上运行作为服务），那么你需要确保代理“forwarded”的报头被应用程序截获和处理。Spring Boot应用程序中的嵌入式Tomcat容器会自动执行“X-Forwarded-\\*”头的显式配置。如果弄错了这个标志，你的应用程序本身所呈现的链接将是错误的（错误的主机，端口或协议）。

### Eureka的健康检查 {#eureka-health-check}

默认情况下， Eureka使用客户端的心跳决定一个客户端是否“存活”（“UP”状态），

