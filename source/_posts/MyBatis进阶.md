---
title: MyBatis进阶
categories:
  - Java数据库开发与实战
  - 02_MyBatis从入门到进阶
tags:
  - muke
abbrlink: 15993
date: 2020-04-08 10:55:22
---

:star2:本文是MyBatis进阶的学习总结:star2:

<!-- more -->

## 1 MyBatis日志管理与动态SQL

### 1.1 MyBatis日志管理

#### 1.1.1 日志

![图片](/images/032_04_01.png)

#### 1.1.2 SLF4J与Logback

![图片](/images/032_04_02.png)

#### 1.1.3 具体实现

```xml
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    <!--
        日志输出级别（优先级高到低）
        error: 错误 - 系统的故障日志
        warn:  警告 - 存在风险或使用不当的日志
        info: 一般性消息
        debug: 程勋内部用户调试信息
        trace: 程序运行的跟踪信息
    -->
    <root level="debug">
        <appender-ref ref="console"/>
    </root>
</configuration>
```

### 1.2 MyBatis动态SQL

![图片](/images/032_04_03.png)

```xml
    <select id="dynamicSQL" parameterType="java.util.Map" resultType="com.jesse.mybatis.entity.Goods">
        SELECT * FROM t_goods
        <where>
            <if test="categoryId != null">
                and category_id = #{categoryId}
            </if>
            <if test="currentPrice != null">
                and current_price &lt; #{currentPrice}
            </if>
        </where>
    </select>
```

```java
    @Test
    public void testDynamicSQL() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            Map param = new HashMap();
            param.put("categoryId", 44);
            param.put("currentPrice", 500);
            //查询条件
            List<Goods> list = session.selectList("goods.dynamicSQL", param);
            for (Goods g:list){
                System.out.println(g.getTitle() + ":" + g.getCategoryId() + ":" +
                        g.getCurrentPrice());
            }
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

## 2 MyBatis二级缓存

### 2.1 缓存

![图片](/images/032_04_04.png)

### 2.2 缓存范围

![图片](/images/032_04_05.png)

### 2.3 二级缓存运行规则

![图片](/images/032_04_06.png)

### 2.4 一级缓存测试

```java
    @Test
    public void testLv1Cache() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            Goods goods = session.selectOne("goods.selectById", 1603);
            Goods goods1 = session.selectOne("goods.selectById", 1603);
            System.out.println(goods.hashCode() + ":" + goods1.hashCode());
            System.out.println(goods.getTitle());
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }

        try{
            session = MyBatisUtils.openSession();
            Goods goods = session.selectOne("goods.selectById", 1603);
            session.commit();//commit提交时对该namespace缓存强制清空
            Goods goods1 = session.selectOne("goods.selectById", 1603);
            System.out.println(goods.hashCode() + ":" + goods1.hashCode());
            System.out.println(goods.getTitle());
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

### 2.5 二级缓存测试

```xml
    <!--开启二级缓存
        eviction 是缓存的清除策略，当缓存对象数量达到上限后，自动触发对应算法对缓存对象清除
            1. LRU - 最近最少使用的：移除最长时间不被使用的对象
            2. FIFO - 先进先出：按对象进入缓存的顺序来移除他们
            3. SOFT - 软引用：移除基于垃圾回收器状态和软引用规则的对象
            4. WEAK - 弱引用：更积极地移除基于垃圾回收器状态和弱引用规则的对象

        flushInterval 代表间隔多长时间自动清空缓存，单位毫秒，600000毫秒 = 10分钟
        size 缓存存储上限，用于保存对象或集合（1个集合是一个对象）的数量上限
        readOnly 设置为true，代表返回只读缓存，每次从缓存取出的是缓存对象本省，这种执行效率较高
                 设置为false, 代表每次取出的是缓存对象的"副本"，每次取出的对象是不同的，这种安全性较高

    -->
    <cache eviction="LRU" flushInterval="600000" size="512" readOnly="true"/>
```

```java
    @Test
    public void testLv2Cache() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            Goods goods = session.selectOne("goods.selectById", 1603);
            System.out.println(goods.hashCode());
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }

        try{
            session = MyBatisUtils.openSession();
            Goods goods = session.selectOne("goods.selectById", 1603);
            System.out.println(goods.hashCode());
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

## 3 MyBatis多表级联查询

- 多表关联查询：两个表通过主外键，在一条SQL语句，完成所有数据的提取
- 多表级联查询：通过一个对象，获得与它关联的另一个对象，执行多条SQL语句

### 3.1 常见实体关系

![图片](/images/032_04_07.png)

### 3.2 OneToMany对象级联查询

```java
package com.jesse.mybatis.entity;

import java.util.List;

public class Goods {
    private Integer goodsId;
    private String title;
    private String subTitle;
    private Float originalCost;
    private Float currentPrice;
    private Float discount;
    private Integer isFreeDelivery;
    private Integer categoryId;
    private List<GoodsDetail> goodsDetails;

    public Integer getGoodsId() {
        return goodsId;
    }

    public void setGoodsId(Integer goodsId) {
        this.goodsId = goodsId;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getSubTitle() {
        return subTitle;
    }

    public void setSubTitle(String subTitle) {
        this.subTitle = subTitle;
    }

    public Float getOriginalCost() {
        return originalCost;
    }

    public void setOriginalCost(Float originalCost) {
        this.originalCost = originalCost;
    }

    public Float getCurrentPrice() {
        return currentPrice;
    }

    public void setCurrentPrice(Float currentPrice) {
        this.currentPrice = currentPrice;
    }

    public Float getDiscount() {
        return discount;
    }

    public void setDiscount(Float discount) {
        this.discount = discount;
    }

    public Integer getIsFreeDelivery() {
        return isFreeDelivery;
    }

    public void setIsFreeDelivery(Integer isFreeDelivery) {
        this.isFreeDelivery = isFreeDelivery;
    }

    public Integer getCategoryId() {
        return categoryId;
    }

    public void setCategoryId(Integer categoryId) {
        this.categoryId = categoryId;
    }

    public List<GoodsDetail> getGoodsDetails() {
        return goodsDetails;
    }

    public void setGoodsDetails(List<GoodsDetail> goodsDetails) {
        this.goodsDetails = goodsDetails;
    }
}
```

```xml
    <mappers>
        <mapper resource="mappers/goods.xml"/>
        <mapper resource="mappers/goods_detail.xml"/>
    </mappers>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="goodsDetail">
    <select id="selectByGoodsId" parameterType="Integer"
            resultType="com.jesse.mybatis.entity.GoodsDetail">
        SELECT * FROM t_goods_detail WHERE goods_id = #{value}
    </select>
</mapper>
```

```xml
    <!--
        resultMap可用于说明一对多或者多对一的映射逻辑
        id 是resultMap属性引用的标志
        type 指向One的实体（Goods）
        -->
    <resultMap id="rmGoods1" type="com.jesse.mybatis.entity.Goods">
        <!--映射goods对象的主键到goods_id字段-->
        <id column="goods_id" property="goodsId"></id>
        <!--
            collection的含义是，在
            SELECT * FROM t_goods LIMIT 0,1 得到结果后，对所有Goods对象遍历得到goods_id字段
            并代入到goodsDetail命名空间的selectByGoodsId的SQL中执行查询，
            将得到的"商品详情"集合赋值给goodsDetails List对象。
            -->
        <collection column="goods_id" property="goodsDetails"
                    select="goodsDetail.selectByGoodsId"/>
    </resultMap>
    <select id="selectOneToMany" resultMap="rmGoods1">
        SELECT * FROM t_goods LIMIT 0,1
    </select>
```

```java
    @Test
    public void testOneToMany() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            List<Goods> list = session.selectList("goods.selectOneToMany");
            for (Goods goods : list) {
                System.out.println(goods.getTitle() + ":" + goods.getGoodsDetails().size());
            }
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

### 3.3 ManyToOne对象级联查询

```xml
    <resultMap id="rmGoodsDetail" type="com.jesse.mybatis.entity.GoodsDetail">
    <id column="gd_id" property="gdId"/>
    <result column="goods_id" property="goodsId"/>
    <association column="goods_id" property="goods" select="goods.selectById"></association>
    </resultMap>
    <select id="selectManyToOne" resultMap="rmGoodsDetail">
        SELECT * FROM t_goods_detail LIMIT 0,20
    </select>
```

```xml
    <select id="selectById" parameterType="Integer" resultType="com.jesse.mybatis.entity.Goods">
        SELECT * FROM t_goods WHERE goods_id = #{value}
    </select>
```

```java
    @Test
    public void testManyToOne() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            List<GoodsDetail> list = session.selectList("goodsDetail.selectManyToOne");
            for (GoodsDetail gd : list) {
                System.out.println(gd.getGdPicUrl() + ":" + gd.getGoods().getTitle());
            }
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

## 4 PageHelper分页插件

### 4.1 分页查询的麻烦事

![图片](/images/032_04_08.png)

### 4.2 PageHelper使用流程

![图片](/images/032_04_09.png)

### 4.3 mysql实现

```xml
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.10</version>
        </dependency>
        <dependency>
            <groupId>com.github.jsqlparser</groupId>
            <artifactId>jsqlparser</artifactId>
            <version>2.0</version>
        </dependency>
```

```xml
    <!--启用Pagehelper分页插件-->
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor">
            <!--设置数据库类型-->
            <property name="helperDialect" value="mysql"/>
            <!--分页合理化-->
            <property name="reasonable" value="true"/>
        </plugin>
    </plugins>
```

```xml
    <select id="selectPage" resultType="com.jesse.mybatis.entity.Goods">
        SELECT * FROM t_goods WHERE current_price &lt; 1000
    </select>
```

```java
    @Test
    public void testSelectPage() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            //startPage方法会自动将下一次查询进行分页
            PageHelper.startPage(2,10);
            Page<Goods> page = (Page)session.selectList("goods.selectPage");
            System.out.println("总页数：" + page.getPages());
            System.out.println("总记录数：" + page.getTotal());
            System.out.println("开始行号：" + page.getStartRow());
            System.out.println("结束行号：" + page.getEndRow());
            System.out.println("当前页码：" + page.getPageNum());
            List<Goods> data = page.getResult();//当前页数据
            for (Goods g : data) {
                System.out.println(g.getTitle());
            }
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

### 4.4 不同数据库的分页实现原理

![图片](/images/032_04_10.png)

![图片](/images/032_04_11.png)

![图片](/images/032_04_12.png)

![图片](/images/032_04_13.png)

## 5 MyBatis整合C3P0连接池

```xml
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.4</version>
        </dependency>
```

```java
package com.jesse.mybatis.datasource;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import org.apache.ibatis.datasource.unpooled.UnpooledDataSourceFactory;
//C3P0与MyBatis兼容使用的数据源工厂
public class C3P0DataSourceFactory extends UnpooledDataSourceFactory {
    public C3P0DataSourceFactory() {
        this.dataSource = new ComboPooledDataSource();
    }
}
```

```xml
    <!--设置默认指向的数据库-->
    <environments default="dev">
        <!--配置环境，不同的环境不同的id名字-->
        <environment id="dev">
            <!--采用JDBC方式对数据库事务进行commit/rollback-->
            <transactionManager type="JDBC"></transactionManager>
            <!--采用连接池方式管理数据库连接-->
            <!--<dataSource type="POOLED">-->
            <dataSource type="com.jesse.mybatis.datasource.C3P0DataSourceFactory">
                <property name="driverClass" value="com.mysql.cj.jdbc.Driver"/>
                <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/babytun?useUnicode=true&amp;characterEncoding=UTF-8&amp;useJDBCCompliantTimezoneShift=true&amp;useLegacyDatetimeCode=false&amp;serverTimezone=UTC"/>
                <property name="user" value="root"/>
                <property name="password" value="1234"/>
                <property name="initialPoolSize" value="5"/>
                <property name="maxPoolSize" value="20"/>
                <property name="minPoolSize" value="5"/>
            </dataSource>
        </environment>
    </environments>
```

## 6 MyBatis批处理

### 6.1 批量增加

```xml
   <!--
       INSERT INTO table
       VALUES ("a", "a1", "a2"),("b", "b1", "b2"),(....)
       局限：
        1、无法获得插入数据的id
        2、批量生成的SQL太长，可能会被服务器拒绝
       -->
    <insert id="batchInsert" parameterType="java.util.List">
        INSERT INTO t_goods(title, sub_title, original_cost, current_price, discount, is_free_delivery, category_id)
        VALUES
        <foreach collection="list" item="item" index="index" separator=",">
            (#{item.title}, #{item.subTitle}, #{item.originalCost}, #{item.currentPrice}, #{item.discount}, #{item.isFreeDelivery}, #{item.categoryId})
        </foreach>
    </insert>
```

```java
    @Test
    public void testBatchInsert() throws Exception {
        SqlSession session = null;
        try{
            long st = new Date().getTime();
            session = MyBatisUtils.openSession();
            List list = new ArrayList();
            for (int i = 0; i < 10000; i++) {
                Goods goods = new Goods();
                goods.setTitle("测试商品");
                goods.setSubTitle("测试子标题");
                goods.setOriginalCost(200f);
                goods.setCurrentPrice(100f);
                goods.setDiscount(0.5f);
                goods.setIsFreeDelivery(1);
                goods.setCategoryId(43);

                list.add(goods);
            }
            session.insert("goods.batchInsert", list);
            session.commit();//提交事务数据
            long et = new Date().getTime();
            System.out.println("执行时间：" + (et -st) + "毫秒");
        }catch (Exception e){
            if (session != null){
                session.rollback();//回滚事务
            }
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

### 6.2 批量删除

```xml
    <delete id="batchDelete" parameterType="java.util.List">
        DELETE FROM t_goods WHERE goods_id in
        <foreach collection="list" item="item" index="index" open="(" close=")" separator=",">
            #{item}
        </foreach>
    </delete>
```

```java
    @Test
    public void testBatchDelete() throws Exception {
        SqlSession session = null;
        try{
            long st = new Date().getTime();
            session = MyBatisUtils.openSession();
            List list = new ArrayList();
            list.add(1920);
            list.add(1921);
            list.add(1922);
            session.delete("goods.batchDelete", list);
            session.commit();//提交事务数据
            long et = new Date().getTime();
            System.out.println("执行时间：" + (et -st) + "毫秒");
        }catch (Exception e){
            if (session != null){
                session.rollback();//回滚事务
            }
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

## 7 MaBatis注解开发方式（略）
