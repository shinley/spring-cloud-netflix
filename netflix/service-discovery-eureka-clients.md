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

\(以上例子，完全是正常的Spring boot应用程序\) 在这个例子中我们使用@EnableEurekaClient注解，但只有Eureka可以使用`@EnableDiscoveryClient`注解。 还需要一些配置来定位Eureka服务器。 例如：

application.yml

```
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
```

where "defaultZone" is a magic string fallback value that provides the service URL for any client that doesn’t express a preference \(i.e. it’s a useful default\). 

其中的“defaultZone"是一个神奇的可回退的字符串值， 它为那些没有指定XXx的客户端，提供了服务的URL。

从环境（Environment）中获取的默认应用程序名称（服务ID）、虚拟主机和非安全端口分别是`${spring.application.name}, ${spring.application.name}`和`${server.port}`



