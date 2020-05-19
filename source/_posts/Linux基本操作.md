---
title: Linux基本操作
categories:
  - Java数据库开发与实战
  - 03_Redis数据库与Linux下项目部署
tags:
  - muke
abbrlink: 46411
date: 2020-04-20 15:22:30
---

:star2:本文是Linux基本操作的学习总结:star2:

<!-- more -->

## 1 Linux目录结构以及操作命令

### 1.1 目录结构

- /bin  - Essential User Binaries
- /boot - Static Boot Files
- /dev  - Device Files
- /etc  - Configuration Files
- /home - Home Folders
- /lib  - Essential Shared Libraries
- /mnt  - Temporary Mount Points
- /opt  - Optional Packages
- /proc - Kernel & Process Files
- /root - Root Home Directory
- /sbin - System Administration Binaries
- /srv  - Service Data
- /usr  - User Binaries & Read-Only Data
- /tmp  - Temporary Files
- /var  - Variable Data Files

![图片](/images/033_01_01.png)

![图片](/images/033_01_02.png)

### 1.2 目录管理命令

- 目录操作：ls、pwd、cd、mkdir、rmdir
- 路径表示：绝对路径/和相对路径. .. ~

### 1.3 文件管理命令

- 文件创建：touch
- 文件编辑：vi
- 文件查看：cat/more/less/head/tail

#### 1.3.1 vi编辑器

![图片](/images/033_01_03.png)

```txt
vi 文件名称

命令模式:

h  j  k  l
左 下 上 右

dd 剪切当前行
yy 复制
p 下一行黏贴  P 上一行黏贴

a在光标后插入   A在当行末插入
i在光标前插入   I在当行首插入
o在当前行之下插入 O在上一行插入

编辑模式

:
最末行模式
:set nu 显示行号
:w 保存
:wq 保存并退出
:q! 不保存退出
```

### 1.4 目录及文件管理命令

- 复制：cp
- 移动：mv
- 删除：rm
- 查找：which/whereis/find

## 2 用户管理以及群组管理

### 2.1 用户管理

- 查看：who
- 创建用户：useradd
- 设置密码：passwd
- 删除用户：userdel

### 2.2 群组管理

- 查看群组：groups
- 创建群组：groupsadd
- 删除群组：groupdel
- 用户群组修改：usermod

### 2.3 查看用户和群组

- 查看用户：/etc/passwd
- 查看群组：/etc/group

## 3 权限与角色

### 3.1 权限

![图片](/images/033_01_04.png)

### 3.2 角色

![图片](/images/033_01_05.png)

### 3.3 权限与角色设置

- 修改所有者：chown
- 修改所有者和组：chown
- 修改所属组：chgrp
- 权限修改：chmod

## 4 压缩与解压缩

![图片](/images/033_01_06.png)

![图片](/images/033_01_07.png)

## 5 软件包的安装与卸载

### 5.1 源码包安装

- 下载源码包：curl、wget
- 解压：tar
- 进入目录：cd
- 编译前配置: ./configure
- 生成Makefile
- 编译：make
- 编译安装：make install
- 删除：make clean
- 删除目录

### 5.2 rpm包安装

- 下载rpm安装包
- 安装：rpm -ivh 软件包 -i安装 -v显示详细信息 -h显示进度
- 查询是否安装：rpm -q 软件包
- 查询包信息：rpm -qi 安装包
- 查询安装位置：rpm -ql 安装包
- 卸载：rpm -e 安装包

### 5.3 yum安装管理rpm包

- 查询：yum list 名称
- 安装：yum [-y] install 软件包 -y自动回答yes
- 更新：yum [-y] update 软件包
- 卸载：yum [-y] remove 软件包
