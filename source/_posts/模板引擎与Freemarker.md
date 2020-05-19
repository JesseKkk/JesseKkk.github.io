---
title: 模板引擎与Freemarker
categories:
  - JavaWeb入门
  - 04_Java-Web进阶
tags:
  - muke
abbrlink: 23548
date: 2020-02-27 09:36:21
---

:star2:本文是模板引擎与Freemarker的学习总结:star2:

<!-- more -->

## 1 模板引擎

### 1.1 什么是模板引擎

![图片](/images/024_06_01.png)

![图片](/images/024_06_02.png)

### 1.2 主流的模板引擎

![图片](/images/024_06_03.png)

## 2 Freemarker

### 2.1 什么是Freemarker

![图片](/images/024_06_04.png)

### 2.2 JSP与Freemarker区别

![图片](/images/024_06_05.png)

### 2.3 第一个Freemarker

```java
package com.jesse.freemarker;

import java.io.IOException;
import java.io.OutputStreamWriter;
import java.util.HashMap;
import java.util.Map;

import freemarker.core.ParseException;
import freemarker.template.Configuration;
import freemarker.template.MalformedTemplateNameException;
import freemarker.template.Template;
import freemarker.template.TemplateException;
import freemarker.template.TemplateNotFoundException;

public class FreemarkerSample1 {

    public static void main(String[] args) throws TemplateNotFoundException, MalformedTemplateNameException, ParseException, IOException, TemplateException {
        // TODO Auto-generated method stub
        //1. 加载模板
        //创建核心配置对象
        Configuration config = new Configuration(Configuration.VERSION_2_3_29);
        //设置加载的目录
        config.setClassForTemplateLoading(FreemarkerSample1.class, "");
        //得到模板对象
        Template t = config.getTemplate("sample1.ftl");
        //2. 创建数据
        Map<String,Object> data = new HashMap<String,Object>();
        data.put("site", "新浪");
        data.put("url", "http://www.sina.com");
        //3. 产生输出
        t.process(data, new OutputStreamWriter(System.out));
    }

}
```

```ftl
${site}
${url}
```

## 3 Freemarker基本语法

### 3.1 FTL取值

![图片](/images/024_06_06.png)

### 3.2 分支判断

![图片](/images/024_06_07.png)

![图片](/images/024_06_08.png)

### 3.3 Example1

```java
package com.jesse.freemarker.entity;

import java.util.Date;
import java.util.Map;

public class Computer {
    private String sn;
    private String model;
    private int state;
    private String user;
    private Date dop;
    private Float price;
    private Map info;

    public Computer() {

    }

    public Computer(String sn, String model, int state, String user, Date dop, Float price, Map info) {
        super();
        this.sn = sn;
        this.model = model;
        this.state = state;
        this.user = user;
        this.dop = dop;
        this.price = price;
        this.info = info;
    }
    public String getSn() {
        return sn;
    }
    public void setSn(String sn) {
        this.sn = sn;
    }
    public String getModel() {
        return model;
    }
    public void setModel(String model) {
        this.model = model;
    }
    public int getState() {
        return state;
    }
    public void setState(int state) {
        this.state = state;
    }
    public String getUser() {
        return user;
    }
    public void setUser(String user) {
        this.user = user;
    }
    public Date getDop() {
        return dop;
    }
    public void setDop(Date dop) {
        this.dop = dop;
    }
    public Float getPrice() {
        return price;
    }
    public void setPrice(Float price) {
        this.price = price;
    }
    public Map getInfo() {
        return info;
    }
    public void setInfo(Map info) {
        this.info = info;
    }

}
```

```java
package com.jesse.freemarker;

import java.io.IOException;
import java.io.OutputStreamWriter;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

import com.jesse.freemarker.entity.Computer;

import freemarker.core.ParseException;
import freemarker.template.Configuration;
import freemarker.template.MalformedTemplateNameException;
import freemarker.template.Template;
import freemarker.template.TemplateException;
import freemarker.template.TemplateNotFoundException;

public class FreemarkerSample1 {

    public static void main(String[] args) throws TemplateNotFoundException, MalformedTemplateNameException, ParseException, IOException, TemplateException {
        // TODO Auto-generated method stub
        //1. 加载模板
        //创建核心配置对象
        Configuration config = new Configuration(Configuration.VERSION_2_3_29);
        //设置加载的目录
        config.setClassForTemplateLoading(FreemarkerSample1.class, "");
        //得到模板对象
        Template t = config.getTemplate("sample1.ftl");
        //2. 创建数据
        Map<String,Object> data = new HashMap<String,Object>();
        data.put("site", "新浪");
        data.put("url", "http://www.sina.com");
        data.put("date", new Date());
        data.put("number", 837183.883217);
        Map info = new HashMap();
        info.put("cpu", "i5");
        Computer c1 = new Computer("1234567", "ThinkPad", 2, "李四", new Date(), 12900f, info);
        data.put("computer", c1);
        //3. 产生输出
        t.process(data, new OutputStreamWriter(System.out));
    }

}
```

```ftl
<#-- FreeMarker取值 -->
${site}
${url}
<#-- !默认值 -->
${author!"不存在的属性"}
<#-- ?string格式化输出 -->
${date?string("yyyy年MM月dd日 HH:mm:ss SSS")}
${number?string("0.00")}
<#if computer.sn == "1234567">
重要设备
</#if>
SN:${computer.sn}
型号:${computer.model}
<#if computer.state == 1>
状态：正在使用
<#elseif computer.state == 2>
状态：闲置
<#elseif computer.state == 3>
状态：已作废
</#if>

<#switch computer.state>
    <#case 1>
        状态：正在使用
        <#break>
    <#case 2>
        状态：闲置
        <#break>
    <#case 3>
        状态：已作废
        <#break>
    <#default>
        状态：无效状态
</#switch>

<#-- ??代表判断对象是否为空,true不为空,false为空 -->
<#if computer.user??>
用户:${computer.user}
</#if>
采购时间:${computer.dop?string("yyyy-MM-dd")}
采购价格:${computer.price?string("0.00")}
配置信息：
--------------
CPU:${computer.info["cpu"]}
内存: ${computer.info["memory"]!"内存不存在!"}
```

### 3.4 list迭代列表与Map

![图片](/images/024_06_09.png)

![图片](/images/024_06_10.png)

### 3.5 Example2

```java
package com.jesse.freemarker.entity;

import java.util.Date;
import java.util.Map;

public class Computer {
    private String sn;
    private String model;
    private int state;
    private String user;
    private Date dop;
    private Float price;
    private Map info;

    public Computer() {

    }

    public Computer(String sn, String model, int state, String user, Date dop, Float price, Map info) {
        super();
        this.sn = sn;
        this.model = model;
        this.state = state;
        this.user = user;
        this.dop = dop;
        this.price = price;
        this.info = info;
    }
    public String getSn() {
        return sn;
    }
    public void setSn(String sn) {
        this.sn = sn;
    }
    public String getModel() {
        return model;
    }
    public void setModel(String model) {
        this.model = model;
    }
    public int getState() {
        return state;
    }
    public void setState(int state) {
        this.state = state;
    }
    public String getUser() {
        return user;
    }
    public void setUser(String user) {
        this.user = user;
    }
    public Date getDop() {
        return dop;
    }
    public void setDop(Date dop) {
        this.dop = dop;
    }
    public Float getPrice() {
        return price;
    }
    public void setPrice(Float price) {
        this.price = price;
    }
    public Map getInfo() {
        return info;
    }
    public void setInfo(Map info) {
        this.info = info;
    }



```

```java
package com.jesse.freemarker;

import java.io.IOException;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Date;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import com.jesse.freemarker.entity.Computer;

import freemarker.core.ParseException;
import freemarker.template.Configuration;
import freemarker.template.MalformedTemplateNameException;
import freemarker.template.Template;
import freemarker.template.TemplateException;
import freemarker.template.TemplateNotFoundException;

public class FreemarkerSample2 {

    public static void main(String[] args) throws TemplateNotFoundException, MalformedTemplateNameException, ParseException, IOException, TemplateException {
        // TODO Auto-generated method stub
        //1. 加载模板
        //创建核心配置对象
        Configuration config = new Configuration(Configuration.VERSION_2_3_29);
        //设置加载的目录
        config.setClassForTemplateLoading(FreemarkerSample2.class, "");
        //得到模板对象
        Template t = config.getTemplate("sample2.ftl");
        //2. 创建数据
        Map<String,Object> data = new HashMap<String,Object>();
        List<Computer> computers = new ArrayList<>();
        computers.add(new Computer("1234567", "ThinkPad", 2, null, new Date(), 12900f, new HashMap()));
        computers.add(new Computer("1234367", "HP XXX", 1, "李四", new Date(), 13900f, new HashMap()));
        computers.add(new Computer("1234767", "DELL XXX", 3, "张三", new Date(), 11900f, new HashMap()));
        computers.add(new Computer("1234967", "ACER XXX", 1, "王五", new Date(), 12500f, new HashMap()));
        computers.add(new Computer("1234867", "MSI XXX", 2, "赵六", new Date(), 12930f, new HashMap()));
        data.put("computers", computers);
        //LinkedHashMap可以保证数据按存放顺序进行提取
        Map computerMap = new LinkedHashMap();
        for(Computer c : computers) {
            computerMap.put(c.getSn(), c);
        }
        data.put("computer_map", computerMap);
        //3. 产生输出
        t.process(data, new OutputStreamWriter(System.out));
    }

}
```

```ftl
<#list computers as c>
序号:${c_index + 1}  <#-- 迭代变量_index保存了循环的索引,从0开始 -->
SN:${c.sn}
型号:${c.model}
<#switch c.state>
<#case 1>
状态:使用中
<#break>
<#case 2>
状态:闲置
<#break>
<#case 3>
状态:已作废
<#break>
</#switch>
<#if c.user??>
用户:${c.user}
</#if>
采购日期:${c.dop?string("yyyy-MM-dd")}
采购价格:${c.price?string("0.00")}
-------------------------------------------------
</#list>

===============================
<#list computer_map?keys as k>
${k}-${computer_map[k].model}
${computer_map[k].price?string("0.00")}
</#list>
```

## 4 Freemarker内建函数

```java
package com.jesse.freemarker.entity;

import java.util.Date;
import java.util.Map;

public class Computer {
    private String sn;
    private String model;
    private int state;
    private String user;
    private Date dop;
    private Float price;
    private Map info;

    public Computer() {

    }

    public Computer(String sn, String model, int state, String user, Date dop, Float price, Map info) {
        super();
        this.sn = sn;
        this.model = model;
        this.state = state;
        this.user = user;
        this.dop = dop;
        this.price = price;
        this.info = info;
    }
    public String getSn() {
        return sn;
    }
    public void setSn(String sn) {
        this.sn = sn;
    }
    public String getModel() {
        return model;
    }
    public void setModel(String model) {
        this.model = model;
    }
    public int getState() {
        return state;
    }
    public void setState(int state) {
        this.state = state;
    }
    public String getUser() {
        return user;
    }
    public void setUser(String user) {
        this.user = user;
    }
    public Date getDop() {
        return dop;
    }
    public void setDop(Date dop) {
        this.dop = dop;
    }
    public Float getPrice() {
        return price;
    }
    public void setPrice(Float price) {
        this.price = price;
    }
    public Map getInfo() {
        return info;
    }
    public void setInfo(Map info) {
        this.info = info;
    }


}
```

```java
package com.jesse.freemarker;

import java.io.IOException;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Date;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import com.jesse.freemarker.entity.Computer;

import freemarker.core.ParseException;
import freemarker.template.Configuration;
import freemarker.template.MalformedTemplateNameException;
import freemarker.template.Template;
import freemarker.template.TemplateException;
import freemarker.template.TemplateNotFoundException;

public class FreemarkerSample3 {

    public static void main(String[] args) throws TemplateNotFoundException, MalformedTemplateNameException, ParseException, IOException, TemplateException {
        // TODO Auto-generated method stub
        //1. 加载模板
        //创建核心配置对象
        Configuration config = new Configuration(Configuration.VERSION_2_3_29);
        //设置加载的目录
        config.setClassForTemplateLoading(FreemarkerSample3.class, "");
        //得到模板对象
        Template t = config.getTemplate("sample3.ftl");
        //2. 创建数据
        Map<String,Object> data = new HashMap<String,Object>();
        data.put("name", "jackson");
        data.put("brand", "bmw");
        data.put("words", "first blood");
        data.put("n", 37981.83);
        data.put("date", new Date());
        List<Computer> computers = new ArrayList<>();
        computers.add(new Computer("1234567", "ThinkPad", 2, null, new Date(), 12900f, new HashMap()));
        computers.add(new Computer("1234367", "HP XXX", 1, "李四", new Date(), 13900f, new HashMap()));
        computers.add(new Computer("1234767", "DELL XXX", 3, "张三", new Date(), 11900f, new HashMap()));
        computers.add(new Computer("1234967", "ACER XXX", 1, "王五", new Date(), 12500f, new HashMap()));
        computers.add(new Computer("1234867", "MSI XXX", 2, "赵六", new Date(), 12930f, new HashMap()));
        data.put("computers", computers);
        //3. 产生输出
        t.process(data, new OutputStreamWriter(System.out));
    }

}
```

```ftl
<#-- http://freemarker.foofun.cn -->
${name?cap_first}
${brand?upper_case}
${brand?length}
${words?replace("blood", "*****")}
${words?index_of("blood")}
<#-- 利用?string实现三目运算符的操作 -->
${(words?index_of("blood") != -1)?string("包含敏感词汇", "不包含敏感词汇")}

${n?round}
${n?floor}
${n?ceiling}

公司共有${computers?size}台电脑
第一台:${computers?first.model}
最后一台:${computers?last.model}

<#list computers?sort_by("price")?reverse as c>
    ${c.sn}-${c.price}
</#list>
```

## 5 案例：员工信息表（Freemarker与Servlet整合）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>fm-web</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <servlet>
      <servlet-name>freemarker</servlet-name>
      <servlet-class>freemarker.ext.servlet.FreemarkerServlet</servlet-class>
      <init-param>
          <param-name>TemplatePath</param-name>
          <param-value>/WEB-INF/ftl</param-value>
      </init-param>
  </servlet>
  <servlet-mapping>
      <servlet-name>freemarker</servlet-name>
      <url-pattern>*.ftl</url-pattern>
  </servlet-mapping>
</web-app>
```

```java
package com.jesse.freemaker.servlet;

public class Employee {
    private Integer empno;
    private String ename;
    private String department;
    private String job;
    private Float salary;

    public Employee(){

    }

    public Employee(Integer empno, String ename, String department, String job, Float salary) {
        super();
        this.empno = empno;
        this.ename = ename;
        this.department = department;
        this.job = job;
        this.salary = salary;
    }
    public Integer getEmpno() {
        return empno;
    }
    public void setEmpno(Integer empno) {
        this.empno = empno;
    }
    public String getEname() {
        return ename;
    }
    public void setEname(String ename) {
        this.ename = ename;
    }
    public String getDepartment() {
        return department;
    }
    public void setDepartment(String department) {
        this.department = department;
    }
    public String getJob() {
        return job;
    }
    public void setJob(String job) {
        this.job = job;
    }
    public Float getSalary() {
        return salary;
    }
    public void setSalary(Float salary) {
        this.salary = salary;
    }
}
```

```java
package com.jesse.freemaker.servlet;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


/**
 * Servlet implementation class ListServlet
 */
@WebServlet("/list")
public class ListServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public ListServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        List<Employee> list = new ArrayList<Employee>();
        list.add(new Employee(1100, "孔金星", "Java工程师", "项目经理", 20000f));
        list.add(new Employee(1101, "赖玉财", "光学工程师", "研发经理", 25000f));
        request.setAttribute("employeeList", list);
        request.getRequestDispatcher("/employee.ftl").forward(request, response);
    }

}
```

```ftl

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>员工列表</title>
    <link href="css/bootstrap.css" type="text/css" rel="stylesheet"></link>

    <script type="text/javascript" src="js/jquery-1.11.1.min.js"></script>
    <script type="text/javascript" src="js/bootstrap.js"></script>

    <style type="text/css">
        .pagination {
            margin: 0px
        }

        .pagination > li > a, .pagination > li > span {
            margin: 0 5px;
            border: 1px solid #dddddd;
        }

        .glyphicon {
            margin-right: 3px;
        }

        .form-control[readonly] {
            cursor: pointer;
            background-color: white;
        }
        #dlgPhoto .modal-body{
            text-align: center;
        }
        .preview{

            max-width: 500px;
        }
    </style>
    <script>
        $(function () {

            $("#btnAdd").click(function () {
                $('#dlgForm').modal()
            });
        })


    </script>
</head>
<body>

<div class="container">
    <div class="row">
        <h1 style="text-align: center">IMOOC员工信息表</h1>
        <div class="panel panel-default">
            <div class="clearfix panel-heading ">
                <div class="input-group" style="width: 500px;">
                    <button class="btn btn-primary" id="btnAdd"><span class="glyphicon glyphicon-zoom-in"></span>新增
                    </button>
                </div>
            </div>

            <table class="table table-bordered table-hover">
                <thead>
                <tr>
                    <th>序号</th>
                    <th>员工编号</th>
                    <th>姓名</th>
                    <th>部门</th>
                    <th>职务</th>
                    <th>工资</th>
                    <th>&nbsp;</th>
                </tr>
                </thead>
                <tbody>
                <#list employeeList as emp>
                <tr>
                    <td>${emp_index + 1}</td>
                    <td>${emp.empno?string("0")}</td>
                    <td>${emp.ename}</td>
                    <td>${emp.department}</td>
                    <td>${emp.job}</td>
                    <td style="color: red;font-weight: bold">￥${emp.salary?string("0.00")}</td>

                </tr>
                </#list>
                </tbody>
            </table>
        </div>
    </div>
</div>

<!-- 表单 -->
<div class="modal fade" tabindex="-1" role="dialog" id="dlgForm">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span>
                </button>
                <h4 class="modal-title">新增员工</h4>
            </div>
            <div class="modal-body">
                <form action="#" method="post" >
                    <div class="form-group">
                        <label for="empno">员工编号</label>
                        <input type="text" name="empno" class="form-control" id="empno" placeholder="请输入员工编号">
                    </div>
                    <div class="form-group">
                        <label for="ename">员工姓名</label>
                        <input type="text" name="ename" class="form-control" id="ename" placeholder="请输入员工姓名">
                    </div>
                    <div class="form-group">
                        <label>部门</label>
                        <select id="dname" name="department" class="form-control">
                            <option selected="selected">请选择部门</option>
                            <option value="市场部">市场部</option>
                            <option value="研发部">研发部</option>
                            <option value="后勤部">后勤部</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label>职务</label>
                        <input type="text" name="job" class="form-control" id="sal" placeholder="请输入职务">
                    </div>

                    <div class="form-group">
                        <label for="sal">工资</label>
                        <input type="text" name="salary" class="form-control" id="sal" placeholder="请输入工资">
                    </div>

                    <div class="form-group" style="text-align: center;">
                        <button type="submit" class="btn btn-primary">保存</button>
                    </div>
                </form>
            </div>

        </div><!-- /.modal-content -->
    </div><!-- /.modal-dialog -->
</div><!-- /.modal -->


</body>
</html>
```
