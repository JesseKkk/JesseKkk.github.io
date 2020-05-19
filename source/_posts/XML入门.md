---
title: XML入门
categories:
  - JavaWeb入门
  - 03_Java-Web
tags:
  - muke
abbrlink: 62356
date: 2020-02-11 15:43:10
---

:star2:本文是XML入门的学习总结:star2:

<!-- more -->

## 1 XML介绍与用途

- XML = Extensible Markup Language

### 1.1 XML与HTML的比较

![图片](/images/023_01_01.png)

### 1.2 XML的用途

![图片](/images/023_01_02.png)

## 2 XML的标签书写规则

### 2.1 合法的标签名

![图片](/images/023_01_03.png)

### 2.2 合理的使用属性

![图片](/images/023_01_04.png)

### 2.3 特殊字符与CDATA标签

![图片](/images/023_01_052.png)

![图片](/images/023_01_051.png)

![图片](/images/023_01_05.png)

### 2.4 有序的子元素

![图片](/images/023_01_06.png)

## 3 XML的语义约束

![图片](/images/023_01_07.png)

### 3.1 DTD语义约束

![图片](/images/023_01_08.png)

#### 3.1.1 DTD使用规则

![图片](/images/023_01_09.png)

#### 3.1.2 DTD定义节点数量

![图片](/images/023_01_10.png)

#### 3.1.3 DTD引用规则

![图片](/images/023_01_11.png)

##### 3.1.4 Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hr SYSTEM "hr.dtd">
<!-- 人力资源管理系统 -->
<hr>
        <employee no="3309">
            <name>张三</name>
            <age>31</age>
            <salary>4000</salary>
                <department>
                        <dname>会计部</dname>
                        <address>XX大厦-B103</address>
                </department>
        </employee>
        <employee no="3310">
                <name>张三</name>
                <age>31</age>
                <salary>4000</salary>
                <department>
                        <dname>会计部</dname>
                        <address>XX大厦-B103</address>
                </department>
        </employee>
</hr>
```

```dtd
<?xml version="1.0" encoding="UTF-8"?>
<!ELEMENT hr (employee+)>
<!ELEMENT employee (name,age,salary,department)>
<!ATTLIST employee no CDATA "" >
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
<!ELEMENT salary (#PCDATA)>
<!ELEMENT department (dname,address)>
<!ELEMENT dname (#PCDATA)>
<!ELEMENT address (#PCDATA)>
```

### 3.2 XML Schema

![图片](/images/023_01_12.png)

#### 3.2.1 Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 人力资源管理系统 -->
<hr xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xsi:noNamespaceSchemaLocation="hr.xsd">
        <employee no="3309">
                <name>张三</name>
                <age>31</age>
                <salary>4000</salary>
                <department>
                        <dname>会计部</dname>
                        <address>XX大厦-B103</address>
                </department>
        </employee>
        <employee no="3310">
                <name>张三</name>
                <age>31</age>
                <salary>4000</salary>
                <department>
                        <dname>会计部</dname>
                        <address>XX大厦-B103</address>
                </department>
        </employee>
</hr>
```

```xsd
<?xml version="1.0" encoding="UTF-8"?>
<schema xmlns="http://www.w3.org/2001/XMLSchema" >
    <element name="hr">
<!--         complexType标签含义是复杂节点，包含子节点是必须使用这个标签 -->
        <complexType>
            <sequence>
                <element name="employee" minOccurs="1" maxOccurs="999">
                    <complexType>
                        <sequence>
                            <element name="name"  type="string"></element>
                            <element name="age" >
                                <simpleType>
                                    <restriction base="integer">
                                        <minInclusive value="18"></minInclusive>
                                        <maxInclusive value="60"></maxInclusive>
                                    </restriction>
                                </simpleType>
                            </element>
                            <element name="salary"  type="integer"></element>
                            <element name="department">
                                <complexType>
                                    <sequence>
                                        <element name="dname"  type="string"></element>
                                        <element name="address"  type="string"></element>
                                    </sequence>
                                </complexType>
                            </element>
                        </sequence>
                        <attribute name="no" type="string" use="required"></attribute>
                    </complexType>
                </element>
            </sequence>
        </complexType>
    </element>
</schema>
```

## 4 Java解析XML

### 4.1  DOM文档对象模型

![图片](/images/023_01_12.png)

### 4.2 dom4j

![图片](/images/023_01_13.png)

#### 4.2.1 dom4j读XML案例

```java
package com.jessekkk.dom4j;

import java.util.List;

import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

public class HrReader {
    public void readXml(){
        String file = "d:/hr.xml";
        //SAXReader类是读取XML文件的核心类，用于将XML解析后以”树“的形式保存在内存中
        SAXReader reader = new SAXReader();
        try {
            Document document = reader.read(file);
            //获取XML文档的根节点，即hr标签
            Element root = document.getRootElement();
            //elements方法用于获取所有指定的标签的集合
            List<Element> employees = root.elements("employee");
            for(Element employee : employees){
                //element方法用于获取唯一的子节点对象
                Element name = employee.element("name");
                String empName = name.getText();//getText()用于获取标签文本
                System.out.println(empName);
                System.out.println(employee.elementText("age"));
                System.out.println(employee.elementText("salary"));
                Element department = employee.element("department");
                System.out.println(department.element("dname").getText());
                System.out.println(department.element("address").getText());
                Attribute att = employee.attribute("no");
                System.out.println(att.getText());
            }

        } catch (DocumentException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        HrReader reader = new HrReader();
        reader.readXml();
    }
}
```

#### 4.2.2 dom4j写XML案例

```java
package com.jessekkk.dom4j;

import java.io.FileOutputStream;
import java.io.OutputStreamWriter;
import java.io.Writer;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

public class HrWriter {
    public void writeXml() {
        String file = "d:/hr.xml";
        SAXReader reader = new SAXReader();
        try {
            Document document = reader.read(file);
            Element root = document.getRootElement();
            Element employee = root.addElement("employee");
            employee.addAttribute("no", "3311");
            Element name = employee.addElement("name");
            name.setText("李铁柱");
            employee.addElement("age").setText("37");
            employee.addElement("salary").setText("3600");
            Element department = employee.addElement("department");
            department.addElement("dname").setText("人事部");
            department.addElement("address").setText("XX大厦-B105");
            Writer writer = new OutputStreamWriter(new FileOutputStream(file), "UTF-8");
            document.write(writer);
            writer.close();
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        HrWriter writer = new HrWriter();
        writer.writeXml();
    }
}
```

## 5 XPath路径表达式

![图片](/images/023_01_14.png)

### 5.1 基本表达式

![图片](/images/023_01_15.png)

![图片](/images/023_01_16.png)

### 5.2 谓语表达式

![图片](/images/023_01_17.png)

### 5.3 Jaxen介绍

![图片](/images/023_01_18.png)

#### 5.3.1 Example

```java
package com.jessekkk.dom4j;

import java.util.List;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.SAXReader;

public class XPathTestor {
    public void xpath(String xpathExp) {
        String file = "d:/hr.xml";
        SAXReader reader = new SAXReader();
        try {
            Document document = reader.read(file);
            List<Node> nodes = document.selectNodes(xpathExp);
            for(Node node : nodes) {
                Element emp = (Element)node;
                System.out.println(emp.attributeValue("no"));
                System.out.println(emp.elementText("name"));
                System.out.println(emp.elementText("age"));
                System.out.println(emp.elementText("salary"));
                System.out.println("=======================");
            }
        } catch (DocumentException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        XPathTestor testor = new XPathTestor();
//        testor.xpath("/hr/employee");
//        testor.xpath("//employee");
//        testor.xpath("//employee[salary<4000]");
//      testor.xpath("//employee[name='李铁柱']");
//      testor.xpath("//employee[@no=3309]");
//        testor.xpath("//employee[1]");
//        testor.xpath("//employee[last()]");
//        testor.xpath("//employee[position()<3]");
        testor.xpath("//employee[1] | //employee[3]");
    }
}
```
