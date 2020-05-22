---
title: JSP入门
categories:
  - JavaWeb入门
  - 03_Java-Web
tags:
  - imooc
abbrlink: 4637
date: 2020-02-13 20:18:10
---

:star2:本文是JSP入门的学习总结:star2:

<!-- more -->

## 1 JSP概念和执行流程

- JSP = Java Server Pages

### 1.1 JSP介绍

![图片](/images/023_03_01.png)

### 1.2 Servlet缺点

![图片](/images/023_03_02.png)

### 1.3 JSP特点

![图片](/images/023_03_03.png)

### 1.4 第一个jsp

```jsp
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <table>
        <tr>
            <th>year</th>
            <th>salary</th>
        </tr>
        <%
            for(int i = 0; i <=50; i++){
                out.println("<tr>");
                out.println("<td>" + i + "</td>");
                int sal = 0;
                if(i <=5){
                    sal =1500 + i * 150;
                }else if(i > 5 && i <= 10){
                    sal = 1500 + 150 * 5 + 300 * (i - 5);
                }else if(i > 10){
                    sal = 1500 + 150 * 5 + 300 * 5  + 375  * (i-10);
                }
                out.println("<td>" + sal + "</td>");
                out.println("</tr>");
            }

        %>
        <tr>
            <td>0</td>
            <td>1500</td>
        </tr>
        <tr>
            <td>1</td>
            <td>1650</td>
        </tr>
    </table>
</body>
</html>
```

## 2 JSP执行过程

![图片](/images/023_03_04.png)

### 2.1 Example

![图片](/images/023_03_05.png)

## 3 JSP语法

### 3.1 JSP代码块

![图片](/images/023_03_06.png)

### 3.2 JSP声明构造块

![图片](/images/023_03_07.png)

### 3.3 JSP输出指令

![图片](/images/023_03_08.png)

### 3.4 JSP处理指令

![图片](/images/023_03_09.png)

### 3.5 Example

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <%@ page import="java.util.*,java.text.*"  %>
    <%!
        boolean isPrime(int num){
            boolean flag = true;
            for(int j = 2; j < num; j++){
                if(num % j == 0){
                    flag = false;
                    break;
                }
            }
            return flag;
        }
    %>
    <%
        List<Integer> primes = new ArrayList();
        for(int i = 2; i <=1000; i++){
            boolean flag = isPrime(i);
            if(flag == true){
                //out.println("<h1>" + i + "</h1>");
                primes.add(i);
            }
        }
    %>
    <%
        for(int p : primes){
            //out.println("<h1>" + p + "是质数<h1>");
    %>
            <h1 style="color:red;"><%=p %>是质数</h1>
    <%
        }
    %>
</body>
</html>
```

## 4 JSP页面重用

![图片](/images/023_03_10.png)
