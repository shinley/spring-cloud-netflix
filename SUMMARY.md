# Summary

* [Introduction](README.md)
* [spring cloud netflix](netflix.md)
  * [Service Discovery: Eureka Clients](netflix/service-discovery-eureka-clients.md)
    * [How to Include Eureka Client](netflix/service-discovery-eureka-clients.md#how-to-include-eureka-client)
    * [Registering with Eureka](netflix/registering-with-eureka.md)
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
  * [Circuit Breaker: Hystrix Dashboard](netflix/circuit-breaker-hystrix-dashboard.md)
    * How to Include Hystrix Dashboard
    * Turbine
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
    * The Routes Endpoint
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

