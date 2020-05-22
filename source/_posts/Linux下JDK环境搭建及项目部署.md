---
title: Linux下JDK环境搭建及项目部署
categories:
  - Java数据库开发与实战
  - 03_Redis数据库与Linux下项目部署
tags:
  - imooc
abbrlink: 61824
date: 2020-04-21 19:40:33
---

:star2:本文是Linux下JDK环境搭建及项目部署的学习总结:star2:

<!-- more -->

## 1 操作系统的准备

- 开发环境：Windows10
- 远程环境：Unix （CentOS7）
- ssh和sftp工具：MobaXterm
- 文件：JDK tar.gz
- 文件：Tomcat tar.gz

## 2 软件配置

### 2.1 Java配置

```sh
#################jdk config /etc/profile####################
JAVA_HOME=/opt/jdk1.8.0_251
CLASSPATH=.:$JAVA_HOME/lib
PATH=$PATH:$JAVA_HOME/bin

export JAVA_HOME CLASSPATH PATH
```

```bash
source /etc/profile
```

### 2.2 tomcat(免配置)

## 3 项目发布

- 项目打包：war
- 文件上传：tomcat_home/webapps/
- 启动测试：tomcat_home/bin/startup.sh | shutdown.sh

## 4 MySQL安装

## 5 参考

- CentOS7安装MySQL8：  
<https://blog.csdn.net/qq_38591756/article/details/82958333>

- linux系统启动，执行的配置文件：  
<https://www.cnblogs.com/DengGao/p/linux-profile.html>  

- 如何定位Linux配置文件的加载过程：  
<https://blog.csdn.net/qq_36402372/article/details/82991098>  
