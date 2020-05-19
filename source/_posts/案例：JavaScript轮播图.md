---
title: 案例：JavaScript轮播图
categories:
  - JavaWeb入门
  - 02_JavaScript与前端案例
tags:
  - muke
abbrlink: 64816
date: 2020-02-10 21:03:10
---

:star2:本文是JavaScript轮播图的学习总结:star2:

<!-- more -->

## 1 业务需求

- 完成JavaScript轮播图的设计
- 开发语言HTML+CSS+JS

## 2 项目构架

![图片](/images/022_05_01.png)

## 3 项目展示

![图片](/images/022_05_02.png)

## 4 项目源码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Slide-Show</title>
    <link rel="stylesheet" href="css/slide.css">
</head>
<body>
    <div class="main" id="main">
        <div class="menu-box"></div>
        <!-- 子菜单 -->
        <div class="sub-menu hide" id="sub-menu">
            <!-- 手机、配件  -->
            <div class="inner-box">
                <div class="sub-inner-box">
                    <div class="title">
                        手机、配件
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">手机通讯:</span>
                        <a href="#">手机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">手机维修</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">以旧换新</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">手机配件:</span>
                        <a href="#">手机壳</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">手机存储卡</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">数据线</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">充电器</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">电池</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">运营商:</span>
                        <a href="#">中国联通</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">中国移动</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">中国电信</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">普通设备:</span>
                        <a href="#">智能手环</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">智能家居</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">智能手表</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">其他配件</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">娱乐:</span>
                        <a href="#">耳机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">音响</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">收音机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">麦克风</a>
                    </div>
                </div>
            </div>
            <!-- 电脑  -->
            <div class="inner-box">
                <div class="sub-inner-box">
                    <div class="title">
                        电脑
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">手机通讯:</span>
                        <a href="#">手机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">手机维修</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">以旧换新</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">手机配件:</span>
                        <a href="#">手机壳</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">手机存储卡</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">数据线</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">充电器</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">电池</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">运营商:</span>
                        <a href="#">中国联通</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">中国移动</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">中国电信</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">普通设备:</span>
                        <a href="#">智能手环</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">智能家居</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">智能手表</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">其他配件</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">娱乐:</span>
                        <a href="#">耳机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">音响</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">收音机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">麦克风</a>
                    </div>
                </div>
            </div>
            <!-- 家用电器  -->
            <div class="inner-box">
                <div class="sub-inner-box">
                    <div class="title">
                        家用电器
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">手机通讯:</span>
                        <a href="#">手机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">手机维修</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">以旧换新</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">手机配件:</span>
                        <a href="#">手机壳</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">手机存储卡</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">数据线</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">充电器</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">电池</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">运营商:</span>
                        <a href="#">中国联通</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">中国移动</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">中国电信</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">普通设备:</span>
                        <a href="#">智能手环</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">智能家居</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">智能手表</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">其他配件</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">娱乐:</span>
                        <a href="#">耳机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">音响</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">收音机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">麦克风</a>
                    </div>
                </div>
            </div>
            <!-- 家具  -->
            <div class="inner-box">
                <div class="sub-inner-box">
                    <div class="title">
                        家具
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">手机通讯:</span>
                        <a href="#">手机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">手机维修</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">以旧换新</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">手机配件:</span>
                        <a href="#">手机壳</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">手机存储卡</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">数据线</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">充电器</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">电池</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">运营商:</span>
                        <a href="#">中国联通</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">中国移动</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">中国电信</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">普通设备:</span>
                        <a href="#">智能手环</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">智能家居</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">智能手表</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">其他配件</a>
                    </div>
                    <div class="sub-row">
                        <span class="bold mr10">娱乐:</span>
                        <a href="#">耳机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">音响</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">收音机</a>
                        <span class="mr10 ml10">/</span>
                        <a href="#">麦克风</a>
                    </div>
                </div>
            </div>
        </div>
        <!-- 主菜单 -->
        <div class="menu-content" id="menu-content">
            <div class="menu-item">
                <a href="">
                    <span>手机、配件</span>
                    <i>&#xe665;</i>
                </a>
            </div>
            <div class="menu-item">
                <a href="">
                    <span>电脑</span>
                    <i>&#xe665;</i>
                </a>
            </div>
            <div class="menu-item">
                <a href="">
                    <span>家用电器</span>
                    <i>&#xe665;</i>
                </a>
            </div>
            <div class="menu-item">
                <a href="">
                    <span>家具</span>
                    <i>&#xe665;</i>
                </a>
            </div>
        </div>
        <!-- 图片轮播 -->
        <div class="banner" id="banner">
            <a href="">
                <div class="banner-slide slide1 slide-active"></div>
            </a>
            <a href="">
                <div class="banner-slide slide2"></div>
            </a>
            <a href="">
                <div class="banner-slide slide3"></div>
            </a>
        </div>

        <!-- 上一张、下一张按钮 -->
        <a href="javascript:void(0)" class="button prev" id="prev"></a>
        <a href="javascript:void(0)" class="button next" id="next"></a>

        <!-- 原点导航 -->
        <div class="dots" id="dots">
            <span class="active"></span>
            <span class=""></span>
            <span class=""></span>
        </div>
        <script type="text/javascript" src="js/slide.js"></script>
    </div>
</body>
</html>
```

```css
*{
    margin: 0;
    padding: 0;
}

@font-face {font-family: 'iconfont';
    src: url('../img/font/iconfont.eot');
    src: url('../img/font/iconfont.eot') format('embedded-opentype'),
    url('../img/font/iconfont.woff') format('woff'),
    url('../img/font/iconfont.ttf') format('truetype'),
    url('../img/font/iconfont.svg#iconfont') format('svg');
}

ul{
    list-style: none;
}

a:link,a:visited{
    text-decoration: none;
    color: #333;
}

body{
    font-family: "微软雅黑";
    color: #14191e;
}

.main{
    width: 1200px;
    height: 460px;
    margin: 30px auto;
    overflow: hidden;
    position: relative;
}

.banner{
    width: 1200px;
    height: 460px;
    overflow: hidden;
    /* position: relative;    */
}

.banner-slide{
    width: 1200px;
    height: 460px;
    /* position: absolute; */
    background-repeat: no-repeat;
    display: none;
}
.slide1{
    background-image: url("../img/bg1.jpg");
}

.slide2{
    background-image: url("../img/bg2.jpg");
}

.slide3{
    background-image: url("../img/bg3.jpg");
}
.slide-active{
    display: block;
}

.button{
    position: absolute;
    width: 40px;
    height: 80px;
    left: 244px;
    top: 50%;
    margin-top: -40px;
    background: url(../img/arrow.png) no-repeat center center;
}

.button:hover{
    background-color: #333;
    opacity: 0.8;
    filter: alpha(opacity=80);
}

.prev{
    transform: rotate(180deg);
}

.next{
    left: auto;
    right: 0;
}

.dots{
    position: absolute;
    right: 20px;
    bottom: 24px;
    text-align: right;
}

.dots span{
    display: inline-block;
    width: 12px;
    height: 12px;
    line-height: 12px;
    border-radius: 50%;
    background: rgba(7,17,27,0.4);
    box-shadow: 0 0 0 2px rgba(255,255,255,0.8) inset;
    margin-left: 8px;
    cursor: pointer;
}

.dots span.active{
    box-shadow: 0 0 0 2px rgba(7,17,27,0.4) inset;
    background: #fff;
}

.menu-box{
    width: 244px;
    height: 460px;
    position: absolute;
    left: 0;
    top: 0;
    background: rgba(7, 17, 27, 0.5);
    opacity: 0.5;
    z-index: 1;
}

.menu-content{
    width: 244px;
    height: 454px;
    position: absolute;
    left: 0;
    top: 0;
    z-index: 2;
    padding-top: 6px;
}

.menu-item{
    height: 64px;
    line-height: 66px;
    font-size: 16px;
    padding: 0 24px;
    position: relative;
}

.menu-item a:link,.menu-item a:visited{
    color: #fff;
}

.menu-item a{
    display: block;
    /* border-bottom: 1px solid rgba(255, 255, 255, 0.2); */
    padding: 0 8px;
    height: 64px;
}

.menu-item i{
    position: absolute;
    right: 32px;
    top: 2px;
    font-size: 24px;
    font-family: "iconfont";
    font-style: normal;
    font-weight: normal;
    color: rgba(255, 255, 255, 0.5)
}

.sub-menu{
    width: 730px;
    height: 458px;
    border: 1px solid #d9dde1;
    background: #fff;
    position: absolute;
    left: 244px;
    top: 0;
    z-index: 999;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.1);
}

.inner-box{
    width: 100%;
    height: 100%;
    background: url(../img/fe.png) no-repeat;
    display: none;
}

.sub-inner-box{
    width: 652px;
    margin-left: 40px;
    overflow: hidden ;
}

.title{
    color: #f01414;
    font-size: 16px;
    line-height: 16px;
    margin: 28px 0 30px 0;
    font-weight: bold;
}

.sub-row{
    margin-bottom: 25px;
}

.bold{
    font-weight: bold;
}

.mr10{
    margin-right: 10px;
}

.ml10{
    margin-left: 10px;
}

.hide{
    display: none;
}
```

```js
var index = 0,
    timer = null,
    dd = 0,
    pics = byId("banner").getElementsByTagName("div"),
    dots = byId("dots").getElementsByTagName("span"),
    prev = byId("prev"),
    next = byId("next"),
    len = pics.length,
    menu = byId("menu-content"),
    subMenu = byId("sub-menu"),
    innerBox = subMenu.getElementsByClassName("inner-box"),
    menuItems = menu.getElementsByClassName("menu-item");

// 封装一个代替getElementById()的方法
function byId(id){
    return typeof(id) === "string"?document.getElementById(id):id;
}

function slideImg(){
    var main = byId("main");
    //滑过清除定时器，离开继续
    main.onmouseover = function(){
        //滑过清除定时器
        if(timer) clearInterval(timer);
    };
    //离开定时器打开
    main.onmouseout = function(){
        timer = setInterval(function(){
            index++;    //len =  3 0 1 2
            if(index >= len){
                index = 0;
            }
            changeImg();
        },3000);
    };
    // 自动在main上触发鼠标离开的时间
    main.onmouseout();

    // 点击圆点来切换图片
    for(var d=0; d<len; d++)
    {
        //给所有span添加一个id的属性，值为d，作为当前span的索引
        dots[d].id = d;
        dots[d].onclick = function(){
            //改变index为当前span的id值
            index = this.id;
            changeImg();
        };
    }

    //下一张
    next.onclick = function(){
        index++;
        if(index >= len) index=0;
        changeImg();
    }

    // 上一张
    prev.onclick = function(){
        index--;
        if(index < 0) index= len-1;
        changeImg();  
    }

    // 导航菜单
    //遍历主菜单，且绑定事件
    for(var m=0;m<menuItems.length;m++)
    {
        // 给每一个menu-item定义data-index属性，作为索引
        menuItems[m].setAttribute("data-index",m);
        menuItems[m].onmouseover = function(){
            subMenu.className = "sub-menu";
            var idx = this.getAttribute("data-index");
            dd = idx;
            // 遍历所有子菜单，将每一个都隐藏
            for(var j=0; j<innerBox.length;j++){
                innerBox[j].style.display = "none";
                menuItems[j].style.background = "none";
            }
            menuItems[dd].style.background = "rgba(0,0,0,0.1)";
            innerBox[idx].style.display = "block";
        };
    }
    menu.onmouseout = function(){
        subMenu.className = "sub-menu hide";
        for(var k=0; k<innerBox.length; k++)
        {
            menuItems[k].style.background = "none";
        }
    }

    subMenu.onmouseover = function(){
        this.className = "sub-menu";
        menuItems[dd].style.background = "rgba(0,0,0,0.1)";
    }

    subMenu.onmouseout = function(){
        this.className = "sub-menu hide";
        for(var k=0; k<innerBox.length; k++)
        {
            menuItems[k].style.background = "none";
        }
    }
}

function changeImg(){
    //遍历banner下所有的div及dots下所有的span，将div隐藏，将span清除类
    for(var i = 0; i<len; i++){
        pics[i].style.display = "none";
        dots[i].className = "";
    }
    // 根据index索引找到当前div的当前span，将其显示出来和设为当前
    pics[index].style.display = "block";
    dots[index].className = "active";
}
slideImg();
```
