[TOC]



### 生命周期

简介:

> Maven的生命周期是抽象的，这意味着生命周期本身不做任何实际的工作，在Maven的设计中，实际的任务(如编译源代码)都交由插件来完成。这种思想与设计模式中的模板方法(Template Method)非常相似。模板方法模式在父类中定义算法的整体结构，子类可以通过实现或者重写父类的方法来控制实际的行为，这样既保证了算法有足够的可扩展性，又能够严格控制算法的整体结构。
>
> Maven有三套独立的生命周期，它们分别是clean、default和site。 clean生命周期的目的是清理项目，default生命周期的目的是构建项目，而site生命周期的目的是建立项目站点。
>
> 每套生命周期都包含一些阶段(phase)，这些阶段是有顺序的，并且后面的阶段依赖于前面的阶段，用于和Maven最直接的交互方式就是调用这些生命周期阶段。以clean生命周期为例，它包含的阶段有pre-clean、clean和post-clean。当用户调用clean的时候，只有pre-clean阶段得以执行;当调用clean的时候，pre-clean和clean阶段会得以顺序执行
>
> 较之于生命周期阶段的前后依赖关系，三套生命周期本身是相互独立的，用户可以仅仅调用clean声明周期的某个阶段，或者仅仅调用default声明周期的某个阶段，而不会对其他声明周期产生任何影响。例如，当用户调用clean生命周期的clean阶段时，不会触发defualt声明周期的任何阶段，反之亦然。



#### clean生命周期

> clean生命周期的目的是清理项目，它包含三个阶段

**pre-clean ** 执行一些清理前需要完成的工作

**clean** 清理上一次构建生成的文件

**post-clean** 执行一些清理后需要完成的工作



#### default生命周期

> default生命周期定义了真正构建时所需要执行的所有步骤，它是所有生命周期中最核心的部分，其包含的部分阶段如下


**validate**
**initialize**
**generate-sources**
**process-sources**
**process-sources** 处理项目主资源文件。一般来说，是对src/main/resources目录的内容进行变量替换等工作后，复制到项目输出的主classpath目录中
**generate-resources**
**compile** 编译项目的主源码。一般来说，是编译src/main/java目录下的Java文件至项目输出的主classpath目录中
**process-classes**
**generate-test-sources**
**process-test-sources** 处理项目测试资源文件。一般来说，是对src/test/resources目录的内容进行变量替换等工作后，复制到项目输出的测试classpath目录中。
**generate-test-resources**
**process-test-resources**
**test-compile** 编译项目的测试代码。一般来说，是编译src/test/java目录下的Java文件至项目输出的测试classpath目录中
**process-test-classes**
**test** 使用单元测试框架运行测试，测试代码不会被打包或部署
**prepare-package**
**package** 接受编译好的代码，打包成可发布的格式，如JAR
**pre-integration-test**
**integration-test**
**post-integration-test**
**verify**
**install** 将包安装到Maven本地仓库，供本地其他Maven项目使用
**deploy** 将最终的包复制到远程仓库，供其他开发人员和Maven项目使用



#### site生命周期

> site生命周期的目的是建立和发布项目站点，Maven能够基于POM所包含的信息，自动生成一个友好的站点，方便团队交流和发布项目信息。

**pre-site** 执行一些在生成项目站点之前需要完成的工作
**site** 生成项目站点文档
**post-site** 执行一些在生成项目站点之后需要完成的工作
**site-deploy** 将生成的项目站点发布到服务器上





### 约定优于配置

这些配置都在maven的超级POM中，任务一个Maven项目都隐式地继承超级POM

对于maven3 超级POM位于  $MAVEN_HOME/lib/maven-model-builder-3.6.3.jar/org/apache/maven/model/pom-4.0.0.xml



#### 目录结构

maven会假设用户的项目是这样的

源码目录为 ${project.basedir}/src/main/java

编译输出目录为 ${project.build.directory}/classes

打包方式为 jar

包输出目录为 ${project.basedir}/targe

包名为 ${project.artifactId}-${project.version}

资源目录为 ${project.basedir}/src/main/resources





## Maven属性

有时候我们希望spring的配置文件可以读取maven的pom.xml



例:

> pom.xml中 配置多个profile
>
> 因为maven属性只有在pom中才会被解析，所以需要让Maven解析资源文件中的Maven属性。
>
> 资源文件处理其实是maven-resources-plugin做的事情，它的默认行为只是将项目主资源文件复制到主代码编译输出目录中，将测试资源文件复制到测试代码编译输出目录中。不过只要开启资源过滤，该插件就能解析资源文件中的Maven属性。
>
> Maven默认的主资源目录和测试资源目录的定义时在超级POM中。要为资源目录开启过滤，只要在此基础上增加一行filtering配置即可
>
> 激活profile可以根据命令行、setting文件显式激活、系统属性激活、系统属性且值确定时激活、操作系统环境激激活、文件是否存在激活、默认激活
>
> 命令激活使用-P 例: `mvn clean install -Ponline`

```xml
<project>
...
  
  <profiles>
    <profile>
      <id>dev</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>

      <properties>
        <profile.active>dev</profile.active>
      </properties>
    </profile>


    <profile>
      <id>online</id>
      <properties>
        <profile.active>k8s</profile.active>
      </properties>
    </profile>
  </profiles>
  
  ...
  
  <build>
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
      </resource>
    </resources>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
  
...
</project>
```



> spring配置文件中

```properties
logging.config=classpath:logback-@profile.active@.xml
```







































