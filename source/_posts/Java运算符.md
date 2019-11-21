---
title: Java运算符
categories:
  - Java基础
tags:
  - muke
abbrlink: 46231
date: 2019-11-19 10:46:29
---

## 表达式

表达式由运算符和操作数组成

## 运算符

![图片](/images/011_03_01.png)

## 算术运算符

算术运算和字符串连接区别：

    int num1 = 10;
    int num2 = 5;
    int result;
    result = num1 + num2;

    System.out.println(num1 + num2);    #算术运算
    System.out.println("" + num1 + num2);   #字符串连接

分子分母都是整型时，结果为整除后的结果：

    System.out.println(13 / 5);   #2
    System.out.println(13.0 / 5);   #2.6

### 自增自减运算符

x++和++x的区别：

    int x = 4;  
    int y = (x++)+5;
    System.out.println("x = " + x + ",y = " + y);   #x = 5;y = 9
    
    int x = 4;  
    int y = (++x)+5;
    System.out.println("x = " + x + ",y = " + y);   #x = 5;y = 10

## 逻辑运算符

&&和||为短路运算符，如果第一个表达式的值能够决定表达式最后的结果，剩下的表达式不再计算：

    int n = 3;
    boolean b = (3 < 7) || ((n++) < 2)  #b = true;n = 3;

## 运算符的优先级

![图片](/images/011_03_02.png)

## 单目，双目，三目运算符

区别：操作数的个数
单目：++，--
双目：+，-
三目：?:
