[TOC]

# Idea安装文档

### 环境介绍:

系统版本:macOS Big Sur 11.2.2

Idea版本:IntelliJ IDEA 2021.1 (Ultimate Edition)



### 一、IDEA常用配置

#### 1.配置自己喜欢的护眼色

##### 1.1 设置代码区颜色

Preferences.. -> Ediotr -> Color Scheme -> General -> Text -> Default text
将Background设置为护眼色 `R:199; G:237; B:204;`

##### 1.2 设置console区颜色

Preferences.. -> Ediotr -> Color Scheme -> Console Colors -> Console -> Background 

将Background设置为护眼色 `R:199; G:237; B:204;`

##### 1.3 设置左侧工作区颜色

Preferences.. -> Appearance & Behavior -> File Colors 

新增Project Files 并配置颜色  颜色选择custom 并设置为`R:199; G:237; B:204;`



#### 2.取消区分大小写提示

Preferences.. -> Ediotr -> General -> Code Completion 

将Match case取消勾选



#### 3.配置自己喜欢的`idea.properties`

/Applications/IntelliJ IDEA.app/Contents/bin/idea.properties

或者Help -> Edit Custom Properties在配置目录创建一个空的idea.properties文件，在这个文件中修改也可以

或者可以通过IDEA_PROPERTIES环境变量制定idea.properties文件的位置，该文件的属性将覆盖默认文件和IntelliJ IDEA配置目录中的相应属性。

| 属性                           | 描述                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| idea.max.content.load.filesize | IDEA能够打开的最大文件大小(以千字节为单位)。使用大文件可能会影响编辑器性能并增加内存消耗，默认值为20000 |
| idea.max.intellisense.filesize | IDEA为其提供编码帮助的最大文件大小(以千字节为单位)。大文件的编码帮助可能会影响编辑器性能并增加内存消耗，默认值为2500 |
| idea.cycle.buffer.size         | 控制台循环缓冲区的最大大小(以千字节为单位)。如果控制台输出大小超过此值，则会删除最早的行，要禁用循环缓冲区限制，可设置`idea.cycle.buffer.size=disabled`（还是限制一下好) |
| idea.max.vcs.loaded.size.kb    | IDEA加载的最大大小(以千字节为单位),用于在比较更改时显示过去的文件内存，默认值为20480 |



#### 4.设置tabs关闭策略

默认限制最大开启10个选项卡，如果达到10个选项卡，就会触发closing policy。

Preferences -> Editor -> General -> Editor Tabs -> Closing Policy 里面可以设置最大开启的标签数量以及关闭策略，可以根据需要进行修改。



#### 5.开启面包屑导航

Preferences -> Editor -> General -> Breadcrumbs  勾选上Java



#### 6.导包相关配置

Preferences -> Editor -> General  -> Auto Import 

Optimize imports on the fly 会自动帮我们去除没有用的包

Add unambiguous imports on the fly 快速添加明确的导入



Preferences -> Editor -> Code Style -> Java

Class count to use import with '*':  默认是5代表当同一个包内有5个类被导入用\*替换 可以改为999

Names count to use static import with '*': 默认是3，代表当一个类中多少个静态名称被导入用\*替换 可以改为999



#### 7.设置idea编译内存

Preferences -> Build, Execution, Deployment -> Compiler 

修改Shared build process head size (Mbytest:) 1024





#### 8.配置idea文件模板

文件模板是我们创建新文件的默认内容规范，根据创建的文件类型，模板提供该类型所有文件所需的初始化代码和格式。

Preferences -> Editor -> File and Code Templates 

- Files选项卡里面包含用于创建新文件的模板
- Includes选项卡里面包含可重复使用的内容片段，用于插入文件模板



##### 语法:
文件和代码模板使用Velocity模板语言
- 固定文本(标记、代码、注释等)，按原样呈现
- 变量，由他们的值替换
- 各种指令，#parse、#set、#if 等



创建几个自己喜欢的Includes

- Attribute
```Velocity
@Data
@NoArgsConstructor
@AllArgsConstructor
@Accessors(chain = true)
```

- Controller
```Velocity
@Slf4j
@RestController
@RequestMapping("/")
```

- Service
```Velocity
@Slf4j
@Service
```


- JavaInfo

```Velocity
#set( $MyName = "v_luozongle" )



/**
 *
 * @author ${MyName}
 * @author ${USER}
 * @since ${YEAR}-${MONTH}-${DAY}
*/
```



然后创建自己的个性Files

- Controller
```Velocity
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end


#parse("File Header.java")
#parse("JavaInfo.java")
#parse("Controller.java")
public class ${NAME}Controller {
}
```

- Service
```Velocity
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end

#parse("File Header.java")
#parse("JavaInfo.java")
#parse("Service.java")
public class ${NAME}ServiceImpl {
}
```


- Data
```Velocity
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end

#parse("File Header.java")
#parse("JavaInfo.java")
#parse("Attribute.java")
public class ${NAME} {
}
```

- Class
```Velocity
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end

#parse("File Header.java")
#parse("JavaInfo.java")
public class ${NAME} {
}
```



#### 9.配置idea实时模板

Preferences -> Editor -> Live Templates

实时模板可以帮助我们快速创建我们想要的东西 

比如: sout   psvm   main  iter



> \$END\$ 的意思是创建完成后光标的位置

##### 9.1 创建thread类的实时模板

Abbreviation: tr       Description:创建一个Thread

```Template
Thread thread = new Thread(() -> {

});

thread.start();
$END$

```



##### 9.2 创建测试类

Abbreviation: test       Description:创建一个方法

```Template
@Test
public void test$testMethodName$() {
    $END$
}

```


> 配置\$testMethodName\$变量的值，点击`Edit variables`进行配置
```variables
testMethodName:  groovyScript("return UUID.randomUUID().toString().replaceAll(\"-\", \"\");");
```



#### 10.配置idea后缀模板

Preferences -> Editor -> General -> Postfix Completion

实时模板可以与后缀模板的区别:

> sout -> System.out.println("");   # 是实时模板的效果
> "hello world".sout -> System.out.println("hello world"); # 是后缀模板的效果



>  \$EXPR\$ 是我们输入的类名，配置好以后 例: `thread.tr ` 就可以帮助我们生成一个引用名称为thread的类

tr   帮助我们快速创建一个线程类

```expression
Thread $EXPR$ = new Thread(() -> {
    $END$  
});

$EXPR$.start();
```



t

```expression
Thread $EXPR$ = new Thread() {
    @Override
    public void run() {
        $END$
    }
};

$EXPR$.start();
```







### 二、IDEA性能优化

#### 1.设置IDEA内存

Help -> Edit Custom VM Options 

```properties
-Xms2048m
-Xmx4096m
-XX:ReservedCodeCacheSize=1024m
```

打开内存监视器

View -> Appearance -> Status Bar Widgets -> Memory Indicator





### 三、IDEA命令行
mac如果想使用命令行可以使用open命令
在PATH环境变量的目录中创建shell脚本
例如: 在/usr/local/bin 目录下创建idea脚本

```sh
#!/bin/sh
open -na "IntelliJ IDEA.app" --args "$@"
```



```shell
sudo chmod a+x /usr/local/bin/idea
```



例子:

```shll
idea     # 启动IDEA

idea ~/HelloWorld    # 打开一个项目

idea --line 78 ~/HelloWorld/pom.xml   # 在78行上打开一个特定文件

idea diff ~/HelloWorld/README.md ~/HelloWorld/README.md  # 可以比较两个文件之间的diff(还可以比较三个文件)


```







### 四、IDEA常用插件

#### 1. arthas idea

用处:可以帮助我们方便的生成arthas命令。



#### 2. Gerrit

作用:如果我们有CodeReview，推代码的时候就不能直接推到例如master了，需要推到 /refs/for/master，这个插件就可以帮助我们方便的推到/refs/for/master



#### 3. Maven Helper

作用:依赖分析插件



#### 4.PlantUML integration

作用:画时序图的工具



#### 5. Statistic

作用:帮助我们统计代码行数



#### 6.Lombok

作用:lombok必备插件



#### 7.SequenceDiagram

作用:可以帮助我们生成代码时序图



#### 9.RestfulTookkit

作用:只要输入url路径，立即跳转到指定方法 但是这个插件在2019年后已经不维护了，不支持新版本的idea，推荐使用另一个插件RestfulTool



#### 10.RestfulTool

作用:提供RestfulTool列表展示所有接口，还支持接口查询(option + command + /)

但是这个快捷键与Comment with Block Comment的快捷键冲突，需要在preferences -> Keymap -> Main Menu -> Code -> Comment with Block Comment将(option + command + /)取消。



#### 11.CodeGlance

作用:可以在右侧预览代码，并且还可以放大



#### 12.Translation

作用:翻译英文



#### 13.Grep Console

作用:可以分颜色显示日志



#### 14.Rainbow Brackets

作用:彩虹色的括号



#### 15.GsonFromat

作用:可以将json转成实体类的字段



#### 16.Mybatis Log Plugin

作用:可以将mybatis执行的sql打印出来



#### 17.Free Mybatis Plugin

作用:可以方便的将mybatis的接口和xml关联起来，点击一下就可以互相跳转



#### 18. Git Commit template

作用:可以帮助我们很方便的写出commit记录











































