---
title: Servlet与JSP进阶
categories:
  - JavaWeb入门
  - 03_Java-Web
tags:
  - muke
abbrlink: 3620
date: 2020-02-14 09:36:10
---

:star2:本文是Servlet与JSP进阶的学习总结:star2:

<!-- more -->

## 1 请求与响应

### 1.1 请求的结构

- Post具有请求体，Get无请求体

![图片](/images/023_04_01.png)

### 1.2 多端开发技巧

```java
package com.jesse.servlet;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class UserAgentServlet
 */
@WebServlet("/ua")
public class UserAgentServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public UserAgentServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // TODO Auto-generated method stub
        String  userAgent = request.getHeader("User-Agent");
        response.setContentType("text/html;charset=utf-8");
        response.getWriter().println(userAgent);
        String output = "";
        if(userAgent.indexOf("Windows NT") != -1) {
            output = "<h1>这是PC端首页</h1>";
        }else  if(userAgent.indexOf("iPhone") != -1 || userAgent.indexOf("Android") != -1) {
            output = "<h1>这是移动端首页</h1>";
        }
        response.getWriter().println(output);
    }

}
```

### 1.3 响应的结构

![图片](/images/023_04_02.png)

### 1.4 HTTP常见状态码

![图片](/images/023_04_03.png)

### 1.5 ContentType的作用

![图片](/images/023_04_04.png)

## 2 请求转发与重定向

### 2.1 内容

![图片](/images/023_04_05.png)

### 2.2 Example

```java
package com.jesse.servlet.direct;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CheckLoginServlet
 */
@WebServlet("/direct/check")
public class CheckLoginServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public CheckLoginServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("用户登陆成功");

        //实现了请求转发的功能
        //request.getRequestDispatcher("/direct/index").forward(request, response);
        //响应重定向需要增加contextPath
        response.sendRedirect("/servlet_advanced/direct/index");
    }

}
```

### 2.3 请求转发和响应重定向的区别

![图片](/images/023_04_06.png)

![图片](/images/023_04_07.png)

### 2.4 设置请求自带属性

![图片](/images/023_04_08.png)

#### 2.4.1 Example

```java
package com.jesse.servlet.direct;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CheckLoginServlet
 */
@WebServlet("/direct/check")
public class CheckLoginServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public CheckLoginServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("用户登陆成功");
        request.setAttribute("username", "admin");
        //实现了请求转发的功能
        request.getRequestDispatcher("/direct/index").forward(request, response);
        //响应重定向需要增加contextPath
        //response.sendRedirect("/servlet_advanced/direct/index");
    }

}
```

```java
package com.jesse.servlet.direct;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class IndexServlet
 */
@WebServlet("/direct/index")
public class IndexServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public IndexServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String username = (String)request.getAttribute("username");
        response.getWriter().println("This is index page!current username is " + username);
    }
}
```

## 3 Cookie与Session

### 3.1 Cookie

![图片](/images/023_04_09.png)

#### 3.1.1 Example

```java
package com.jesse.servlet.cookie;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class JesseLoginServlet
 */
@WebServlet("/cookies/login")
public class JesseLoginServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public JesseLoginServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("用户登录成功");
        Cookie cookie = new Cookie("user", "admin");
        cookie.setMaxAge(60*60*24*7);
        response.addCookie(cookie);
        response.getWriter().println("login success");
    }

}
```

```java
package com.jesse.servlet.cookie;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class JesseIndexServlet
 */
@WebServlet("/cookies/index")
public class JesseIndexServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public JesseIndexServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Cookie[] cs = request.getCookies();
        if(cs == null) {
            response.getWriter().println("user not login");
            return;
        }
        String user = null;
        for(Cookie c : cs) {
            System.out.println(c.getName() + " : " +  c.getValue() );
            if(c.getName().equals("user")) {
                user = c.getValue();
                break;
            }
        }
        if(user == null) {
            response.getWriter().println("user not login");
        }else {
            response.getWriter().println("user:" + user);
        }
    }

}
```

### 3.2 Session

![图片](/images/023_04_10.png)

#### 3.2.1 Session原理

![图片](/images/023_04_11.png)

#### 3.2.2 Example

```java
package com.jesse.servlet.session;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 * Servlet implementation class SessionLoginServlet
 */
@WebServlet("/session/login")
public class SessionLoginServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public SessionLoginServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("用户登录成功");
        //获取到用户会话Session对象
        HttpSession session = request.getSession();
        session.setAttribute("name", "张三");
        //request.getRequestDispatcher("/session/index").forward(request, response);
    }

}
```

```java
package com.jesse.servlet.session;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 * Servlet implementation class SessionIndexServlet
 */
@WebServlet("/session/index")
public class SessionIndexServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public SessionIndexServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        HttpSession session =request.getSession();
        String name = (String)session.getAttribute("name");
        response.setContentType("text/html;charset=utf-8");
        response.getWriter().println("这是首页，当前用户为：" + name);
    }

}
```

### 3.3 ServletContext

![图片](/images/023_04_12.png)

#### 3.3.1 Example

```java
package com.jessee.servlet.servletcontext;

import java.io.IOException;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ServletContextInitServlet
 */
@WebServlet("/servletcontext/init")
public class ServletContextInitServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public ServletContextInitServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ServletContext context = request.getServletContext();
        context.setAttribute("copy", "jesse");
        response.getWriter().println("init success");
    }
}
```

```java
package com.jessee.servlet.servletcontext;

import java.io.IOException;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ServletContextDefalutServlet
 */
@WebServlet("/servletcontext/default")
public class ServletContextDefalutServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public ServletContextDefalutServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ServletContext context = request.getServletContext();
        String copy = (String)context.getAttribute("copy");
        response.getWriter().println("<h1>"  + copy +  "</h1>");
    }

}
```

### 3.4 Java Web三大作用域对象

![图片](/images/023_04_13.png)

## 4 中文乱码解决

![图片](/images/023_04_14.png)

## 5 web.xml配置详解

![图片](/images/023_04_15.png)

## 6 JSP内置对象

![图片](/images/023_04_16.png)

## 7 Java Web打包与发布

![图片](/images/023_04_17.png)
