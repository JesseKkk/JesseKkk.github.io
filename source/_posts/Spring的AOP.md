---
title: Spring的AOP
categories:
  - SSM到Spring Boot入门与综合实战
  - 01_Spring从入门到进阶
tags:
  - muke
abbrlink: 63389
date: 2020-05-04 08:41:15
---

:star2:本文是Spring的AOP的学习总结:star2:

<!-- more -->

## 1 概述

### 1.1 AOP的概述

![图片](/images/041_03_01.png)

![图片](/images/041_03_02.png)

![图片](/images/041_03_03.png)

### 2.1 AOP的相关术语

![图片](/images/041_03_04.png)

![图片](/images/041_03_05.png)

![图片](/images/041_03_06.png)

## 2 AOP底层实现

### 2.1 JDK动态代理

```java
package com.jesse.aop.demo1;

public interface UserDao {
    public void save();

    public void update();

    public void delete();

    public void find();
}

```

```java
package com.jesse.aop.demo1;

public class UserDaoImpl implements UserDao {
    @Override
    public void save() {
        System.out.println("保存用户...");
    }

    @Override
    public void update() {
        System.out.println("修改用户...");

    }

    @Override
    public void delete() {
        System.out.println("删除用户...");

    }

    @Override
    public void find() {
        System.out.println("查询用户...");

    }
}

```

```java
package com.jesse.aop.demo1;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class MyJDKProxy implements InvocationHandler {
    private UserDao userDao;

    public MyJDKProxy(UserDao userDao){
        this.userDao = userDao;
    }

    public Object createProxy(){
        Object proxy = Proxy.newProxyInstance(userDao.getClass().getClassLoader(),userDao.getClass().getInterfaces(),this);
        return proxy;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        if("save".equals(method.getName())){
            System.out.println("权限校验...");
            return method.invoke(userDao,args);
        }
        return method.invoke(userDao,args);
    }
}

```

```java
package com.jesse.aop.demo1;

import org.junit.Test;

public class SpringDemo1 {
    @Test
    public void demo1(){
        UserDao userDao = new UserDaoImpl();

        UserDao proxy = (UserDao)new MyJDKProxy(userDao).createProxy();

        proxy.save();
        proxy.update();
        proxy.delete();
        proxy.find();
    }
}

```

### 2.2 CGLIB的动态代理

![图片](/images/041_03_07.png)

```java
package com.jesse.aop.demo2;

public class ProductDao {
    public void save(){
        System.out.println("保存商品...");
    }

    public void update(){
        System.out.println("修改商品...");
    }

    public void delete(){
        System.out.println("删除商品...");
    }

    public void find(){
        System.out.printf("查询商品...");
    }
}

```

```java
package com.jesse.aop.demo2;

import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

public class MyCglibProxy implements MethodInterceptor {
    private ProductDao productDao;

    public MyCglibProxy(ProductDao productDao){
        this.productDao = productDao;
    }

    public Object createProxy(){
        //1.创建核心类
        Enhancer enhancer = new Enhancer();

        //2.设置父类
        enhancer.setSuperclass(productDao.getClass());

        //3.设置回调
        enhancer.setCallback(this);

        //4.生成代理
        Object proxy = enhancer.create();

        return proxy;
    }

    @Override
    public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        if ("save".equals(method.getName())){
            System.out.println("权限校验");
            return methodProxy.invokeSuper(proxy,args);
        }
        return methodProxy.invokeSuper(proxy,args);
    }
}

```

```java
package com.jesse.aop.demo2;

import org.junit.Test;

public class SpringDemo2 {
    @Test
    public void demo1(){
        ProductDao productDao = new ProductDao();

        ProductDao proxy = (ProductDao)new MyCglibProxy(productDao).createProxy();
        proxy.save();
        proxy.update();
        proxy.delete();
        proxy.find();
    }
}

```

### 2.3 代理知识点总结

![图片](/images/041_03_08.png)

![图片](/images/041_03_09.png)

## 3 Spring的AOP一般切面编程案例

### 3.1 Spring的AOP通知类型的介绍

![图片](/images/041_03_10.png)

![图片](/images/041_03_11.png)

### 3.2 Spring的AOP切面类型

![图片](/images/041_03_12.png)

### 3.3 Advisor切面案例

![图片](/images/041_03_13.png)

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
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
```

```java
package com.jesse.aop.demo3;

public interface StudentDao {
    public void find();

    public void save();

    public void update();

    public void delete();
}

```

```java
package com.jesse.aop.demo3;

public class StudentDaoImpl implements StudentDao {
    @Override
    public void find() {
        System.out.println("学生查询...");
    }

    @Override
    public void save() {
        System.out.println("学生保存...");
    }

    @Override
    public void update() {
        System.out.println("学生更新...");
    }

    @Override
    public void delete() {
        System.out.println("学生删除...");
    }
}

```

```java
package com.jesse.aop.demo3;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class MyBeforeAdvice implements MethodBeforeAdvice {
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println("前置增强");
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置目标类-->
    <bean id="studentDao" class="com.jesse.aop.demo3.StudentDaoImpl"/>

    <!--前置通知类型-->
    <bean id="myBeforeAdvice" class="com.jesse.aop.demo3.MyBeforeAdvice"/>

    <!--Spring的AOP产出代理对象-->
    <bean  id="studentDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
        <!--配置目标类-->
        <property name="target" ref="studentDao"/>
        <!--实现接口-->
        <property name="proxyInterfaces" value="com.jesse.aop.demo3.StudentDao"/>
        <!--采用拦截的名称-->
        <property name="interceptorNames" value="myBeforeAdvice"/>
        <property name="optimize" value="true"/>
    </bean>
</beans>
```

```java
package com.jesse.aop.demo3;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class SpringDemo3 {

    @Resource(name="studentDaoProxy")
    private StudentDao studentDao;

    @Test
    public void demo1(){
        studentDao.find();
        studentDao.save();
        studentDao.update();
        studentDao.delete();
    }
}

```

### 3.3 PointcutAdvisor切面案例

![图片](/images/041_03_14.png)

```java
package com.jesse.aop.demo4;

public class CustomerDao {
    public void find(){
        System.out.println("查询客户...");
    }

    public void save(){
        System.out.println("保存客户...");
    }

    public void update(){
        System.out.println("修改客户...");
    }

    public void delete(){
        System.out.println("删除客户...");
    }
}

```

```java
package com.jesse.aop.demo4;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;


public class MyAroundAdvice implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        System.out.println("环绕前增强");

        Object obj = methodInvocation.proceed();

        System.out.println("环绕后增强");
        return null;
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置目标类-->
    <bean id="customerDao" class="com.jesse.aop.demo4.CustomerDao"/>

    <!--配置通知-->
    <bean id="myAroundAdvice" class="com.jesse.aop.demo4.MyAroundAdvice"/>

    <!--一般的切面是使用通知作为切面的，因为要对目标类的某个方法进行增强就需要配置一个带有切入点-->
    <bean id="myAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <!--pattern中配置正则表达式：.任意植入 *任意次数-->
        <!--<property name="pattern" value=".*"/>-->
        <property name="patterns" value=".*save.*,.*delete.*"/>
        <property name="advice" ref="myAroundAdvice"/>
    </bean>

    <!--配置产生代理-->
    <bean id="customerDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="target" ref="customerDao"/>
        <property name="proxyTargetClass" value="true"/>
        <property name="interceptorNames" value="myAdvisor"/>
    </bean>
</beans>
```

```java
package com.jesse.aop.demo4;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext2.xml")
public class SpringDemo4 {

    //@Resource(name = "customerDao")
    @Resource(name = "customerDaoProxy")
    private CustomerDao customerDao;

    @Test
    public void demo1(){
        customerDao.find();
        customerDao.save();
        customerDao.update();
        customerDao.delete();
    }
}

```

## 4 Spring的传统AOP的动态代理

### 4.1 Spring的传统AOP的自动代理

![图片](/images/041_03_15.png)

### 4.2 Spring的传统AOP的基于Bean名称的自动代理

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置目标类-->
    <bean id="studentDao" class="com.jesse.aop.demo5.StudentDaoImpl"/>
    <bean id="customerDao" class="com.jesse.aop.demo5.CustomerDao"/>

    <!--配置增强-->
    <bean id="myBeforeAdvice" class="com.jesse.aop.demo5.MyBeforeAdvice"/>
    <bean id="myAroundAdvice" class="com.jesse.aop.demo5.MyAroundAdvice"/>

    <bean class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator">
        <property name="beanNames" value="*Dao"/>
        <property name="interceptorNames" value="myBeforeAdvice"/>
    </bean>
</beans>
```

```java
package com.jesse.aop.demo5;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext3.xml")
public class SpringDemo5 {

    @Resource(name = "studentDao")
    private StudentDao studentDao;

    @Resource(name = "customerDao")
    private CustomerDao customerDao;

    @Test
    public void demo1(){
        studentDao.find();
        studentDao.save();
        studentDao.update();
        studentDao.delete();

        customerDao.find();
        customerDao.save();
        customerDao.update();
        customerDao.delete();
    }
}

```

### 4.3 Spring的传统AOP的基于切面信息的自动代理

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置目标类-->
    <bean id="studentDao" class="com.jesse.aop.demo6.StudentDaoImpl"/>
    <bean id="customerDao" class="com.jesse.aop.demo6.CustomerDao"/>

    <!--配置增强-->
    <bean id="myBeforeAdvice" class="com.jesse.aop.demo6.MyBeforeAdvice"/>
    <bean id="myAroundAdvice" class="com.jesse.aop.demo6.MyAroundAdvice"/>

    <!--配置切面-->
    <bean id="myAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <property name="pattern" value="com\.jesse\.aop\.demo6\.CustomerDao\.save"/>
        <property name="advice" ref="myAroundAdvice"/>
    </bean>

    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"/>
</beans>
```

```java
package com.jesse.aop.demo6;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext4.xml")
public class SpringDemo6 {
    @Resource(name = "studentDao")
    private StudentDao studentDao;

    @Resource(name = "customerDao")
    private CustomerDao customerDao;

    @Test
    public void demo1(){
        studentDao.find();
        studentDao.save();
        studentDao.update();
        studentDao.delete();

        customerDao.find();
        customerDao.save();
        customerDao.update();
        customerDao.delete();
    }
}

```
