# Lombok

[TOC]



### lombok.config

简介:

lombok.config可以在整个项目甚至整个工作区配置lombok功能

lombok.config是在lombok1.14中引入的

lombok.config可以在任何目录中创建文件并将配置指令放入其中，适用于此目录和子目录中的所有源文件

lombok.config中可以包含注释 任何以#开头都被视为注释



在lombok v1.18.12中，还可以使用在配置文件顶部使用import导入文件，导入的文件不必被调用lombok.config，扩展名可以是任意

```config
import ../configuration/model.confg
或者
import /etc/lombok/model.config
```





常用配置

```config
clear config.stopbubbling
config.stopbubbling=true
lombok.accessors.chain=true
lombok.toString.callSuper=CALL
lombok.equalsAndHashCode.callSuper=CALL
lombok.addLombokGeneratedAnnotation=true
```

> lombok.addLombokGeneratedAnnotation=true 主要为了生成@lombok.Generated  对 JaCoCo（具有内置支持）或其他样式检查器和代码覆盖率工具有用 可以增加单测覆盖率



查询lombok版本支持的所有配置列表

```bash
java -jar lombok.jar config -g -v
```





```tex
##
## Key : checkerframework
## Type: checkerframework-version
##
## If set with the version of checkerframework.org (in major.minor, or just 'true' for the latest supported version), create relevant checkerframework.org annotations for code lombok generates (default: false).
##
## Examples:
#
# clear checkerframework
# checkerframework = major.minor (example: 2.9 - and no higher than 3.0) or true or false
#

##
## Key : config.stopBubbling
## Type: boolean
##
## Tell the configuration system it should stop looking for other configuration files (default: false).
## 告诉lombok这是根目录，应该停止寻找其它配置文件(默认false)
##
## Examples:
#
# clear config.stopBubbling
# config.stopBubbling = [false | true]
#

##
## Key : lombok.accessors.chain
## Type: boolean
##
## Generate setters that return 'this' instead of 'void' (default: false).
## 生成返回 'this' 而不是 void的setter
##
## Examples:
#
# clear lombok.accessors.chain
# lombok.accessors.chain = [false | true]
#

##
## Key : lombok.accessors.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Accessors is used.
## 如果使用@Accessors则发出警告(自测WARNING没生效,ERROR只有编译时才报)
##
## Examples:
#
# clear lombok.accessors.flagUsage
# lombok.accessors.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.accessors.fluent
## Type: boolean
##
## Generate getters and setters using only the field name (no get/set prefix) (default: false).
## 仅使用字段名，没有(get/set前缀)生成getter setter方法
## 
## Examples:
#
# clear lombok.accessors.fluent
# lombok.accessors.fluent = [false | true]
#

##
## Key : lombok.accessors.prefix
## Type: list of string
##
## Strip this field prefix, like 'f' or 'm_', from the names of generated getters and setters.
## 从生成的getter和setter的名称中去掉这个字段前缀比如'f' 或 'm_'
##
## Examples:
#
# clear lombok.accessors.prefix
# lombok.accessors.prefix += <text>
# lombok.accessors.prefix -= <text>
#

##
## Key : lombok.addGeneratedAnnotation
## Type: boolean
##
## Generate @javax.annotation.Generated on all generated code (default: false). Deprecated, use 'lombok.addJavaxGeneratedAnnotation' instead.
## 对生成的代码使用 @javax.annotation.Generated 默认值flase 已经过时
## 推荐使用lombok.addJavaxGeneratedAnnotation
##
## Examples:
#
# clear lombok.addGeneratedAnnotation
# lombok.addGeneratedAnnotation = [false | true]
#

##
## Key : lombok.addJavaxGeneratedAnnotation
## Type: boolean
##
## Generate @javax.annotation.Generated on all generated code (default: follow lombok.addGeneratedAnnotation).
## 对生成的代码使用 @javax.annotation.Generated
##
## Examples:
#
# clear lombok.addJavaxGeneratedAnnotation
# lombok.addJavaxGeneratedAnnotation = [false | true]
#

##
## Key : lombok.addLombokGeneratedAnnotation
## Type: boolean
##
## Generate @lombok.Generated on all generated code (default: false).
## 对生成的代码生成 @lombok.Generated
##
## Examples:
#
# clear lombok.addLombokGeneratedAnnotation
# lombok.addLombokGeneratedAnnotation = [false | true]
#

##
## Key : lombok.allArgsConstructor.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @AllArgsConstructor is used.
## 如果使用@AllArgsConstructor 则发出警告
##
## Examples:
#
# clear lombok.allArgsConstructor.flagUsage
# lombok.allArgsConstructor.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.anyConstructor.addConstructorProperties
## Type: boolean
##
## Generate @ConstructorProperties for generated constructors (default: false).
## 为生成的函数生成@ConstructorProperties
##
## Examples:
#
# clear lombok.anyConstructor.addConstructorProperties
# lombok.anyConstructor.addConstructorProperties = [false | true]
#

##
## Key : lombok.anyConstructor.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if any of the XxxArgsConstructor annotations are used.
## 对于任何使用XxxArgsConstructor的注解则发出警告
## 
## Examples:
#
# clear lombok.anyConstructor.flagUsage
# lombok.anyConstructor.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.anyConstructor.suppressConstructorProperties
## Type: boolean
##
## Suppress the generation of @ConstructorProperties for generated constructors (default: false).
## 禁止为生成的构造函数生成 @ConstructorProperties
##
## Examples:
#
# clear lombok.anyConstructor.suppressConstructorProperties
# lombok.anyConstructor.suppressConstructorProperties = [false | true]
#

##
## Key : lombok.builder.className
## Type: string
##
## Default name of the generated builder class. A * is replaced with the name of the relevant type (default = *Builder).
## 生成的生成器类默认名称，默认为*Builder
##
## Examples:
#
# clear lombok.builder.className
# lombok.builder.className = <text>
#

##
## Key : lombok.builder.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Builder is used.
## 为使用@Builder注解的发出警告
## Examples:
#
# clear lombok.builder.flagUsage
# lombok.builder.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.cleanup.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Cleanup is used.
## 为使用@Cleanup注解的发出警告
## Examples:
#
# clear lombok.cleanup.flagUsage
# lombok.cleanup.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.copyableAnnotations
## Type: list of type-name
##
## Copy these annotations to getters, setters, with methods, builder-setters, etc.
##
## Examples:
#
# clear lombok.copyableAnnotations
# lombok.copyableAnnotations += <fully.qualified.Type>
# lombok.copyableAnnotations -= <fully.qualified.Type>
#

##
## Key : lombok.data.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Data is used.
##
## Examples:
#
# clear lombok.data.flagUsage
# lombok.data.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.delegate.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Delegate is used.
##
## Examples:
#
# clear lombok.delegate.flagUsage
# lombok.delegate.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.equalsAndHashCode.callSuper
## Type: enum (lombok.core.configuration.CallSuperType)
##
## When generating equals and hashCode for classes that extend something (other than Object), either automatically take into account superclass implementation (call), or don't (skip), or warn and don't (warn). (default = warn).
## 当为子类生成equals和hashCode时，是否考虑父类实现(call) 不考虑(skip) 或者警告(warn)
##
## Examples:
#
# clear lombok.equalsAndHashCode.callSuper
# lombok.equalsAndHashCode.callSuper = [CALL | SKIP | WARN]
#

##
## Key : lombok.equalsAndHashCode.doNotUseGetters
## Type: boolean
##
## Don't call the getters but use the fields directly in the generated equals and hashCode method (default = false).
## 生成equals和hashCode是不使用getter 直接使用字段
## Examples:
#
# clear lombok.equalsAndHashCode.doNotUseGetters
# lombok.equalsAndHashCode.doNotUseGetters = [false | true]
#

##
## Key : lombok.equalsAndHashCode.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @EqualsAndHashCode is used.
##
## Examples:
#
# clear lombok.equalsAndHashCode.flagUsage
# lombok.equalsAndHashCode.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.experimental.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if an experimental feature is used.
##
## Examples:
#
# clear lombok.experimental.flagUsage
# lombok.experimental.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.extensionMethod.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @ExtensionMethod is used.
##
## Examples:
#
# clear lombok.extensionMethod.flagUsage
# lombok.extensionMethod.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.extern.findbugs.addSuppressFBWarnings
## Type: boolean
##
## Generate @edu.umd.cs.findbugs.annotations.SuppressFBWarnings on all generated code (default: false).
##
## Examples:
#
# clear lombok.extern.findbugs.addSuppressFBWarnings
# lombok.extern.findbugs.addSuppressFBWarnings = [false | true]
#

##
## Key : lombok.fieldDefaults.defaultFinal
## Type: boolean
##
## If true, fields, in any file (lombok annotated or not) are marked as final. Use @NonFinal to override this.
##
## Examples:
#
# clear lombok.fieldDefaults.defaultFinal
# lombok.fieldDefaults.defaultFinal = [false | true]
#

##
## Key : lombok.fieldDefaults.defaultPrivate
## Type: boolean
##
## If true, fields without any access modifier, in any file (lombok annotated or not) are marked as private. Use @PackagePrivate or an explicit modifier to override this.
##
## Examples:
#
# clear lombok.fieldDefaults.defaultPrivate
# lombok.fieldDefaults.defaultPrivate = [false | true]
#

##
## Key : lombok.fieldDefaults.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @FieldDefaults is used.
##
## Examples:
#
# clear lombok.fieldDefaults.flagUsage
# lombok.fieldDefaults.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.fieldNameConstants.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @FieldNameConstants is used.
##
## Examples:
#
# clear lombok.fieldNameConstants.flagUsage
# lombok.fieldNameConstants.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.fieldNameConstants.innerTypeName
## Type: identifier-name
##
## The default name of the inner type generated by @FieldNameConstants. (default: 'Fields').
##
## Examples:
#
# clear lombok.fieldNameConstants.innerTypeName
# lombok.fieldNameConstants.innerTypeName = <javaIdentifier>
#

##
## Key : lombok.fieldNameConstants.uppercase
## Type: boolean
##
## The default name of the constants inside the inner type generated by @FieldNameConstants follow the variable name precisely. If this config key is true, lombok will uppercase them as best it can. (default: false).
##
## Examples:
#
# clear lombok.fieldNameConstants.uppercase
# lombok.fieldNameConstants.uppercase = [false | true]
#

##
## Key : lombok.getter.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Getter is used.
##
## Examples:
#
# clear lombok.getter.flagUsage
# lombok.getter.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.getter.lazy.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Getter(lazy=true) is used.
##
## Examples:
#
# clear lombok.getter.lazy.flagUsage
# lombok.getter.lazy.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.getter.noIsPrefix
## Type: boolean
##
## If true, generate and use getFieldName() for boolean getters instead of isFieldName().
##
## Examples:
#
# clear lombok.getter.noIsPrefix
# lombok.getter.noIsPrefix = [false | true]
#

##
## Key : lombok.helper.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Helper is used.
##
## Examples:
#
# clear lombok.helper.flagUsage
# lombok.helper.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.apacheCommons.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @CommonsLog is used.
##
## Examples:
#
# clear lombok.log.apacheCommons.flagUsage
# lombok.log.apacheCommons.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.custom.declaration
## Type: custom-log-declaration
##
## Define the generated custom logger field.
##
## Examples:
#
# clear lombok.log.custom.declaration
# lombok.log.custom.declaration = my.cool.Logger my.cool.LoggerFactory.createLogger()(TOPIC,TYPE)
#

##
## Key : lombok.log.custom.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @CustomLog is used.
##
## Examples:
#
# clear lombok.log.custom.flagUsage
# lombok.log.custom.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.fieldIsStatic
## Type: boolean
##
## Make the generated logger fields static (default: true).
## 将生成的日志记录属性设置为静态
##
## Examples:
#
# clear lombok.log.fieldIsStatic
# lombok.log.fieldIsStatic = [false | true]
#

##
## Key : lombok.log.fieldName
## Type: identifier-name
##
## Use this name for the generated logger fields (default: 'log').
## 设置生成的日志属性名称
## Examples:
#
# clear lombok.log.fieldName
# lombok.log.fieldName = <javaIdentifier>
#

##
## Key : lombok.log.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if any of the log annotations is used.
##
## Examples:
#
# clear lombok.log.flagUsage
# lombok.log.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.flogger.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Flogger is used.
##
## Examples:
#
# clear lombok.log.flogger.flagUsage
# lombok.log.flogger.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.javaUtilLogging.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Log is used.
##
## Examples:
#
# clear lombok.log.javaUtilLogging.flagUsage
# lombok.log.javaUtilLogging.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.jbosslog.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @JBossLog is used.
##
## Examples:
#
# clear lombok.log.jbosslog.flagUsage
# lombok.log.jbosslog.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.log4j.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Log4j is used.
##
## Examples:
#
# clear lombok.log.log4j.flagUsage
# lombok.log.log4j.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.log4j2.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Log4j2 is used.
##
## Examples:
#
# clear lombok.log.log4j2.flagUsage
# lombok.log.log4j2.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.slf4j.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Slf4j is used.
##
## Examples:
#
# clear lombok.log.slf4j.flagUsage
# lombok.log.slf4j.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.log.xslf4j.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @XSlf4j is used.
##
## Examples:
#
# clear lombok.log.xslf4j.flagUsage
# lombok.log.xslf4j.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.noArgsConstructor.extraPrivate
## Type: boolean
##
## Generate a private no-args constructor for @Data and @Value (default: true).
##
## Examples:
#
# clear lombok.noArgsConstructor.extraPrivate
# lombok.noArgsConstructor.extraPrivate = [false | true]
#

##
## Key : lombok.noArgsConstructor.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @NoArgsConstructor is used.
##
## Examples:
#
# clear lombok.noArgsConstructor.flagUsage
# lombok.noArgsConstructor.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.nonNull.exceptionType
## Type: enum (lombok.core.configuration.NullCheckExceptionType)
##
## The type of the exception to throw if a passed-in argument is null (Default: NullPointerException).
##
## Examples:
#
# clear lombok.nonNull.exceptionType
# lombok.nonNull.exceptionType = [NullPointerException | IllegalArgumentException | Assertion | JDK | GUAVA]
#

##
## Key : lombok.nonNull.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @NonNull is used.
##
## Examples:
#
# clear lombok.nonNull.flagUsage
# lombok.nonNull.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.onX.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if onX is used.
##
## Examples:
#
# clear lombok.onX.flagUsage
# lombok.onX.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.requiredArgsConstructor.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @RequiredArgsConstructor is used.
##
## Examples:
#
# clear lombok.requiredArgsConstructor.flagUsage
# lombok.requiredArgsConstructor.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.setter.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Setter is used.
##
## Examples:
#
# clear lombok.setter.flagUsage
# lombok.setter.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.singular.auto
## Type: boolean
##
## If true (default): Automatically singularize the assumed-to-be-plural name of your variable/parameter when using {@code @Singular}.
##
## Examples:
#
# clear lombok.singular.auto
# lombok.singular.auto = [false | true]
#

##
## Key : lombok.singular.useGuava
## Type: boolean
##
## Generate backing immutable implementations for @Singular on java.util.* types by using guava's ImmutableList, etc. Normally java.util's mutable types are used and wrapped to make them immutable.
##
## Examples:
#
# clear lombok.singular.useGuava
# lombok.singular.useGuava = [false | true]
#

##
## Key : lombok.sneakyThrows.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @SneakyThrows is used.
##
## Examples:
#
# clear lombok.sneakyThrows.flagUsage
# lombok.sneakyThrows.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.superBuilder.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @SuperBuilder is used.
##
## Examples:
#
# clear lombok.superBuilder.flagUsage
# lombok.superBuilder.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.synchronized.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Synchronized is used.
##
## Examples:
#
# clear lombok.synchronized.flagUsage
# lombok.synchronized.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.toString.callSuper
## Type: enum (lombok.core.configuration.CallSuperType)
##
## When generating toString for classes that extend something (other than Object), either automatically take into account superclass implementation (call), or don't (skip), or warn and don't (warn). (default = warn).
## 为子类生成toString时，考虑超类实现(call) 不考虑(skip) 不考虑并警告(warn)
## Examples:
#
# clear lombok.toString.callSuper
# lombok.toString.callSuper = [CALL | SKIP | WARN]
#

##
## Key : lombok.toString.doNotUseGetters
## Type: boolean
##
## Don't call the getters but use the fields directly in the generated toString method (default = false).
## 生成toString是 不使用getter方法
## Examples:
#
# clear lombok.toString.doNotUseGetters
# lombok.toString.doNotUseGetters = [false | true]
#

##
## Key : lombok.toString.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @ToString is used.
##
## Examples:
#
# clear lombok.toString.flagUsage
# lombok.toString.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.toString.includeFieldNames
## Type: boolean
##
## Include the field names in the generated toString method (default = true).
##
## Examples:
#
# clear lombok.toString.includeFieldNames
# lombok.toString.includeFieldNames = [false | true]
#

##
## Key : lombok.utilityClass.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @UtilityClass is used.
##
## Examples:
#
# clear lombok.utilityClass.flagUsage
# lombok.utilityClass.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.val.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if 'val' is used.
##
## Examples:
#
# clear lombok.val.flagUsage
# lombok.val.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.value.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @Value is used.
##
## Examples:
#
# clear lombok.value.flagUsage
# lombok.value.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.var.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if 'var' is used.
##
## Examples:
#
# clear lombok.var.flagUsage
# lombok.var.flagUsage = [WARNING | ERROR | ALLOW]
#

##
## Key : lombok.with.flagUsage
## Type: enum (lombok.core.configuration.FlagUsageType)
##
## Emit a warning or error if @With is used.
##
## Examples:
#
# clear lombok.with.flagUsage
# lombok.with.flagUsage = [WARNING | ERROR | ALLOW]
#
```







