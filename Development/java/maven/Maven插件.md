[TOC]

## Maven插件



#### 1.git-commit-id-plugin

> github地址: https://github.com/git-commit-id/git-commit-id-maven-plugin

作用:可以在编译产出中包含git信息，方便找到对应的代码版本

方式一:自己配置
	增加插件仓库地址

```xml
	<pluginRepositories>
    <pluginRepository>
        <id>mvnrepository</id>
        <name>mvn Repository</name>
        <url>https://mvnrepository.com</url>
        <layout>default</layout>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <updatePolicy>daily</updatePolicy>
        </releases>
    </pluginRepository>
</pluginRepositories>
```



配置插件

```xml
	<plugin>
	<groupId>pl.project13.maven</groupId>
	<artifactId>git-commit-id-plugin</artifactId>
	<version>4.0.4</version>
	<executions>
	    <execution>
	        <id>get-the-git-infos</id>
	        <goals>
	            <goal>revision</goal>
	        </goals>
	        <phase>initialize</phase>
	    </execution>
	</executions>
	
	<configuration>
	    <dotGitDirectory>${project.basedir}/.git</dotGitDirectory>
	    <prefix>git</prefix>
	    <dateFormat>yyyy-MM-dd'T'HH:mm:ssZ</dateFormat>
	    <dateFormatTimeZone>${user.timezone}</dateFormatTimeZone>
	    <verbose>false</verbose>
	    <generateGitPropertiesFile>true</generateGitPropertiesFile>
	    <generateGitPropertiesFilename>${project.build.outputDirectory}/git.properties</generateGitPropertiesFilename>
	    <format>properties</format>
	    <skipPoms>true</skipPoms>
	    <injectAllReactorProjects>false</injectAllReactorProjects>
	    <failOnNoGitDirectory>false</failOnNoGitDirectory>
	    <failOnUnableToExtractRepoInfo>false</failOnUnableToExtractRepoInfo>
	    <skip>false</skip>
	    <offline>true</offline>
	    <runOnlyOnce>false</runOnlyOnce>
	    <abbrevLength>7</abbrevLength>
	    <gitDescribe>
	        <skip>false</skip>
	        <always>true</always>
	        <abbrev>7</abbrev>
	        <dirty>-dirty</dirty>
	        <match>*</match>
	        <tags>false</tags>
	        <forceLongFormat>false</forceLongFormat>
	    </gitDescribe>
	    <validationShouldFailIfNoMatch>false</validationShouldFailIfNoMatch>
	    <evaluateOnCommit>HEAD</evaluateOnCommit>
	    <useBranchNameFromBuildEnvironment>true</useBranchNameFromBuildEnvironment>
	    <injectIntoSysProperties>true</injectIntoSysProperties>
	</configuration>
</plugin>
```



方式二:如果使用springboot，springboot的父pom中已经在`pluginManagement`中给我们配置好了，我们只需要继承它就好了

```xml
<plugin>
  <groupId>pl.project13.maven</groupId>
  <artifactId>git-commit-id-plugin</artifactId>
</plugin>
```



需要注意的点有：

1. dotGitDirectory 参数是告诉插件.git目录位置，默认是${project.basedir}/.git 但这个值只适用于单模块项目，如果是多模块需要手动指定例如 ${project.basedir}/../.git 
2. offline 当设置为true时，插件将不会访问远程仓库，任何操作都会使用repo的本地状态。如果设置为false,它将会执行`git fetch`操作，例如确定`ahead`和`behind`分支信息，在5.xx版本之前，默认是false。
3. failOnNoGitDirectory 当找不到.git目录时插件是否应该失败，当设置为false并且没有找到.git目录时，插件将跳过执行
4. failOnUnableToExtractRepoInfo 当无法获得足够的数据来完成允许，是否构建失败

​        