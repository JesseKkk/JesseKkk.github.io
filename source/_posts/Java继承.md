---
title: Java继承
categories:
  - Java入门
  - 02_Java面向对象
tags:
  - imooc
abbrlink: 49511
date: 2019-12-05 15:45:51
---

:star2:本文是Java继承的学习总结:star2:

<!-- more -->

## 1 继承

### 1.1 概念

![图片](/images/012_03_01.png)

### 1.2 语法

![图片](/images/012_03_02.png)

### 1.3 初始化顺序

![图片](/images/012_03_03.png)

## 2 super

### 2.1 super的使用

![图片](/images/012_03_04.png)

### 2.2 注意事项

![图片](/images/012_03_05.png)

### 2.3 super PK this

![图片](/images/012_03_06.png)

## 3 方法重写 PK 方法重载

![图片](/images/012_03_07.png)

## 4 访问修饰符

![图片](/images/012_03_08.png)

## 5 Object类

![图片](/images/012_03_09.png)

## 6 final

![图片](/images/012_03_10.png)

### 6.1 static和final的区别

- static修饰的时候代表对象是静态
- final修饰的时候代表对象只能赋值一次，

```java
static int a=1;
static final b=1;
```

这里a和b的区别在于，a在程序里可以被重新赋值为2或3或等等的整数，而b在程序里不能被重新赋值，b永远都为1，也就是说b是一个常量。

```java
final int c=1;
static final b=1;
```

这里c和b的区别在于，b存放在静态空间，不会在程序运行时被释放，它永远占着内存直到程序终止，而c在程序用完它而不会再用到它的时候就会被自动释放，不再占用内存。当一个常数或字符串我们需要在程序里反复反复使用的时候，我们就可以把它定义为static final，这样内存就不用重复的申请和释放空间。

## 7 注解

![图片](/images/012_03_11.png)
