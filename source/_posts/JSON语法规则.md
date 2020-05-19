---
title: JSON语法规则
categories:
  - JavaWeb入门
  - 04_Java-Web进阶
tags:
  - muke
abbrlink: 29717
date: 2020-02-19 10:28:13
---

:star2:本文是JSON语法规则的学习总结:star2:

<!-- more -->

## 1 JSON

### 1.1 JSON介绍

![图片](/images/024_01_01.png)

### 1.2 JSON语法规则

![图片](/images/024_01_02.png)

![图片](/images/024_01_03.png)

### 1.3 Javascript访问JSON对象

```json
[
    {
        "empno": 1100,
        "ename": "李宁",
        "job": "软件工程师",
        "hiredate": "2017-05-12",
        "salary": 13000,
        "dname": "研发部"
    },
    {
        "empno": 1101,
        "ename": "王乐",
        "job": "光学工程师",
        "hiredate": "2018-05-12",
        "salary": 20000,
        "dname": "市场部",
        "customers": [
            {
                "cname": "李东"
            },
            {
                "cname": "小明"
            }
        ]
    }
][
    {
        "empno": 1100,
        "ename": "李宁",
        "job": "软件工程师",
        "hiredate": "2017-05-12",
        "salary": 13000,
        "dname": "研发部"
    },
    {
        "empno": 1101,
        "ename": "王乐",
        "job": "光学工程师",
        "hiredate": "2018-05-12",
        "salary": 20000,
        "dname": "市场部",
        "customers": [
            {
                "cname": "李东"
            },
            {
                "cname": "小明"
            }
        ]
    }
]
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
    var json = [
        {
            "empno": 1100,
            "ename": "李宁",
            "job": "软件工程师",
            "hiredate": "2017-05-12",
            "salary": 13000,
            "dname": "研发部"
        },
        {
            "empno": 1101,
            "ename": "王乐",
            "job": "光学工程师",
            "hiredate": "2018-05-12",
            "salary": 20000,
            "dname": "市场部",
            "customers": [
                {
                    "cname": "李东"
                },
                {
                    "cname": "小明"
                }
            ]
        }
    ];
    console.log(json);
    for(var i = 0; i < json.length; i++){
        var emp =json[i];
        document.write("<h1>");
        document.write(emp.empno);
        document.write(", " + emp.name);
        document.write(", " + emp.job);
        document.write(", " + emp.hiredate);
        document.write(", " + emp.salary);
        document.write(", " + emp.dname);
        document.write("</h1>");
        if(emp.customers != null){
            for(var j = 0; j < emp.customers.length; j++){
                var cus = emp.customers[j];
                document.write("<h2>");
                document.write(cus.cname);
                document.write("</h2>");
            }
        }
    }
</script>
</head>
<body>

</body>
</html>
```

### 1.4 js中将字符串转为JSON

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>字符串转JSON</title>
<script type="text/javascript">
    var str = "{\"class_name\" : \"五年级四班\"}";
    var json = JSON.parse(str);
    console.log(str);
    console.log(json);
    document.write("班级：" + json.class_name);
</script>
</head>
<body>

</body>
</html>
```

### 1.4 js中JSON转为字符串

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSON转字符串</title>
<script type="text/javascript">
    var json1 = {"class_name" :  "五年级四班"};
    var str1 = JSON.stringify(json1);
    console.log(json1);
    console.log(str1);
    var json2 = {};
    json2.class_name = "五年级五班";
    json2.floor = "逸夫楼四层";
    json2.teacher = "孔金星";
    console.info(json2);
</script>
</head>
<body>

</body>
</html>
```

## 2 FastJSON

### 2.1 JSON与Java交互

![图片](/images/024_01_04.png)

### 2.2 FastJSON对象序列化与反序列化

```java
package com.jesse.json;

import java.util.Date;

import com.alibaba.fastjson.annotation.JSONField;

public class Employee {
    private Integer empno;
    private String ename;
    private String job;
    @JSONField(name = "hiredate", format = "yyy-MM-dd")
    private Date hdate;
    private Float salary;
    @JSONField(serialize = false)
    private String dname;
    public Employee(Integer empno, String ename, String job, Date hdate, Float salary, String dname) {
        super();
        this.empno = empno;
        this.ename = ename;
        this.job = job;
        this.hdate = hdate;
        this.salary = salary;
        this.dname = dname;
    }

    public Integer getEmpno() {
        return empno;
    }
    public void setEmpno(Integer empno) {
        this.empno = empno;
    }
    public String getEname() {
        return ename;
    }
    public void setEname(String ename) {
        this.ename = ename;
    }
    public String getJob() {
        return job;
    }
    public void setJob(String job) {
        this.job = job;
    }
    public Date getHdate() {
        return hdate;
    }
    public void setHdate(Date hdate) {
        this.hdate = hdate;
    }
    public Float getSalary() {
        return salary;
    }
    public void setSalary(Float salary) {
        this.salary = salary;
    }
    public String getDname() {
        return dname;
    }
    public void setDname(String dname) {
        this.dname = dname;
    }


}
```

```java
package com.jesse.json;

import java.util.Calendar;

import com.alibaba.fastjson.JSON;

public class FastJsonSimple1 {
    public static void main(String[] args) {
        Calendar c = Calendar.getInstance();
        c.set(2019, 0, 30, 0, 0, 0);
        Employee emp = new Employee(1110, "孔金星", "Java工程师", c.getTime(), 20000f, "研发部");
        //FastJSON中提供JSON对象，完成对象与JSON字符串的相互转换
        String json = JSON.toJSONString(emp);
        System.out.println(json);
        Employee em = JSON.parseObject(json, Employee.class);
        System.out.println(em.getEname());
    }
}
```

### 2.3 FastJSON对象数组序列化与反序列化

```java
package com.jesse.json;

import java.util.ArrayList;
import java.util.List;

import com.alibaba.fastjson.JSON;

public class FastJsonSimple2 {
    public static void main(String[] args) {
        List<Employee> emplist = new ArrayList<Employee>();
        for(int i = 1; i <= 100; i++) {
            Employee emp = new Employee();
            emp.setEmpno(1000 + i);
            emp.setEname("员工" + i);
            emplist.add(emp);
        }
        String json = JSON.toJSONString(emplist);
        System.out.println(json);
        List<Employee> emps = JSON.parseArray(json, Employee.class);
        for(Employee e : emps) {
            System.out.println(e.getEmpno() + e.getEname());
        }
    }
}
```
