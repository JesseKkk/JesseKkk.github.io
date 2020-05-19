---
title: Bug：Servlet输出乱码问题
categories:
  - Bug
  - Servlet
tags:
  - bug
abbrlink: 38234
date: 2020-04-01 16:30:30
---

:star2:本文是Servlet输出乱码的处理:star2:

<!-- more -->

## 1 工作环境

- Tomacat_8.5.50+Servlet3.1

## 2 问题

![图片](/images/b001_02_01.png)

![图片](/images/b001_02_02.png)

## 3 解决

- 添加response.setContentType("text/html;charset=UTF-8")，

## 4 思考

- 理解response.setCharacter()和response.setContentType()的区别
- request.setCharacterEncoding()来指定输出的数据为utf-8编码
- response.setContentType()通知浏览器，服务器发送过来的数据是什么编码，如何解析
