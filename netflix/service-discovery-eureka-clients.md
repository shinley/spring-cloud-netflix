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

### 认证Eureka服务 {#auth-with-eureka}

如果其中一个`eureka.client.serviceUrl.defaultZone`URL中嵌入了凭证（如：[http://user:password@localhost:8761/eureka），则HTTP基本身份验证将自动添加到您的eureka客户端。对于更复杂的需求，您可以创建一个类型为\`DiscoveryClientOptionalArgs\`的@Bean，并将\`ClientFilter\`实例注入到其中，所有这些都将应用于从客户端到服务器的调用。](http://user:password@localhost:8761/eureka），则HTTP基本身份验证将自动添加到您的eureka客户端。对于更复杂的需求，您可以创建一个类型为`DiscoveryClientOptionalArgs`的@Bean，并将`ClientFilter`实例注入到其中，所有这些都将应用于从客户端到服务器的调用。)

> **注意**：
>
> 由于Eureka的限制，不可能支持每个服务器的基本身份验证凭据，因此只有第一个被找到的被使用。

## 状态页和健康指标 {#status-page-and-health-indicator}

Eureka实例的状态页面和运行状况指示器，默认情况下分别为“/ info”和“/ health”，它们是Spring Boot Actuator应用程序中有用端点的默认位置。 如果您使用非默认上下文路径或servlet路径（例如`server.servletPath = /foo`）或管理端点路径（例如`management.contextPath = /admin`），则需要更改这些，即使是执行程序应用程序。 例：

application.yml

```
eureka:
  instance:
    statusPageUrlPath: ${management.context-path}/info
    healthCheckUrlPath: ${management.context-path}/health
```

这些链接显示在客户端的元数据中，并在某些情况下用于决定是否将请求发送到应用程序，因此正确的配置它们是非常有用的。

### 注册安全的应用程序 {#register-secure-app}

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

默认情况下， Eureka使用客户端的心跳决定一个客户端是否“存活”（“UP”状态），除非特别声明Discovery Client不根据Spring Boot Actuator传播应用程序的当前运行状况检查状态。这也就是说客户端注册成功后，Eureka将永远宣布应用程序处于“UP”状态。通过启用Eureka的健康检查可以改变这种行为，从而将应用程序状态传播给Eureka。因此，每个其他应用程序将不会向“UP”状态以外的应用程序发送请求。

application.yml

```
eureka:
  client:
    healthcheck:
      enabled: true
```

**WARNNING:**

`eureka.client.healthcheck.enabled=true` 只能设置在`application.yml`配置文件中， 设置在`bootstrap.yml`中将导致不可预知的问题，比如会导致注册到Eureka中的应用为UNKNOW状态。

如如果你想自已控制健康检查， 你可以考虑实现你自已的`com.netflix.appinfo.HealthCheckHandler`。

### 服务实例和客户端注册到Eureka的元数据

很值得花一点时间了解Eureka的元数据是如何工作的， 所以你可以在你的平台上使用这些元数据。有一些标准的元数据比如：IP地址、端口号、状态页和健康检查。它们被发布到注册中心，并且被客户端使用，来直接访问服务提供者。 额外的元素据可以添加到注册实例的eureka.instance.metadataMap中，这些信息可以被远程 的客户端访问， 但是一般不会改变客户端的行为，除非知道元数据的含义。下面介绍几种特殊情况， Spring Cloud已经为元数据ma赋于了含义。

#### 在Cloudfoundry上使用Eureka

Cloudfoundry有一个全局路由器，所以同一个应用程序的所有实例都具有相同的主机名（在具有相似架构的其他PaaS解决方案中也是如此）。这不一定是使用Eureka的障碍，但是如果您使用路由器（建议，甚至是强制性的，具体取决于您的平台的设置方式），则需要明确设置主机名和端口号（安全或不安全）），以便他们使用路由器。您可能还需要使用实例元数据，以便您可以区分客户端上的实例（例如，在自定义负载均衡器中）。默认情况下，eureka.instance.instanceId就是vcap.application.instance\_id。例如：

application.yml

```
eureka:
  instance:
    hostname: ${vcap.application.uris[0]}
    nonSecurePort: 80
```

根据Cloudfoundry实例中安全规则的设置方式，您可以注册并使用主机VM的IP地址进行直接的服务到服务调用。此功能尚未在Pivotal Web Services（PWS）上提供。

### 在AWS上使用Eureka

如果计划将应用程序部署到AWS云端，则Eureka实例必须配置为AWS感知，这可以通过以下方式定制EurekaInstanceConfigBean来完成：

```
@Bean
@Profile("!default")
public EurekaInstanceConfigBean eurekaInstanceConfig(InetUtils inetUtils) {
  EurekaInstanceConfigBean b = new EurekaInstanceConfigBean(inetUtils);
  AmazonInfo info = AmazonInfo.Builder.newBuilder().autoBuild("eureka");
  b.setDataCenterInfo(info);
  return b;
}
```

### 修改Eureka的Instance ID

Netflix Eureka实例注册了与其主机名相同的ID（即每个主机只有一个服务）。Spring Cloud Eureka提供了一个明智的默认，如下所示：

`${spring.cloud.client.hostname}:${spring.application.name}:${spring.application.instance_id:${server.port}}}`.

比如：`myhost:myappname:8080`.

使用Spring Cloud可以通过在eureka.instance.instanceId中提供唯一的标识符来覆盖此方法。

例如：

application.yml

```
eureka:
  instance:
    instanceId: ${spring.application.name}:${vcap.application.instance_id:${spring.application.instance_id:${random.value}}}
```

使用这个元数据和在localhost上部署的多个服务实例，随机值将会生效，以使实例是唯一的。在Cloudfoundry中，vcap.application.instance\_id将在Spring Boot应用程序中自动填充，因此不需要随机值。

### 使用Eureka客户端

一旦你的应用使用了`@EnableDiscoveryClient`（或者`@EnableEurekaClient`）， 你就可以使用它从Eureka Server发现服务实例。一种方法是使用本机com.netflix.discovery.EurekaClient（与Spring Cloud DiscoveryClient相反）。比如：

```
@Autowired
private EurekaClient discoveryClient;

public String serviceUrl() {
    InstanceInfo instance = discoveryClient.getNextServerFromEureka("STORES", false);
    return instance.getHomePageUrl();
}
```

**注意：**

不要在`@PostConstruct`注解的方法或`@Scheduled`注解的方法上（或者ApplicationContext可能还没有被启动的任何地方）使用EurekaClient。它在SmartLifecycle中进行初始化（阶段= 0），所以您可以依赖它的最早可用在另一个具有较高阶段SmartLifecycle中。

### Native Netflix EurekaClient的替代方案

并不是必须要使用原生的Netflix EurekaClient, 通常封装后的使用起来更方便。Spring Cloud支持Feign（一个REST客户端构建器）以及Spring RestTemplate，它使用逻辑Eureka服务标识符（VIP）而不是物理URL。要使用固定的物理服务器列表配置功能区，您可以将&lt;client&gt;.ribbon.listOfServers设置为以逗号分隔的物理地址（或主机名）列表，其中&lt;client&gt;是客户端的ID。

您还可以使用org.springframework.cloud.client.discovery.DiscoveryClient，它为Netflix不是特定的发现客户端提供一个简单的API，例如:

```
@Autowired
private DiscoveryClient discoveryClient;

public String serviceUrl() {
    List<ServiceInstance> list = discoveryClient.getInstances("STORES");
    if (list != null && list.size() > 0 ) {
        return list.get(0).getUri();
    }
    return null;
}
```

### 为什么注册服务这么慢？

作为一个实例包括定期心跳到注册中心（通过客户端的serviceUrl），默认持续时间为30秒。注册中心服务器和客户端在本地缓存中都具有相同的元数据（因此可能需要3个心跳），客户端才能发现服务。您可以使用eureka.instance.leaseRenewalIntervalInSeconds更改期限，这将加快将客户端连接到其他服务的过程。在生产中，最好遵守默认值，因为服务器内部有一些计算可以对租赁更新期进行假设。









