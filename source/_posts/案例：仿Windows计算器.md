---
title: 案例：仿Windows计算器
categories:
  - JavaWeb入门
  - 02_JavaScript与前端案例
tags:
  - muke
abbrlink: 25523
date: 2020-02-03 15:39:10
---

:star2:本文是仿Windows计算器的学习总结:star2:

<!-- more -->

## 1 业务需求

- 开发语言HTML+CSS+JS

![图片](/images/022_02_01.png)

## 2 项目流程

- 首先用html搭建出构架
- 其次用css进行页面布局设计
- 最后用js进行初始化，并给按钮添加事件

## 3 项目展示

![图片](/images/022_02_02.png)

## 4 项目源码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Calculator</title>
    <link rel="stylesheet" type="text/css" href="calc.css">
    <script type="text/javascript" src="calc.js"></script>
    <script type="text/javascript" src="js/jesse.js"></script>
</head>
<body onload="init(),init_jesse()">
    <div id="div1">
        <div id="div2">
            <input type="text" name="num" id="num"/>
        </div>
        <div id="div3">
            <input type="button" value="c" name="" id="">
            <input type="button" value="←" name="" id="">
            <input type="button" value="+/-" name="" id="">
            <input type="button" value="/" name="" id="">
            <input type="button" value="1" name="" id="">
            <input type="button" value="2" name="" id="">
            <input type="button" value="3" name="" id="">
            <input type="button" value="*" name="" id="">
            <input type="button" value="4" name="" id="">
            <input type="button" value="5" name="" id="">
            <input type="button" value="6" name="" id="">
            <input type="button" value="-" name="" id="">
            <input type="button" value="7" name="" id="">
            <input type="button" value="8" name="" id="">
            <input type="button" value="9" name="" id="">
            <input type="button" value="0" name="" id="">
            <input type="button" value="." name="" id="">
            <input type="button" value="=" name="" id="">
            <input type="button" value="m" name="" id="jesse">
        </div>
    </div>
</body>
</html>
```

```css
*{
    margin: 0px;
    padding: 0px;
}
div{
    width: 170px;
}
#div1{
    top: 60px;
    left: 100px;
    position: absolute;
}
input[type="button"]{
    width: 30px;
    margin-right: 5px;
}
input[type="text"]{
    width: 149px;
    text-align: right;
    background-color: #ffffff;
    border: 1px solid;
    padding-right: 5px;
    box-sizing: border-box;
}
input[type="button"]:hover{
    background-color: yellow;
    border: 1px solid;
}
```

```js
// 标签的初始化
function init(){
    var num = document.getElementById("num");
    num.value=0;
    num. disabled="disabled";
    var oButton=document.getElementsByTagName("input");
    var btn_num1;
    var fh;

    for(var i = 0; i<oButton.length; i++){
        oButton[i].onclick=function(){
            if(!isNaN(this.value))
            {
                // num.value=(num.value + this.value)*1;
                if(isNull(num.value)){
                    num.value=this.value;
                }else{
                    num.value=num.value + this.value;
                }
            }else{
                // alert("fei");
                var btn_num = this.value;
                switch(btn_num){
                    case "+":
                        btn_num1=Number(num.value);
                        num.value=0;
                        fh="+";
                        break;
                    case "-":
                        btn_num1=Number(num.value);
                        num.value=0;
                        fh="-";
                        break;
                    case "*":
                        btn_num1=Number(num.value);
                        num.value=0;
                        fh="*";
                        break;
                    case "/":
                        btn_num1=Number(num.value);
                        num.value=0;
                        fh="/";
                        break;
                    case ".":
                        num.value=dec_number(num.value);
                        break;
                    case "←":
                        num.value=back(num.value);
                        break;
                        num.value=0;
                        break;
                    case "+/-":
                        num.value=sign(num.value);
                        break;
                    case "=":
                        switch(fh){
                            case "+":
                                num.value=btn_num1 + Number(num.value);  
                                break;
                            case "-":
                                num.value=btn_num1 - Number(num.value);  
                                break;
                            case "*":
                                num.value=btn_num1 * Number(num.value);  
                                break;
                            case "/":
                                if(Number(num.value)==0)
                                {
                                    alert("除数不能是0")
                                    num.value=0;
                                }else{
                                    num.value=btn_num1 / Number(num.value);  
                                }
                                break;
                        }  
                        break;
                }
            }
        }
    }
}
// 正负号
function sign(n){
    // if(n.indexOf("-")==-1){
    //     n="-"+n;
    // }else{
    //     n=n.substr(1,n.length);
    // }
    n=Number(n)*-1;
    return n;
}
// 退位键
function back(n){
    n=n.substr(0,n.length-1);
    if(isNull(n)){
        n=0;
    }
    return n;
}
// 小数点
function dec_number(n)
{
    if(n.indexOf(".")==-1){
        n = n + ".";
    }
    return n;
}
// 验证文本框是否为空或者是0
function isNull(n){
    if(n == "0" || n.length == 0)
    {
        return true;
    }else
    {
        return false;
    }
}
```

```js
function init_jesse(){
    document.getElementById("jesse").onclick=function(){
        window.location.href="https://jessekkk.github.io/";
    }
}
```
