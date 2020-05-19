---
title: git-rebase命令实战
categories:
  - Git
tags:
  - git
abbrlink: 51499
date: 2019-11-28 14:40:00
---

:star2:本文记录git rebase的使用方法:star2:

<!-- more -->

## 1 Git操作

```bash
git checkout master
git pull
git checkout test
git rebase -i HEAD~2  //合并提交 --- 2表示合并两个
git rebase master---->git status--->解决冲突--->git add .--->git rebase --continue
git checkout master
git merge test
git push
```

## 2 模拟操作

![图片](/images/2019-11-28_150759.png)
![图片](/images/2019-11-28_150939.png)
![图片](/images/2019-11-28_151031.png)

## 3 参考

- git在工作中正确的使用方式----git rebase篇：
<https://blog.csdn.net/nrsc272420199/article/details/85555911>

- git(十六) git rebase 实战：  
<https://blog.csdn.net/wzq6578702/article/details/76736008>
