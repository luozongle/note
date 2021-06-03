[TOC]

# Idea常见问题



## 代码显示警告

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



