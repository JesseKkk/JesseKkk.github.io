---
title: Spring的Bean管理
categories:
  - SSM到Spring Boot入门与综合实战
  - 01_Spring从入门到进阶
tags:
  - muke
abbrlink: 39043
date: 2020-04-27 10:20:15
---

:star2:本文是Spring的Bean管理的学习总结:star2:

<!-- more -->

## 1 Spring工厂类介绍

### 1.1 Spring工厂类

![图片](/images/041_02_01.png)

### 1.2 Example

```java
    @Test
    /**
     * 传统方式开发
     */
    public void demo1(){
        //UserService userService = new UserServiceImpl();
        UserServiceImpl userService = new UserServiceImpl();
        //设置属性：
        userService.setName("张三");
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

    @Test
    /**
     * 读取磁盘系统中的配置文件
     */
    public void demo3(){
        //创建Spring的工厂类：
        ApplicationContext applicationContext = new FileSystemXmlApplicationContext("c:\\applicationContext.xml");
        //通过工厂获得类
        UserService userService =  (UserService)applicationContext.getBean("userService");
        userService.sayHello();
    }

    @Test
    /**
     * 传统方式的工厂类：BeanFactory
     */
    public void demo4(){
        //创建工厂类：
        BeanFactory beanFactory = new XmlBeanFactory(new ClassPathResource("applicationContext.xml"));
        //通过工厂获得类：
        UserService userService =  (UserService)beanFactory.getBean("userService");
        userService.sayHello();
    }

    @Test
    /**
     * 传统方式的工厂类：BeanFactory
     */
    public void demo5(){
        //创建工厂类：
        BeanFactory beanFactory = new XmlBeanFactory(new FileSystemResource("c:\\applicationContext.xml"));
        //通过工厂获得类：
        UserService userService =  (UserService)beanFactory.getBean("userService");
        userService.sayHello();
    }

```

## 2 Spring的Bean管理-XML方式（上）

### 2.1 Bean的实例化三种方式

![图片](/images/041_02_02.png)

#### 2.1.1 方式一

```java
package com.jesse.ioc.demo2;

/**
 * Bean的实例化的三种方式：采用无参数的构造方法的方式
 */
public class Bean1 {
    public Bean1(){
        System.out.println("Bean1被实例化了...");
    }
}
```

```xml
    <!--第一种：无参构造器的方式-->
    <bean id="bean1" class="com.jesse.ioc.demo2.Bean1"/>
```

```java
    @Test
    public void demo1(){
        //创建工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂获得类的实例：
        Bean1 bean1 = (Bean1)applicationContext.getBean("bean1");
    }
```

#### 2.1.2 方式二

```java
package com.jesse.ioc.demo2;

/**
 * Bean的实例化三种方式：静态工厂实例化方式
 */
public class Bean2 {
}
```

```java
package com.jesse.ioc.demo2;

/**
 * Bean2的静态工厂
 */
public class Bean2Factory {
    public static Bean2 createBean2(){
        System.out.println("Bean2Factory已经执行");
        return new Bean2();
    }
}
```

```xml
    <!--第二种：静态工厂的方式-->
    <bean id="bean2" class="com.jesse.ioc.demo2.Bean2Factory" factory-method="createBean2"/>
```

```java
    @Test
    public void demo2(){
        //创建工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂获得类的实例：
        Bean2 bean2 = (Bean2)applicationContext.getBean("bean2");
    }
```

#### 2.1.3 方式三

```java
package com.jesse.ioc.demo2;

/**
 * Bean的实例化三种方式：实例工厂实例化
 */
public class Bean3 {
}
```

```java
package com.jesse.ioc.demo2;

public class Bean3Factory {
    public Bean3 createBean3(){
        System.out.println("Bean3Factory执行了...");
        return new Bean3();
    }
}
```

```xml
    <bean id="bean3Factory"  class="com.jesse.ioc.demo2.Bean3Factory"/>
    <bean id="bean3" factory-bean="bean3Factory" factory-method="createBean3"/>
```

```java
    @Test
    public void demo3(){
        //创建工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂获得类的实例：
        Bean3 bean3 = (Bean3)applicationContext.getBean("bean3");
    }
```

### 2.2 Bean的常用配置

![图片](/images/041_02_03.png)

#### 2.2.1 Bean的作用域

![图片](/images/041_02_04.png)

#### 2.2.2 Example

```java
package com.jesse.ioc.demo3;

public class Person {
}
```

```xml
    <!--Bean的作用范围-->
    <bean id="person" class="com.jesse.ioc.demo3.Person" scope="prototype"/>
```

```java
    @Test
    public void demo1() {
        //创建工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂获得类的实例：
        Person person1 = (Person) applicationContext.getBean("person");
        Person person2 = (Person) applicationContext.getBean("person");

        System.out.println(person1);
        System.out.println(person2);
    }
```

### 2.3 Bean的生命周期的配置

![图片](/images/041_02_05.png)

```java
package com.jesse.ioc.demo3;

public class Man {
    public Man(){
        System.out.println("Man被实例化了...");
    }

    public void init(){
        System.out.println("Man被初始化了...");
    }

    public void destory(){
        System.out.println("Man被销毁了...");
    }
}
```

```xml
   <bean id="man" class="com.jesse.ioc.demo3.Man" init-method="init" destroy-method="destory"/>
```

```java
    @Test
    public void demo2() {
        //创建工厂
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂获得类的实例：
        Man man = (Man) applicationContext.getBean("man");

        System.out.println(man);

        applicationContext.close();
    }
```

### 2.4 Bean的生命周期的完整过程

![图片](/images/041_02_06.png)

![图片](/images/041_02_07.png)

![图片](/images/041_02_08.png)

```java
package com.jesse.ioc.demo3;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.BeanNameAware;
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

public class Man implements BeanNameAware, ApplicationContextAware, InitializingBean, DisposableBean {
    private String name;

    public void setName(String name) {
        System.out.println("第二步：设置属性");
        this.name = name;
    }

    public Man(){
        System.out.println("第一步：初始化...");
    }

    public void init(){
        System.out.println("第七步：Man被初始化了...");
    }

    public void destory(){
        System.out.println("第十一步：Man被销毁了...");
    }

    @Override
    public void setBeanName(String s) {
        System.out.println("第三步：设置Bean的名称" + s);
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        System.out.println("第四步：了解工厂的信息");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("第六步：属性设置后");
    }

    public void run(){
        System.out.println("第九步：执行业务方法");
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("第十步：执行Spring的销毁方法");
    }
}

```

```java
package com.jesse.ioc.demo3;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

public class MyBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object o, String s) throws BeansException {
        System.out.println("第五步：初始化前方法...");
        return o;
    }

    @Override
    public Object postProcessAfterInitialization(Object o, String s) throws BeansException {
        System.out.println("第八步：初始化后方法...");
        return o;
    }
}
```

```xml
    <bean id="man" class="com.jesse.ioc.demo3.Man" init-method="init" destroy-method="destory">
        <property name="name" value="小明"/>
    </bean>

    <bean class="com.jesse.ioc.demo3.MyBeanPostProcessor"/>
```

```java
    @Test
    public void demo2() {
        //创建工厂
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂获得类的实例：
        Man man = (Man) applicationContext.getBean("man");

        man.run();

        applicationContext.close();
    }
```

### 2.5 BeanPostProcessor的作用（演示如何增强类方法）

```java
package com.jesse.ioc.demo3;

public interface UseDao {
    public void findAll();

    public void save();

    public void update();

    public void delete();
}

```

```java
package com.jesse.ioc.demo3;

public class UseDaoImpl implements UseDao {
    @Override
    public void findAll() {
        System.out.println("查询用户...");
    }

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
}

```

```java
package com.jesse.ioc.demo3;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class MyBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        //System.out.println("第五步：初始化前方法...");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(final Object bean, String beanName) throws BeansException {
        //System.out.println("第八步：初始化后方法啊...");
        if ("useDao".equals(beanName)){
            Object proxy = Proxy.newProxyInstance(bean.getClass().getClassLoader(), bean.getClass().getInterfaces(), new InvocationHandler() {
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    if ("save".equals(method.getName())){
                        System.out.println("权限校验==========");
                        return method.invoke(bean,args);
                    }
                    return method.invoke(bean,args);
                }
            });
            return proxy;
        }else {
            return bean;
        }
    }
}

```

```xml
    <bean class="com.jesse.ioc.demo3.MyBeanPostProcessor"/>

    <bean id="useDao" class="com.jesse.ioc.demo3.UseDaoImpl"/>
```

```java
    @Test
    public void demo3() {
        //创建工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂获得类的实例：
        UseDao useDao = (UseDao) applicationContext.getBean("useDao");

        useDao.findAll();
        useDao.save();
        useDao.update();
        useDao.delete();

    }
```

## 3 Spring的Bean管理-XML方式（下）

### 3.1 属性注入的方式

![图片](/images/041_02_09.png)

### 3.2 构造方法的属性注入

![图片](/images/041_02_10.png)

```java
package com.jesse.ioc.demo4;

public class User {
    private String name;
    private Integer age;

    public User(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```

```xml
    <!--Bean的构造方法的属性注入-->
    <bean id="user" class="com.jesse.ioc.demo4.User">
        <constructor-arg name="name" value="小明"/>
        <constructor-arg name="age" value="23"/>
    </bean>
```

```java
    @Test
    public void demo1(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = (User)applicationContext.getBean("user");
        System.out.println(user);
    }
```

### 3.3 set方法的属性注入

![图片](/images/041_02_11.png)

```java
package com.jesse.ioc.demo4;

public class Person {
    private String name;
    private Integer age;
    private Cat cat;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", cat=" + cat +
                '}';
    }
}

```

```java
package com.jesse.ioc.demo4;

public class Cat {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Cat{" +
                "name='" + name + '\'' +
                '}';
    }
}

```

```xml
    <!--Bean的set方法的属性注入-->
    <bean id="person" class="com.jesse.ioc.demo4.Person">
        <property name="name" value="李四"/>
        <property name="age" value="32"/>
        <property name="cat" ref="cat"/>
    </bean>

    <bean id="cat" class="com.jesse.ioc.demo4.Cat">
        <property name="name" value="ketty"/>
    </bean>
```

```java
    @Test
    public void demo2(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        Person person = (Person)applicationContext.getBean("person");
        System.out.println(person);
    }
```

### 3.4 p名称空间的属性注入

![图片](/images/041_02_12.png)

```xml
       xmlns:p="http://www.springframework.org/schema/p"
    <!--Bean的p名称空间的属性注入-->
    <bean id="person" class="com.jesse.ioc.demo4.Person" p:name="大黄" p:age="34" p:cat-ref="cat"/>

    <bean id="cat" class="com.jesse.ioc.demo4.Cat" p:name="小黄">
    </bean>
```

### 3.5 SpEL的属性注入

![图片](/images/041_02_13.png)

```java
package com.jesse.ioc.demo4;

public class Category {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Category{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

```java
package com.jesse.ioc.demo4;

public class ProductInfo {

    public Double calculatePrice(){
        return Math.random() * 199;
    }
}
```

```java
package com.jesse.ioc.demo4;

public class Product {
    private String name;
    private Double price;

    private Category category;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public Category getCategory() {
        return category;
    }

    public void setCategory(Category category) {
        this.category = category;
    }

    @Override
    public String toString() {
        return "Product{" +
                "name='" + name + '\'' +
                ", price=" + price +
                ", category=" + category +
                '}';
    }
}
```

```xml
    <!--Bean的SpEL的属性注入-->
    <bean id="category" class="com.jesse.ioc.demo4.Category">
        <property name="name" value="#{'服装'}"/>
    </bean>

    <bean id="productInfo" class="com.jesse.ioc.demo4.ProductInfo"/>

    <bean id="product" class="com.jesse.ioc.demo4.Product">
        <property name="name" value="#{'男装'}"/>
        <property name="price" value="#{productInfo.calculatePrice()}"/>
        <property name="category" value="#{category}"/>
    </bean>
```

```java
    @Test
    public void demo3(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        Product product = (Product)applicationContext.getBean("product");
        System.out.println(product);
    }
```

### 3.6 复杂类型的属性注入

![图片](/images/041_02_14.png)

```java
package com.jesse.ioc.demo5;

import java.util.*;

public class CollectionBean {
    private String[] arrs; //数组类型
    private List<String> list; //List集合类型
    private Set<String> set; //Set集合类型
    private Map<String,Integer> map; //Map集合类型
    private Properties properties; //属性类型

    public String[] getArrs() {
        return arrs;
    }

    public void setArrs(String[] arrs) {
        this.arrs = arrs;
    }

    public List<String> getList() {
        return list;
    }

    public void setList(List<String> list) {
        this.list = list;
    }

    public Set<String> getSet() {
        return set;
    }

    public void setSet(Set<String> set) {
        this.set = set;
    }

    public Map<String, Integer> getMap() {
        return map;
    }

    public void setMap(Map<String, Integer> map) {
        this.map = map;
    }

    public Properties getProperties() {
        return properties;
    }

    public void setProperties(Properties properties) {
        this.properties = properties;
    }

    @Override
    public String toString() {
        return "CollectionBean{" +
                "arrs=" + Arrays.toString(arrs) +
                ", list=" + list +
                ", set=" + set +
                ", map=" + map +
                ", properties=" + properties +
                '}';
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- UserService的创建权交给了Spring -->
    <!--<bean id="userService" class="com.jesse.ioc.demo1.UserServiceImpl">
        &lt;!&ndash;设置属性&ndash;&gt;
        <property name="name" value="李四"/>
    </bean>-->

    <!--Bean的实例化的三种方式-->
    <!--第一种：无参构造器的方式-->
    <!--<bean id="bean1" class="com.jesse.ioc.demo2.Bean1"/>-->
    <!--第二种：静态工厂的方式-->
    <!--<bean id="bean2" class="com.jesse.ioc.demo2.Bean2Factory" factory-method="createBean2"/>-->
    <!--第二种：实例工厂的方式-->
    <!--<bean id="bean3Factory"  class="com.jesse.ioc.demo2.Bean3Factory"/>
    <bean id="bean3" factory-bean="bean3Factory" factory-method="createBean3"/>-->

    <!--Bean的作用范围-->
    <!--<bean id="person" class="com.jesse.ioc.demo3.Person" scope="prototype"/>-->

    <!--<bean id="man" class="com.jesse.ioc.demo3.Man" init-method="init" destroy-method="destory">
        <property name="name" value="小明"/>
    </bean>-->

    <!--<bean class="com.jesse.ioc.demo3.MyBeanPostProcessor"/>

    <bean id="useDao" class="com.jesse.ioc.demo3.UseDaoImpl"/>-->

    <!--Bean的构造方法的属性注入-->
    <!--<bean id="user" class="com.jesse.ioc.demo4.User">
        <constructor-arg name="name" value="小明"/>
        <constructor-arg name="age" value="23"/>
    </bean>-->

    <!--Bean的set方法的属性注入-->
    <!--<bean id="person" class="com.jesse.ioc.demo4.Person">
        <property name="name" value="李四"/>
        <property name="age" value="32"/>
        <property name="cat" ref="cat"/>
    </bean>

    <bean id="cat" class="com.jesse.ioc.demo4.Cat">
        <property name="name" value="ketty"/>
    </bean>-->

    <!--Bean的p名称空间的属性注入-->
    <bean id="person" class="com.jesse.ioc.demo4.Person" p:name="大黄" p:age="34" p:cat-ref="cat"/>

    <bean id="cat" class="com.jesse.ioc.demo4.Cat" p:name="小黄">
    </bean>

    <!--Bean的SpEL的属性注入-->
    <bean id="category" class="com.jesse.ioc.demo4.Category">
        <property name="name" value="#{'服装'}"/>
    </bean>

    <bean id="productInfo" class="com.jesse.ioc.demo4.ProductInfo"/>

    <bean id="product" class="com.jesse.ioc.demo4.Product">
        <property name="name" value="#{'男装'}"/>
        <property name="price" value="#{productInfo.calculatePrice()}"/>
        <property name="category" value="#{category}"/>
    </bean>

    <!--集合类型的属性注入-->
    <bean id="collectionBean" class="com.jesse.ioc.demo5.CollectionBean">
        <!--数组类型-->
        <property name="arrs">
            <list>
                <value>aaa</value>
                <value>bbb</value>
                <value>ccc</value>
            </list>
        </property>
        <!--List集合的属性注入-->
        <property name="list">
            <list>
                <value>111</value>
                <value>222</value>
                <value>333</value>
            </list>
        </property>
        <!--Set集合的属性注入-->
        <property name="set">
            <set>
                <value>ddd</value>
                <value>eee</value>
                <value>fff</value>
            </set>
        </property>
        <!--Map集合的属性注入-->
        <property name="map">
            <map>
                <entry key="aaa" value="111"/>
                <entry key="bbb" value="222"/>
                <entry key="ccc" value="333"/>
            </map>
        </property>
        <!--Properties的属性注入-->
        <property name="properties">
            <props>
                <prop key="username">root</prop>
                <prop key="password">1234</prop>
            </props>
        </property>
    </bean>
</beans>
```

```java
package com.jesse.ioc.demo5;

import com.jesse.ioc.demo4.Person;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SpringDemo5 {
    @Test
    public void demo1(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        CollectionBean collectionBean = (CollectionBean)applicationContext.getBean("collectionBean");
        System.out.println(collectionBean);
    }
}

```

## 4 Spring的Bean管理-注解方式

### 4.1 Bean的管理

![图片](/images/041_02_15.png)

```java
package com.jesse.demo1;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

/**
 * Spring的Bean管理的注解方式：
 */
//@Component("userService")
@Service("userService")
public class UserService {
    public String sayHello(String name){
        return "Hello" + name;
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启注解扫描-->
    <context:component-scan base-package="com.jesse.demo1"/>
</beans>
```

```java
package com.jesse.demo1;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SpringDemo1 {

    @Test
    public void demo1(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");

        UserService userService = (UserService) applicationContext.getBean("userService");

        String s = userService.sayHello("张三");

        System.out.println(s);
    }
}

```

### 4.2 属性注入的注解

![图片](/images/041_02_16.png)

![图片](/images/041_02_17.png)

```java
package com.jesse.demo1;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

/**
 * Spring的Bean管理的注解方式：
 */
//@Component("userService")
@Service("userService")
public class UserService {
    @Value("米饭")
    private String something;
/*    @Autowired
    @Qualifier("userDao")*/
    @Resource(name = "userDao")
    private UserDao userDao;

    public String sayHello(String name){
        return "Hello" + name;
    }

    public void eat(){
        System.out.println("eat:"+something);
    }

    public void save(){
        System.out.println("Service中保存用户...");
        userDao.save();
    }
}

```

```java
package com.jesse.demo1;

import org.springframework.stereotype.Repository;

@Repository("userDao")
public class UserDao {

    public void save(){
        System.out.println("DAO中保存用户...");
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启注解扫描-->
    <context:component-scan base-package="com.jesse.demo1"/>
</beans>
```

```java
    @Test
    public void demo3(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");

        UserService userService = (UserService) applicationContext.getBean("userService");

        userService.save();
    }
```

### 4.3 其他注解

#### 4.3.1 @PostConstruct和@PreDestory

![图片](/images/041_02_18.png)

```java
package com.jesse.demo2;

import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

@Component("bean1")
public class Bean1 {

    @PostConstruct
    public void init(){
        System.out.println("initBean...");
    }

    public void say(){
        System.out.println("say....");
    }

    @PreDestroy
    public void destory(){
        System.out.println("destoryBean...");
    }
}

```

```java
    @Test
    public void demo1(){
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");

        Bean1 bean1 = (Bean1) applicationContext.getBean("bean1");

        bean1.say();

        applicationContext.close();
    }
```

#### 4.3.2 @Scope

![图片](/images/041_02_19.png)

```java
package com.jesse.demo2;

import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component("bean2")
@Scope("prototype")
public class Bean2 {
}

```

```java
    @Test
    public void demo2(){
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");

        Bean2 bean1 = (Bean2) applicationContext.getBean("bean2");
        Bean2 bean2 = (Bean2) applicationContext.getBean("bean2");

        System.out.println(bean1 == bean2);
    }
```

## 5 Spring的XML和注解整合开发

![图片](/images/041_02_20.png)

```java
package com.jesse.demo3;

public class CategoryDao {
    public void save(){
        System.out.println("CategoryDao中的save方法执行了...");
    }
}
```

```java
package com.jesse.demo3;

public class ProductDao {
    public void save(){
        System.out.println("ProductDao的save方法执行了...");
    }
}
```

```java
package com.jesse.demo3;

import javax.annotation.Resource;

public class ProductService {
    @Resource(name = "categoryDao")
    private CategoryDao categoryDao;
    @Resource(name = "productDao")
    private ProductDao productDao;

/*    public void setCategoryDao(CategoryDao categoryDao) {
        this.categoryDao = categoryDao;
    }

    public void setProductDao(ProductDao productDao) {
        this.productDao = productDao;
    }*/

    public void save(){
        System.out.println("ProductService的save方法执行了...");
        categoryDao.save();
        productDao.save();
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启注解扫描-->
<!--    <context:component-scan base-package="com.jesse"/>-->
    <context:annotation-config/>
    <bean id="productService" class="com.jesse.demo3.ProductService">
        <!--<property name="productDao" ref="productDao"/>
        <property name="categoryDao" ref="categoryDao"/>-->
    </bean>

    <bean id="productDao" class="com.jesse.demo3.ProductDao"/>

    <bean id="categoryDao" class="com.jesse.demo3.CategoryDao"/>
</beans>
```

```java
package com.jesse.demo3;

import com.jesse.demo2.Bean2;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SpringDemo3 {

    @Test
    public void demo1(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");

        ProductService productService = (ProductService)applicationContext.getBean("productService");

        productService.save();
    }
}
```
