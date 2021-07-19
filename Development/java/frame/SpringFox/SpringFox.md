# SpringFox

[toc]


## 一、简介

SpringFox: SpringFox是一个通过扫描代码提取代码中的信息，生成API文档的工具，API的格式不止Swagger，还有很多其他格式，但我们比较常用的是swagger

比如我们常用的注解@Api @ApiModel等，这些注解是swagger-core的。springfox-swagger2这个包依赖了swagger-core这个包。但是swagger-core只支持JAX-RS2,并不支持我们常用的Spring MVC。这就是springfox-swagger的作用了，它将上面那些用于JAX-RS2的注解适配到了Spring MVC上。







## 二、增加依赖

以前使用2.x版本的时候，一般来说我们需要添加如下两个依赖

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```



但是3.0只需要添加下面的starter就ok

```xml
<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-boot-starter</artifactId>
  <version>3.0.0</version>
</dependency>
```

## 三、配置swagger

```java
@EnableSwagger2
public class Swagger2Config {

    @Bean
    public Docket webApiConfig(){
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("全数据防线管理接口")
                .apiInfo(webApiInfo())
                .select()
                //只显示以api开头的页面
                .paths(PathSelectors.regex("/all-data/admin/.*"))
                .build();
    }

    @Bean
    public Docket adminApiConfig(){

        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("全数据防线业务接口")
                .apiInfo(adminApiInfo())
                .select()
                //只显示以admin开头的页面
                .paths(PathSelectors.regex("/all-data/api/.*"))
                .build();
    }

    private ApiInfo webApiInfo(){
        return new ApiInfoBuilder()
                .title("全数据防线业务运管")
                .description("任务配置、预警查询")
                .version("1.0")
                //.contact(new Contact("baidu", "http://www.baidu.com", "xxxx@baidu.com"))
                .build();
    }

    private ApiInfo adminApiInfo(){
        return new ApiInfoBuilder()
                .title("全数据防线管理接口")
                .description("底库、人员、设备管理")
                .version("1.0")
                .build();
    }
}
```

  

每个Docket 都是一个分组

  

## 四、接口地址

接口地址

2.x

>文件接口地址: http://ip:port/v2/api-docs
>
>文档页面地址: http://ip:port/swagger-ui.html



3.x

> 文档接口地址: http://ip:port/v3/api-docs
>
> 文档页面地址: http://ip:port/swagger-ui/ 或  http://ip:port/swagger-ui/index.html





## 五、常用注解

#### swagger2

1. @Api : 用在请求的类上，表示对类的说明，也代表了这个类是swagger2的资源

   > 默认每个controller一个分组标签，分组名默认为controller类名小写中间用-分隔。可以对方法进行单独分组，

   参数:

   ```
   tags: API文档控制的标签列表。标签可用于按资源或任何其他限定符对操作进行逻辑分组(分组名)
   ```

2. @ApiOperation: 用于方法，表示一个http请求访问该方法的操作

   参数:

   ```
   value: 对此方法的简要说明。为了在Swagger-UI中获得适当的可见性，应该少于120个字符
   notes: 对此方法的详细描述
   tags: API文档的标签列表，标签可用于按资源或任何其他限定符对方法进行逻辑分组。非空值将覆盖从@Api#value或@Api#tags接收的值
   response:
   responseContainer:
   httpMethod:
   ```

3. @ApiModel: 用于响应实体类上，用于说明实体类作用

   参数:

   ```
   value: 为model提供一个名称，默认使用类名
   description: 对类的描述
   ```

4. @ApiModelProperty: 用于属性上，描述实体类的属性

   参数:

   ```
   value: 此属性的简要说明
   name: 被覆盖的属性名称
   allowableValues: 此属性可以接受的值,有三种写法 
   								1. {first, second, third} 设置值列表，用,分隔
   								2. {range[1,5]}或{range(1,5)}或{range[1,5)} 设置值的范围，方括号表示包含最大最小值，圆括号不包含
   								3. {range[1,infinity]} 设置一个最小最大值,使用范围格式相同，但用"infinity"或"-infinity"作为第二个值
   dataType: 参数的数据类型，该值将覆盖从类属性读取的数据类型
   required: 指定是否需要该参数
   position: 允许显式排序类中的属性
   hidden: 允许在Swagger模型中隐藏模型属性
   example: 属性的示例值
   ```

5. @ApiImplicitParams: 用在请求的方法上，包含多个@ApilmplicitParam

6. @ApiImplicitParam: 用于方法，表示单独的请求参数

   > 好像不太适用于RequestBody，加了这个以后  不能显示注释在类上面的注释了

   参数:

   ```
   name: 参数名称
   value: 参数的简要说明
   defaultValue: 参数默认值(感觉使用example更好一点)
   allowableValues: 此属性可以接受的值,有三种写法 
   								1. {first, second, third} 设置值列表，用,分隔
   								2. {range[1,5]}或{range(1,5)}或{range[1,5)} 设置值的范围，方括号表示包含最大最小值，圆括号不包含
   								3. {range[1,infinity]} 设置一个最小最大值,使用范围格式相同，但用"infinity"或"-infinity"作为第二个值
   required: 指定是否需要该参数
   allowMultiple: 指定参数是否可以出现多次来接受多个值
   dataType: 参数的数据类型
   paramType: 参数类型,有效值为 path、query、body、header、form
   example: 示例值
   ```

7. @ApiParam(): 用于方法，参数，字段说明 表示对参数的要求和说明 (swagger3中实测没反应，不知道原因)

   > 与@ApiImplicitParam功能类似，建议使用@ApiImplicitParam

   参数:

   ```
   name: 参数名称
   value: 参数的简要说明
   defaultvalue: "参数的默认值"
   required: "true"
   example: 示例值
   ```

8. @ApiResponses: 用在请求的方法上，包含多个@ApiResponse

   参数:

   ```
   
   ```
   





























