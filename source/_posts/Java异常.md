---
title: Java异常
categories:
  - Java入门
  - 03_Java常用工具
tags:
  - imooc
abbrlink: 48508
date: 2019-12-10 15:32:09
---

:star2:本文是Java异常的学习总结:star2:

<!-- more -->

## 1 异常概念

![图片](/images/013_01_01.png)

## 2 异常的分类

- Error类和RuntimeException类为非受查异常，其他的都为受查异常

![图片](/images/013_01_02.png)

## 3 异常处理

- 包括：抛出异常和捕获异常
- 5个关键字：try、catch、finally、throw、throws

![图片](/images/013_01_03.png)

### 3.1 捕获处理异常（try-catch-finally）

- try-catch-finally
1、注意多重catch的使用规则
2、注意finally语句中return一定会被执行
- throws
1、抛出不处理

### 3.2 抛出异常（throw & throws）

![图片](/images/013_01_04.png)

注意：当子类重写父类抛出异常的方法时，声明的异常必须是父类方法所声明异常的同类或子类

## 4 自定义异常

- 可以通过自定义异常描述特定业务产生的异常类型
- 所谓自定义异常，就是定义一个类，去继承Throwable类或者它的子类

## 5 异常链

![图片](/images/013_01_05.png)

```java
//方案1
throw new Exception("我是新的异常1", e);
//方案2
Exception e1 = new Exception("我是新的异常2");
e1.initCause(e);
throw e1;

```
