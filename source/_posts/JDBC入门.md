---
title: JDBC入门
categories:
  - Java数据库开发与实战
  - 01_初识数据库操作
tags:
  - muke
abbrlink: 34705
date: 2020-03-20 16:10:33
---

:star2:本文是JDBC入门的学习:star2:

<!-- more -->

## 1 JDBC的概述

![图片](/images/031_04_01.png)

## 2 JDBC的API

### 2.1 JDBC入门

![图片](/images/031_04_02.png)

#### 2.1.1 第一个JDBC程序

```java
package com.jesse.jdbc.demo1;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import org.junit.Test;

public class JDBCDemo1 {

    @Test
    public void demo1() {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            // 1.加载驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            // 2.获得连接
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/jdbctest" +
            "?useSSL=false&serverTimezone=Hongkong&useUnicode=true&characterEndocing=utf-8", "root", "1234");
            // 3.创建执行SQL语句的对象，并且执行SQL
            // 3.1创建执行SQL的对象
            String sql = "SELECT * FROM user";
            stmt = conn.createStatement();
            // 3.2执行sql
            rs = stmt.executeQuery(sql);
            while(rs.next()) {
                int uid = rs.getInt("uid");
                String username = rs.getString("username");
                String password = rs.getString("password");
                String name = rs.getString("name");
                System.out.println(uid + "  " + username + "  " + password + "  " + name);
            }
        } catch (ClassNotFoundException | SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } finally {
            // 4.释放资源
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException sqlEx) {
                }
                rs = null;
            }

            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                stmt = null;
            }

            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                conn = null;// 垃圾回收机制会更早回收对象。
            }
        }
    }
}

```

### 2.2 JDBC的API-DriverManager

![图片](/images/031_04_03.png)

### 2.3 JDBC的API-Connection

![图片](/images/031_04_04.png)

### 2.4 JDBC的API-Statement

![图片](/images/031_04_05.png)

### 2.5 JDBC的API-ResultSet

![图片](/images/031_04_06.png)

## 3 JDBC的CRUB操作

```java
package com.jesse.jdbc.demo1;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import org.junit.Test;

public class JDBCDemo2 {

    @Test
    /**
     *  查询操作
     */
    public void demo4() {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            //加载驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //获得Connection对象
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/jdbctest" +
                    "?useSSL=false&serverTimezone=Hongkong&useUnicode=true&characterEndocing=utf-8",
                    "root", "1234");
            //获得SQL执行对象
            stmt = conn.createStatement();
            //SQL语句
            String sql =  "SELECT * FROM user";
            //执行SQL语句，返回结果集
            rs = stmt.executeQuery(sql);
            //遍历结果集
            while(rs.next()) {
                int uid = rs.getInt("uid");
                String username = rs.getString("username");
                String password = rs.getString("password");
                String name = rs.getString("name");
                System.out.println(uid + "  " + username + "  " + password + "  " + name);
            }
        }catch(Exception e) {
            e.printStackTrace();
        }finally {
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                rs = null;
            }

            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                stmt = null;
            }

            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                conn = null;
            }
        }
    }

    @Test
    /*
     *  删除操作
     */
    public void demo3() {
        Connection conn = null;
        Statement stmt = null;
        try {
            // 加载驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            // 获得连接
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/jdbctest" +
                    "?useSSL=false&serverTimezone=Hongkong&useUnicode=true&characterEndocing=utf-8",
                    "root", "1234");
            // 获得SQL的执行对象
            stmt = conn.createStatement();
            // SQL语句
            String sql = "DELETE FROM user WHERE uid=4";
            //执行SQL
            int i = stmt.executeUpdate(sql);
            if (i >0) {
                System.out.println("删除成功");
            }
        } catch(Exception e) {
            e.printStackTrace();
        } finally {
            // 释放资源
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                stmt = null;
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                conn = null;
            }
        }
    }

    @Test
    /*
     *  修改操作
     */
    public void demo2() {
        Connection conn = null;
        Statement stmt = null;
        try {
            // 加载驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            // 获得连接
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/jdbctest" +
                    "?useSSL=false&serverTimezone=Hongkong&useUnicode=true&characterEndocing=utf-8",
                    "root", "1234");
            // 获得SQL的执行对象
            stmt = conn.createStatement();
            // SQL语句
            String sql = "UPDATE user SET password='555' WHERE uid=4";
            //执行SQL
            int i = stmt.executeUpdate(sql);
            if (i >0) {
                System.out.println("修改成功");
            }
        } catch(Exception e) {
            e.printStackTrace();
        } finally {
            // 释放资源
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                stmt = null;
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                conn = null;
            }
        }
    }

    @Test
    /*
     *  保存操作
     */
    public void demo1() {
        Connection conn = null;
        Statement stmt = null;
        try {
            // 注册驱动：
            Class.forName("com.mysql.cj.jdbc.Driver");
            // 获得连接
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/jdbctest" +
                    "?useSSL=false&serverTimezone=Hongkong&useUnicode=true&characterEndocing=utf-8",
                    "root", "1234");
            //获得执行SQL语句的对象：
            stmt = conn.createStatement();
            // 编写SQL：
            String sql = "INSERT user VALUES (null,'ddd','444','孔金星')";
            //执行SQL：
            int i = stmt.executeUpdate(sql);
            if(i>0) {
                System.out.println("保存成功");
            }
        }catch(Exception e) {
            e.printStackTrace();
        }finally {
            if(stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                stmt = null;
            }
            if(conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                conn = null;
            }

        }

    }
}

```

## 4 JDBC工具类的抽取

```properties
driverClass = com.mysql.cj.jdbc.Driver
url = jdbc:mysql://127.0.0.1:3306/jdbctest?useSSL=false&serverTimezone=Hongkong&useUnicode=true&characterEndocing=utf-8
username = root
password = 1234
```

```java
package com.jesse.jdbc.utils;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class JDBCUtils {

    private static final String driverClass;
    private static final String url;
    private static final String username;
    private static final String password;

    static {
        // 加载属性文件并解析
        Properties props = new Properties();
        // 如何获得属性文件的输入流
        // 通常情况下使用类的加载器的方式进行获取：
        InputStream is =JDBCUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
        try {
            props.load(is);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        driverClass = props.getProperty("driverClass");
        url = props.getProperty("url");
        username = props.getProperty("username");
        password = props.getProperty("password");
    }

    /**
     * 加载资源
     * @throws ClassNotFoundException
     */
    public static void loadDriver() throws ClassNotFoundException {
        Class.forName(driverClass);
    }

    /**
     *  连接资源
     * @throws Exception
     */
    public static Connection createConnection() throws Exception {
        loadDriver();
        Connection conn = DriverManager.getConnection(url,username,password);
        return conn;
    }

    /**
     *  释放资源
     */
    public static void release(Statement stmt, Connection conn) {
        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            stmt = null;
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            conn = null;
        }
    }

    public static void release(Statement stmt, Connection conn, ResultSet rs) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            rs = null;
        }

        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            stmt = null;
        }

        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            conn = null;
        }
    }

}

```

```java
package com.jesse.jdbc.demo1;

import java.sql.Connection;
import java.sql.Statement;

import org.junit.Test;

import com.jesse.jdbc.utils.JDBCUtils;

public class JDBCDemo3 {

    @Test
    /**
     *  测试JDBCUtils
     */
    public void demo1() {
        Connection conn = null;
        Statement stmt = null;
        try {
            //获得连接
            conn = JDBCUtils.createConnection();
            //获得SQL执行执行对象
            stmt = conn.createStatement();
            //SQL语句
            String sql = "INSERT user VALUES (NULL,'eee','555','小六')";
            //执行SQL语句
            int i = stmt.executeUpdate(sql);
            if (i>0) {
                System.out.println("保存成功！");
            }
        }catch(Exception e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.release(stmt, conn);
        }
    }

}

```

## 5 SQL注入漏洞演示与解决

![图片](/images/031_04_07.png)

```java
package com.jesse.jdbc.demo2;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;

import org.junit.Test;

import com.jesse.jdbc.utils.JDBCUtils;

public class JDBCDemo4 {

    @Test
    /**
     *  演示JDBC的注入的漏洞
     */
    public void demo1() {
        //boolean flag = JDBCDemo4.login2("aaa","111");//正常登录
        //boolean flag = JDBCDemo4.login2("aaa' OR '1=1 ","adfasd");//SQL注入漏洞方式一
        boolean flag = JDBCDemo4.login2("aaa' -- ","fasdfasd");//SQL注入漏洞方式二

        if(flag == true) {
            System.out.println("登陆成功！");
        }else {
            System.out.println("登录失败！");
        }
    }

    /**
     * 避免SQL注入漏洞的方法
     */
    public static boolean login2(String username,String password) {
        Connection conn  = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        boolean flag = false;
        try {
            //获得连接
            conn = JDBCUtils.createConnection();
            //SQL语句
            String sql = "SELECT * FROM user WHERE username= ? AND password= ? ";
            //预处理SQL语句
            pstmt = conn.prepareStatement(sql);
            // 设置参数
            pstmt.setString(1, username);
            pstmt.setString(2, password);
            // 执行SQL语句
            rs = pstmt.executeQuery();
            if(rs.next()) {
                flag = true;
            }else {
                flag = false;
            }
        }catch(Exception e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.release(pstmt, conn, rs);
        }
        return flag;
    }

    /**
     *  产生SQL注入漏洞的方法
     * @param username
     * @param password
     * @return
     */
    public static boolean login(String username, String password) {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        boolean flag = false;
        try {
            //获得连接
            conn = JDBCUtils.createConnection();
            //获得SQL执行对象
            stmt = conn.createStatement();
            //SQL语句
            String sql = "SELECT * FROM user WHERE username='" +username +  "' AND password='" + password + "'";
            //SQL语句执行
            rs = stmt.executeQuery(sql);
            if(rs.next()) {
                flag = true;
            }else {
                flag = false;
            }
        }catch(Exception e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.release(stmt, conn, rs);
        }
        return flag;
    }
}

```

## 6 C3P0连接池

### 6.1 连接池

- 连接池市创建和管理一个连接的缓冲池的技术，这些连接准备好被任何需要它们的线程使用。

![图片](/images/031_04_08.png)

![图片](/images/031_04_09.png)

### 6.2 C3P0的使用

```xml
<!-- 文件名：c3p0-config.xml 路径：classPath -->
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>

  <default-config>
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl"> jdbc:mysql://127.0.0.1:3306/jdbctest?useSSL=false&amp;serverTimezone=Hongkong&amp;useUnicode=true&amp;characterEndocing=utf-8</property>
    <property name="user">root</property>
    <property name="password">1234</property>
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">20</property>
  </default-config>
  
</c3p0-config>
```

```java
package com.jesse.jdbc.utils;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

import com.mchange.v2.c3p0.ComboPooledDataSource;

public class JDBCUtils2 {
    private static final ComboPooledDataSource dataSource = new ComboPooledDataSource();
    /**
     *  连接资源
     * @throws Exception
     */
    public static Connection createConnection() throws Exception {
        Connection conn = dataSource.getConnection();
        return conn;
    }

    /**
     *  释放资源
     */
    public static void release(Statement stmt, Connection conn) {
        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            stmt = null;
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            conn = null;
        }
    }

    public static void release(Statement stmt, Connection conn, ResultSet rs) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            rs = null;
        }

        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            stmt = null;
        }

        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            conn = null;
        }
    }
}

```

```java
package com.jesse.jdbc.demo3;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

import org.junit.Test;

import com.jesse.jdbc.utils.JDBCUtils;
import com.jesse.jdbc.utils.JDBCUtils2;
import com.mchange.v2.c3p0.ComboPooledDataSource;

/**
 *  连接池的测试类
 * @author Jesse
 *
 */
public class DataSourceDemo1 {

    @Test
    /**
     *  使用配置文件的方式
     */
    public void demo2() {
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            // 创建连接池：
            //ComboPooledDataSource dataSource = new ComboPooledDataSource();
            //获得连接对象
            //conn = dataSource.getConnection();
            conn = JDBCUtils2.createConnection();
            //编写SQL
            String sql = "SELECT * FROM user";
            //预编译SQL
            pstmt = conn.prepareStatement(sql);
            //设置参数
            //执行SQL：
            rs = pstmt.executeQuery();
            while(rs.next()) {
                System.out.println(rs.getInt("uid") +
                        "  " +rs.getString("username") +
                        "  " + rs.getString("password") +
                        "  " + rs.getString("name")
                        );
            }
        }catch(Exception e) {
            e.printStackTrace();
        }finally {
            JDBCUtils2.release(pstmt, conn, rs);
        }
    }

    @Test
    /**
     * 手动设置连接池
     */
    public void demo1() {
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            // 创建连接池：
            ComboPooledDataSource dataSource = new ComboPooledDataSource();
            //设置连接池的参数：
            dataSource.setDriverClass("com.mysql.cj.jdbc.Driver");
            //dataSource.setJdbcUrl("jdbc:mysql:///jdbctest");
            dataSource.setJdbcUrl("jdbc:mysql://127.0.0.1:3306/jdbctest" +
                    "?useSSL=false&serverTimezone=Hongkong&useUnicode=true&characterEndocing=utf-8");
            dataSource.setUser("root");
            dataSource.setPassword("1234");
            dataSource.setMinPoolSize(20);
            dataSource.setInitialPoolSize(3);
            //获得连接对象
            conn = dataSource.getConnection();
            //编写SQL
            String sql = "SELECT * FROM user";
            //预编译SQL
            pstmt = conn.prepareStatement(sql);
            //设置参数
            //执行SQL：
            rs = pstmt.executeQuery();
            while(rs.next()) {
                System.out.println(rs.getInt("uid") +
                        "  " +rs.getString("username") +
                        "  " + rs.getString("password") +
                        "  " + rs.getString("name")
                        );
            }
        }catch(Exception e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.release(pstmt, conn, rs);
        }
    }
}

```
