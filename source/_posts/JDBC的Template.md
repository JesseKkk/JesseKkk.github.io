---
title: JDBC的Template
categories:
  - SSM到Spring Boot入门与综合实战
  - 01_Spring从入门到进阶
tags:
  - imooc
abbrlink: 15513
date: 2020-05-06 10:11:33
---

:star2:本文是基于JDBC的Template总结:star2:

<!-- more -->

## 1 JDBC的Template概念

![图片](/images/041_05_01.png)

![图片](/images/041_05_02.png)

![图片](/images/041_05_03.png)

## 2 JDBC的Template基本使用

### 2.1 环境搭建

```xml
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.19</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
    </dependencies>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx.xsd">

    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/selection_course?useUnicode=true&amp;characterEncoding=UTF-8&amp;useJDBCCompliantTimezoneShift=true&amp;useLegacyDatetimeCode=false&amp;serverTimezone=GMT%2B8"/>
        <property name="username" value="root"/>
        <property name="password" value="1234"/>
    </bean>
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```

### 2.1 execute方法

- 执行DDL语句

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

public class Test {

    private JdbcTemplate jdbcTemplate;
    {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
        jdbcTemplate = (JdbcTemplate)context.getBean("jdbcTemplate");
    }

    @org.junit.Test
    public void testExecute(){
        jdbcTemplate.execute("create table user1(id int, name varchar(20))");
    }
```

### 2.2 update与batchUpdate方法

- 执行增删改

```java
    @org.junit.Test
    public void testUpdate(){
        String sql = "INSERT INTO student(name,sex) VALUES(?,?)";
        jdbcTemplate.update(sql, new Object[]{"张飞","男"});
    }

    @org.junit.Test
    public void testUpdate2(){
        String sql = "UPDATE student SET sex=? WHERE id=?";
        jdbcTemplate.update(sql, "女",1);
    }

    @org.junit.Test
    public void testBatchUpdate(){
        String[] sqls={
          "INSERT INTO student(name,sex) VALUES('关羽','女')",
          "INSERT INTO student(name,sex) VALUES('刘备','男')",
          "UPDATE student SET sex='男' WHERE id=1"
        };
        jdbcTemplate.batchUpdate(sqls);
    }

    @org.junit.Test
    public void testBatchUpdate2(){
        String sql = "INSERT INTO selection(student,course) VALUES(?,?)";
        List<Object[]> list = new ArrayList<Object[]>();
        list.add(new Object[]{1, 1001});
        list.add(new Object[]{1,1003});
        jdbcTemplate.batchUpdate(sql,list);
    }
```

### 2.3 query与queryXXX方法

- 执行查询

```java
package com.jesse.sc.entity;

import java.util.Date;

public class Student {
    private int id;
    private String name;
    private String sex;
    private Date born;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Date getBorn() {
        return born;
    }

    public void setBorn(Date born) {
        this.born = born;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                ", born=" + born +
                '}';
    }
}
```

```java
    @org.junit.Test
    public void testQuerySimple1(){
        String sql = "SELECT count(*) FROM student";
        int count = jdbcTemplate.queryForObject(sql,Integer.class);
        System.out.println(count);
    }

    @org.junit.Test
    public void testQuerySimple2(){
        String sql = "SELECT name FROM student WHERE sex=?";
        List<String> names = jdbcTemplate.queryForList(sql,String.class,"男");
        System.out.println(names);
    }

    @org.junit.Test
    public void testQueryMap1(){
        String sql = "SELECT * FROM student WHERE id = ?";
        Map<String,Object> stu = jdbcTemplate.queryForMap(sql, 1);
        System.out.println(stu);
    }

    @org.junit.Test
    public void testQueryMap2(){
        String sql = "SELECT * FROM student";
        List<Map<String,Object>> stus = jdbcTemplate.queryForList(sql);
        System.out.println(stus);
    }

    @org.junit.Test
    public void testQueryEntity1(){
        String sql = "SELECT * FROM student WHERE id = ?";
        Student stu = jdbcTemplate.queryForObject(sql, new StudentRowMapper(), 1);
        System.out.println(stu);
    }

    @org.junit.Test
    public void testQueryEntity2(){
        String sql = "SELECT * FROM student";
        List<Student> stus = jdbcTemplate.query(sql, new StudentRowMapper());
        System.out.println(stus);
    }

    private class StudentRowMapper implements RowMapper<Student>{
        public Student mapRow(ResultSet resultSet, int i) throws SQLException {
            Student stu = new Student();
            stu.setId(resultSet.getInt("id"));
            stu.setName(resultSet.getString("name"));
            stu.setSex(resultSet.getString("sex"));
            stu.setBorn(resultSet.getDate("born"));
            return stu;
        }
    }
```

### 2.4 Example

```java
package com.jesse.sc.entity;

import java.util.Date;

public class Student {
    private int id;
    private String name;
    private String sex;
    private Date born;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Date getBorn() {
        return born;
    }

    public void setBorn(Date born) {
        this.born = born;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                ", born=" + born +
                '}';
    }
}

```

```java
package com.jesse.sc.dao;

import com.jesse.sc.entity.Student;

import java.util.List;

public interface StudentDao {
    void insert(Student stu);
    void update(Student stu);
    void delete(int id);
    Student select(int id);
    List<Student> selectAll();
}

```

```java
package com.jesse.sc.dao.impl;

import com.jesse.sc.dao.StudentDao;
import com.jesse.sc.entity.Student;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Repository;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

@Repository
public class StudentDaoImpl implements StudentDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public void insert(Student stu) {
        String sql = "INSERT INTO student(name,sex,born) VALUES(?,?,?)";
        jdbcTemplate.update(sql, stu.getName(), stu.getSex(), stu.getBorn());
    }

    public void update(Student stu) {
        String sql = "UPDATE student SET name=?,sex=?,born=? WHERE id=?";
        jdbcTemplate.update(sql,stu.getName(),stu.getSex(),stu.getBorn(),stu.getId());
    }

    public void delete(int id) {
        String sql = "DELETE FROM student WHERE id=?";
        jdbcTemplate.update(sql,id);
    }

    public Student select(int id) {
        String sql = "SELECT * FROM student WHERE id=?";
        return jdbcTemplate.queryForObject(sql, new StudentRowMapper(), id);
    }

    public List<Student> selectAll() {
        String sql = "SELECT * FROM student WHERE id=?";
        return jdbcTemplate.query(sql, new StudentRowMapper());
    }

    private class StudentRowMapper implements RowMapper<Student> {
        public Student mapRow(ResultSet resultSet, int i) throws SQLException {
            Student stu = new Student();
            stu.setId(resultSet.getInt("id"));
            stu.setName(resultSet.getString("name"));
            stu.setSex(resultSet.getString("sex"));
            stu.setBorn(resultSet.getDate("born"));
            return stu;
        }
    }
}
```

## 3 优缺点

- 优点：对比原生JDBC，更加方便灵活
- 缺点：SQL与Java代码掺杂，功能不够丰富（引出MaBatis）
