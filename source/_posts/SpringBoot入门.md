---
title: SpringBoot入门
categories:
  - SSM到Spring Boot入门与综合实战
  - 03_Spring Boot实战
tags:
  - imooc
abbrlink: 3600
date: 2020-06-16 10:26:22
---

:star2:本文是SpringBoot入门的总结:star2:

<!-- more -->

## 1 SpringBoot概述

### 1.1 什么是SpringBoot

![图片](/images/043_01_01.png)

### 1.2 传统SSM应用开发流程

![图片](/images/043_01_02.png)

### 1.3 SpringBoot应用开发流程

![图片](/images/043_01_03.png)

### 1.4 核心特性

![图片](/images/043_01_04.png)

## 2 SpringBoot应用开发

### 2.1 项目准备

![图片](/images/043_01_05.png)

![图片](/images/043_01_06.png)

### 2.2 使用Maven构建SpringBoot项目

- 引入依赖

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.1.RELEASE</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

- 创建目录结构

![图片](/images/043_01_07.png)

- 设置Controller

```java
@Controller
public class MyController {

    @RequestMapping("/out")
    @ResponseBody
    public String out() {
        return "success";
    }
}
```

- 设置Application，启动SpringBoot应用

```java
//说明这是一个SpringBoot应用的入口类
@SpringBootApplication
public class MySpringBootApplication {
    public static void main(String[] args) {
        //启动SpringBoot应用
        SpringApplication.run(MySpringBootApplication.class);
    }
}
```

### 2.3 使用SpringInitializr创建SpringBoot项目

![图片](/images/043_01_08.png)

![图片](/images/043_01_09.png)

- 设置Controller

```java
@Controller
public class MyController {

    @RequestMapping("/out")
    @ResponseBody
    public String out() {
        return "success";
    }
}
```

## 3 SpringBoot配置详情

### 3.1 SpringBoot启动流程与常用配置

![图片](/images/043_01_10.png)

![图片](/images/043_01_11.png)

![图片](/images/043_01_12.png)

### 3.2 SpringBoot配置文件

![图片](/images/043_01_13.png)

![图片](/images/043_01_14.png)

- 对比

```properties
server.port=80
# debug->info->warn->error->fatal
logging.level.root=info
logging.file.name=e:/myspringboot.log
debug=true
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=123
```

```yml
server:
  port: 80

logging:
  level:
    root: info
  file:
    name: e:/myspringboot.log

debug: true

spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test
    username: root
    password: 123456
```

### 3.3 SpringBoot自定义配置项

![图片](/images/043_01_15.png)

```yml
mall:
  config:
    name: 爱美商城
    description: 这是一家化妆品特卖网
    host-sales: 20
    show-advert: true
```

```java
@Controller
public class MyController {

    @Value("${mall.config.name}")
    private String name;
    @Value("${mall.config.description}")
    private String description;
    @Value("${mall.config.host-sales}")
    private Integer hotSales;
    @Value("${mall.config.show-advert}")
    private Boolean showAdvert;

    @RequestMapping("/info")
    @ResponseBody
    public String info() {
        return String.format("name:%s,description:%s,hot-sales=%s,show-advert:%s",
                name,description,hotSales,showAdvert);
    }
}
```

### 3.4 环境配置文件

![图片](/images/043_01_16.png)

```yml
spring:
  profiles:
    active: prd
```

```yml
server:
  port: 8080

logging:
  level:
    root: info
  file:
    name: e:/myspringboot.log

debug: true

spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test
    username: root
    password: 123456

mall:
  config:
    name: 爱美商城
    description: 这是一家化妆品特卖网
    host-sales: 20
    show-advert: true
```

```yml
server:
  port: 80

logging:
  level:
    root: info
  file:
    name: /local/user/app-prd.log

debug: false

spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://155.32.35.88:3306/test
    username: root
    password: adfas23!

mall:
  config:
    name: 优美商城
    description: 这是一家化妆品特卖网
    host-sales: 20
    show-advert: true
```

## 4 打包与运行

- 点击package生成jar包
- 运行时默认优先读取同级目录下的配置文件
