[TOC]

# Maven安装文档



### 环境信息

系统版本:macOS Big Sur 11.2.2

JDK版本:Open JDK 1.8.0_292  (Zulu 8.54.0.21-CA-macos-aarch64)

Maven版本:3.8.1



## 一、环境检查

### 1.检查JDK的安装

```shell
echo $JAVA_HOME
java -version
```





## 二、安装maven



### 1.解压下载好的maven压缩包

```shell
tar -xvf apache-maven-3.8.1-bin.tar
```



### 2.将maven的目录配置到PATH变量中

```shell
export PATH=$PATH:/opt/apache-maven-3.8.1/bin
```



### 3.配置settings.xml
Maven用户可以选择配置$M2_HOME/conf/settings.xml或者~/.m2/settings.xml。前者是全局范围的，整台机器上的所有用户都会受到该配置的影响，而后者是用户范围内的，只有当前用户才会受到该配置的影响。

推荐使用用户范围的settings.xml，主要是为了避免无意识的影响到系统中的其他用户。如果有切实的需求，需要统一系统中所有用户的settings.xml配置，当然应该使用全局范围的settings.xml。

除了影响范围这一因素，配置用户范围settings.xml文件还便于maven升级。直接修改conf目录下的settings.xml会导致Maven升级不便，每次升级到新版本的maven，都需要复制settings.xml。如果使用~/.m2目录下的settings.xml，就不会影响到maven安装文件，升级时就不需要触动settings.xml文件。



#### 3.1 配置本地仓库
默认情况下，不管windows还是linux或者Mac，每个用户在自己的用户目录下都有一个路径名为.m2/repository/的仓库目录。如果想自定义本地仓库目录地址。这时，可以编辑 settings.xml文件，设置localRepostiroy元素的值为想要的仓库地址。例如:

``` xml
<settings>
	<localRepository>/Data/apache/maven/repository</localRepository>
</settings>
```

但是默认情况下，~/.m2/settings.xml文件是不存在的，用户需要从Maven安装目录复制$M2_HOME/conf/settings.xml文件再进行编辑。 

安装好Maven后，如果不执行任何Maven命令，本地仓库目录是不存在的。当用户输入第一条Maven命令之后，Maven才会创建本地仓库，然后根据配置和需要，从远程仓库下载构件至本地仓库。



#### 3.2 配置远程仓库

##### 中央仓库介绍:

由于最原始的本地仓库是空的，Maven必须知道至少一个可用的远程仓库，才能在执行Maven命令的时候下载到需要的构件。中央仓库就是这样一个默认的远程仓库，Maven的安装文件自带了中央仓库的配置。可以通过查看jar文件 `$M2_HOME/lib/maven-model-builder-3.8.1.jar` 中的`org/apache/maven/model/pom-4.0.0.xml` 可以看到

``` xml
<repositories>
  <repository>
    <id>central</id>
    <name>Central Repository</name>
    <url>https://repo.maven.apache.org/maven2</url>
    <layout>default</layout>
    <snapshots>
      <enabled>false</enabled>
    </snapshots>
  </repository>
</repositories>
```

包含上面这段配置的文件是所有Maven项目都会继承的超级POM,这段配置使用id central对中央仓库进行唯一标识。



##### 在settings.xml中配置自定义远程仓库

```xml
<project>
 ...
  <repositorys>
  	<repository>
      <id>jboss</id>
      <name>JBoss Repository</name>
      <url>http://repository.jboss.com/maven2</url>
      <release>
      	<enabled>true</enabled>
        <updatePolicy>daily</updatePolicy>
        <checksumPolicy>ignore</checksumPolicy>
      </release>
      <snapshots>
      	<enabled>false</enabled>
      </snapshots>
      <layout>default</layout>
    </repository>
  </repositorys>
</project>
```

解释: 在**repositories**元素下，可以使用**repository**子元素声明一个或者多个远程仓库，该例中声明了一个id为jboss，名称为JBoss Repository的仓库。任何一个仓库声明的id必须是唯一的。尤其需要注意的是，maven自带的中央仓库使用的id为central，如果其他的仓库的声明也使用该id,就会覆盖中央仓库的配置。该配置中的url值指向了仓库的地址，一般来说，该地址都基于http协议，Maven用户都可以在浏览器中打开仓库地址浏览构件。

**release**和**snapshots**元素用来可控制Maven对于发布版构件和快照版构件的下载。

**layout**元素值**default**表示仓库的布局是Maven2及Maven3的默认布局，而不是Maven1的布局。

对于**release**和**snapshots**来说，除了**enabled**,它们还包含另外两个子元素**updatePolicy**和**checksumPolicy**

元素**updatePolicy**用来配置maven从远程仓库检查更新的频率，默认的值是daily,表示Maven每天检查一次。其他可用的值包括:

never -从不检查更新，always -每次构建都检查更新，interval -每隔X分钟检查一次更新(X为任意整数)

元素**checksumPolicy**用来配置Maven检查检验和文件的策略。当构件被部署到Maven仓库中时，会同时部署对应的校验和文件。在下载构件的时候，Maven会验证校验和文件，如果校验和验证失败，会怎么办? 当checksumPolicy的值为默认warn时，maven会在执行构件时输出警告信息，其他可用的值包括:  fail -maven遇到校验和错误就让构建失败，ignore -使Maven完全忽略校验和错误。



#### 3.3 远程仓库的认证

大部分远程仓库无需认证就可以访问，但有时候出于安全方面的考虑，我们需要提供认证信息才能访问一些远程仓库。配置认证信息和配置仓库信息不同，仓库信息可以直接配置在POM文件中，但是认证信息必须配置在settings.xml中。这是因为POM往往是被提交到代码仓库中供所有成员访问的，而settings.xml一般只放在本机。因此，在settings.xml中配置认证信息更为安全。

例:给id为my-proj的仓库配置认证信息

```xml
<settings>
  ...
  <servers>
  	<server>
    	<id>my-porj</id>
      <username>repo-user</username>
      <password>repo-pwd/</password>
    </server>
  </servers>
</settings>
```



#### 3.4 配置镜像

如果仓库X可以提供仓库Y存储的所有内容，那么就可以认为X是Y的一个镜像。换句话说，任何一个可以从仓库Y获得的构件，都能够从它的镜像中获取。

```xml
<settings>
...
  <mirrors>
  	<mirror>
    	<id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexux/content/groups/public</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
  </mirrors>
</settings>
```

该例子中，&lt;mirrorOf&gt;的值为central，表示该配置为中央仓库的镜像，任何对于中央仓库的请求都会转至该镜像。另外三个元素 id、name、url与一般仓库配置无异，表示该镜像仓库的唯一标识符、名称及地址。类似的，如果该镜像需要认证，也可以基于该id配置仓库认证。

关于镜像的一个更为常见的用法是结合私服。由于私服可以代理任何外部的公共仓库(包括中央仓库)，因此，对于组织内部的Maven用户来说，使用一个私服地址就等于使用了所有需要的外部仓库，这可以将配置集中到私服，从而简化Maven本身的配置。这种情况下，任何需要的构件都可以从私服获得，私服就是所有仓库的镜像。

例:

```xml
<settings>
...
  <mirrors>
  	<mirror>
    	<id>internal-repository</id>
      <name>Internal Repository Manager</name>
      <url>http://192.168.1.100/maven2</url>
      <mirrorOf>*</mirrorOf>
    </mirror>
  </mirrors>
</settings>
```

该例中 &lt;mirrorOf&gt;的值为星号，表示该配置是所有Maven仓库的镜像，任何对于远程仓库的请求都会被转至http://192.168.1.100/maven2。如果该镜像需要认证，则配置一个id为intrnal-repository的&lt;server&gt;即可。

&lt;mirrorOf&gt;*&lt;/mirrorOf&gt; :匹配所有远程仓库

&lt;mirrorOf&gt; external:*&lt;/mirrorOf&gt; :匹配所有远程仓库，使用localhost的除外，使用file://协议的除外。也就是说匹配所有不在本机上的远程仓库

&lt;mirrorOf&gt;repo1,repo2&lt;/mirrorOf&gt; : 匹配仓库repo1,repo2，使用逗号分隔多个远程仓库。

&lt;mirrorOf&gt;*,!repo1&lt;/mirrorOf&gt; : 匹配所有远程仓库，repo1除外，使用感叹号将仓库从匹配中排除。

需要注意的是，由于镜像仓库完全屏蔽了被镜像仓库，当镜像仓库不稳定或者停止服务的时候，Maven仍将无法访问被镜像仓库，因而将无法下载构件。



### 4.修改 $M2_HOME/conf/settings.xml

Maven从版本3.8.1起默认阻止外部HTTP存储库（请参见https://maven.apache.org/docs/3.8.1/release-notes.html）

所以需要下面的xml注释掉 否则无法访问http的maven仓库下载控件

```xml
<mirror>
  <id>maven-default-http-blocker</id>
  <mirrorOf>external:http:*</mirrorOf>
  <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
  <url>http://0.0.0.0/</url>
  <blocked>true</blocked>
</mirror>
```







## 三、问题排查

### 1.如果用命令编译失败

可以使用 mvn -e 查看错误的完整堆栈跟踪

或者使用 -X 开关打开完整的debug日志记录



### 2.如果mvn命令编译是成功的，但是idea中依然有依赖相关问题

2.1.重启idea

2.2.mvn  idea:idea     # 为当前项目创建/更新IDEA工作区（将各个模块创建为IDEA模块）

2.3.File -> Invalidate Caches



mvn idea:idea这个插件其实已经过期了，但有时候还是有点用的(这是一个Maven插件，用于为Maven项目生成IntelliJ IDEA文件。不是用于Maven的IDEA插件)

这个插件有五个目标

- idea:idea  # 用于执行此插件的其它三个目标:project, module 和 workspace
- idea:project # 用于生成IntelliJ IDEA项目所需的项目文件(*.ipr)
- idea:module # 用于生成IntelliJ IDEA模块所需的模块文件(*.iml)
- idea:workspace # 用于生成IntelliJ IDEA项目所需的工作区文件(*.iws)
- idea:clean # 用于删除与IntelliJ IDEA相关的文件。

































