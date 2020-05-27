---
title: RESTful风格的应用
categories:
  - SSM到Spring Boot入门与综合实战
  - 02_Spring的MVC入门与SSM整合开发
tags:
  - imooc
abbrlink: 5138
date: 2020-05-26 20:21:22
---

:star2:本文是RESTful风格的应用的开发总结:star2:

<!-- more -->

## 1 开发第一个RESTful应用

### 1.1 RESTful开发风格

![图片](/images/042_02_01.png)

![图片](/images/042_02_02.png)

![图片](/images/042_02_03.png)

![图片](/images/042_02_04.png)

### 1.2 开发第一个RESTful应用

```java
@Controller
@RequestMapping("/restful")
public class RestfulController {

    @GetMapping("/request")
    @ResponseBody
    public String doGetRequest() {
        return "{\"message\":\"返回查询结果\"}";
    }
}
```

## 2 RESTful基本使用

### 2.1 实现RESTful实验室

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>RESTful实验室</title>
    <script src="jquery-3.3.1.min.js"></script>
    <script>
        $(function () {
            $("#btnGet").click(function () {
                $.ajax({
                    url : "/restful/request",
                    type : "get",
                    dataType : "json",
                    success : function (json) {
                        $("#message").text(json.message);
                    }
                })
            });
        })

        $(function () {
            $("#btnPost").click(function () {
                $.ajax({
                    url : "/restful/request",
                    type : "post",
                    dataType : "json",
                    success : function (json) {
                        $("#message").text(json.message)
                    }
                })
            });
        })

        $(function () {
            $("#btnPut").click(function () {
                $.ajax({
                    url : "/restful/request",
                    type : "put",
                    dataType : "json",
                    success : function (json) {
                        $("#message").text(json.message)
                    }
                })
            });
        })

        $(function () {
            $("#btnDelete").click(function () {
                $.ajax({
                    url : "/restful/request",
                    type : "delete",
                    dataType : "json",
                    success : function (json) {
                        $("#message").text(json.message)
                    }
                })
            });
        })
    </script>
</head>
<body>
    <input type="button" id="btnGet" value="发送Get请求"/>
    <input type="button" id="btnPost" value="发送Post请求"/>
    <input type="button" id="btnPut" value="发送Put请求"/>
    <input type="button" id="btnDelete" value="发送Delete请求"/>
    <h1 id="message"></h1>
</body>
</html>
```

```java
package com.jesse.restful.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

/**
 * Created by Kong on 2020/5/26.
 */
@Controller
@RequestMapping("/restful")
public class RestfulController {

    @GetMapping("/request")
    @ResponseBody
    public String doGetRequest() {
        return "{\"message\":\"返回查询结果\"}";
    }

    @PostMapping("/request")
    @ResponseBody
    public String doPostRequest(){
        return "{\"message\":\"数据新建成功\"}";
    }

    @PutMapping("/request")
    @ResponseBody
    public String doPutRequest(){
        return "{\"message\":\"数据更新成功\"}";
    }

    @DeleteMapping("/request")
    @ResponseBody
    public String doDeleteRequest(){
        return "{\"message\":\"数据删除成功\"}";
    }
}

```

### 2.2 RestController注解与路径变量

```java
//@Controller
@RestController
@RequestMapping("/restful")
public class RestfulController {

    @GetMapping("/request")
    //@ResponseBody
    public String doGetRequest() {
        return "{\"message\":\"返回查询结果\"}";
    }

    //  POST    /article/1
    //  POST    /restful/request/100
    @PostMapping("/request/{rid}")
    //@ResponseBody
    public String doPostRequest(@PathVariable("rid") Integer requestId){
        return "{\"message\":\"数据新建成功\",\"id\":" +requestId +"}";
    }

    @PutMapping("/request")
    //@ResponseBody
    public String doPutRequest(){
        return "{\"message\":\"数据更新成功\"}";
    }

    @DeleteMapping("/request")
    //@ResponseBody
    public String doDeleteRequest(){
        return "{\"message\":\"数据删除成功\"}";
    }
}
```

### 2.3 简单请求与非简单请求

![图片](/images/042_02_05.png)

```java
    @PostMapping("/request/{rid}")
    //@ResponseBody
    public String doPostRequest(@PathVariable("rid") Integer requestId, Person person){
        System.out.println(person.getName() + ":" + person.getAge());
        return "{\"message\":\"数据新建成功\",\"id\":" +requestId +"}";
    }

    @PutMapping("/request")
    //@ResponseBody
    public String doPutRequest(Person person){
        System.out.println(person.getName() + ":" + person.getAge());
        return "{\"message\":\"数据更新成功\"}";
    }
```

```xml
    <!--增加过滤器，扩展springmvc的处理请求能力-->
    <filter>
        <filter-name>formContentFilter</filter-name>
        <filter-class>org.springframework.web.filter.FormContentFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>formContentFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### 2.4 JSON序列化

```xml
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-core</artifactId>
        <version>2.9.9</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.9</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-annotations</artifactId>
        <version>2.9.9</version>
    </dependency>
```

```java
public class Person {
    private String name;
    private Integer age;
    private Date birthday;
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

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
}
```

```java
    @GetMapping("/person")
    public Person findByPersonId(Integer id){
        Person p = new Person();
        if (id==1){
            p.setName("lily");
            p.setAge(23);
        }else if (id==2){
            p.setName("smith");
            p.setAge(22);
        }
        return p;
    }

    @GetMapping("/persons")
    public List<Person> findPersons(){
        List<Person> list = new ArrayList<Person>();
        Person p1 = new Person();
        p1.setName("lily");
        p1.setAge(23);
        p1.setBirthday(new Date());

        Person p2 = new Person();
        p2.setName("smith");
        p2.setAge(22);
        p2.setBirthday(new Date());
        list.add(p1);
        list.add(p2);
        return list;
    }
```

## 3 跨域问题

### 3.1 浏览器同源策略

![图片](/images/042_02_06.png)

![图片](/images/042_02_07.png)

![图片](/images/042_02_08.png)

### 3.2 Spring的MVC解决跨域访问

![图片](/images/042_02_09.png)

![图片](/images/042_02_10.png)

- @CrossOrigin-Controller跨域注解

```java
@RestController
@RequestMapping("/restful")
//maxAge对于非简单请求的预检请求进行缓冲，避免每次都要发送两个请求
@CrossOrigin(origins = {"http://localhost:8080","http://www.imooc.com"}, maxAge = 3600)
public class RestfulController
```

- mvc:cors-Spring的MVC全局跨域配置

```xml
    <mvc:default-servlet-handler/>
    <mvc:cors>
        <mvc:mapping path="/restful/**"
                     allowed-origins="http://localhost:8080,http://www.imooc.com"
                     max-age="3600"/>
    </mvc:cors>
```
