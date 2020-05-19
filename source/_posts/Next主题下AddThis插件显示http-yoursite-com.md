---
title: 'Next主题下AddThis插件显示http://yoursite.com'
categories:
  - Hexo
  - 02_Next主题优化 
tags:
  - blog-bug
abbrlink: 44179
date: 2019-11-06 21:08:10
---

:star2:本文解决AddThis插件显示<http://yoursite.com>问题:star2:

<!-- more -->

## 1 解决方案

打开站点配置文件`_config.yml`，修改

```yml
  # URL
  ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
  url: http:/***.github.io
```
