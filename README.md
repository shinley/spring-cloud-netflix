# 《spring cloud netflix》翻译邀请

## 如何领取

通过发issue领取想要翻译的文章，每次领取一章或一节（根据内容长短），翻译完后再领取其他章节。领取完成之后，建议在一个星期内翻译完成，或者自已指定完成的时间，如果不能完成翻译，也欢迎你邀请其他同学和你一起完成翻译。**请谨慎领取.**

翻译后的文档不会用于营利目的， 仅供大家交流学习使用。翻译人可以把自已的名字加在自已翻译的章节的大标题下面。由于没办法去跟进每一篇译文的进展，所以为了防止很多文章领取了没有翻译，影响沉别人翻译。 所以长时间没有翻译的，会释放让别人认领。

spring cloud netflix官方地址：[http://cloud.spring.io/spring-cloud-static/spring-cloud-netflix/1.3.4.RELEASE/](http://cloud.spring.io/spring-cloud-static/spring-cloud-netflix/1.3.4.RELEASE/)

github书稿地址：[https://github.com/shinley/spring-cloud-netflix](https://github.com/shinley/spring-cloud-netflix)

一定要使用gitbook支持的markdown语法来书写，最后可以由gitbook生成电子阅。

## 协作流程

首先fork我的项目

1. 把fork过去的项目也就是你的项目clone到你的本地
2. 运行`git remote add netflix git@github.com:shinley/spring-cloud-netfilix.git`把我的库添加为远端库, 其中的add netflix是给远程仓库设置别名。 此处的别名是netflix
3. 运行`git pull netflix master`拉取并合并到本地分支, 拉取我的仓库时，要加上你设置的别名。
4. 翻译内容
5. commit后push到自己的库（`git push origin master`）
6. 登录Github在你首页可以看到一个`pull request`按钮，点击它，填写一些说明信息，然后提交即可。

1-3是初始化操作，执行一次即可。在翻译前必须执行第4步同步我的库（这样避免冲突），然后执行5-7既可。

# 章节分布：

大家领取翻译的章节后，我会把名字标注在每一节的后面， 表示已被人领取，可以选择未被领取的章节进行翻译。

* [spring cloud netflix](netflix.md)

* [Service Discovery: Eureka Clients](netflix/service-discovery-eureka-clients.md)

  * How to Include Eureka Client

  * Registering with Eureka

  * Authenticating with the Eureka Server

  * Status Page and Health Indicator

  * Registering a Secure Application

  * Eureka’s Health Checks

  * Eureka Metadata for Instances and Clients

  * Using the EurekaClient

  * Alternatives to the native Netflix EurekaClient

  * Why is it so Slow to Register a Service?

  * Zones

* Service Discovery: Eureka Server

  * How to Include Eureka Server

  * How to Run a Eureka Server

  * High Availability, Zones and Regions

  * Standalone Mode

  * Peer Awareness

  * Prefer IP Address

* [Circuit Breaker: Hystrix Clients](netflix/circuit-breaker-hystrix-clients.md)

  * How to Include Hystrix

  * Propagating the Security Context or using Spring Scopes

  * Health Indicator

  * Hystrix Metrics Stream

  * Circuit Breaker: Hystrix Dashboard\]\(netflix/circuit-breaker-hystrix-dashboard.md\)

  * How to Include Hystrix Dashboard

  * urbine

  * Turbine Stream

* [Client Side Load Balancer: Ribbon](netflix/client-side-load-balancer-ribbon.md)

  * How to Include Ribbon

  * Customizing the Ribbon Client

  * Customizing the Ribbon Client using properties

  * Using Ribbon with Eureka

  * Example: How to Use Ribbon Without Eureka

  * Example: Disable Eureka use in Ribbon

  * Using the Ribbon API Directly

  * Caching of Ribbon Configuration

* [Declarative REST Client: Feign](netflix/declarative-rest-client-feign.md)

  * How to Include Feign

  * Overriding Feign Defaults

  * Creating Feign Clients Manually

  * Feign Hystrix Support

  * Feign Hystrix Fallbacks

  * Feign and @Primary

  * Feign Inheritance Support

  * Feign request/response compression

  * Feign logging

  * External Configuration: Archaius

* [Router and Filter: Zuul](netflix/router-and-filter-zuul.md)

  * How to Include Zuul

  * Embedded Zuul Reverse Proxy

  * Zuul Http Client

  * Cookies and Sensitive Headers

  * Ignored Headers

  * he Routes Endpoint

  * Strangulation Patterns and Local Forwards

  * Uploading Files through Zuul

  * Query String Encoding

  * Plain Embedded Zuul

  * Disable Zuul Filters

  * Providing Hystrix Fallbacks For Routes

  * [Zuul Developer Guide](netflix/router-and-filter-zuul/zuul-developer-guide.md)

  * The Zuul Servlet

  * Zuul RequestContext

  * Custom Zuul Filter examples

  * How to Write a Pre Filter

  * How to Write a Route Filter

  * How to Write a Post Filter

  * How Zuul Errors Work

  * Zuul Eager Application Context Loading

  * Polyglot support with Sidecar

  * RxJava with Spring MVC

* [Metrics: Spectator, Servo, and Atlas](netflix/metrics-spectator-servo-and-atlas.md)

  * Dimensional vs. Hierarchical Metrics

  * Default Metrics Collection

  * [Metrics Collection: Spectator](netflix/metrics-spectator-servo-and-atlas/metrics-collection-spectator.md)

    * Spectator Counter

    * Spectator Timer

    * Spectator Gauge

    * Spectator Distribution Summaries

    * Metrics Collection: Servo

    * Creating Servo Monitors

    * [Metrics Backend: Atlas](netflix/metrics-spectator-servo-and-atlas/metrics-backend-atlas.md)

      * Global tags

      * Using Atlas

      * Retrying Failed Requests

      * Configuration

      * Zuul



