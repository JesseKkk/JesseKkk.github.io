---
title: Spring事务管理
categories:
  - SSM到Spring Boot入门与综合实战
  - 01_Spring从入门到进阶
tags:
  - muke
abbrlink: 27920
date: 2020-05-07 08:19:33
---

:star2:本文是Spring的事务管理总结:star2:

<!-- more -->

## 1 概述

![图片](/images/041_06_01.png)

![图片](/images/041_06_02.png)

## 2 MySQL事务处理

### 2.1 基本语句

![图片](/images/041_06_03.png)

### 2.2 事务并发问题

![图片](/images/041_06_04.png)

- 解决方式：只能读取永久化数据

![图片](/images/041_06_05.png)

- 解决方式：锁ROW

![图片](/images/041_06_06.png)

- 解决方式：锁TABLE

### 2.3 事务隔离级别

![图片](/images/041_06_07.png)

```txt
--设置会话事务隔离级别
set session transaction isolation level read uncommitted;

--脏读
--事务A
begin;
select stock from products where id='100001';
rollback;
--事务B
begin;
update products set stock=0 where id='100001';
rollback;

--不可重复读
--事务A
begin;
select stock from products where id='100001';
select stock from products where id='100001';
rollback;
--事务B
begin;
update products set stock=0 where id='100001';
commit;

--幻读
--事务A
begin;
update products set stock=0;
select * from products;
rollback;
--事务B
begin;
insert into products values('100005','',1999,100,'正常');
commit;
```

## 3 JDBC事务处理

### 3.1 基本语句

![图片](/images/041_06_08.png)

```java
package com.jesse.os.dao;

import org.junit.Test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class OrderTest {
    private String driver = "com.mysql.cj.jdbc.Driver";
    private String url = "jdbc:mysql://127.0.0.1:3306/os?useSSL=false&serverTimezone=Hongkong&useUnicode=true&characterEndocing=utf-8";
    private String username = "root";
    private String password = "1234";

    @Test
    public void addOrder(){
        try {
            Class.forName(driver);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        Connection connection = null;
        try {
            connection = DriverManager.getConnection(url, username, password);
            connection.setAutoCommit(false);
            Statement statement = connection.createStatement();
            statement.execute("INSERT INTO orders VALUES('100002','100001',2,2499,now(),null,null,'刘备','13300000000','成都','代发货')");
            statement.execute("UPDATE products SET stock=stock-2 WHERE id='100001'");
            statement.close();
            connection.commit();
        } catch (SQLException e) {
            e.printStackTrace();
            try {
                connection.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        } finally {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}

```

### 3.2 事务隔离级别

![图片](/images/041_06_09.png)

## 4 Spring事务处理

### 4.1 基本概念

#### 4.1.1 Spring事务处理API

![图片](/images/041_06_10.png)

#### 4.1.2 TransactionDefinition接口

![图片](/images/041_06_11.png)

![图片](/images/041_06_12.png)

### 4.2 Spring编程式事务处理

![图片](/images/041_06_13.png)

#### 4.2.1 基于底层API的编程式事务管理

```java
    public void addOrder(Order order) {
        order.setCreateTime(new Date());
        order.setStatus("待付款");

        TransactionStatus transactionStatus = transactionManager.getTransaction(transactionDefinition);

        try {
            orderDao.insert(order);
            Product product = productDao.select(order.getProductsId());
            product.setStock(product.getStock()-order.getNumber());
            productDao.update(product);
            transactionManager.commit(transactionStatus);
        } catch (TransactionException e) {
            transactionManager.rollback(transactionStatus);
            e.printStackTrace();
        }
    }
```

```xml
    <import resource="spring-dao.xml"/>
    <context:component-scan base-package="com.jesse.os.service.impl1"/>
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <bean id="transactionDefinition" class="org.springframework.transaction.support.DefaultTransactionDefinition">
        <property name="propagationBehaviorName" value="PROPAGATION_REQUIRED"/>
    </bean>
```

```java
    @Test
    public void testAddOrder(){
        Order order = new Order("100006", "100002", 2, 1799, "", "", "");
        orderService.addOrder(order);

    }
```

#### 4.2.2 基于TransactionTemplate的编程式事务管理

```java
    public void addOrder(final Order order) {
        order.setCreateTime(new Date());
        order.setStatus("待付款");

        transactionTemplate.execute(new TransactionCallback() {
            public Object doInTransaction(TransactionStatus transactionStatus) {
                try {
                    orderDao.insert(order);
                    Product product = productDao.select(order.getProductsId());
                    product.setStock(product.getStock()-order.getNumber());
                    productDao.update(product);
                } catch (TransactionException e) {
                    e.printStackTrace();
                    transactionStatus.setRollbackOnly();
                }
                return null;
            }
        });
    }
```

```xml
    <import resource="spring-dao.xml"/>
    <context:component-scan base-package="com.jesse.os.service.impl2"/>
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
        <property name="transactionManager" ref="transactionManager"/>
    </bean>
```

### 4.3 Spring声明式事务处理

![图片](/images/041_06_14.png)

![图片](/images/041_06_15.png)

#### 4.3.1 基于拦截器

```java
    public void addOrder(Order order) {
        order.setCreateTime(new Date());
        order.setStatus("待付款");

        orderDao.insert(order);
        Product product = productDao.select(order.getProductsId());
        product.setStock(product.getStock()-order.getNumber());
        productDao.update(product);
    }
```

```xml
    <import resource="spring-dao.xml"/>
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <bean id="orderServiceTarget" class="com.jesse.os.service.impl.OrderServiceImpl"/>
    <bean id="transactionInterceptor" class="org.springframework.transaction.interceptor.TransactionInterceptor">
        <property name="transactionManager" ref="transactionManager"/>
        <property name="transactionAttributes">
            <props>
                <prop key="get*">PROPAGATION_REQUIRED</prop>
                <prop key="find*">PROPAGATION_REQUIRED</prop>
                <prop key="search*">PROPAGATION_REQUIRED</prop>
                <prop key="*">PROPAGATION_REQUIRED</prop>
            </props>
        </property>
    </bean>
    <bean id="orderService" class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="target" ref="orderServiceTarget"/>
        <property name="interceptorNames">
            <list>
                <idref bean="transactionInterceptor"/>
            </list>
        </property>
    </bean>
```

#### 4.3.2 简化拦截器

```xml
    <import resource="spring-dao.xml"/>
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <bean id="orderServiceTarget" class="com.jesse.os.service.impl.OrderServiceImpl"/>
    <bean id="orderService" class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean">
        <property name="transactionManager" ref="transactionManager"/>
        <property name="transactionAttributes">
            <props>
                <prop key="get*">PROPAGATION_REQUIRED</prop>
                <prop key="find*">PROPAGATION_REQUIRED</prop>
                <prop key="search*">PROPAGATION_REQUIRED</prop>
                <prop key="*">PROPAGATION_REQUIRED</prop>
            </props>
        </property>
        <property name="target" ref="orderServiceTarget"/>
    </bean>
```

#### 4.3.3 基于tx命名空间

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd">

    <import resource="spring-dao.xml"/>
    <context:component-scan base-package="com.jesse.os.service.impl"/>
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="get*" propagation="REQUIRED"/>
            <tx:method name="find*" propagation="REQUIRED"/>
            <tx:method name="search*" propagation="REQUIRED"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <aop:config>
        <aop:pointcut id="poincut" expression="execution(* com.jesse.os.service.impl.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="poincut"/>
    </aop:config>
</beans>
```

#### 4.3.4 基于注解

```xml
    <import resource="spring-dao.xml"/>
    <context:component-scan base-package="com.jesse.os.service.impl6"/>
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <tx:annotation-driven transaction-manager="transactionManager"/>
```

```java
    @Transactional(propagation = Propagation.REQUIRED)
    public void addOrder(Order order) {
        order.setCreateTime(new Date());
        order.setStatus("待付款");

        orderDao.insert(order);
        Product product = productDao.select(order.getProductsId());
        product.setStock(product.getStock()-order.getNumber());
        productDao.update(product);
    }
```
