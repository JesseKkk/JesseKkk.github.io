---
title: Bug：IDEA中Tomcat日志信息乱码
categories:
  - Bug
  - IDEA
tags:
  - bug
abbrlink: 15702
date: 2020-04-01 09:30:20
---

:star2:本文是IDEA中Tomcat日志信息乱码的处理:star2:

<!-- more -->

## 1 工作环境

- windows10(GBK)+IDEA_2019.3.4+Tomacat_8.5.50

## 2 问题

![图片](/images/b001_01_01.png)

## 3 解决

- 步骤一：打开tomacat目录中的conf/logging.properties,配置编码全部为UTF-8

![图片](/images/b001_01_02.png)

- 步骤二：打开IDEA的窗口中的File/Settings.../Editor/File Encodings,配置编码全部为UTF-8

![图片](/images/b001_01_03.png)

- 步骤三：打开IDEA中窗口中的Help/Edit Custom VM Options...,添加-Dfile.encoding=UTF-8

![图片](/images/b001_01_04.png)

- 步骤四：打开IDEA中窗口中Tomcat的Edit configurations,在VM options文本框中添加-Dfile.encoding=UTF-8

![图片](/images/b001_01_05.png)

- 步骤五：重启IDEA
