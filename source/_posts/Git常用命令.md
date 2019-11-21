---
title: Git常用命令
categories:
  - Git
abbrlink: 7065
date: 2019-11-06 21:23:48
---

## 前言

总结git的常用命令

## 安装

安装地址：<http://git-scm.com>

## Git的配置

    git config --global user.name "Jesse"               #Git配置
    git config --global user.email "kongjinxing0@gmail.com"
    git config --global color-ui true
    git config --global alias.it init 

    <https://github.com/markgandolfo/git-bash-completion> #补全
    echo  source ~/git-completion.bash >> ~/.bashrc

    --global: 修改当前用户的全局配置文件 ~/.gitconfig
    --system: 修改所有用户的全局配置文件 /etc/gitconfig
    无参数：   修改本地仓库的配置文件    .git/config
    
## 常用命令

    git init                #新建仓库
    git config              #Git配置
    git add、git commit     #工作区修改、保存修改、提交修改
    git status              #查看状态
    git log                 #查看提交历史
    git diff                #查看提交差异
    git show                #查看某个提交具体修改
    git clone               #克隆一个远程仓库
    git rm -rf              #删除文件
    git reset               #恢复
    git grep                #查看提交历史
    git revert              #提交修改
    git rebase              #重新排序
    git merge               #分支合并

## 参考
- 廖雪峰的官方网站：  
<https://www.liaoxuefeng.com/wiki/896043488029600/896067008724000>

- Git零基础实战教程：  
<https://edu.51cto.com/course/5939.html>  

- CodeSheep的Git操作与命令:  
<https://mp.weixin.qq.com/s/swnwBiuyVmhs5iPqv3H6BQ>
