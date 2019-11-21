---
title: Next主题下live2d和busuanzi插件不兼容
categories:
  - Hexo
tags:
  - blog-bug
abbrlink: 58679
date: 2019-11-06 20:51:04
---

打开\themes\next\layout\_third-party\statistics\busuanzi-counter.swig,将

    <span class="post-meta-item" id="busuanzi_container_site_uv" style="display: none;">
    <span class="post-meta-item" id="busuanzi_container_site_pv" style="display: none;">

修改  

    <span class="post-meta-item" id="busuanzi_container_site_uv" style="display: none;"></span> 
    <span class="post-meta-item" id="busuanzi_container_site_pv" style="display: none;"></span>    



