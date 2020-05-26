---
title: Spring的MVC入门与数据绑定
categories:
  - SSM到Spring Boot入门与综合实战
  - 02_Spring的MVC入门与SSM整合开发
tags:
  - imooc
abbrlink: 41209
date: 2020-05-25 18:02:22
---

:star2:本文是Spring的MVC入门与数据绑定的开发总结:star2:

<!-- more -->

## 1 Spring的MVC初体验

### 1.1 Spring的MVC

![图片](/images/042_01_01.png)

![图片](/images/042_01_02.png)

### 1.2 Spring的MVC环境配置

![图片](/images/042_01_03.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.jesse</groupId>
    <artifactId>secondspringmvc</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
    </dependencies>

</project>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <!--
            DispatcherServlet是Spring MVC最核心的对象
            DispatcherServlet用于拦截Http请求，
            并根据请求的URL调用与之对应的Controller方法，来完成Http请求的处理
        -->
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--applicationContext.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <!--
            在Web应用启动时自动创建Spring IOC容器，
            并初始化DispatcherServlet
        -->
        <load-on-startup>0</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--"/"拦截所有请求（不包括.jsp）,"/*"拦截所有请求（包括jsp）-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mv="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--
        context:component-scan 标签作用
        在Spring IOC初始化过程中，自动创建并管理com.jesse.springmvc及子包中
        拥有以下注解的对象。
        @Component
        @Repository
        @Service
        @Controller
    -->
    <context:component-scan base-package="com.jesse.springmvc"/>
    <!--启动Spring MVC的注解开发模式-->
    <mvc:annotation-driven/>
    <!--将图片/JS/CSS等静态资源排除在外，可提高执行效率-->
    <mvc:default-servlet-handler/>
</beans>
```

```java
package com.jesse.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * Created by Kong on 2020/5/25.
 */
@Controller
public class TestController {

    @GetMapping("/t")   //localhost/t
    @ResponseBody   //直接向响应输出字符串数据，不跳转页面
    public String test(){
        return "Hello Spring MVC";
    }
}

```

## 2 Spring的MVC数据绑定

### 2.1 URL-Mapping

![图片](/images/042_01_04.png)

![图片](/images/042_01_05.png)

```java
package com.jesse.springmvc.controller;

import com.jesse.springmvc.entity.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

/**
 * Created by Kong on 2020/5/25.
 */
@Controller
@RequestMapping("/um")
public class URLMappingController {
    @GetMapping("/g")
    //@RequestMapping(value = "/g", method = RequestMethod.GET) 它与GetMapping等价
    @ResponseBody
    public String getMapping(){
        return "This is get method";
    }

    //注意浏览器只能提交get请求，只有表单能够提交post请求
    @PostMapping("/p")
    @ResponseBody
    public String postMapping(){
        return "This is post method";
    }

}

```

### 2.2 Controller方法参数接收请求参数

```java
    @GetMapping("/g")
    //@RequestMapping(value = "/g", method = RequestMethod.GET) 它与GetMapping等价
    @ResponseBody
    public String getMapping(@RequestParam("manager_name") String managerName){
        System.out.println("managerName:" + managerName);
        return "This is get method";
    }

    @PostMapping("/p")
    @ResponseBody
    public String postMapping(String username, Long password){
        //User u = new User()
        //u.setUsername(username)
        //request.getParameter()
        System.out.println(username + ":" + password);
        return "This is post method";
    }
```

### 2.3 Controller实体对象接收请求参数

```java
package com.jesse.springmvc.entity;

/**
 * Created by Kong on 2020/5/25.
 */
public class User {
    private String username;
    private Long password;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Long getPassword() {
        return password;
    }

    public void setPassword(Long password) {
        this.password = password;
    }
}

```

```java
    @PostMapping("/p1")
    @ResponseBody
    public String postMapping1(User user, String  username){
        System.out.println(user.getUsername() + ":" + user.getPassword());
        return "This is post method";
    }
```

### 2.4 接收表单复合数据

```html
        <form action="./apply" method="post">
        <h3>您的姓名</h3>
        <input name="name" class="text"  style="width: 150px">
        <h3>您正在学习的技术方向</h3>
        <select name="course" style="width: 150px">
            <option value="java">Java</option>
            <option value="h5">HTML5</option>
            <option value="python">Python</option>
            <option value="php">PHP</option>
        </select>
        <div>
            <h3>您的学习目的：</h3>
            <input type="checkbox" name="purpose" value="1">就业找工作
            <input type="checkbox" name="purpose" value="2">工作要求
            <input type="checkbox" name="purpose" value="3">兴趣爱好
            <input type="checkbox" name="purpose" value="4">其他
        </div>
        <div style="text-align: center;padding-top:10px" >
            <input type="submit" value="提交" style="width:100px">
        </div>
        </form>
```

```java
@Controller
public class FormController {
    //@PostMapping("/apply")
    @ResponseBody
    public String apply(@RequestParam(value = "n",defaultValue = "ANON") String name, String course, Integer[] purpose){
        System.out.println(name);
        System.out.println(course);
        for (Integer p : purpose) {
            System.out.println(p);
        }
        return "SUCCESS";
    }

    //@PostMapping("/apply")
    @ResponseBody
    public String apply(String name, String course, @RequestParam List<Integer> purpose){
        System.out.println(name);
        System.out.println(course);
        for (Integer p : purpose) {
            System.out.println(p);
        }
        return "SUCCESS";
    }

    //@PostMapping("/apply")
    @ResponseBody
    public String apply(Form form){
        return "SUCCESS";
    }

    //Map接收复合数据，存在数据丢失问题
    @PostMapping("/apply")
    @ResponseBody
    public String apply(@RequestParam Map map){
        return "SUCCESS";
    }
}
```

### 2.5 关联对象赋值

```html
        <h3>收货人</h3>
        <input name="delivery.name" class="text"  style="width: 150px">
        <h3>联系电话</h3>
        <input name="delivery.mobile" class="text"  style="width: 150px">
        <h3>收货地址</h3>
        <input name="delivery.address" class="text"  style="width: 150px">
```

```java
public class Form {
    private String name;
    private String course;
    private List<Integer> purpose;
    private Delivery delivery = new Delivery();

    public Delivery getDelivery() {
        return delivery;
    }

    public void setDelivery(Delivery delivery) {
        this.delivery = delivery;
    }
```

```java
    @PostMapping("/apply")
    @ResponseBody
    public String applyDelivery(Form form){
        System.out.println(form.getDelivery().getName());
        return "SUCCESS";
    }
```

### 2.6 日期类型转换

- 注解方式

```java
    @PostMapping("/p1")
    @ResponseBody
    public String postMapping1(User user, String  username,@DateTimeFormat(pattern = "yyyy-MM-dd") Date createTime){
        System.out.println(user.getUsername() + ":" + user.getPassword());
        return "This is post method";
    }
```

- 全局方式（全局和注解同时存在，全局优先）

```java
package com.jesse.springmvc.converter;

import org.springframework.core.convert.converter.Converter;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * Created by Kong on 2020/5/26.
 */
public class MyDateConverter implements Converter<String, Date> {
    public Date convert(String s) {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        try {
            Date d = sdf.parse(s);
            return d;
        } catch (ParseException e) {
            return null;
        }
    }
}
```

```xml
    <mvc:annotation-driven conversion-service="conversionService"/>
    <bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.jesse.springmvc.converter.MyDateConverter"/>
            </set>
        </property>
    </bean>
```

## 3 中文乱码问题解决

![图片](/images/042_01_06.png)

### 3.1 解决请求中的中文乱码

- Tomcat8.0版本以后，Get请求默认URIEncoding="UTF-8"
- Post请求解决中文乱码：

```xml
    <filter>
        <filter-name>characterFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### 3.2 解决响应中的中文乱码

```xml
    <mvc:annotation-driven conversion-service="conversionService">
        <mvc:message-converters>
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <property name="supportedMediaTypes">
                    <list>
                        <!--response.setContentType("text/html;charset=utf-8)-->
                        <value>text/html;charset=utf-8</value>
                    </list>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
```

## 4 响应输出

![图片](/images/042_01_07.png)

### 4.1 @ResponseBody

![图片](/images/042_01_08.png)

### 4.2 ModelAndView

![图片](/images/042_01_09.png)

```java
    @GetMapping("/view")
    public ModelAndView showView(Integer id){
        ModelAndView mav = new ModelAndView("/view.jsp");
        User user = new User();
        if (id == 1){
            user.setUsername("lily");
        }else if (id == 2){
            user.setUsername("smith");
        }
        mav.addObject("USER", user);
        return mav;
    }
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>I'm view page</h1>
    <hr>
    <h2>Username:${USER.username}</h2>
</body>
</html>
```

## 5 ModelAndView对象核心用法

![图片](/images/042_01_10.png)

```java
    @GetMapping("/view")
    public ModelAndView showView(Integer id){
        //ModelAndView mav = new ModelAndView("redirect:/view.jsp");
        ModelAndView mav = new ModelAndView();
        mav.setViewName("redirect:/view.jsp");
        User user = new User();
        if (id == 1){
            user.setUsername("lily");
        }else if (id == 2){
            user.setUsername("smith");
        }
        mav.addObject("USER", user);
        return mav;
    }
```

```java
    //String与ModelMap代替ModelAndView
    //Controller方法返回String的情况
    //1. 方法被@ResponseBody描述，SpringMVC直接响应String字符串本身
    //2. 方法不存在@ResponseBody，则SpringMVC处理String指代的视图（页面）
    @GetMapping("/view1")
    //@ResponseBody
    public String showView1(Integer id, ModelMap modelMap){
        String view = "/view.jsp";
        User user = new User();
        if (id == 1){
            user.setUsername("lily");
        }else if (id == 2){
            user.setUsername("smith");
        }
        modelMap.addAttribute("USER", user);
        return view;
    }
```

## 6 Spring的MVC整合FreeMarker

![图片](/images/042_01_11.png)

![图片](/images/042_01_12.png)

![图片](/images/042_01_13.png)

```java
@Controller
@RequestMapping("/fm")
public class FreemarkerController {
    @GetMapping("/test")
    public ModelAndView showTest(){
        ModelAndView mav = new ModelAndView("/test");
        User user = new User();
        user.setUsername("andy");
        mav.addObject("USER",user);
        return mav;
    }
}
```
