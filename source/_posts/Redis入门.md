---
title: Redis入门
categories:
  - Java数据库开发与实战
  - 03_Redis数据库与Linux下项目部署
tags:
  - muke
abbrlink: 3392
date: 2020-04-21 21:28:33
---

:star2:本文是Redis的学习总结:star2:

<!-- more -->

## 1 Redis简介

- REmote DIctionary Server

![图片](/images/033_02_01.png)

![图片](/images/033_02_02.png)

## 2 Redis安装与常用配置

### 2.1 安装

```sh
wget http://download.redis.io/releases/redis-5.0.8.tar.gz
tar xzf redis-5.0.8.tar.gz
cd redis-5.0.8
make
```

### 2.2 基本配置

![图片](/images/033_02_03.png)

## 3 数据类型

### 3.1 String

![图片](/images/033_02_04.png)

![图片](/images/033_02_05.png)

### 3.2 Hash

![图片](/images/033_02_06.png)

![图片](/images/033_02_07.png)

### 3.3 List

![图片](/images/033_02_08.png)

```txt
rpush listkey c b a  -右侧插入
lpush listkey f e d  -左侧插入
rpop listkey         -右侧弹出
lpop listkey         -左侧弹出
lrange listkey 0 -1  -查询所有
```

### 3.4 Set与Zset

![图片](/images/033_02_09.png)

```txt
sadd set1 c       -插入
smembers set1     -查看
sinter set1 set2  -交集查询
sunion set1 set2  -并集查询
sdiff set1 set2   -差集查询

zadd zset1 99 c               -插入
zrange zset1 0 -1             -查询所有
zrange zset1 0 -1 withscores  -查询包含分数
zrangebyscore zset1 100 101   -范围查询
```

## 4 Java中使用Redis

### 4.1 Jedis介绍

![图片](/images/033_02_10.png)

### 4.2 Jedis操作String、Hash和List

```xml
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.9.0</version>
        </dependency>
```

```java
package com.jesse;

import redis.clients.jedis.Jedis;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class JedisTestor {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("192.168.2.4",6379);
        try{
            jedis.auth("123");
            jedis.select(4);
            System.out.println("Redis连接成功");
            //String
            jedis.set("sn","7781-9938");
            String sn = jedis.get("sn");
            System.out.println(sn);
            jedis.mset(new String[]{"title","婴幼儿奶粉","num","2"});
            List<String> goods = jedis.mget(new String[]{"sn", "title", "num"});
            System.out.println(goods);
            Long num = jedis.incr("num");
            System.out.println(num);
            //Hash
            jedis.hset("student:3312","name","张晓明");
            String name = jedis.hget("student:3312", "name");
            System.out.println(name);
            Map<String,String> studentMap = new HashMap();
            studentMap.put("name","李兰");
            studentMap.put("age","18");
            studentMap.put("id","3313");
            jedis.hmset("student:3312",studentMap);
            Map<String, String> smap = jedis.hgetAll("student:3312");
            System.out.println(smap);
            //List
            jedis.del("letter");
            jedis.rpush("letter", new String[]{"d","e","f"});
            jedis.lpush("letter", new String[]{"c","b","a"});
            List<String> letter = jedis.lrange("letter", 0, -1);
            jedis.lpop("letter");
            jedis.rpop("letter");
            letter = jedis.lrange("letter", 0, -1);
            System.out.println(letter);

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            jedis.close();
        }
    }
}
```

### 4.3 Jedis缓存String

```java
package com.jesse;

public class Goods {
    private Integer goodsId;
    private String goodsName;
    private String description;
    private Float price;

    public Goods() {
    }

    public Goods(Integer goodsId, String goodsName, String description, Float price) {
        this.goodsId = goodsId;
        this.goodsName = goodsName;
        this.description = description;
        this.price = price;
    }

    public Integer getGoodsId() {
        return goodsId;
    }

    public void setGoodsId(Integer goodsId) {
        this.goodsId = goodsId;
    }

    public String getGoodsName() {
        return goodsName;
    }

    public void setGoodsName(String goodsName) {
        this.goodsName = goodsName;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public Float getPrice() {
        return price;
    }

    public void setPrice(Float price) {
        this.price = price;
    }
}
```

```xml
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.61</version>
        </dependency>
```

```java
package com.jesse;

import com.alibaba.fastjson.JSON;
import redis.clients.jedis.Jedis;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class CacheSample {
    public CacheSample(){
        Jedis jedis = new Jedis("192.168.1.1");
        try{
            List<Goods> goodsList = new ArrayList<Goods>();
            goodsList.add(new Goods(8818,"红富士苹果","",3.5f));
            goodsList.add(new Goods(8819,"进口脐橙","",5f));
            goodsList.add(new Goods(8820,"进口香蕉","",25f));
            jedis.auth("123");
            jedis.select(3);
            for (Goods goods : goodsList) {
                String json = JSON.toJSONString(goods);
                System.out.println(json);
                String key = "goods:" + goods.getGoodsId();
                jedis.set(key, json);
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            jedis.close();
        }
    }

    public static void main(String[] args) {
        new CacheSample();
        System.out.printf("请输入要查询的商品编号：");
        String goodsId = new Scanner(System.in).next();
        Jedis jedis = new Jedis("192.168.1.1");
        try{
            jedis.auth("123");
            jedis.select(3);
            String key = "goods:" + goodsId;
            if (jedis.exists(key)){
                String json = jedis.get(key);
                System.out.println(json);
                Goods g = JSON.parseObject(json, Goods.class);
                System.out.println(g.getGoodsName());
                System.out.println(g.getPrice());
            }else{
                System.out.println("您输入的商品编号不存在，请重新输入！");
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            jedis.close();
        }
    }
}
```

## 5 参考

- CentOS7使用firewalld打开关闭防火墙与端口：  
<https://www.cnblogs.com/moxiaoan/p/5683743.html>

- 网络7层协议：  
<https://blog.csdn.net/cc1949/article/details/79063439?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1>  

- 网络数据传输socket和http优缺点：  
<https://www.cnblogs.com/timeismoney/p/7357434.html>

- Unix目录结构的来历：  
<https://www.ruanyifeng.com/blog/2012/02/a_history_of_unix_directory_structure.html>

- Linux（centos）安装vim：  
<https://www.cnblogs.com/Jason-Xiang/p/11750846.html>
