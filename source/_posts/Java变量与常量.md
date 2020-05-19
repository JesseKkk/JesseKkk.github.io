---
title: Java变量与常量
categories:
  - Java入门
  - 01_Java基本语法
tags:
  - muke
abbrlink: 22399
date: 2019-11-07 09:19:35
---

:star2:本文是Java变量与常量的学习总结:star2:

<!-- more -->

## 1 标识符

标识符的命名规则：

![图片](/images/011_02_01.png)

## 2 关键字

![图片](/images/011_02_02.png)

## 3 变量

变量的三个元素： 变量类型、变量名和变量值

### 3.1 变量名

![图片](/images/011_02_03.png)

![图片](/images/011_02_04.png)

### 3.2 数据类型（变量类型）

![图片](/images/011_02_05.png)

#### 3.2.1 基本数据类型

![图片](/images/011_02_06.png)

### 3.3 整型、浮点和字符型字面量

八进制：以0开头 #023
十六进制：以0x或0X开头  #0x23,0X23
注意long, float, double数据的表示
double类型数据表示最大
char有ASCII和Unicode两种编码方式：

```java
char a = 'a';
char a = '\005d';
```

## 4 变量的存储类型

1. 寄存器：最快的存储区, 由编译器根据需求进行分配,我们在程序中无法控制
2. 栈(stack)：存放基本类型的变量数据和对象的引用，但对象本身不存放在栈中，而是存放在堆（new 出来的对象）或者常量池中（字符串常量对象存放在常量池中）
3. 堆(heap)：存放所有new出来的对象。
4. 静态域：存放静态成员（static定义的）
5. 常量池：存放字符串常量和基本类型常量（public static final）
6. 非RAM存储：硬盘等永久存储空间

## 5 变量的类型转换

类型转换包括隐式类型转换和强制类型转换

![图片](/images/011_02_07.png)

## 6 常量

final声明
变量名需要全部大写

## 7 FieldModifier顺序

```java
public protected private static final transient volatile
```
