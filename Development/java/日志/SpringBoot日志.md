



SpringBoot使用Commons Logging(JCL)记录所有的日志，离开了底层的实现。为JUL，Log4j2和Logback提供了默认配置。每种情况，都预先配置了控制台输出和可选日志文件输出。

通常我们不需要更改日志记录依赖项，由Spring默认配置。



默认情况下，Spring日志会打印到控制台，我们还可以使用--debug 标记打开debug模式

```bash
java -jar myapp.jar --debug
```

> 当然，也可以在配置文件中指定 debug=true

当启动为debug模式时，一些核心日志(嵌入式容器、Hibernate、SpringBoot) 将输出更多信息，但是我们的应用程序日志不会打印debug级别的日志。



默认情况下，SpringBoot只会输出到console中，并不会输出到日志文件中。如果我们还想输出到日志文件中，需要使用 `log.file.name`或`logging.file.path`属性

| logging.file.name | logging.file.path  | Example  | Description                                                  |
| ----------------- | ------------------ | -------- | ------------------------------------------------------------ |
| (none)            | (none)             |          | 只会输出到console中                                          |
| Specific file     | (none)             | my.log   | 输出到指定的文件中，名字可以是确切的位置或相对于当前目录     |
| (none)            | Specific direcotry | /var/log | 在/var/log中创建spring.log  名字可以是确切的位置或相对于当前目录 |

> 默认情况下，达到10MB就会滚动



所有的日志系统都可以在spring环境中设置级别

> 这个和`debug=true` 不一样  这种可能包含了spring的核心日志  但是`debug=true` 只会将核心日志改为debug

```properties
logging.level.<logger-name>=<level> 
# TRACE,DEBUG,INFO,WARN,ERROR,FATAL,OFF

logging.level.root=warn
# root logger可以这么配置
```





有时候我们需要把相关的记录器组合到一起，让他们具有相同的配置。例如我们可能会更改所有Tomcat相关的记录器的日志级别，但是可能我们比较难记住顶级包名，为了解决这个问题，SpringBoot允许我们在spring环境中定义日志记录组

例:

```properties
logging.group.tomcat=org.apache.catalina,org.apache.coyote,org.apache.tomcat

logging.level.tomcat=trace
```



spring预先制定了2个日志组

| Name | Loggers                                                      |
| ---- | ------------------------------------------------------------ |
| web  | `org.springframework.core.codec`, `org.springframework.http`, `org.springframework.web`, `org.springframework.boot.actuate.endpoint.web`, `org.springframework.boot.web.servlet.ServletContextInitializerBeans` |
| sql  | `org.springframework.jdbc.core`, `org.hibernate.SQL`, `org.jooq.tools.LoggerListener` |





自定义日志配置

可以通过在类路径上包括相应的库来激活各种日志系统，并且可以通过`logging.config`属性配置日志配置文件的具体位置

例:

```properties
# 指定类路径下logback.xml
logging.config=classpath:logback.xml

# 指定类路径下配置文件 具体在maven中配置
logging.config=classpath:logback-@profile.active@.xml

logging.config= 

# 启动jar时通过指定
--logging.config=/root/apps/all-data-def/logback-online.xml "
```





根据不同的日志系统，会加载以下文件

| Logging System         | Customization                                                |
| ---------------------- | ------------------------------------------------------------ |
| Logback                | `logback-spring.xml`,`logback-spring.groovy`,`logback.xml`或`logback.groovy` |
| Log4j2                 | `log4j2-spring.xml`或`log4j2.xml`                            |
| JDK(Java Util Logging) | `logging.properties`                                         |

> 建议对日记记录配置使用-spring类型，例如 `logback-spring.xml`而不是`logback.xml`，如果使用标准配置，Spring无法完全控制日志初始化
>
> JUL存在类加载问题，从可执行jar运行时可能会导致问题，不推荐使用JUL



优先读取logback.xml   然后是logback-spring.xml

如果使用的是 logback-dev.xml或logback-online.xml需要使用logging.config 指定

> 我们可以利用maven的profile动态指定配置logback.xml

```properties
logging.config=classpath:logback-@profile.active@.xml
```





















