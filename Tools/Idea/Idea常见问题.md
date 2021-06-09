[TOC]

# Idea常见问题



## 一、代码显示警告

如果代码提示警告，我们需要禁用或禁止代码检查(inspections)

#### 禁用检查

##### 1.在设置中禁用检查

```Preferences -> Editor -> Inspections  找到对应的检查关闭```

##### 2.在编辑器中禁用检查

在要禁用的行上按`⌥⏎` -> Inspections ...禁用相关检查



#### 禁止检查

在Java中取消检查类、方法或字段，需要使用`@SuppressWarnings`注解。

对于语句使用`//noinspection`添加注释



可以直接在要禁止的行上按`⌥⏎` -> Inspections ...禁止相关检查





## 二、Idea提示缺少依赖

解决步骤:

### 1.先使用mvn命令进行编译判断是否正常

```shell
mvn clean package -DskipTests
```
如果命令也无法编译成功，则排查maven是否能连接远程仓库，远程仓库是否有对应依赖


### 2.如果mvn命令编译是成功的，但是idea中依然有依赖相关问题，尝试以下步骤

2.1.重启idea   

2.2.`mvn  idea:idea`     # 为当前项目创建/更新IDEA工作区（将各个模块创建为IDEA模块）

2.3 删除当前项目的.idea目录

2.3.File -> Invalidate Caches(成本最高，会导致所有项目重新加载)



mvn idea:idea这个插件其实已经过期了，但有时候还是有点用的(这是一个Maven插件，用于为Maven项目生成IntelliJ IDEA文件。不是用于Maven的IDEA插件)

这个插件有五个目标

- idea:idea  # 用于执行此插件的其它三个目标:project, module 和 workspace
- idea:project # 用于生成IntelliJ IDEA项目所需的项目文件(*.ipr)
- idea:module # 用于生成IntelliJ IDEA模块所需的模块文件(*.iml)
- idea:workspace # 用于生成IntelliJ IDEA项目所需的工作区文件(*.iws)
- idea:clean # 用于删除与IntelliJ IDEA相关的文件。