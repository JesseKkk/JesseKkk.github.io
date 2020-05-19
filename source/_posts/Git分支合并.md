---
title: Git分支合并
categories:
  - Git
tags:
  - git
abbrlink: 57502
date: 2019-11-27 15:51:44
---

:star2:本文总结了Git分支合并方法:star2:

<!-- more -->

## 1 fast-forward

```bash
git checkout master
git merge test
```

![图片](/images/git001.gif)

### 1.1 合并前

![图片](/images/2019-11-27_170740.png)

### 1.2 合并后

![图片](/images/2019-11-27_170832.png)

## 2 no-ff

```bash
git checkout master
git merge --no-ff test
```

![图片](/images/git002.gif)

### 2.1 合并前

![图片](/images/2019-11-27_171419.png)

### 2.2 合并后

![图片](/images/2019-11-27_171626.png)

## 3 squash

```bash
git checkout master
git merge --squash test
git commit -m "add B1 B2"
```

![图片](/images/git003.gif)

### 3.1 合并前

![图片](/images/2019-11-27_181633.png)

### 3.2 合并后

![图片](/images/2019-11-27_181730.png)

## 4 分叉合并时merge和rebase区别

### 4.1 merge

```bash
git checkout test
git merge master
```

![图片](/images/2019-11-27_183326.png)

![图片](/images/2019-11-27_183423.png)

#### 4.1.1 合并前

![图片](/images/2019-11-27_184239.png)

#### 4.1.2 合并后

![图片](/images/2019-11-27_184416.png)

### 4.2 rebase

```bash
git checkout test
git rebase master
```

![图片](/images/2019-11-27_185410.png)

![图片](/images/2019-11-27_185454.png)

#### 4.2.1 合并前

![图片](/images/2019-11-27_185535.png)

#### 4.2.2 合并后

![图片](/images/2019-11-27_185649.png)

## 5 cherry-pick

- 用于单个提交合并，通常是引用别人开发分支上的提交

## 6 注意

- 绝不要在公共的分支上使用rebase。在你运行git rebase之前，一定要问问你自己“有没有别人正在这个分支上工作？”

## 7 参考

- 图解4种git合并分支方法：  
<https://div.io/topic/2030>

- Git分支合并选择：  
<https://www.cnblogs.com/MuYunyun/p/6876413.html?utm_source=itdadao&utm_medium=referral>  
