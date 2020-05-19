---
title: Java包装类
categories:
  - Java入门
  - 03_Java常用工具
tags:
  - muke
abbrlink: 64297
date: 2019-12-18 14:58:14
---

:star2:本文是Java包装类的学习总结:star2:

<!-- more -->

## 1 包装类与基本数据类型

![图片](/images/013_02_01.png)

## 2 基本数据类型和包装类的转换

```java
public class WrapTestOne {
    public static void main(String[] args) {
        //装箱：把基本数据类型转换为包装类
        //1、自动装箱
        int t1 = 2;
        Integer t2 = t1;

        //2、手动装箱
        Integer t3 = new Integer(3);

        //测试
        System.out.println("t1 = " + t1);
        System.out.println("t2 = " + t2);
        System.out.println("t3 = " + t3);

        //拆箱：把包装类转换为基本数据类型
        //1、自动拆箱
        int t4 = t2;

        //2、手动拆箱
        int t5 = t2.intValue();

        //测试
        System.out.println("t4 = " + t4);
        System.out.println("t5 = " + t5);
    }
}

```

## 3 基本数据类型和字符串之间的转换

```java
public class WrapTestTwo {
    public static void main(String[] args) {
        //基本数据类型转换为字符串
        int t1 = 2;
        String t2 = Integer.toString(t1);

        //测试
        System.out.println("t2 = " + t2);

        //字符串转换为基本数据类型
        //1、包装类的parse
        int t3 = Integer.parseInt(t2);

        //2、包装类的valueOf
        int t4 = Integer.valueOf(t2);

        //测试
        System.out.println("t3 = " + t3);
        System.out.println("t4 = " + t4);
    }
}
```

## 4 包装类对象的比较

![图片](/images/013_02_02.png)

- 结果

![图片](/images/013_02_03.png)
