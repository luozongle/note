



spring-boot-actuator模块提供了Spring Boot的所有生产就绪功能，启用这些功能的推荐方法就是增加对spring-boot-starter-actuator 的starter依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```



Actuator端点可以让我们监控应用程序并与之交互。Spring Boot包含许多内置端点，并允许我们添加自己的断点。

当端点启动和公开时，它被认为是可用的。内置端点只有在可用时才会自动配置。大多数应用程序选择通过HTTP公开，其中断点的ID和前缀/actuator被映射到URL。例如，默认情况下 health断点映射到/actuator/health。



常用的端点

| ID               | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| auditevents      | 公开当前应用程序的审计事件信息                               |
| beans            | 显示应用程序所有Spring bean的完整列表                        |
| caches           | 公开可用的缓存                                               |
| conditions       | 显示在配置和自动配置类上评估的条件以及它们匹配或不匹配的原因 |
| configprops      | 显示所有的整理列表 @ConfigurationProperties                  |
| env              | 从Spring中的ConfigurableEnvironment中暴露配置                |
| flyway           | 显示任何已应用的Flyway数据库迁移                             |
| health           | 显示应用程序健康信息                                         |
| httptrace        | 显示HTTP跟踪信息                                             |
| info             | 显示应用程序信息                                             |
| integrationgraph | 显示spring集成图，需要spring-integration-core依赖            |
| loggers          | 显示并修改日志                                               |
| liquibase        | 显示所有Liquibase数据库的迁移                                |
| metrics          | 显示当前应用程序的指标信息                                   |
| mappings         | 显示所有@RequestMapping路径的整理列表                        |
| scheduledtasks   | 显示应用程序中的scheduled任务                                |
| sessions         | 允许从Spring session支持的会话存储中检索和删除用户会话。需要使用Spring session基于Servlet的web应用程序 |
| shutdown         | 让应用程序关闭，默认禁用                                     |
| startup          | 显示ApplicationStartup收集的启动步骤数据                     |
| threaddump       | dump线程信息                                                 |



如果应用程序是web应用程序(Spring MVC,Spring WebFlux或Jersey)，可以使用以下附加端点

| ID         | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| heapdump   | 返回hprof堆转储文件。需要一个HotSpot JVM                     |
| jolokia    | 通过HTTP公开JMV bean(当Jolokia在类路径上时，不适用于WebFlux)。需要依赖于jolokia-core |
| logfile    | 返回日志文件的内容(如果已设置logging.file.name或logging.file.path属性)。支持使用HTTP Range头来检索日志文件的部分内容 |
| prometheus | 以Prometheus服务器可以抓取的格式公开指标。需要依赖于micrometer-registry-prometheus |







启动端点

默认情况下启动了除shutdown以外的所有端点。要配置端点的启用，使用management.endpoint.\<id\>.enabled属性

例:启动shutdown端点

```properties
management.endpoint.shutdown.enabled=true
```



如果希望端点是选择加入而不是选择退出，可以将management.endpoints.enabled-by-default属性设置为false，并使用单个端点enabled属性重新选择加入

```properties
management.endpoints.enabled-by-default=false
management.endpoints.info.enabled=true
```

>  禁用的端点从应用程序上下文中完全删除,如果只是想更改端点公开的方式，可以使用include和exclude特性



端点暴露的配置

| 属性                                      | 默认值      |
| ----------------------------------------- | ----------- |
| management.endpoints.jmx.exposure.exclude |             |
| management.endpoints.jmx.exposure.include | *           |
| management.endpoints.web.exposure.exclude |             |
| management.endpoints.web.exposure.include | info,health |

> include配置了包含的属性，exclude配置了排除的属性，排除属性优于包含属性。



例: 要停止JMX公开所有端点而仅公开health和info端点

```properties
management.endpoints.jmx.exposure.include=health,info
```



例:要公开除env和beans端点以外的所有内容

```properties
management.endpoints.web.exposure.include=*
management.endpoints.web.exposure.exclude=env,beans
```

> \* 在yml中具有特殊含义，因此必须加引号



端点会自动缓存不带任何参数的读取操作的响应，可以修改端点缓存响应的时间

例:将beans端点缓存的生效时间设置为10秒:

```properties
management.endpoint.beans.cache.time-to-libe=10s
```



CORS支持

跨域资源共享式一种W3C规范。如果使用Spring MVC或Spring WebFlux，则可以配置Actuator的Web端点以支持此类场景。

默认情况下禁用CORS支持，并且仅management.endpoints.web.cors.allowed-origins在设置属性后启用。以下配置允许GET和POST来自example.com域的调用

```properties
management.endpoints.web.cors.allowed-origins=https://example.com
management.endpoints.web.cors.allowed-methods=GET,POST
```





自定义端点



















