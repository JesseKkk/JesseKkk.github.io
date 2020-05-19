---
title: Java泛型
categories:
  - Java入门
  - 03_Java常用工具
tags:
  - muke
abbrlink: 42613
date: 2019-12-23 10:43:50
---

:star2:本文是Java泛型的学习总结:star2:

<!-- more -->

## 1 Java泛型

- 本质是参数化类型，使得操作的数据类型被指定为一个参数。

## 2 泛型作为方法参数

- 变量声明的类型必须匹配传递给实际对象的类型
- ? super表示它个父类
- ? extends表示它的子类

```java
public class AnimalPlay {

    public void listPlay(List< ? extends Animal> list)
    {
        for (Animal test: list)
        {
            test.play();
        }
    }
}
```

## 3 自定义泛型类

```java
public class NumGeneric<T> {
    private T num;
    public T getTum() {
        return num;
    }
    public void setNum(T num)
    {
        this.num = num;
    }

    public static void main(String[] args)
    {
        NumGeneric<Integer> intNum = new NumGeneric<>();
        intNum.setNum(10);
        System.out.println("intNum = " + intNum.getTum());

        NumGeneric<Float> floatNum = new NumGeneric<>();
        floatNum.setNum(5.0f);
        System.out.println("floatNum = " + floatNum.getTum());
    }

}
```

## 4 泛型方法

```java
public class GenericMethod {
    public <T extends Number> void printValue(T t)
    {
        System.out.println(t);
    }

    public static void main(String[] args) {
        GenericMethod gm = new GenericMethod();
        gm.printValue(124);
        gm.printValue(5f);
//        gm.printValue("hello");
    }
}
}
