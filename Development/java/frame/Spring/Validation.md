

引入依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```





常用注解:

- @AssertFalse
  - 注解的元素必须为false (允许为null)
  - 支持boolean和Boolean
- @AssertTrue
  - 注解的元素必须为true (允许为null)
  - 支持boolean和Boolean
- @DecimalMax
  - 注解的元素必须是一个数字,元素必须小于或等于指定的maximum,inclusive参数可以指定是否包含maximum值 (允许为null)
  - 支持BigDecimal、BigInteger、CharSequence、byte、short、int、long和它们的包装类
- @DecimalMin
  - 注解的元素必须是一个数字,元素必须大于或等于指定的minimum,inclusive参数可以指定是否包含minimum值 (允许为null)
  - 支持BigDecimal、BigInteger、CharSequence、byte、short、int、long和它们的包装类
- @Digits
  - 注解的元素必须是一个数字，元素必须是允许的精度 integer整数精度  fraction小数精度 (允许为null)
  - 支持BigDecimal、BigInteger、CharSequence、byte、short、int、long和它们的包装类
- @Email
  - 注解的元素必须是一个合法的email地址 (允许为null)
  - 支持CharSequence
- @Future
  - 注解元素必须是未来的某个时刻、时间或者日期 (允许为null)
  - 支持 java.util.Date、java.util.Calendar、java.time.Instant、java.time.LocalDate、java.time.LocalDateTime、java.time.LocalTime、java.time.MonthDay、java.time.OffsetDateTime、java.time.OffsetTime、java.time.Year、java.time.YearMonth、java.time.ZonedDateTime、java.time.chrono.HijrahDate、java.time.chrono.JapaneseDate、java.time.chrono.MinguoDate、java.time.chrono.ThaiBuddhistDate
- @FutureOrPresent
  - 注释元素必须是现在或未来的某个时刻， 时间或者日期 (允许为null)
  - 支持 java.util.Date、java.util.Calendar、java.time.Instant、java.time.LocalDate、java.time.LocalDateTime、java.time.LocalTime、java.time.MonthDay、java.time.OffsetDateTime、java.time.OffsetTime、java.time.Year、java.time.YearMonth、java.time.ZonedDateTime、java.time.chrono.HijrahDate、java.time.chrono.JapaneseDate、java.time.chrono.MinguoDate、java.time.chrono.ThaiBuddhistDate
- @Max
  - 注解元素必须是一个数字，必须小于或等于maximum (允许为null)
  - 支持BigDecimal、BigInteger、byte、short、int、long和它们的包装类
- @Min
  - 注解元素必须是一个数字，必须大于或等于maximum (允许为null)
  - 支持BigDecimal、BigInteger、byte、short、int、long和它们的包装类
- @Negative
  - 注解元素必须是一个负数，不允许为0 (允许为null)
  - 支持BigDecimal、BigInteger、byte、short、int、long、float、double和它们的包装类
- @MegativeOrZero
  - 注解元素必须是一个负数或者0 (允许为null)
  - 支持BigDecimal、BigInteger、byte、short、int、long、float、double和它们的包装类
- @NotBlank
  - 注解元素不能为null并且至少包含一个非空白字符
  - 支持CharSequence类型
- @NotEmpty
  - 注解元素不能为null或空
  - 支持的类型有: CharSequence(根据length判断)、Collection(根据size判断)、Map(根据size判断)、Array(根据length判断)
- @NotNull
  - 注解元素不能为null
  - 支持任意类型
- @Null
  - 注解元素必须是null
  - 支持任意类型
- @Past
  - 注解元素必须是过去的某个时刻、时间或者日期 (允许为null)
  - 支持 java.util.Date、java.util.Calendar、java.time.Instant、java.time.LocalDate、java.time.LocalDateTime、java.time.LocalTime、java.time.MonthDay、java.time.OffsetDateTime、java.time.OffsetTime、java.time.Year、java.time.YearMonth、java.time.ZonedDateTime、java.time.chrono.HijrahDate、java.time.chrono.JapaneseDate、java.time.chrono.MinguoDate、java.time.chrono.ThaiBuddhistDate
- @PastOrPresent
  - 注解元素必须是过去或现在的某个时刻、时间或者日期 (允许为null)
  - 支持 java.util.Date、java.util.Calendar、java.time.Instant、java.time.LocalDate、java.time.LocalDateTime、java.time.LocalTime、java.time.MonthDay、java.time.OffsetDateTime、java.time.OffsetTime、java.time.Year、java.time.YearMonth、java.time.ZonedDateTime、java.time.chrono.HijrahDate、java.time.chrono.JapaneseDate、java.time.chrono.MinguoDate、java.time.chrono.ThaiBuddhistDate
- @Pattern
  - 注解元素必须匹配指定的正则表达式  (允许为null)
  - 支持CharSequence
- @Positive
  - 注解元素必须为正数,不允许为0 (允许为null)
  - 支持BigDecimal、BigInteger、byte、short、int、long、float、double和它们的包装类
- @PositiveOrZero
  - 注解元素必须为正数或0 (允许为null)
  - 支持BigDecimal、BigInteger、byte、short、int、long、float、double和它们的包装类
- @Size
  - 带注解的元素必须在指定的边界之间(包含)   (允许为null)
  - 支持的类型有: CharSequence(根据length判断)、Collection(根据size判断)、Map(根据size判断)、Array(根据length判断)

