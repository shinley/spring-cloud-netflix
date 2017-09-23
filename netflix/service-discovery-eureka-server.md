## 服务发现：Eureka服务端

### 如何加入Eureka Server服务端的依赖

要在你的项目中加入Eureka Server,  只需要加入 group Id为`org.springframework.cloud ，`artifact id为`spring-cloud-starter-eureka-server`的起步依赖即可。有关使用当前Spring Cloud发布版本设置构建系统的详细信息，请参阅[Spring Cloud Project page](http://projects.spring.io/spring-cloud/)页面。

### 如何运行Eureka Server

Eureka server的例子：

```
@SpringBootApplication
@EnableEurekaServer
public class Application {

    public static void main(String[] args) {
        new SpringApplicationBuilder(Application.class).web(true).run(args);
    }

}
```

Eureka服务器具有一个带有UI的主页，以及每一个普通Eureka功能下的提供了HTTP端点，路径为/eureka/\* 。

Eureka background reading: see [flux capacitor](https://github.com/cfregly/fluxcapacitor/wiki/NetflixOSS-FAQ#eureka-service-discovery-load-balancer) and [google group discussion](https://groups.google.com/forum/?fromgroups#!topic/eureka_netflix/g3p2r7gHnN0).

**注意：**

由于Gradle的依赖关系解析规则和父bom功能的缺乏，仅仅依靠spring-cloud-starter-eureka-server可能会导致应用程序启动失败。要弥补这一点，必须添加Spring Boot Gradle插件，Spring Cloud 起步依赖的父 bom必须像这样导入：

build.gradle

```
buildscript {
  dependencies {
    classpath("org.springframework.boot:spring-boot-gradle-plugin:1.3.5.RELEASE")
  }
}

apply plugin: "spring-boot"

dependencyManagement {
  imports {
    mavenBom "org.springframework.cloud:spring-cloud-dependencies:Brixton.RELEASE"
  }
}
```

### 高可用， Zones 和 Regions

Eureka服务器没有后端存储，但注册表中的服务实例都必须发送心跳以保持其注册更新（所以这可以在内存中完成）。客户端也具有eureka注册的内存缓存（因此，他们不必每次请求服务端都到注册中心获取注册表中注册的服务）。

默认情况下，每个Eureka服务器也是Eureka客户端，并且需要（至少一个）服务URL来定位其它Eureka服务器。如果不提供,它也能正常的运行和工作， 但是它会产生很多的关于无法注册到其它对等Eureka服务器的噪音日志。

有关客户端侧的Zones和Regions的支持，也可查查看[below for details of Ribbon support](http://cloud.spring.io/spring-cloud-static/Dalston.SR3/#spring-cloud-ribbon)

### 单机模式

只要有某种监视器或弹性运行时保持它的存活（例如Cloud Foundry）,两个缓存（客户端和服务器端缓存）和心跳机制的结合使单台的Eureka服务器对故障具有相当的弹性。 在单机模式下，你可能更倾向于关闭客户端行为， 使它不再尝试注册到其它不存在的Eureka Server, 例如：

application.yml \(单机模式Eureka Server\)

```
server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    registerWithEureka: false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

注注意：`serviceUrl`指向了与本地实例相同的主机。

### \(Eureka Server\) 对等服务互相感知

Eureka可以通过运行多个实例并要求他们相互注册而变得更有弹性。实际上，这是默认的行为，所以你需要做的就是给它配置一个serviceUrl使它成为一个有效的给对等体，例如

application.yml \(两个对等且互相感知的 Eureka Servers\)

```
---
spring:
  profiles: peer1
eureka:
  instance:
    hostname: peer1
  client:
    serviceUrl:
      defaultZone: http://peer2/eureka/

---
spring:
  profiles: peer2
eureka:
  instance:
    hostname: peer2
  client:
    serviceUrl:
      defaultZone: http://peer1/eureka/
```

在这个例子中，我们有一个YAML，通过spring profile配置可以在相同的服务器上的两个hosts上运行两个的Eureka Server服务, 你可以使用这个配置来测试单个hosts上的对等感知（通过/etc/hosts来解析主机名， 在生产环境中没有多大用处）。 实际上，如果你在知道自己的主机名的机器上运行，则不需要eureka.instance.hostname配置（默认情况下使用java.net.InetAddress查找）

您可以向系统添加多个对等体，只要它们至少一个边界彼此连接，则它们将在它们之间同步注册。如果对等体在物理上分离（在数据中心内或多个数据中心之间），则系统原则上可以在“分裂脑类型” 故障中存活。

### 配置IP地址

在某些情况下，你可以更想让Eureka公布服务的IP地址， 而不是主机名。将eureka.instance.preferIpAddress设置为true，当应用程序向eureka注册时，它将使用其IP地址而不是其主机名。













