---
title: Java输入输出流
categories:
  - Java入门
  - 03_Java常用工具
tags:
  - muke
abbrlink: 64267
date: 2019-12-24 12:45:54
---

:star2:本文是Java输入输出流的学习总结:star2:

<!-- more -->

## 1 File类的使用

### 1.1 三种文件构造方式

```java

//File file1 = new File("C:\\Users\\Kong\\Desktop\\kong\\xing.txt");

//File file1 = new File("C:\\Users","Kong\\Desktop\\kong\\xing.txt");

File file =new File("C:\\Users");
File file1 = new File(file,"Kong\\Desktop\\kong\\xing.txt");
```

### 1.2 常用API

```java
bollean es = file.exists();
file.createNewFile();
file.mkdir();
file.mkdirs();
String na = file.getName()
String pa = file.getParent()
boolean cd = file.canRead()
boolean we = file.canWrite()
String gh = file.getPath()
String ah = file.getAbsolutePath()
```

## 2 字节流

- 二进制文件传输（图片）
- 注意flush()和close()

![图片](/images/013_06_01.png)

![图片](/images/013_06_02.png)

### 2.1 方式一

![图片](/images/013_06_03.png)

### 2.2 方式二

![图片](/images/013_06_04.png)

## 3 字符流

- 字符文件传输及字节字符转换
- Reader和Writer
- 注意构造器包装类型

### 3.1 方式一

![图片](/images/013_06_06.png)

### 3.2 方式二

![图片](/images/013_06_05.png)

## 4 序列化（Serializable）

```java
public class GoodsTest {
    public static void main(String[] args) {
        //Goods类要实现Serializable
        Goods goods1 = new Goods("gd001","电脑",3000);
        try {
            FileOutputStream fos = new FileOutputStream("kong.txt");
            ObjectOutputStream oos = new ObjectOutputStream(fos);
            FileInputStream fis = new FileInputStream("kong.txt");
            ObjectInputStream ois = new ObjectInputStream(fis);

            //将对象信息写入文件
            oos.writeObject(goods1);
            oos.writeBoolean(true);
            oos.flush();

            //读对象信息
            Goods goods = (Goods)ois.readObject();
            System.out.println(goods);
            System.out.println(ois.readBoolean());

            fis.close();
            ois.close();
            fos.close();
            oos.close();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e)
        {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
