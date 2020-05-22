---
title: Maven入门
categories:
  - Java数据库开发与实战
  - 02_MyBatis从入门到进阶
tags:
  - imooc
abbrlink: 63612
date: 2020-04-03 11:10:44
---

:star2:本文是Maven入门的学习总结:star2:

<!-- more -->

## 1 Maven的配置

- 步骤一：Window/Preferences/Maven/Installations/add

![图片](/images/032_02_001.png)

- 步骤一：Window/Preferences/Maven/User Setting

![图片](/images/032_02_002.png)

## 2 Maven的简介和使用

### 2.1 Maven介绍

![图片](/images/032_02_01.png)

### 2.2 Maven核心特性

![图片](/images/032_02_02.png)

### 2.3 Maven的坐标

![图片](/images/032_02_03.png)

### 2.4 Maven的目录结构

![图片](/images/032_02_04.png)

### 2.5 Maven依赖管理

![图片](/images/032_02_05.png)

![图片](/images/032_02_06.png)

### 2.6 Maven本地仓库和中央仓库

![图片](/images/032_02_07.png)

### 2.7 项目打包（jar）

![图片](/images/032_02_08.png)

#### 2.7.1 流程

- 步骤一：安装插件

```xml
     <plugins>
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-assembly-plugin</artifactId>
              <version>2.5.5</version>
              <configuration>
                  <archive>
                      <manifest>
                          <mainClass>com.imooc.maven.PinyinTestor</mainClass>
                      </manifest>
                  </archive>
                  <descriptorRefs>
                      <descriptorRef>jar-with-dependencies</descriptorRef>
                  </descriptorRefs>
              </configuration>
          </plugin>
      </plugins>
```

- 步骤二： Run Configurations.../Maven Build/New Configuration，配置运行

![图片](/images/032_02_09.png)

![图片](/images/032_02_10.png)

## 3 Maven构建web项目

### 3.1 创建

- 步骤一：创建Server

- 步骤二：File/New MavenProjct/Create a simple project

![图片](/images/032_02_11.png)

- 步骤三：JRE System Library目录/Properties

![图片](/images/032_02_12.png)

- 步骤四：工程目录/Properties/Java Compiler

![图片](/images/032_02_13.png)

- 步骤五：main目录/New Folder/webapp
- 步骤六：工程目录/Properties/Project Facets/Convert fo facted form...

![图片](/images/032_02_14.png)

- 步骤七：Futher configuration available

![图片](/images/032_02_15.png)

- 步骤八：工程目录/Properties/Deployment Assembly/Add/Java Build Path Entries/Next/Maven Dependencies

![图片](/images/032_02_16.png)

### 3.2 打包（war）

- 步骤一：安装插件

```xml
    <packaging>war</packaging>
    <build>
        <finalName>maven-web</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.2.2</version>
            </plugin>
        </plugins>
    </build>
```

- 步骤二：Run Configurations.../Maven Build/New Configuration

![图片](/images/032_02_17.png)

## 4 Maven常用命令

![图片](/images/032_02_18.png)
