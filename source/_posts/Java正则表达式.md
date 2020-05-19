---
title: Java正则表达式
categories:
  - JavaWeb入门
  - 04_Java-Web进阶
tags:
  - muke
abbrlink: 33549
date: 2020-02-24 15:04:24
---

:star2:本文是Java正则表达式的学习总结:star2:

<!-- more -->

## 1 正则表达式基础语法

### 1.1 正则表达式的介绍

![图片](/images/024_03_01.png)

### 1.2 字符范围匹配

![图片](/images/024_03_02.png)

### 1.3 元字符

![图片](/images/024_03_03.png)

### 1.4 多次重复匹配

![图片](/images/024_03_04.png)

### 1.5 定位匹配

![图片](/images/024_03_05.png)

### 1.6 贪婪模式和非贪婪模式

![图片](/images/024_03_06.png)

![图片](/images/024_03_07.png)

### 1.7 表达式分组

![图片](/images/024_03_08.png)

## 2 正则表达式应用

### 2.1 JavaScript表单验证

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <form action="#" method="post" id="frmInfo">
        <div id="err" style="color:red">
        </div>
        <div>
            姓名：<input id="name" name="name" placeholder="张三"/>
        </div>
        <div>
            身份证：<input id="indo" name="indo" placeholder="410882199406054013"/>
        </div>
        <div>
            <input type="submit"/>
        </div>
    </form>
    <script type="text/javascript">
        document.getElementById("frmInfo").onsubmit = function(){
            //在JS中定义正则表达式对象方式：/正则表达式/
            var regex1 = /^[\u4e00-\u9fa5]{2,8}$/;
            var regex2 = /^[1234568]\d{16}[0-9xX]$/;
            var name =  document.getElementById("name").value;
            var indo = document.getElementById("idno").value;
            //利用正则校验数据，true代表校验成功
            if(regex1.test(name) == false){
                document.getElementById("err").innerHTML = "无效姓名";
                return false;
            }else if(regex2.test(indo) == false){
                document.getElementById("err").innerHTML = "无效身份证号";
                return false;
            }else{
                alert("验证通过，准备提交！")
                return true;
            }
        }
    </script>
</body>
</html>
```

### 2.2 Java中Web页面信息提取

```html
<!DOCTYPEhtml>
<html>
<head>
<meta charset="UTF-8">
<title>国际主要城市</title>
</head>
<body>
    <h1>国际主要城市</h1>
    <ul>
        <li>纽约NewYork</li>
        <li>伦敦London</li>
        <li>东京Tokyo</li>
        <li>巴黎Paris</li>
        <li>香港HongKong</li>
        <li>新加坡Singapore</li>
        <li>悉尼Sydney</li>
        <li>米兰Milano</li>
        <li>上海Shanghai</li>
        <li>北京Beijing</li>
        <li>马德里Madrid</li>
        <li>莫斯科Moscow</li>
        <li>首尔Seoul</li>
        <li>曼谷Bangkok</li>
        <li>多伦多Toronto</li>
        <li>布鲁塞尔Brussels</li>
        <li>芝加哥Chicago</li>
        <li>吉隆坡KualaLumpur</li>
        <li>孟买Mumbai</li>
        <li>华沙Warsaw</li>
        <li>圣保罗SaoPaulo</li>
        <li>苏黎世Zurich</li>
        <li>阿姆斯特丹Amsterdam</li>
        <li>墨西哥城MexicoCity</li>
        <li>雅加达Jakarta</li>
        <li>都柏林Dublin</li>
        <li>曼谷Bangkok</li>
        <li>台北Taipei</li>
        <li>伊斯坦布尔Istanbul</li>
        <li>里斯本Lisbon</li>
        <li>罗马Rome</li>
        <li>法兰克福Frankfurt</li>
        <li>斯德哥尔摩Stockholm</li>
        <li>布拉格Prague</li>
        <li>维也纳Vienna</li>
        <li>布达佩斯Budapest</li>
        <li>雅典Athens</li>
        <li>加拉加斯Caracas</li>
        <li>洛杉矶LosAngeles</li>
        <li>新西兰NewZealand</li>
        <li>圣地亚哥SanDiego</li>
        <li>布宜诺斯艾利斯BuenosAires</li>
        <li>华盛顿Washington</li>
        <li>墨尔本Melbourne</li>
        <li>约翰内斯堡Johannesburg</li>
        <li>亚特兰大Atlanta</li>
        <li>巴塞罗那Barcelona</li>
        <li>旧金山SanFrancisco</li>
        <li>马尼拉Manila</li>
        <li>波哥大Bogota</li>
        <li>特拉维夫TelAviv-Yafo</li>
        <li>新德里NewDelhi</li>
        <li>迪拜Dubai</li>
        <li>布加勒斯特Bucharest</li>
        <li>奥斯陆Oslo</li>
        <li>柏林Berlin</li>
        <li>赫尔辛基Helsinki</li>
        <li>日内瓦Geneva</li>
        <li>利雅得Riyadh</li>
        <li>哥本哈根Copenhagen</li>
        <li>汉堡Hamburg</li>
        <li>开罗Cairo</li>
        <li>卢森堡Luxembourg</li>
        <li>班加罗尔Bangalore</li>
        <li>达拉斯Dallas</li>
        <li>科威特城Kuwaitcity</li>
        <li>波士顿Boston</li>
        <li>慕尼黑Munich</li>
        <li>迈阿密Miami</li>
        <li>利马Lima</li>
        <li>基辅Kiev</li>
        <li>休斯顿Houston</li>
        <li>广州Guangzhou</li>
        <li>贝鲁特Beirut</li>
        <li>卡拉奇Karachi</li>
        <li>索菲亚Sophia</li>
        <li>蒙得维的亚Montevideo</li>
        <li>里约热内卢RioDEJaneiro</li>
        <li>胡志明市HoChiMinhCity</li>
        <li>蒙特利尔Montreal</li>
        <li>内罗毕Nairobi</li>
        <li>巴拿马城Panamacity</li>
        <li>金奈Chennai</li>
        <li>布里斯班Brisbane</li>
        <li>卡萨布兰卡Casablanca</li>
        <li>丹佛Denver</li>
        <li>基多Quito</li>
        <li>斯图加特Stuttgart</li>
        <li>温哥华Vancouver</li>
        <li>麦纳麦MaiNaMai</li>
        <li>危地马拉市Guatemalacity</li>
        <li>开普敦CapeTown</li>
        <li>圣何塞SanJose</li>
        <li>西雅图Seattle</li>
        <li>深圳Shenzhen</li>
        <li>珀斯Perth</li>
        <li>加尔各答Calcutta</li>
        <li>安特卫普Antwerp</li>
        <li>费城Philadelphia</li>
        <li>鹿特丹Rotterdam</li>
        <li>拉各斯Lagos</li>
        <li>波特兰Portland</li>
        <li>底特律Detroit</li>
        <li>曼彻斯特Manchester</li>
        <li>惠灵顿Wellington</li>
        <li>里加Riga</li>
        <li>爱丁堡Edinburgh</li>
        <li>圣彼得堡StPetersburg</li>
        <li>圣迭戈SanDiego</li>
        <li>伊斯兰堡Islamabad</li>
        <li>伯明翰Birmingham</li>
        <li>多哈Doha</li>
        <li>阿拉木图AlmaAtaAlmaty</li>
        <li>卡尔加里Calgary</li>
    </ul>
</body>
</html>
```

```java
package com.jesse.regex;

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FilterReader;
import java.io.InputStreamReader;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexSample {
    public static void main(String[] args) {
        StringBuilder content = new StringBuilder();
        try {
            FileInputStream fis = new FileInputStream("E:\\FileWeb\\regex\\WebContent\\sample.html");
            InputStreamReader isr = new InputStreamReader(fis, "UTF-8");
            //FileReader isr = new FileReader("E:\\FileWeb\\regex\\WebContent\\sample.html");
            BufferedReader bf = new BufferedReader(isr);
            String lineText = "";
            while((lineText = bf.readLine()) != null) {
                //System.out.println(lineText);
                content.append(lineText + "\n");
            }
            bf.close();
            //System.out.println(content);
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        //1. 创建正则表达式对象
        Pattern p = Pattern.compile("<li>([\\u4e00-\\u9fa5]{2,10})([a-zA-Z]+)</li>");
        //2. 匹配正则表达式
        Matcher m = p.matcher(content);
        //3. 查找匹配的结果
        while(m.find()){
            //System.out.println(m.group(0));
            String chs = m.group(1);
            String eng = m.group(2);
            System.out.println(chs + "-" + eng);
        }
    }
}
```
