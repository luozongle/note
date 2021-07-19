# Knife4j

[TOC]


## 一、简介
Knife4j是为Java MVC框架集成Swagger生成Api文档的增强解决方案,前身是swagger-bootstrap-ui,取名kni4j是希望她能像一把匕首一样小巧,轻量,并且功能强悍!

Knife4j的前身是`swagger-bootstrap-ui`，为了契合微服务的架构发展,由于原来`swagger-bootstrap-ui`采用的是后端Java代码+前端Ui混合打包的方式,在微服务架构下显的很臃肿,因此项目正式更名为`knife4j`

更名后主要专注的方面

- 前后端Java代码以及前端Ui模块进行分离,在微服务架构下使用更加灵活
- 提供专注于Swagger的增强解决方案,不同于只是改善增强前端Ui部分

**官方文档：**[https://doc.xiaominfo.com/knife4j/documentation/](https://doc.xiaominfo.com/knife4j/documentation/)

**备份文档地址**：[https://xiaoym.gitee.io/knife4j/](https://xiaoym.gitee.io/knife4j/)

**gitee地址**: [https://gitee.com/xiaoym/knife4j]

**github地址**: [https://github.com/xiaoymin/swagger-bootstrap-ui]

**效果(2.0版)：**[http://knife4j.xiaominfo.com/doc.html](http://knife4j.xiaominfo.com/doc.html)

**效果(旧版本)：**[http://swagger-bootstrap-ui.xiaominfo.com/doc.html](http://swagger-bootstrap-ui.xiaominfo.com/doc.html)

**示例:**[https://gitee.com/xiaoym/swagger-bootstrap-ui-demo](https://gitee.com/xiaoym/swagger-bootstrap-ui-demo)

**交流：** [https://doc.xiaominfo.com/knife4j/documentation/help.html](https://doc.xiaominfo.com/knife4j/documentation/help.html)

**源码分析**:[https://www.xiaominfo.com/2019/05/20/springfox-0/](https://www.xiaominfo.com/2019/05/20/springfox-0/)



## 二、开始使用

使用过程和springfox非常类似

> 使用springboot作为例子

### 1.引入依赖

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>2.0.7</version>
</dependency>
```

> 2.x版本底层依赖了springfox2 OpenAPI规范是v2，3.x版本底层依赖springfox3，OpenAPI规范是v3
>
> knife4j本身已经引入了springfox，使用过程中不需要单独引入springfox的具体版本，否则会导致版本冲突



### 2.快速开始
knife4j和直接使用springfox几乎相同，但是需要注意的是springfox使用 @EnableSwagger2 启用swagger，knife4j使用@EnableSwagger2WebMvc启动swagger

### 3.访问knife4j
> knife4j地址和swagger不同 
```url
http://ip:port/doc.html
```





























































