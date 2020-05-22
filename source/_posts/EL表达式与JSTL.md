---
title: EL表达式与JSTL
categories:
  - JavaWeb入门
  - 03_Java-Web
tags:
  - imooc
abbrlink: 36589
date: 2020-02-18 10:15:13
---

:star2:本文是EL表达式与JSTL的学习总结:star2:

<!-- more -->

## 1 EL表达式

### 1.1 什么是EL表达式

![图片](/images/023_05_01.png)

### 1.2 EL的作用域对象

![图片](/images/023_05_02.png)

### 1.3 EL表达式的输出

![图片](/images/023_05_03.png)

## 2 JSTL

### 2.1 JSTL是什么

![图片](/images/023_05_04.png)

### 2.2 下载JSTL标签库

![图片](/images/023_05_05.png)

### 2.3  JSTL的标签库种类

![图片](/images/023_05_06.png)

### 2.4 引用JSTL核心库

![图片](/images/023_05_07.png)

### 2.5 Example1

```java
package com.jesse.jstl;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class JstlServlet
 */
@WebServlet("/jstl")
public class JstlServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    /**
     * @see HttpServlet#HttpServlet()
     */
    public JstlServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setAttribute("score", 78);
        request.setAttribute("grade", "B");
        request.getRequestDispatcher("/core.jsp").forward(request, response);
    }

}
```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri = "http://java.sun.com/jsp/jstl/core"  prefix = "c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <h1>${requestScope.score}</h1>
    <c:if test="${score >= 60 }">
        <h1 style = "color:green">恭喜，你已通过测试</h1>
    </c:if>
    <c:if test="${score < 60 }">
        <h1 style = "color:red">对不起，再接再厉</h1>
    </c:if>
    <!-- choose when otherwise -->
    ${grade}
    <c:choose>
        <c:when test="${grade == 'A'}">
            <h2>你很优秀</h2>
        </c:when>
        <c:when test="${grade == 'B' }">
            <h2>不错呦</h2>
        </c:when>
        <c:when test="${grade == 'C' }">
            <h2>水平一般，需要提高</h2>
        </c:when>
        <c:otherwise>
            <h2>一切随缘吧</h2>
        </c:otherwise>
    </c:choose>
</body>
</html>
```

### 2.5 Example2

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix="fmt"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core"  prefix="c"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <%
        request.setAttribute("amt", 1987654.326);
        request.setAttribute("now", new java.util.Date());
        request.setAttribute("html", "<a href='index.html'>index</a>");
        request.setAttribute("nothing", null);
    %>
    <h2>${now }</h2>
    <!-- formateDate pattern
    yyyy - 四位年
    MM  - 两位月
    dd   - 两位日
    HH  - 24小时制 
    hh    -  12小时制
    mm  - 分钟
    ss     - 秒数
    SSS  - 毫秒  
     -->
     <h2>
        <fmt:formatDate value="${requestScope.now }" pattern="yyyy年MM月dd日HH时mm分ss秒SSS毫秒"/>
    </h2>
    <h2>${amt }</h2>
    <h2>
        ¥<fmt:formatNumber value = "${amt }" pattern = "0,000.00"></fmt:formatNumber>元
    </h2>
    <h2>null默认值：<c:out value="${nothing }" default="无"></c:out></h2>
    <h2><c:out value="${html }" escapeXml="true"></c:out></h2>
</body>
</html>
```

### 3 案例：员工信息表

```java
package com.jesse.employee;

public class Employee {
    private Integer empno;
    private String ename;
    private String department;
    private String job;
    private Float salary;

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
package com.jesse.employee;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContext;
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
        ServletContext context = request.getServletContext();
        if(context.getAttribute("employees") == null) {
            List<Employee> list = new ArrayList<Employee>();
            list.add(new Employee(1100, "孔金星", "Java工程师", "项目经理", 20000f));
            list.add(new Employee(1101, "张东方", "光学工程师", "研发经理", 25000f));
            context.setAttribute("employees", list);
        }
        request.getRequestDispatcher("/employee.jsp").forward(request, response);
    }

}
```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
 <%@ taglib uri="http://java.sun.com/jsp/jstl/core"  prefix="c"%>
 <%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>
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
        <h1 style="text-align: center">JESSE员工信息表</h1>
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

                <c:forEach items = "${applicationScope.employees }"  var = "emp"  varStatus="idx">
                <tr>
                    <td>${idx.index+1 }</td>
                    <td>${emp.empno }</td>
                    <td>${emp.ename }</td>
                    <td>${emp.department }</td>
                    <td>${emp.job }</td>
                    <td style="color: red;font-weight: bold">￥<fmt:formatNumber value = "${emp.salary }" pattern="0,000.00"></fmt:formatNumber> </td>
                </tr>
                </c:forEach>

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
                <form action="/employee/create" method="post" >
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

```java
package com.jesse.employee;

import java.io.IOException;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CreateServlet
 */
@WebServlet("/create")
public class CreateServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public CreateServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("UTF-8");
        String empno = request.getParameter("empno");
        String ename = request.getParameter("ename");
        String department = request.getParameter("department");
        String job =  request.getParameter("job");
        String salary = request.getParameter("salary");
        Employee emp = new Employee(Integer.parseInt(empno), ename, department, job, Float.parseFloat(salary));
        ServletContext context = request.getServletContext();
        List employees = (List)context.getAttribute("employees");
        employees.add(emp);
        context.setAttribute("employees", employees);
        request.getRequestDispatcher("/employee.jsp").forward(request, response);

    }

}
```
