---
title: Servlet入门
categories:
  - JavaWeb入门
  - 03_Java-Web
tags:
  - muke
abbrlink: 7490
date: 2020-02-12 11:25:10
---

:star2:本文是Servlet入门的学习总结:star2:

<!-- more -->

## 1 课程概述与Tomcat

### 1.1 软件发展史

![图片](/images/023_02_01.png)

### 1.2 B/S模式执行流程

![图片](/images/023_02_02.png)

### 1.3 请求与响应

![图片](/images/023_02_03.png)

### 1.4 J2EE

![图片](/images/023_02_04.png)

#### 1.4.1 J2EE的13个功能模块

![图片](/images/023_02_05.png)

### 1.5 Apache Tomcat

![图片](/images/023_02_06.png)

#### 1.5.1 J2EE与Tomcat的关系

![图片](/images/023_02_07.png)

#### 1.5.2 Tomcat与Servlet的关系

![图片](/images/023_02_08.png)

## 2 Servlet创建与生命周期

### 2.1 第一个Servlet

#### 2.1.1 执行流程

![图片](/images/023_02_09.png)

#### 2.1.2 源码

```java
package com.jesse.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class FirstServlet extends HttpServlet{

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //接收请求发来的参数
        String name = request.getParameter("name");
        String html = "<h1 style='color:red'>hi," + name + "!</h1><hr/>";
        System.out.println("返回给浏览器的响应数据为：" + html);
        PrintWriter out = response.getWriter();
        out.println(html);    //将html发送回浏览器
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>FirstServlet</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <servlet>
      <!-- servlet的别名 -->
      <servlet-name>first</servlet-name>
      <servlet-class>com.jesse.servlet.FirstServlet</servlet-class>
  </servlet>
  <!-- 将Servlet与URL绑定 -->
 <servlet-mapping>
      <servlet-name>first</servlet-name>
    <url-pattern>/hi</url-pattern>
 </servlet-mapping>
</web-app>
```

### 2.2 标准Java-Web工程结构

![图片](/images/023_02_10.png)

#### 2.2.1 Example

```java
package com.jesse.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class SimpleServlet extends HttpServlet{
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        PrintWriter out =  response.getWriter();    //向浏览器输出的数据流
        out.println("<a href='http://www.baidu.com'>Baidu</a>");
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>FirstServlet</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <servlet>
      <!-- servlet的别名 -->
      <servlet-name>first</servlet-name>
      <servlet-class>com.jesse.servlet.FirstServlet</servlet-class>
  </servlet>
  <!-- 将Servlet与URL绑定 -->
 <servlet-mapping>
      <servlet-name>first</servlet-name>
    <url-pattern>/hi</url-pattern>
 </servlet-mapping>

   <servlet>
      <!-- servlet的别名 -->
      <servlet-name>simple</servlet-name>
      <servlet-class>com.jesse.servlet.SimpleServlet</servlet-class>
  </servlet>
  <!-- 将Servlet与URL绑定 -->
 <servlet-mapping>
      <servlet-name>simple</servlet-name>
    <url-pattern>/simple</url-pattern>
 </servlet-mapping>
</web-app>
```

### 2.3 Servlet开发步骤

![图片](/images/023_02_11.png)

### 2.4 Servlet访问方法

![图片](/images/023_02_12.png)

### 2.5 请求参数的发送与接收

![图片](/images/023_02_13.png)

#### 2.5.1 发送

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>学员信息登记表</title>
</head>
<body>
    <h1>学员信息登记表</h1>
    <form action="/FirstServlet/simple">
        姓名：<input name="name"/>
        <br/>
        电话：<input name= "mobile"/>
        <br/>
        <select name="sex"  style="width:100px;padding:5px;">
            <option value="male" >男</option>
            <option value="female">女</option>
        </select>
        <br/>
        特长：
        <input type="checkbox" name="spec" value="English"/>英语
        <input type="checkbox" name="spec" value="Program"/>编程
        <input type="checkbox" name="spec" value="Speech"/>演讲
        <input type="checkbox" name="spec" value="Swimming"/>游泳
        <br/>
        <input type="submit" value="提交">
        <br/>
    </form>
</body>
</html>
```

#### 2.5.2 接收

```java
package com.jesse.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class SimpleServlet extends HttpServlet{
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String name = request.getParameter("name");
        String mobile = request.getParameter("mobile");
        String sex = request.getParameter("sex");
        String[] specs = request.getParameterValues("spec");
        PrintWriter out =  response.getWriter();    //向浏览器输出的数据流
        out.println("<h1>name=" + name + "</h1>");
        out.println("<h1>mobile=" + mobile + "</h1>");
        out.println("<h1>sex=" + sex + "</h1>");
        for(String a : specs) {
            out.println("<h1>spec=" + a + "</h1>");
        }
        out.println("<a href='http://www.baidu.com'>Baidu</a>");
    }
}
```

### 2.6 Get与Post请求

#### 2.6.1 Get与Post请求方法

![图片](/images/023_02_14.png)

#### 2.6.2 Get与Post应用场景

![图片](/images/023_02_15.png)

#### 2.6.3 Get与Post处理方式

![图片](/images/023_02_16.png)

#### 2.6.4 Example

```java
package com.jesse.servlet;

import java.io.IOException;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class RequestMethodServlet extends HttpServlet{
    //处理get请求
    public void doGet(HttpServletRequest request , HttpServletResponse response) throws IOException {
        String name = request.getParameter("name");
        response.getWriter().println("<h1 style='color:green'>" + name + "</h1>");
    }

    //处理post请求
    public void doPost(HttpServletRequest request , HttpServletResponse response) throws IOException {
        String name = request.getParameter("name");
        response.getWriter().println("<h1 style='color:red'>" + name + "</h1>");
    }
}
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>学员信息登记表</title>
</head>
<body>
    <h1>学员信息登记表</h1>
    <form action="/FirstServlet/request_method" method="post">
        姓名：<input name="name"/>
        <br/>
        电话：<input name= "mobile"/>
        <br/>
        <select name="sex"  style="width:100px;padding:5px;">
            <option value="male" >男</option>
            <option value="female">女</option>
        </select>
        <br/>
        特长：
        <input type="checkbox" name="spec" value="English"/>英语
        <input type="checkbox" name="spec" value="Program"/>编程
        <input type="checkbox" name="spec" value="Speech"/>演讲
        <input type="checkbox" name="spec" value="Swimming"/>游泳
        <br/>
        <input type="submit" value="提交">
        <br/>
    </form>
</body>
</html>
```

### 2.7 Servlet的生命周期

![图片](/images/023_02_17.png)

#### 2.7.1 Example

```java
package com.jesse.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class FirstServlet extends HttpServlet{

    public FirstServlet() {
        System.out.println("正在创建FirstServlet对象");
    }

    @Override
    public void init(ServletConfig config) throws ServletException {
        System.out.println("正在初始化FirstServlet对象");
    }

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String add1 = request.getParameter("add1");
        int a = 0;
        try {
            a = Integer.parseInt(add1);
        } catch (NumberFormatException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        String add2 = request.getParameter("add2");
        int b = 0;
        try {
            b = Integer.parseInt(add2);
        } catch (NumberFormatException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

        response.setContentType("text/html;charset=utf-8"); //2
        response.setCharacterEncoding("utf-8"); //3
        PrintWriter out = response.getWriter();
        out.println("<h1>加法计算器</h1>");
        out.println("<h2>运算结果为："+ (a+b) + "</h2>");
    }

    @Override
    public void destroy() {
        System.out.println("正在销毁servlet对象");
    }

}
```

## 3 注解配置Servlet

![图片](/images/023_02_18.png)

### 3.1 Example

```java
package com.jesse.servlet;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/anno")
public class AnnotationServlet extends HttpServlet{

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // TODO Auto-generated method stub
        resp.getWriter().println("I'm annotation servlet!");
    }

}
```

## 4 启动时加载Servlet

### 4.1 方式一

![图片](/images/023_02_19.png)

```java
package com.jesse.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;

public class CreateServlet extends HttpServlet{

    @Override
    public void init() throws ServletException {
        System.out.println("正在创建数据库");
    }

}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>FirstServlet</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>

  <servlet>
      <!-- servlet的别名 -->
      <servlet-name>create</servlet-name>
      <servlet-class>com.jesse.servlet.CreateServlet</servlet-class>
      <!-- 数字越小，越先启动 -->
      <load-on-startup>0</load-on-startup>  
  </servlet>
</web-app>
```

### 4.2 方式二（注解）

```java
package com.jesse.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;

//注解方式必须设置url
@WebServlet(urlPatterns="/unused",loadOnStartup=2)
public class AnalysisServlet extends HttpServlet{

    @Override
    public void init() throws ServletException {
        // TODO Auto-generated method stub
        System.out.println("正在分析结果");
    }

}
```
