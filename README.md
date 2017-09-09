# 《spring cloud netflix》翻译邀请

## 如何领取

通过发issue领取想要翻译的文章，每次领取一章或一节（根据内容长短），翻译完后再领取其他章节。领取完成之后，建议在一个星期内翻译完成，或者自已指定完成的时间，如果不能完成翻译，也欢迎你邀请其他同学和你一起完成翻译。**请谨慎领取.**

翻译后的文档不会用于营利目的， 仅供大家交流学习使用。翻译人可以把自已的名字加在自已翻译的章节的大标题下面。由于没办法去跟进每一篇译文的进展，所以为了防止很多文章领取了没有翻译，影响沉别人翻译。 所以长时间没有翻译的，会释放让别人认领。

spring cloud netflix：http://cloud.spring.io/spring-cloud-static/spring-cloud-netflix/1.3.4.RELEASE/
github地址：https://github.com/shinley/spring-cloud-netflix

大家最好使用gitbook来进行管理草稿，最后可以由gitbook生成电子阅。 

# 章节分布：
大家领取翻译的章节后，我会把名字标注在每一节的后面， 表示已被人领取，可以选择未被领取的章节进行翻译。

* [spring cloud netflix](chapter1.md)

* [Service Discovery: Eureka Clients](chapter1/service-discovery-eureka-clients.md)

  - How to Include Eureka Client

  - Registering with Eureka

  - Authenticating with the Eureka Server

  - Status Page and Health Indicator

  - Registering a Secure Application

  - Eureka’s Health Checks

  - Eureka Metadata for Instances and Clients

  - Using the EurekaClient

  - Alternatives to the native Netflix EurekaClient

  - Why is it so Slow to Register a Service?

  - Zones

  - Service Discovery: Eureka Server

  - How to Include Eureka Server

  - How to Run a Eureka Server

  - High Availability, Zones and Regions

  - Standalone Mode

  - Peer Awareness

  - Prefer IP Address

- [Circuit Breaker: Hystrix Clients](chapter1/circuit-breaker-hystrix-clients.md)

  - How to Include Hystrix

  - Propagating the Security Context or using Spring Scopes

  - Health Indicator

  - Hystrix Metrics Stream

  - Circuit Breaker: Hystrix Dashboard](chapter1/circuit-breaker-hystrix-dashboard.md)

  - How to Include Hystrix Dashboard

  - urbine

  - Turbine Stream

- [Client Side Load Balancer: Ribbon](chapter1/client-side-load-balancer-ribbon.md)

  - How to Include Ribbon

  - Customizing the Ribbon Client

  - Customizing the Ribbon Client using properties

  - Using Ribbon with Eureka

  - Example: How to Use Ribbon Without Eureka

  - Example: Disable Eureka use in Ribbon

  - Using the Ribbon API Directly

  - Caching of Ribbon Configuration

- [Declarative REST Client: Feign](chapter1/declarative-rest-client-feign.md)

  - How to Include Feign

  - Overriding Feign Defaults

  - Creating Feign Clients Manually

  - Feign Hystrix Support

  - Feign Hystrix Fallbacks

  - Feign and @Primary

  - Feign Inheritance Support

  - Feign request/response compression

  - Feign logging

  - External Configuration: Archaius

- [Router and Filter: Zuul](chapter1/router-and-filter-zuul.md)

  - How to Include Zuul

  - Embedded Zuul Reverse Proxy

  - Zuul Http Client

  - Cookies and Sensitive Headers

  - Ignored Headers

  - he Routes Endpoint

  - Strangulation Patterns and Local Forwards

  - Uploading Files through Zuul

  - Query String Encoding

  - Plain Embedded Zuul

  - Disable Zuul Filters

  - Providing Hystrix Fallbacks For Routes

  - [Zuul Developer Guide](chapter1/router-and-filter-zuul/zuul-developer-guide.md)

  - The Zuul Servlet

  - Zuul RequestContext

  - Custom Zuul Filter examples

  - How to Write a Pre Filter

  - How to Write a Route Filter

  - How to Write a Post Filter

  - How Zuul Errors Work

  - Zuul Eager Application Context Loading

  - Polyglot support with Sidecar

  - RxJava with Spring MVC

- [Metrics: Spectator, Servo, and Atlas](chapter1/metrics-spectator-servo-and-atlas.md)

  - Dimensional vs. Hierarchical Metrics

  - Default Metrics Collection

  - [Metrics Collection: Spectator](chapter1/metrics-spectator-servo-and-atlas/metrics-collection-spectator.md)

    - Spectator Counter

    - Spectator Timer

    - Spectator Gauge

    - Spectator Distribution Summaries

    - Metrics Collection: Servo

    - Creating Servo Monitors

   - [Metrics Backend: Atlas](chapter1/metrics-spectator-servo-and-atlas/metrics-backend-atlas.md)

      - Global tags

      - Using Atlas

      - Retrying Failed Requests

      - Configuration

      - Zuul






