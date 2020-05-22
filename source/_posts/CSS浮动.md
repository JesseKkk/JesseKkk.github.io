---
title: CSS浮动
categories:
  - JavaWeb入门
  - 01_HTML与CSS
tags:
  - imooc
abbrlink: 719
date: 2020-01-06 20:31:40
---

:star2:本文是CSS浮动的学习总结:star2:

<!-- more -->

## 1 内容

![图片](/images/021_03_01.png)

## 2 div

### 2.1 div大小、背景设置

![图片](/images/021_03_02.png)

### 2.2 div溢出处理

- overflow属性

### 2.3 边框、轮廓

- border属性
- outline属性

## 3 盒子模型

- box model

![图片](/images/021_03_03.png)

- box-sizing属性

![图片](/images/021_03_04.png)

## 4 行内元素和块级元素

- 行内元素不会独占一行,相邻的行内元素会排列在同一行里,直到一行排不下,才会换行,其宽度随元素的内容而变化
- 块级元素会独占一行,默认情况下,其宽度自动填满其父元素宽度
- 块级元素常见的有：div......行内元素常见的有：a，span

区别：

- 块级元素可以设置width,height属性
- 行内元素设置width,height属性无效，它的长度高度主要根据内容决定
- 块级元素即使设置了宽度,仍然是独占一行
- 块级元素可以设置margin和padding属性
- 行内元素的margin和padding属性,水平方向padding-left,padding-right,margin-left margin-right都产生边距效果, 但竖直方向的padding-top,padding-bottom,margin-topmargin-bottom却不 会产生边距效果
- 块级元素对应于display:block
- 行内元素对应于display:inline

## 5 浮动

- 浮动类似CAD中图层

### 5.1 定位机制

![图片](/images/021_03_05.png)

### 5.2 float属性

![图片](/images/021_03_06.png)

### 5.3 float包裹和崩溃

- 崩溃 = 子元素设置为浮动，父元素没有设置高度，父一级的块级元素高度发生了破坏。
- 包裹 = 子元素若没有设置宽度，默认为父元素的宽度，此时若子元素设置为float，则宽度自动改为内容的宽度

### 5.4 float其他特性

- 注意div与div元素和div和p元素浮动的区别

### 5.5 清除浮动

```css
/* 方法一 */
#div{
    clear: both;
}

/* 方法二 */
#clearDiv::after{
    content: "";
    visibility: hidden;
    height: 0px;
    display: block;
    clear: both;
}
```
