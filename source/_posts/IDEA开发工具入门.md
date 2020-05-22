---
title: IDEA开发工具入门
categories:
  - Java数据库开发与实战
  - 02_MyBatis从入门到进阶
tags:
  - imooc
abbrlink: 22280
date: 2020-04-01 21:10:20
---

:star2:本文是IDEA开发工具使用的总结:star2:

<!-- more -->

## 1 窗口快捷键

- Alt+Insert：生成代码（如get,set方法，构造方法）
- Ctrl+F/R：当前文件查找/替换
- Ctrl+Shift+F/R：全局查找/替换
- Ctrl+Shift+N：文件快速查找
- Ctrl_+Shift+A： Find Action

## 2 代码快捷键

- Ctrl+←→: 上一个/下一个单词
- Shift+↑↓: 上下框选
- Ctrl+(Shift)+/: 行注释
- Ctrl+Shift+Enter: 自动完成
- Ctrl+Enter: 生成变量名
- Shift+F6: 重命名

## 3 代码快速定位

- Ctrl+(Shift)+E: 最近访问（编辑）的文件列表
- Ctrl+Shift+1-9: 创建书签
- Ctrl+1-9: 快速切换书签
- Alt+←→: 切换页签
- Ctrl+G: 行跳转

## 4 Live Template

- psvm: 生成main方法
- sout: 生成控制台输出
- fori: for循环
- itli: List迭代

### 4.1 自定Live Template

![图片](/images/032_01_01.png)

## 5 运行与打包

- Shift+F9: 调试
- Shift+F10: 运行
- F9: 恢复运行至下一个断点处
- F8:  单步运行（不进入函数）
- F7: 单步运行（进入函数）

### 5.1 打包

- 第一步：File/Project Structure...
- 第二步：Artifacts/new JAR
- 第三步：设置Manifest

![图片](/images/032_01_02.png)

- 第四步：设置导入compile output

![图片](/images/032_01_03.png)

- 第五步：Buid/Buid Artifacts...

## 6 IDEA进行web项目开发与打包

- 第一步：配置创建Project

![图片](/images/032_01_04.png)

- 第二步：配置Tomcat（Context,frame deactivation）

### 6.1 打包

- 第一步：File/Project Structure...
- 第二步：Artifacts/new Web Application Archive
- 第三步：导入compile output
- 第四步：导入Web facet resources
- 第五步：Buid/Buid Artifacts...
