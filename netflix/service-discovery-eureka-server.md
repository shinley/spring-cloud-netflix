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























