---
title: jQuery与Ajax入门
categories:
  - JavaWeb入门
  - 04_Java-Web进阶
tags:
  - muke
abbrlink: 29345
date: 2020-02-19 20:04:13
---

:star2:本文是jQuery与Ajax入门的学习总结:star2:

<!-- more -->

## 1 jQuery

### 1.1 JavaScript库

![图片](/images/024_02_01.png)

### 1.2 jQuery介绍

![图片](/images/024_02_02.png)

### 1.3 jQuery选择器

#### 1.3.1 基本选择器

![图片](/images/024_02_03.png)

#### 1.3.2 层叠选择器

![图片](/images/024_02_04.png)

#### 1.3.3 属性选择器

![图片](/images/024_02_05.png)

#### 1.3.4 位置选择器

![图片](/images/024_02_06.png)

#### 1.3.5 表单选择器

![图片](/images/024_02_07.png)

### 1.4 操作元素属性

![图片](/images/024_02_08.png)

![图片](/images/024_02_09.png)

### 1.5 操作元素CSS属性

![图片](/images/024_02_10.png)

![图片](/images/024_02_11.png)

### 1.6 设置元素内容

![图片](/images/024_02_12.png)

![图片](/images/024_02_13.png)

### 1.7 jQuery中的事件及处理

![图片](/images/024_02_14.png)

![图片](/images/024_02_15.png)

![图片](/images/024_02_16.png)

## 2 Ajax

### 2.1 Ajax的介绍

![图片](/images/024_02_17.png)

### 2.2 Ajax使用

#### 2.2.1 创建与发送

![图片](/images/024_02_191.png)

![图片](/images/024_02_19.png)

![图片](/images/024_02_18.png)

#### 2.2.2 处理响应

![图片](/images/024_02_21.png)

![图片](/images/024_02_22.png)

![图片](/images/024_02_20.png)

#### 2.2.3 Example

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <input id="btnload"  type= "button"  value="加载">
    <div id="divContent"></div>
    <script>
        document.getElementById("btnload").onclick = function(){
            //1. 创建XMLHttpRequest对象
            var xmlhttp;
            if(window.XMLHttpRequest){
                xmlhttp = new XMLHttpRequest();
            }else{
                xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
            }
            console.log(xmlhttp);
            //2. 发送Ajax请求
            xmlhttp.open("GET", "/ajax/content", true);
            xmlhttp.send();
            //3. 处理服务器响应
            xmlhttp.onreadystatechange = function(){
                if(xmlhttp.readyState == 4 && xmlhttp.status == 200){
                    var t = xmlhttp.responseText;
                    alert(t);
                    document.getElementById("divContent").innerHTML = t;
                }
            }
        }
    </script>
</body>
</html>
```

### 2.3 利用Ajax实现新闻列表

```java
package com.jesse.ajax;

public class News {
    private String title;
    private String date;
    private String source;
    private String content;

    public News() {
    }

    public News(String title, String date, String source, String content) {
        super();
        this.title = title;
        this.date = date;
        this.source = source;
        this.content = content;
    }
    public String getTitle() {
        return title;
    }
    public void setTitle(String title) {
        this.title = title;
    }
    public String getDate() {
        return date;
    }
    public void setDate(String date) {
        this.date = date;
    }
    public String getSource() {
        return source;
    }
    public void setSource(String source) {
        this.source = source;
    }
    public String getContent() {
        return content;
    }
    public void setContent(String content) {
        this.content = content;
    }

}
```

```java
package com.jesse.ajax;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.alibaba.fastjson.JSON;

/**
 * Servlet implementation class NewsListServlet
 */
@WebServlet("/news_list")
public class NewsListServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public NewsListServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        List<News> list = new ArrayList<News>();
        list.add(new News("TIOBE:2018年5月全球编程语言排行", "2018-5-1", "TIOBE", "..."));
        list.add(new News("TIOBE:2018年6月全球编程语言排行", "2018-5-1", "TIOBE", "..."));
        list.add(new News("TIOBE:2018年7月全球编程语言排行", "2018-5-1", "TIOBE", "..."));
        list.add(new News("TIOBE:2018年8月全球编程语言排行", "2018-5-1", "TIOBE", "..."));
        list.add(new News("TIOBE:2018年9月全球编程语言排行", "2018-5-1", "TIOBE", "..."));
        String json =JSON.toJSONString(list);
        System.out.println(json);
        response.setContentType("text/html;charset=UTF-8");
        response.getWriter().println(json);
    }

}
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <div id="container">
    </div>
<script type="text/javascript">
    //1. 创建XMLHttpRequest
    var xmlhttp;
    if(window.XMLHttpRequest){
        xmlhttp = new XMLHttpRequest();
    }else{
        xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
    }
    //2. 发送Ajax请求
    xmlhttp.open("GET", "/ajax/news_list", true);
    xmlhttp.send();
    //3. 处理服务器响应
    xmlhttp.onreadystatechange = function(){
        if(xmlhttp.readyState == 4 && xmlhttp.status == 200){
            var text = xmlhttp.responseText;
            console.log(text);
            var json = JSON.parse(text);
            console.log(json);
            var html = "";
            for(var i = 0 ; i < json.length; i++){
                var news = json[i];
                html = html + "<h1>" + news.title + "</h1>";
                html = html + "<h2>" + news.date + "&nbsp;" + news.source + "</h2>";
                html = html + "<hr/>";
            }
            document.getElementById("container").innerHTML = html;
        }
    }
</script>
</body>
</html>
```

### 2.4 同步与异步的区别

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <div id="container">
    </div>
<script type="text/javascript">
    //1. 创建XMLHttpRequest
    var xmlhttp;
    if(window.XMLHttpRequest){
        xmlhttp = new XMLHttpRequest();
    }else{
        xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
    }
    //2. 发送Ajax请求
    //true 代表异步执行  false 代表同步执行
    xmlhttp.open("GET", "/ajax/news_list", true);
    xmlhttp.send();
    console.log("请求发送完成");
    /*同步处理代码
    if(xmlhttp.readyState == 4 && xmlhttp.status == 200){
        var text = xmlhttp.responseText;
        console.log(text);
        var json = JSON.parse(text);
        console.log(json);
        var html = "";
        for(var i = 0 ; i < json.length; i++){
            var news = json[i];
            html = html + "<h1>" + news.title + "</h1>";
            html = html + "<h2>" + news.date + "&nbsp;" + news.source + "</h2>";
            html = html + "<hr/>";
        }
        document.getElementById("container").innerHTML = html;
    }*/
    //3. 处理服务器响应
    xmlhttp.onreadystatechange = function(){
        if(xmlhttp.readyState == 4 && xmlhttp.status == 200){
            var text = xmlhttp.responseText;
            console.log(text);
            var json = JSON.parse(text);
            console.log(json);
            var html = "";
            for(var i = 0 ; i < json.length; i++){
                var news = json[i];
                html = html + "<h1>" + news.title + "</h1>";
                html = html + "<h2>" + news.date + "&nbsp;" + news.source + "</h2>";
                html = html + "<hr/>";
            }
            document.getElementById("container").innerHTML = html;
        }
    }
</script>
</body>
</html>
```

## 3 jQuery对Ajax的支持

### 3.1 介绍

![图片](/images/024_02_23.png)

![图片](/images/024_02_24.png)

### 3.2 Ajax的使用

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript"  src="js/jquery-3.4.1.js"></script>
<script type="text/javascript">
    $(function(){
        $.ajax({
            "url" : "/ajax/news_list",
            "type" : "post",
            //"date" : {"t" : "pypl", "abc" : "123", "uu" : "777"},
            "data" : "t=pypl",
            "dataType" : "json",
            "success" : function(json){
                console.log(json);
                for(var i = 0; i < json.length; i++){
                    console.log(json[i].title);
                    $("#container").append("<h1>" + json[i].title + "</h1>");
                }
            },
            "error" : function(xmlhttp, errorText){
                console.log(xmlhttp);
                console.log(errorText);
                if(xmlhttp.status == "405"){
                    alert("无效的请求方式");
                }else if(xmlhttp.status == "404"){
                    alert("未找到URL资源");
                }else if(xmlhttp.status == "500"){
                    alert("服务器内部错误，请联系管理员");
                }else{
                    alert("产生异常，请联系管理员");
                }
            }
        })
    })
</script>
</head>
<body>
    <div id="container"></div>
    <p></p>
</body>
</html>
```

### 3.3 后台数据实现二级联动菜单

```java
package com.jesse.ajax;

public class Channel {
    private String code;
    private String name;

    public Channel() {

    }

    public Channel(String code, String name) {
        super();
        this.code = code;
        this.name = name;
    }
    public String getCode() {
        return code;
    }
    public void setCode(String code) {
        this.code = code;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }


}
```

```java
package com.jesse.ajax;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.alibaba.fastjson.JSON;

/**
 * Servlet implementation class ChannelServlet
 */
@WebServlet("/channel")
public class ChannelServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public ChannelServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String level = request.getParameter("level");
        String parent = request.getParameter("parent");
        List<Channel> chlist = new ArrayList<Channel>();
        if(level.equals("1")) {
            chlist.add(new Channel("ai", "前沿/区块链/人工智能"));
            chlist.add(new Channel("web", "前端/小程序/JS"));
        }else if(level.equals("2")) {
            if(parent.equals("ai")) {
                chlist.add(new Channel("micro", "微服务"));
                chlist.add(new Channel("blockchain", "区块链"));
                chlist.add(new Channel("other", "..."));
            }else if(parent.equals("web")) {
                chlist.add(new Channel("html", "HTML"));
                chlist.add(new Channel("css", "CSS"));
                chlist.add(new Channel("other", "..."));
            }
        }
        String json = JSON.toJSONString(chlist);
        response.setContentType("text/html;charset=UTF-8");
        response.getWriter().println(json);
    }

}
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="js/jquery-3.4.1.js"></script>
<script type="text/javascript">
    $(function(){
        $.ajax({
            "url" : "/ajax/channel",
            "data" : {"level" : "1"},
            "type" : "get",
            "dataType" : "json",
            "success" : function(json){
                console.log(json);
                for(var i = 0; i < json.length; i++){
                    var ch = json[i];
                    $("#lv1").append("<option value='" + ch.code + "'>" +ch.name + "</option>");
                }
            }
        })
    })

    $(function(){
        $("#lv1").on("change", function(){
            var parent = $(this).val();
            console.log(parent);
            $.ajax({
                "url" : "/ajax/channel",
                "data" : {"level" : "2", "parent" : parent},
                "dataType" : "json",
                "type" : "get",
                "success" : function(json){
                    console.log(json);
                    $("#lv2>option").remove();
                    for(var i = 0 ; i < json.length; i++){
                        var ch = json[i];
                        $("#lv2").append("<option value='" + ch.code + "'>" +ch.name + "</option>");
                    }
                }
            })
        })
    })
</script>
</head>
<body>
    <select id="lv1" style="width:200px;height:30px">
        <option selected="selected">请选择</option>
    </select>
    <select id="lv2" style="width:200px; height:30px"></select>
</body>
</html>
```
