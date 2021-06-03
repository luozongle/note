[TOC]
# JMH使用详解


#### 简介: 

​    JMH(Java Microbenchmark Harness)是一个Java工具，用于构建、运行和分析用Java和其他面向JVM的语言编写的 nano/micro/milli/macro基准测试，与Jmeter的使用场景不同，Jmeter一般是对REST API(Representational State Transfer)进行压测，而JMH关注的粒度更细，它更多的是发现某块代码性能槽点代码，然后对优化方案进行基准测试对比。

官网地址: <http://openjdk.java.net/projects/code-tools/jmh/>



时间单位

| 时间单位        | 英文        | 英文缩写                  |
| --------------- | ----------- | ------------------------- |
| 分              | minute      | min                       |
| 秒              | second      | sec                       |
| 分秒(0.1秒)     | decisecond  | ds                        |
| 厘秒(0.01秒)    | centisecond | cs                        |
| 毫秒(0.001秒)   | millisecond | ms                        |
| 微秒(0.001毫秒) | microsecond | μs                        |
| 纳秒(0.001微秒) | nanosecond  | ns                        |
| 皮秒(0.001纳秒) | picosecond  | ps                        |





用法:

我们在想要测试的基准方法上面加上`@Beanchmark`注解，一般来说，我们会把注解了`@Beanchmark`的方法看作基准的有效负载，即我们想要度量的东西。在同一个类中可以有多个基准方法。

如果基准方法不能结束，那么JMH也不能结束，如果基准方法里面抛出异常，那么JMH也将结束这个基准测试，继续运行下一个基准测试方法。

基本注意事项:

运行JMH基准测试的推荐方法是使用Maven设置依赖于应用程序jar文件的独立项目。这种方法是首选，以确保正确初始化基准并产生可靠的结果。可以在现有项目中或者IDE中运行基准测试，但是设置更复杂，结果更不可靠。



版本信息:

jdk:1.8

jmh:1.3.1

org.openjdk.jmh.annotations



例:

1.生成基准测试项目。

```zsh
mvn archetype:generate \
  -DinteractiveMode=false \
  -DarchetypeGroupId=org.openjdk.jmh \
  -DarchetypeArtifactId=jmh-java-benchmark-archetype \
  -DgroupId=org.sample \
  -DartifactId=test \
  -Dversion=1.0
```



2.建立基准

```zsh
cd test
mvn clean verify
```



3.运行基准测试(TestBenchmark是指定要测试的类)

```zsh
java -jar target/benchmarks.jar TestBenchmark 
```



启动的时候可以通过指定参数覆盖我们代码中对基准测试的配置

例:

```java
java -jar target/benchmarks.jar JMHSample_01_HelloWorld -bm=AverageTime -tu=ns -f=2 -wi=2 -i=2 -w=1 -r=1
  
# 具体参数说明可以使用-h参数查看
java -jar target/benchmarks.jar -h
```



在处理大型项目时，习惯上将基准放在单独的子项目中，然后通过通常的构件依赖项依赖于测试模块。







默认

warmup 10s迭代一次

measurement 迭代5次   10s迭代一次

每次迭代超时时间10min





### 常用注解

#### @Benchmark

例:

>

```java
@Benchmark
```



#### @Warmup

例:

> 基准测试后对代码预热总计5秒(迭代5次，每次1秒)。预热对于压测来说非常重要，如果没有预热过程，压测结果会很不准确。
>
> 作用范围:类、方法
```java
@Warmup(iterations = 5, time = 1, timeUnit = TimeUnit.SECONDS)
```

对应日志:
```log
# Warmup Iteration   1: 730.656 ns/op
# Warmup Iteration   2: 739.078 ns/op
# Warmup Iteration   3: 691.806 ns/op
# Warmup Iteration   4: 692.659 ns/op
# Warmup Iteration   5: 724.768 ns/op
```




#### @Measurement
例:
> 表示循环运行5次，每次1秒
>
> 作用范围:类、方法  
```java
@Measurement(iterations = 5, time = 1, timeUnit = TimeUnit.SECONDS)
```

对应日志:

```log
Iteration   1: 704.387 ns/op
Iteration   2: 702.701 ns/op
Iteration   3: 701.595 ns/op
Iteration   4: 699.627 ns/op
Iteration   5: 700.242 ns/op
```



#### @Fork
例:
> 表示fork多少个线程运行基准测试
>
> 作用范围:类、方法
```java
@Fork(2)
```

对应日志:

```log
# Threads: 1 thread, will synchronize iterations
...
# Fork: 1 of 2
...
# Fork: 2 of 2
```



#### @BenchmarkMode & @OutputTimeUnit
例:

> `@BenchmarkMode(Mode.AverageTime)`和`@OutputTimeUnit(TimeUnit.NANOSECONDS)`一起使用，@BenchmarkMode设置为AverageTime表示的是每次操作需要的平均时间，@OutputTimeUnit设置的是纳秒，所以基准测试单位是ns/op,意思是每次操作的纳秒平均时间
>
>BenchmarkMode可选值:
> 作用范围:类、方法
```java
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.NANOSECONDS)
```
BenchmarkMode中Mode可选值:

- Mode.Throughput    吞吐量(在有时间限制的迭代中连续调用基准方法，计算我们执行这个方法的次数)
- Mode.AverageTime  平均时间()
- Mode.SampleTime    采样时间(对执行时间进行采样，在这种模式下，仍然是在有限时间的迭代中运行方法，但是并不测量总时间，而是测量在某一些基准方法中花费的时间)
- Mode.SingleShotTime   单次触发时间(测量单个方法的调用时间。在这种模式下，迭代时间是没有意义的。如果不想连续调用基准方法时，此模式对于执行冷启动测试非常有用)
- Mode.All    使用所有的模式



OutputTimeUnit中TimeUnit中可选值

- TimeUnit.NANO_SCALE      纳秒(1000皮秒)
- TimeUnit.MICRO_SCALE      微秒(1000纳秒)
- TimeUnit.MILLI_SCALE         毫秒(1000微秒)
- TimeUnit.SECOND_SCALE   秒(1000毫秒)
- TimeUnit.MINUTE_SCALE    分钟(60秒)
- TimeUnit.HOUR_SCALE        小时(60分钟)
- TimeUnit.DAY_SCALE           天(24小时)




对应日志:

```log
# Benchmark mode: Average time, time/op
...
Result "org.sample.MyBenchmark.testMethod":
  824.612 ±(99.9%) 9.119 ns/op [Average]
  (min, avg, max) = (816.520, 824.612, 842.166), stdev = 8.530
  CI (99.9%): [815.492, 833.731] (assumes normal distribution)
...
Benchmark                                    Mode     Cnt       Score       Error   Units
MyBenchmark.testMethod                       avgt      10     693.803 ±     8.659   ns/op
```



#### @State
例:
>
>
> 目标:类、接口、注解、枚举

```java
@State(Scope.Benchmark)
```

@State中Scope可选值
- Benchmark  相同类型的实例将在所有工作线程之间共享
- Group 相同类型的实例将在同一组中的所有线程间共享，每个线程组都将提供自己的状态对象
- Thread 相同类型的实例都是不同的，即使在同一个基准中注入了多个状态对象


#### @Setup
例:

@Setup中Level可选值
- Trial
- Iteration
- Invocation




#### @TearDown
例:









