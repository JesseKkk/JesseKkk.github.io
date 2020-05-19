---
title: Hexo+Next+GitHub-Page搭建个人博客
categories:
  - Hexo
  - 01_博客搭建
abbrlink: 39337
date: 2019-11-06 17:03:39
---

:star2:本文记录如何从零搭建一个个人博客:star2:

<!-- more -->

## 1 搭建环境

- window 10 x64
- git v2.23.0
- node v12.13.0
- npm v6.12.0
- hexo v3.1.0
- next v7.5.0

## 2 搭建步骤

- 1.安装Git
- 2.创建GitHub仓库
- 3.Git与GitHub账号绑定
- 4.安装Node.js
- 5.安装Hexo
- 6.将Hexo部署到GitHub
- 7.发布文章

### 2.1 安装Git

下载并安装 <https://git-scm.com/download/win>

### 2.2 创建GitHub仓库

注册GitHub账号 <https://github.com>
创建仓库，仓库名必须命名为：***.github.io

### 2.3 Git与GitHub账号绑定

命令均在Git Bash下操作

```bash
git config --global user.name "user.name"
git config --global user.email "user.email"
ssh-keygen -t rsa -C "user.email"
cat ~/.ssh/id_rsa.pub  #复制内容到GitHub下的SSH keys
ssh –T git@GitHub.com   #验证绑定是否成功
```

### 2.4 安装Node.js

下载并安装 <https://nodejs.org/dist/v12.13.0/node-v12.13.0-x64.msi>

### 2.5 安装Hexo

```bash
npm config set registry https://registry.npm.taobao.org
npm install -g hexo-cli
hexo init MyBlog
cd MyBlog
npm install
hexo g
hexo s  #浏览器打开http://localhost:4000/，就可以看到博客
```

### 2.6 将Hexo部署到GitHub

打开站点配置文件`_config.yml`，修改

```yml
deploy:
    type: git
    repo: git@github.com:JesseKkk/***.github.io.git
    branch: master
```

安装hexo-deployer-git

```bash
npm install --save hexo-deployer-git
hexo clean
hexo g
hexo d  #浏览器打开http://***.github.io，就可以看到博客
```

### 2.7 发布文章

```bash
hexo n "文章名" #新建文章，路径在MyBlog/source/_posts/***.md
hexo clean
hexo g
hexo d
```

## 3 更换Next主题

```bash
cd MyBlog
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

打开站点配置文件`_config.yml`，修改

```yml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

打开主题配置文件 `\themes\next\_config.yml`，修改

```yml
# Schemes
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini
```

打开Hexo 站点目录下的 `_config.yml` 修改内容如下

```yml
title:              #网站标题
subtitle:           #网站副标题
description:        #网站描述
author: author      #您的名字
language:           #网站使用的语言
url:                #解析地址
```
