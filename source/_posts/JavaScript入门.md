---
title: JavaScript入门
categories:
  - JavaWeb入门
  - 02_JavaScript与前端案例
tags:
  - imooc
abbrlink: 42178
date: 2020-01-16 15:59:20
---

:star2:本文是JavaScript入门的学习总结:star2:

<!-- more -->

## 1 JavaScript

- 脚本编程语言，弱类型语言
- 与Java没有一点关系

## 2 用法

```html
<head>
    <title>Document</title>
    <script type="text/javascript" src="test1.js"></script>
</head>
```

## 3 JavaScript程序设计

### 3.1 自定义函数与数据

- 自定义函数

```js
function fun1()
{
    return ;
}

var n1;//统一var定义变量
```

- 数据类型

![图片](/images/022_01_01.png)

![图片](/images/022_01_02.png)

- 类型转换

![图片](/images/022_01_03.png)

### 3.2 变量与运算符

- 与一般的程序语言变量与运算符类似

### 3.3 程序控制语句

- 与一般的程序控制语句类似

### 3.4 内置函数的学习

![图片](/images/022_01_04.png)

### 3.5 数组

- 与一般的程序语言类似

## 4 JS对表单元素进行设置

### 4.1 利用JS实现日期的三级联动

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script type="text/javascript" src="index.js"></script>
</head>
<body onload="ymd()">
    <form>
        <select name="yyyy" id="yyyy" onchange="selectYmd()"></select>年
        <select name="mm" id="mm" onchange="selectYmd()"></select>月
        <select name="dd" id="dd"></select>日
        <input type="button" value="删除列表框条目" onclick="deleteSelect()">
    </form>
</body>
</html>
```

```js
function ymd()
{
    //获取id=yyyy的控件
    var yyyy = document.getElementById("yyyy");
    var mm = document.getElementById("mm");
    var dd = document.getElementById("dd");
    var date = new Date();
    var year = parseInt(date.getFullYear());

    ininSelect(yyyy,1999,year);
    ininSelect(mm,1,12);
    ininSelect(dd,1,31);
  
    //获取列表框的长度
    var n = yyyy.length;
    //列表框选中某个条目
    yyyy.selectedIndex = Math.round(n/2);
    //将某个列表框的条目数设置为零，效果是删除
    // dd.options.length = 0;

}

//给列表框赋值，传递三个参数：表单元素，开始值，结束值
function ininSelect(obj,start,end)
{
    for(var i = start; i <= end; i++)
    {
        obj.options.add(new Option(i,i));
    }
}

function selectYmd()
{
    var yyyy = document.getElementById("yyyy");
    var mm = document.getElementById("mm");
    var dd = document.getElementById("dd");
    var m = parseInt(mm.value);
    var dayEnd;
    var y = parseInt(yyyy.value);
    if(m == 4 || m==6 || m==9 || m==11)
    {
        dayEnd=30;
    } else if(m ==2)
    {
        dayEnd=28;
        if((y % 4==0 && y % 100 != 0) || y % 400 ==0)
        {
            dayEnd = 29;
        }
    } else
    {
        dayEnd=31;
    }
    dd.options.length = 0;
    ininSelect(dd,1,dayEnd);
}

function deleteSelect()
{
    var dd = document.getElementById("dd");
    // dd.options.remove(1);
    for(var i = dd.length; i >= 0; i--)
    {
        dd.options.remove(0);
    }
}
```

### 4.2 利用JS实现头像更换

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script type="text/javascript" src="test.js"></script>
</head>
<body onload="initLogo()">
    <form>
        <img id="logoImg" src="Image/1.jpg" height=50px width=50px>
        <select id="logo" onchange="selectLogo()"></select>
    </form>
</body>
</html>
```

```js
function initLogo()
{
    var logo = document.getElementById("logo");
    for(i=1;i<=3;i++)
    {
        logo.options.add(new Option(i,i));
    }
}

function selectLogo()
{
    var logo = document.getElementById("logo");
    var n = logo.value;
    var logoImg = document.getElementById("logoImg");
    logoImg.src="Image/" + n + ".jpg";
}
```

### 4.3 利用JS实现复选框设置

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script type="text/javascript" src="test1.js"></script>
</head>
<body>
    <form>
        <input type="checkbox" name="interest" value="1"/><label>游泳</label>
        <input type="checkbox" name="interest" value="2"/><label>爬山</label>
        <input type="checkbox" name="interest" value="3"/><label>看书</label>
        <input type="checkbox" name="interest" value="4"/><label>听歌</label>
        <input type="button" value="全选" id="btn1" onclick="checkInterest()">
        <input type="button" value="反选" id="btn2" onclick="checkInterest1()">
    </form>
</body>
</html>
```

```js
var flag = true;

function checkInterest()
{
    var interest = document.getElementsByName("interest");
    for(i=0;i<interest.length;i++)
    {
        interest[i].checked=flag;
    }
    if(flag)
    {
        document.getElementById("btn1").value = "全不选";
    }
    else
    {
        document.getElementById("btn1").value = "全选";
    }
    flag = !flag; //开关变量
}

function checkInterest1()
{
    var interest = document.getElementsByName("interest");
    for(i=0;i<interest.length;i++)
    {
        interest[i].checked=!interest[i].checked;
        console.log(interest[i].value)
    }
}
```

## 5 事件和DOM

### 5.1 事件

![图片](/images/022_01_05.png)

### 5.2 DOM

- DOM = Document Object Model

![图片](/images/022_01_06.png)

### 5.3 常用的DOM操作

![图片](/images/022_01_07.png)
