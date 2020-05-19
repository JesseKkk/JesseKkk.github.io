---
title: Spring入门
categories:
  - SSM到Spring Boot入门与综合实战
  - 01_Spring从入门到进阶
tags:
  - muke
abbrlink: 35289
date: 2020-04-23 20:19:10
---

:star2:本文是Spring入门的学习总结:star2:

<!-- more -->

## 1 Spring

### 1.1 什么是Spring

![图片](/images/041_01_01.png)

### 1.2 Spring的优点

![图片](/images/041_01_02.png)

![图片](/images/041_01_03.png)

### 1.3 Spring模块

![图片](/images/041_01_04.png)

## 2 Spring的IoC

### 2.1 Spring的IoC底层原理

![图片](/images/041_01_05.png)

### 2.2 Example

```xml
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-expression</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
```

```java
package com.jesse.ioc.demo1;

public interface UserService {
    public void sayHello();
}
```

```java
package com.jesse.ioc.demo1;

public class UserServiceImpl implements UserService {
    @Override
    public void sayHello() {
        System.out.println("Hello Spring");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- UserService的创建权交给了Spring -->
    <bean id="userService" class="com.jesse.ioc.demo1.UserServiceImpl"></bean>

</beans>
```

```java
package com.jesse.ioc.demo1;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SpringDemo1 {

    @Test
    /**
     * 传统方式开发
     */
    public void demo1(){
        UserService userService = new UserServiceImpl();
        userService.sayHello();
    }

    @Test
    /**
     * Spring的方式实现
     */
    public void demo2(){
        //创建Spring的工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂获得类
        UserService userService =  (UserService)applicationContext.getBean("userService");
        userService.sayHello();
    }
}
```

## 3 IoC和DI

![图片](/images/041_01_06.png)
