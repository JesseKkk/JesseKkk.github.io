---
title: Git常用命令
categories:
  - Git
tags:
  - git
abbrlink: 7065
date: 2019-11-06 21:23:48
---

:star2:本文总结了Git常用命令:star2:

<!-- more -->

## 1 安装

安装地址：<http://git-scm.com>

## 2 配置及创建

### 2.1 配置

```bash
git config --global user.name "Jesse" #Git配置
git config --global user.email "kongjinxing0@gmail.com"
git config --global color-ui true
git config --global alias.it init
<https://github.com/markgandolfo/git-bash-completion> #补全
echo  source ~/git-completion.bash >> ~/.bashrc
--global: 修改当前用户的全局配置文件    ~/.gitconfig
--system: 修改所有用户的全局配置文件    /etc/gitconfig
无参数：   修改本地仓库的配置文件       .git/config
```

### 2.2 创建

```bash
git init                #创建仓库
git add file            #提交文件到暂存区
git commit -m <message> #提交所有文件到版本库
```

## 3 时光穿梭

```bash
git status              #查看状态
git show SHA            #查看某个提交信息
git diff file           #查看提交差异
```

### 3.1 版本回退

```bash
git log --pretty=oneline #单行显示
git log -p              #显示每次提交具体的修改
gitk                    #图形化显示提交历史
git reset SHA           #回退版本到工作目录
git reset --soft SHA    #回退版本到暂存区
git reset --hard SHA    #回退版本，修改丢弃
git reflog              #显示每次操作命令
```

### 3.2 撤销修改

```bash
git checkout file         #撤销工作区修改
git restore --staged file #撤销暂存区到工作区
git commit --amend        #修改最后一次提交
```

### 3.3 文件删除

```bash
git rm file             #从工作目录和暂存区中删除文件
```

## 4 远程仓库

```bash
git clone url          #从远程仓库克隆
git remote add  主机名 仓库地址 #添加远程仓库地址
git remote -v           #查看远程仓库信息
git remote rm 主机名    #删除远程仓库
git remote rename <old> <new> #远程仓库重命名
git fetch<远程主机> <远程分支>:<本地分支> #拉取远程更新到本地仓库
git pull <远程主机> <远程分支>:<本地分支> #拉取远程跟新并合并到本地仓库
git push <远程主机> <本地分支>:<远程分支> #推送本地修改到远程仓库
git remote show origin #查看远程分支和本地分支的对应关系
git branch b1 origin/b1 #跟踪远程分支
git push orgin :b1      #删除远程分支
```

## 5 分支管理

### 5.1 创建合并分支

```bash
git branch              #查看分支
git branch <name>       #创建分支
git switch              #切换分支
git switch -c <name>    #创建+切换分支
git merge  dev          #分支合并Fast-forward
git merge --no-ff -m <message> dev #合并分支no-ff
git merge --squash dev  #压合合并squash
git cherry-pick SHA     #合并单个提交
git branch -d           #删除分支
git branch -D           #强制删除没有合并的分支
```

### 5.2 分支冲突解决

```bash
git mergetool           #分支冲突解决工具
git log --graph         #查看分支合并图
```

### 5.3 分支管理策略

![图片](/images/2019-11-26_220410.png)

### 5.4 Bug分支

```bash
git stash               #分支修改隐藏
git stach pop           #恢复分支
```

### 5.5 Rebase

```bash
git swtich dev
git rebase master       #分支衍合
git fetch
git rebase origin/master #拉取远程分支衍合
```

## 6 标签管理

```bash
git tag v1.0            #添加本地标签
git tag                 #查看本地标签
git tag -d v1.0         #删除本地标签
git push origin v1.0    #推送某个标签
git push origin --tags  #推送所有标签
git push origin  :refs/tags/v1.0 #删除远程标签
```

## 7 参考

- 廖雪峰的官方网站：  
<https://www.liaoxuefeng.com/wiki/896043488029600/896067008724000>

- Git零基础实战教程：  
<https://edu.51cto.com/course/5939.html>  

- CodeSheep的Git操作与命令:  
<https://mp.weixin.qq.com/s/swnwBiuyVmhs5iPqv3H6BQ>
