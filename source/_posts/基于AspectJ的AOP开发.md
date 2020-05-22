---
title: 基于AspectJ的AOP开发
categories:
  - SSM到Spring Boot入门与综合实战
  - 01_Spring从入门到进阶
tags:
  - imooc
abbrlink: 24886
date: 2020-05-04 17:02:20
---

:star2:本文是基于AspectJ的AOP开发学习总结:star2:

<!-- more -->

## 1 AspectJ的简介

![图片](/images/041_04_01.png)

## 2 AspectJ的注解开发AOP（上）

### 2.1 环境准备

```xml
 <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
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
      <artifactId>spring-expression</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>aopalliance</groupId>
      <artifactId>aopalliance</artifactId>
      <version>1.0</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.8.9</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
  </dependencies>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--开启AspectJ的注解开发，自动代理-->
    <aop:aspectj-autoproxy/>

</beans>
```

### 2.2 通知类型的介绍

![图片](/images/041_04_02.png)

### 2.3 切入点表达式的定义

![图片](/images/041_04_03.png)

## 3 AspectJ的注解开发AOP（下）

### 3.1 前置通知

![图片](/images/041_04_04.png)

```java
package com.jesse.aspectj.demo1;

public class ProductDao {
    public void save(){
        System.out.println("保存商品");
    }

    public void update(){
        System.out.println("修改商品");
    }

    public void delete(){
        System.out.println("删除商品");
    }

    public void findOne(){
        System.out.println("查询一个商品");
    }

    public void findAll(){
        System.out.println("查询所有商品");
    }
}

```

```java
package com.jesse.aspectj.demo1;

import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

/**
 * 切面类
 */
@Aspect
public class MyAsepctAnno {

    @Before(value = "execution(* com.jesse.aspectj.demo1.ProductDao.save(..))")
    public void before(JoinPoint joinPoint){
        System.out.println("前置通知 " + joinPoint);
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--开启AspectJ的注解开发，自动代理-->
    <aop:aspectj-autoproxy/>

    <!--目标类-->
    <bean id="productDao" class="com.jesse.aspectj.demo1.ProductDao"/>

    <!--定义切面-->
    <bean class="com.jesse.aspectj.demo1.MyAsepctAnno"/>
</beans>
```

```java
package com.jesse.aspectj.demo1;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class SpringDemo1 {

    @Resource(name = "productDao")
    private ProductDao productDao;

    @Test
    public void demo1(){
        productDao.save();
        productDao.update();
        productDao.delete();
        productDao.findAll();
        productDao.findOne();
    }

}

```

### 3.2 后置通知

![图片](/images/041_04_05.png)

```java
    public String update(){
        System.out.println("修改商品");
        return "hello";
    }
```

```java
    @AfterReturning(value = "execution(* com.jesse.aspectj.demo1.ProductDao.update(..))",returning = "result")
    public void afterReturing(Object result){
        System.out.println("后置通知 " + result);
    }
```

### 3.3 环绕通知

![图片](/images/041_04_06.png)

```java
    @Around(value = "execution(* com.jesse.aspectj.demo1.ProductDao.delete(..))")
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前通知");
        Object obj = joinPoint.proceed();//执行目标方法
        System.out.println("环绕后通知");
        return obj;
    }
```

### 3.4 异常抛出通知

![图片](/images/041_04_07.png)

```java
    public void findOne(){
        System.out.println("查询一个商品");
        //int i = 1/0;
    }
```

```java
    @AfterThrowing(value = "execution(* com.jesse.aspectj.demo1.ProductDao.findOne(..))" ,throwing = "e")
    public void afterThrowing(Throwable e){
        System.out.println("异常抛出通知 " + e.getMessage());
    }
```

### 3.5 最终通知

![图片](/images/041_04_08.png)

```java
    public void findAll(){
        System.out.println("查询所有商品");
        //int j = 1/0;
    }
```

```java
    @After(value = "execution(* com.jesse.aspectj.demo1.ProductDao.findAll(..))")
    public void after(){
        System.out.println("最终通知");
    }
```

### 3.6 切点命名

![图片](/images/041_04_09.png)

```java
    @Before(value = "myPointcut1()")
    public void before(JoinPoint joinPoint){
        System.out.println("前置通知 " + joinPoint);
    }

    @Pointcut(value = "execution(* com.jesse.aspectj.demo1.ProductDao.save(..))")
    private void myPointcut1(){}
```

## 4 AspectJ的XML方式开发AOP

### 4.1 前置通知

![图片](/images/041_04_10.png)

```java
package com.jesse.aspectj.demo2;

public interface CustomerDao {
    public void save();
    public void update();
    public void delete();
    public void findOne();
    public void findAll();
}

```

```java
package com.jesse.aspectj.demo2;

public class CustomerDaoImpl implements CustomerDao {
    @Override
    public void save() {
        System.out.println("保存客户");
    }

    @Override
    public void update() {
        System.out.println("修改客户");

    }

    @Override
    public void delete() {
        System.out.println("删除客户");

    }

    @Override
    public void findOne() {
        System.out.println("查询一个客户");

    }

    @Override
    public void findAll() {
        System.out.println("查询多个客户");

    }
}

```

```java
package com.jesse.aspectj.demo2;

public class MyAspectXml {

    //前置通知
    public void before(){
        System.out.println("XML方式的前置通知");
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--XML的配置方式完成AOP的开发-->
    <!--配置目标类-->
    <bean id="customerDao" class="com.jesse.aspectj.demo2.CustomerDaoImpl"/>

    <!--配置切面类-->
    <bean id="myAspectXml" class="com.jesse.aspectj.demo2.MyAspectXml"/>

    <!--aop的相关配置-->
    <aop:config>
        <!--配置切入点-->
        <aop:pointcut id="pointcut1" expression="execution(* com.jesse.aspectj.demo2.CustomerDao.save(..))"/>
        <!--配置AOP的切面-->
        <aop:aspect ref="myAspectXml">
            <!--配置前置通知-->
            <aop:before method="before" pointcut-ref="pointcut1"/>
        </aop:aspect>
    </aop:config>
</beans>
```

```java
package com.jesse.aspectj.demo2;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(value = "classpath:applicationContext2.xml")
public class SpringDemo2 {

    @Resource(name = "customerDao")
    private CustomerDao customerDao;;

    @Test
    public void demo1(){
        customerDao.save();
        customerDao.update();
        customerDao.delete();
        customerDao.findOne();
        customerDao.findAll();
    }
}

```

## 4.2 其他通知类型的配置

```xml
    <!--aop的相关配置-->
    <aop:config>
        <!--配置切入点-->
        <aop:pointcut id="pointcut1" expression="execution(* com.jesse.aspectj.demo2.CustomerDao.save(..))"/>
        <aop:pointcut id="pointcut2" expression="execution(* com.jesse.aspectj.demo2.CustomerDao.update(..))"/>
        <aop:pointcut id="pointcut3" expression="execution(* com.jesse.aspectj.demo2.CustomerDao.delete(..))"/>
        <aop:pointcut id="pointcut4" expression="execution(* com.jesse.aspectj.demo2.CustomerDao.findOne(..))"/>
        <aop:pointcut id="pointcut5" expression="execution(* com.jesse.aspectj.demo2.CustomerDao.findAll(..))"/>
        <!--配置AOP的切面-->
        <aop:aspect ref="myAspectXml">
            <!--配置前置通知-->
            <aop:before method="before" pointcut-ref="pointcut1"/>
            <!--配置后置通知-->
            <aop:after-returning method="afterReturning" pointcut-ref="pointcut2" returning="result"/>
            <!--配置环绕通知-->
            <aop:around method="around" pointcut-ref="pointcut3"/>
            <!--配置异常抛出通知-->
            <aop:after-throwing method="afterThrowing" pointcut-ref="pointcut4" throwing="e"/>
            <!--配置最终通知-->
            <aop:after method="after" pointcut-ref="pointcut5"/>
        </aop:aspect>
    </aop:config>
```
