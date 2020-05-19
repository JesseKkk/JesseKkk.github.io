---
title: MyBatis入门
categories:
  - Java数据库开发与实战
  - 02_MyBatis从入门到进阶
tags:
  - muke
abbrlink: 13864
date: 2020-04-07 10:15:44
---

:star2:本文是MyBatis入门的学习总结:star2:

<!-- more -->

## 1 MaBatis介绍

### 1.1 框架

![图片](/images/032_03_01.png)

### 1.2 SSM开发框架

- Spring提供底层对象管理
- Spring MVC提供Web层面的交互
- MyBatis提供数据库的CRUB便捷操作

![图片](/images/032_03_02.png)

### 1.3 什么是MyBatis

![图片](/images/032_03_03.png)

### 1.4 MyBatis开发流程

![图片](/images/032_03_04.png)

## 2 MyBatis基本使用

### 2.1 MyBatis引入依赖（Maven）

```java
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.1</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.19</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
```

### 2.2 MyBatis环境配置

![图片](/images/032_03_05.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--goods_id ==> goodsId-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <!--设置默认指向的数据库-->
    <environments default="dev">
        <!--配置环境，不同的环境不同的id名字-->
        <environment id="dev">
            <!--采用JDBC方式对数据库事务进行commit/rollback-->
            <transactionManager type="JDBC"></transactionManager>
            <!--采用连接池方式管理数据库连接-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/babytun?useUnicode=true&amp;characterEncoding=UTF-8&amp;useJDBCCompliantTimezoneShift=true&amp;useLegacyDatetimeCode=false&amp;serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper resource="mappers/goods.xml"/>
    </mappers>
</configuration>
```

### 2.3 SqlSessionFactory

![图片](/images/032_03_06.png)

![图片](/images/032_03_07.png)

```java
    @Test
    public void testSqlSessionFactory() throws IOException {
        //利用Reader加载classpath下的mybatis-config.xml核心配置文件
        Reader reader = Resources.getResourceAsReader("mybatis-config.xml");
        //初始化SqlSessionFactory对象，同时解析mybatis-config.xml文件
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
        System.out.println("SesssionFactory加载成功");
        SqlSession sqlSession = null;
        try {
            //创建SqlSession对象，SqlSession是JDBC的扩展类，用于与数据库交互
            sqlSession = sqlSessionFactory.openSession();
            Connection connection = sqlSession.getConnection();
            System.out.println(connection);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (sqlSession != null){
                //如果type="POOLED"，代表连接池，close则是将连接回收到连接池中
                //如果type="UNPOOLED"，代表直连，close则会调用Connection.close()方法
                sqlSession.close();
            }
        }
    }
```

### 2.4 初始化工具类MyBatisUtils

```java
package com.jesse.mybatis.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.Reader;

public class MyBatisUtils {
    private static SqlSessionFactory sqlSessionFactory = null;
    static{
        Reader reader = null;
        try {
            reader = Resources.getResourceAsReader("mybatis-config.xml");
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
        } catch (IOException e) {
            e.printStackTrace();
            throw new ExceptionInInitializerError(e);
        }
    }

    public static SqlSession openSession(){
        return sqlSessionFactory.openSession();
    }

    public static void closeSession(SqlSession session){
        if (session != null){
            session.close();
        }
    }
}
```

### 2.5 MyBatis数据查询步骤

![图片](/images/032_03_08.png)

```java
package com.jesse.mybatis.entity;

public class Goods {
    private Integer goodsId;
    private String title;
    private String subTitle;
    private Float originalCost;
    private Float currentPrice;
    private Float discount;
    private Integer isFreeDelivery;
    private Integer categoryId;

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
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="goods">
    <select id="selectAll" resultType="com.jesse.mybatis.entity.Goods">
        SELECT * FROM t_goods ORDER BY goods_id DESC LIMIT 10
    </select>
</mapper>
```

```java
    @Test
    public void testSelectAll() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            List<Goods> list = session.selectList("goods.selectAll");
            for (Goods g : list){
                System.out.println(g.getTitle());
            }
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

## 3 SQL传参

### 3.1 单参数

```xml
    <select id="selectById" parameterType="Integer" resultType="com.jesse.mybatis.entity.Goods">
        SELECT * FROM t_goods WHERE goods_id = #{value}
    </select>
```

```java
    @Test
    public void testSelectById() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            Goods goods = session.selectOne("goods.selectById", 1603);
            System.out.println(goods.getTitle());
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

### 3.2 多参数

```xml
    <select id="selectByPriceRange" parameterType="java.util.Map" resultType="com.jesse.mybatis.entity.Goods">
        SELECT * FROM t_goods
        WHERE
            current_price BETWEEN #{min} AND #{max}
        ORDER BY current_price
        LIMIT 0,#{lmt}
    </select>
```

```java
    @Test
    public void testSelectByPriceRange() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            Map param = new HashMap();
            param.put("min",100);
            param.put("max",500);
            param.put("lmt",10);
            List<Goods> list = session.selectList("selectByPriceRange", param);
            for (Goods g:list){
                System.out.println(g.getTitle() + ":" + g.getCurrentPrice());
            }
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

## 4 多表关联查询

### 4.1 获取多表关联查询结果

```xml
    <!--优点：易于扩展，易于使用
        缺点：太过灵活，无法进行编译时检查
    -->
    <select id="selectGoodsMap" resultType="java.util.LinkedHashMap">
        SELECT g.*, c.category_name FROM t_goods AS g, t_category AS c
        WHERE g.category_id = c.category_id
    </select>
```

```java
    @Test
    public void testSelectGoodsMap() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            List<Map> list = session.selectList("goods.selectGoodsMap");
            for (Map map:list){
                System.out.println(map);
            }
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

### 4.2 ResultMap结果映射

![图片](/images/032_03_09.png)

```java
package com.jesse.mybatis.dto;

import com.jesse.mybatis.entity.Category;
import com.jesse.mybatis.entity.Goods;
//Data Transfer Object --数据传输对象
public class GoodsDTO {
    private Goods goods = new Goods();
    private Category category = new Category();

    public Goods getGoods() {
        return goods;
    }

    public void setGoods(Goods goods) {
        this.goods = goods;
    }

    public Category getCategory() {
        return category;
    }

    public void setCategory(Category category) {
        this.category = category;
    }
}
```

```xml
    <resultMap id="rmGoods" type="com.jesse.mybatis.dto.GoodsDTO">
        <!--设置主键字段与属性映射-->
        <id property="goods.goodsId" column="goods_id"></id>
        <!--设置非主键字段与属性映射-->
        <result property="goods.title" column="title"></result>
        <result property="goods.subTitle" column="sub_title"></result>
        <result property="goods.originalCost" column="original_cost"></result>
        <result property="goods.currentPrice" column="current_price"></result>
        <result property="goods.discount" column="discount"></result>
        <result property="goods.isFreeDelivery" column="is_free_delivery"></result>
        <result property="goods.categoryId" column="category_id"></result>
        <result property="category.categoryId" column="category_id"></result>
        <result property="category.categoryName" column="category_name"></result>
        <result property="category.parentId" column="parent_id"></result>
        <result property="category.categoryLevel" column="category_level"></result>
        <result property="category.categoryOrder" column="category_order"></result>
    </resultMap>
    <select id="selectGoodsDTO" resultMap="rmGoods">
        SELECT g.*, c.* FROM t_goods AS g, t_category AS c
        WHERE g.category_id = c.category_id
    </select>
```

```java
    @Test
    public void testSelectGoodsDTO() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            List<GoodsDTO> list = session.selectList("goods.selectGoodsDTO");
            for (GoodsDTO g:list){
                System.out.println(g);
            }
        }catch (Exception e){
            throw e;
        }finally {
            MyBatisUtils.closeSession(session);
        }
    }
```

## 5 MyBatis写操作

### 5.1 数据库事务

![图片](/images/032_03_10.png)

### 5.2 INSERT

```xml
    <insert id="insert" parameterType="com.jesse.mybatis.entity.Goods">
        INSERT INTO t_goods(title, sub_title, original_cost, current_price, discount, is_free_delivery, category_id)
        VALUES (#{title}, #{subTitle}, #{originalCost}, #{currentPrice}, #{discount}, #{isFreeDelivery}, #{categoryId});
        <selectKey resultType="Integer" keyProperty="goodsId" order="AFTER">
            SELECT  last_insert_id()
        </selectKey>
    </insert>
```

```java
    @Test
    public void testInsert() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            Goods goods = new Goods();
            goods.setTitle("测试商品");
            goods.setSubTitle("测试子标题");
            goods.setOriginalCost(200f);
            goods.setCurrentPrice(100f);
            goods.setDiscount(0.5f);
            goods.setIsFreeDelivery(1);
            goods.setCategoryId(43);
            //insert()方法返回值代表本次成功插入的记录总数
            int num = session.insert("goods.insert",goods);
            session.commit();//提交事务数据
            System.out.println(goods.getGoodsId());
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

### 5.3 selectKey与useGeneratedKeys

#### 5.3.1 selectKey

![图片](/images/032_03_11.png)

#### 5.3.2 useGeneratedKeys

![图片](/images/032_03_12.png)

#### 5.3.3 二者区别

![图片](/images/032_03_13.png)

![图片](/images/032_03_14.png)

### 5.4 UPDATE

```xml
    <update id="update" parameterType="com.jesse.mybatis.entity.Goods">
        UPDATE t_goods
        SET
            title = #{title},
            sub_title = #{subTitle},
            original_cost = #{originalCost},
            current_price = #{currentPrice},
            discount = #{discount},
            is_free_delivery = #{isFreeDelivery},
            category_id = #{categoryId}
        WHERE
            goods_id = #{goodsId}
    </update>
```

```java
    @Test
    public void testUpdate() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            Goods goods = session.selectOne("goods.selectById", 739);
            goods.setTitle("更新测试商品");
            int num = session.update("goods.update", goods);
            session.commit();//提交事务数据
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

### 5.5 DELETE

```xml
    <delete id="delete" parameterType="Integer">
        DELETE FROM t_goods WHERE goods_id = #{value}
    </delete>
```

```java
    @Test
    public void testDelete() throws Exception {
        SqlSession session = null;
        try{
            session = MyBatisUtils.openSession();
            int num = session.delete("goods.delete", 739);
            session.commit();//提交事务数据
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

## 6 预防SQL注入攻击

![图片](/images/032_03_15.png)

![图片](/images/032_03_16.png)

## 7 MyBatis工作流程

![图片](/images/032_03_17.png)
