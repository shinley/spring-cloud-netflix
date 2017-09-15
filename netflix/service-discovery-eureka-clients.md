# 服务发现：Eureka客户端

服务发现是基于微服务架构的关键原则之一。 尝试配置每个客户端或某种形式的约定可能非常困难，而且非常脆弱。 Eureka是Netflix提供的，可以实现服务发现服务器端和客户端的组件。 可以将服务器配置和部署为高可用性，每个服务器将注册服务的状态复制到其他服务器。

## 如何添加Eureka客户端依赖 {#how-to-include-eureka-client}

要在项目中包含Eureka Client，可以添加group Id为`org.springframework.cloud`和artifact Id 为`spring-cloud-starter-eureka`的起步依赖。 有关使用当前Spring Cloud发布版本，设置构建系统的详细信息，请参阅“Spring Cloud Project”页面。

