---
title: Java反射
categories:
  - Java数据库开发与实战
  - 01_初识数据库操作
tags:
  - muke
abbrlink: 31868
date: 2020-03-23 20:44:30
---

:star2:本文是Java反射的学习:star2:

<!-- more -->

## 1 反射概述

### 1.1 什么是反射

![图片](/images/031_05_01.png)

### 1.2 反射常用对象

![图片](/images/031_05_02.png)

![图片](/images/031_05_03.png)

## 2 反射常用API

### 2.1 Class

#### 2.1.1 Class概述

![图片](/images/031_05_04.png)

#### 2.1.2 Class使用

```java
package com.jesse.reflect.test;

public class Person {
    private String name;
    private String sex;

    public Person() {
    }

    public Person(String name, String sex) {
        super();
        this.name = name;
        this.sex = sex;
    }

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getSex() {
        return sex;
    }
    public void setSex(String sex) {
        this.sex = sex;
    }

    public void eat() {
        System.out.println("努力增胖！");
    }
}

```

```java
package com.jesse.reflect.test;

import org.junit.jupiter.api.Test;

public class ClassTest {

    @Test
    /**
     *  获得Class对象
     *  1、通过类名.class
     *  2、对象.getClass()
     *  3、Class.forName();
     */
    public void demo1() throws ClassNotFoundException {
        // 1、通过类名.class的方式
        Class clazz1 = Person.class;
        // 2、通过对象.getClass()的方式
        Person person = new Person();
        Class clazz2 = person.getClass();
        // 3、Class类forName();获得（推荐）
        Class clazz3 = Class.forName("com.jesse.reflect.test.Person");
    }
}

```

### 2.2 Constructor

#### 2.2.1 Constructor概述

![图片](/images/031_05_05.png)

#### 2.2.2 Constructor使用

```java
package com.jesse.reflect.test;

import java.lang.reflect.Constructor;

import org.junit.Test;

public class ConstructorTest {

    @Test
    /**
     * 获得无参数的构造方法
     *
     */
    public void demo1() throws Exception {
        //获得类的字节码文件对应的对象：
        Class class1 = Class.forName("com.jesse.reflect.test.Person");
        Constructor con = class1.getConstructor();
        Person person = (Person) con.newInstance();// 相当于Person person = new Person();
        person.eat();
    }

    @Test
    /**
     * 获得无参数的构造方法
     *
     */
    public void demo2() throws Exception {
        //获得类的字节码文件对应的对象：
        Class class1 = Class.forName("com.jesse.reflect.test.Person");
        Constructor con = class1.getConstructor(String.class,String.class);
        Person person = (Person) con.newInstance("张三","男");// 相当于Person person = new Person("张三","男");
        System.out.println(person);
    }
}

```

### 2.3 Field

#### 2.3.1 Field概述

![图片](/images/031_05_06.png)

#### 2.3.2 Field使用

```java
package com.jesse.reflect.test;

import java.lang.reflect.Field;

import org.junit.Test;

public class FiledTest {

    private Object object;

    @Test
    //测试共有属性
    public void demo1() throws Exception {
        // 获得Class
        Class c1 = Class.forName("com.jesse.reflect.test.Person");
        // 获得属性
        Field field = c1.getField("name");
        // 操作属性  p.name = "";
        Person p = (Person)c1.newInstance();
        field.set(p, "李四");//p.name = "李四";

        Object obj = field.get(p);
        System.out.println(obj);
    }

     @Test
    // 测试私有属性
    public void demo2() throws Exception {
        // 获得Class
        Class c1 = Class.forName("com.jesse.reflect.test.Person");
        // 获得私有属性
        Field field = c1.getDeclaredField("sex");
        //操作属性
        Person p = (Person)c1.newInstance();
        field.setAccessible(true);
        field.set(p, "男");
        // 获取值
        Object obj = field.get(p);
        System.out.println(obj);
        System.out.println(p);
    }
}

```

### 2.4 Method

#### 2.4.1 Method

![图片](/images/031_05_07.png)

#### 2.4.2 Method

```java
package com.jesse.reflect.test;

import java.lang.reflect.Method;

import org.junit.Test;

public class MethodTest {

    @Test
    // 测试共有的方法
    public void demo1() throws Exception {
        Class c1 = Class.forName("com.jesse.reflect.test.Person");
        // 实例化：
        Person person = (Person)c1.newInstance();
        //获得共有的方法
        Method method = c1.getMethod("eat");
        // 执行该方法
        method.invoke(person); // person.eat();
    }

    @Test
    // 测试私有的方法
    public void demo2() throws Exception {
        Class c1 = Class.forName("com.jesse.reflect.test.Person");
        // 实例化：
        Person person = (Person)c1.newInstance();
        //获得方法：
        Method method = c1.getDeclaredMethod("run");
        //设置私有的属性的访问权限
        method.setAccessible(true);
        //执行该方法：
        method.invoke(person, null);
    }

    @Test
    // 测试私有的方法带参数
    public void demo3() throws Exception {
        Class c1 = Class.forName("com.jesse.reflect.test.Person");
        // 实例化：
        Person person = (Person)c1.newInstance();
        //获得共有的方法
        Method method = c1.getMethod("sayHello" , String.class);
        // 执行该方法
        Object obj = method.invoke(person,"Tom"); // person.eat();
        System.out.println(obj);
    }
}

```
