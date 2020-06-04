---
title: SpringMVC拦截器
categories:
  - SSM到Spring Boot入门与综合实战
  - 02_Spring的MVC入门与SSM整合开发
tags:
  - imooc
abbrlink: 47085
date: 2020-05-27 19:50:20
---

:star2:本文是SpringMVC拦截器的开发总结:star2:

<!-- more -->

## 1 Interceptor拦截器入门及使用技巧

### 1.1 Interceptor拦截器入门

![图片](/images/042_03_01.png)

![图片](/images/042_03_02.png)

![图片](/images/042_03_03.png)

![图片](/images/042_03_04.png)

```xml
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
```

```java
package com.jesse.restful.interceptor;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Created by Kong on 2020/5/27.
 */
public class MyInterceptor implements HandlerInterceptor {
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println(request.getRequestURI() + "-准备执行");
        return true;
    }

    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println(request.getRequestURI() + "-目标处理成功");
    }

    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println(request.getRequestURI() + "-响应内容已产生");
    }
}

```

```xml
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.jesse.restful.interceptor.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

### 1.2 Interceptor使用技巧

- 技巧一：拦截器的过滤范围控制

```java
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/restful/**"/>
            <mvc:exclude-mapping path="/**.ico"/>
            <mvc:exclude-mapping path="/**.jpg"/>
            <mvc:exclude-mapping path="/**.gif"/>
            <mvc:exclude-mapping path="/**.js"/>
            <mvc:exclude-mapping path="/**.css"/>
            <mvc:exclude-mapping path="/resources/**"/>
            <bean class="com.jesse.restful.interceptor.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

- 技巧二：多个拦截器执行顺序

![图片](/images/042_03_05.png)

## 2 开发用户流量拦截器

- 引入logback,Spring5日志默认加载顺序：LOG4J, SLF4J_LAL, SLF4J, JUL

```xml
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>
```

- 配置logback.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <appender name = "console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>[%thread] %d %level %logger{10} - %msg%n</pattern>
        </encoder>
    </appender>
    <appender name="accessHistoryLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>d:/logs/history.%d.log</fileNamePattern>
        </rollingPolicy>
        <encoder>
            <pattern>[%thread] %d %level %logger{10} - %msg%n</pattern>
        </encoder>
    </appender>
    <root level="debug">
        <appender-ref ref="console"/>
    </root>
    <logger name="com.jesse.restful.interceptor.AccessHistoryInterceptor" level="INFO" additivity="false">
        <appender-ref ref="accessHistoryLog"/>
    </logger>
</configuration>
```

- 拦截器输出日志

```java
package com.jesse.restful.interceptor;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Created by Kong on 2020/6/3.
 */
public class AccessHistoryInterceptor implements HandlerInterceptor {
    private Logger logger = LoggerFactory.getLogger(AccessHistoryInterceptor.class);
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        StringBuilder log = new StringBuilder();
        log.append(request.getRemoteAddr());
        log.append("|");
        log.append(request.getRequestURL());
        log.append("|");
        log.append(request.getHeader("user-agent"));
        logger.info(log.toString());
        return true;
    }
}

```

## 3 SpringMVC处理流程

![图片](/images/042_03_06.png)
