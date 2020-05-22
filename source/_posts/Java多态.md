---
title: Java多态
categories:
  - Java入门
  - 02_Java面向对象
tags:
  - imooc
abbrlink: 52601
date: 2019-12-09 21:34:11
---

:star2:本文是Java多态的学习总结:star2:

<!-- more -->

## 1 多态的分类

![图片](/images/012_05_01.png)

## 2 向上和向下类型转换与instanceof运算符

![图片](/images/012_05_02.png)

注意：

- 向上转型
1、父类引用指向子类实例，可以调用子类重写父类的方法以及父类派生的方法，无法调用子类独有的方法
2、小类转型为大类
3、父类中的静态方法无法被子类重写，所以向上转型之后，只能调用父类原有的静态方法

- 向下转型
1、子类引用指向父类对象，此处必须强转，可以调用子类特有的方法
2、instanceof运算符：返回true/false

## 3 抽象类和抽象方法

![图片](/images/012_05_03.png)

## 4 接口

![图片](/images/012_05_04.png)

```java
/**
 * 接口类的访问权限只能为：public和默认
 * Created by Kong on 2019/12/9.
 */
public interface INet {
    /**
     * 接口中抽象方法可以不写abstract关键字
     * 方法访问修饰符默认为public
     */
    void network();

    //接口可以包含常量，默认public static final
    int TEMP = 20;

    //default：默认方法，可以带方法体  jdk1.8后新增
    //可以在实现类中重写，并可以通过接口的引用调用
    //重写调用格式：INet.super.connection();
    default  void connection()
    {
        System.out.println("我是接口中的默认连接");
    }

    //static：静态方法，可以带方法体
    //不可以在实现类中重写，可以用接口名调用
    static void stop()
    {
        System.out.println("我是接口中的静态方法");
    }
}
```

![图片](/images/012_05_05.png)

## 5 内部类

### 5.1 成员内部类

```java
//外部类
public class Person {
    int age;

    public Heart getHeart()
    {
        new Heart().temp = 12;
        return new Heart();
    }

    public void eat()
    {
        System.out.println("人会吃东西");
    }

    //成员内部类
    /**
     * 1、内部类在外部使用时，无法直接实例化，需要借由外部类信息才能完成实例化
     * 2、内部类的访问修饰符，可以任意，但是访问范围会受到影响
     * 3、内部类可以直接访问外部类的成员，如果出现同名属性，优先访问内部类中的定义的
     * 4、可以使用外部类.this.成员的方式，访问外部类中同名的信息
     * 5、外部类访问内部类信息，需要通过内部类实例，无法直接访问
     * 6、内部类编译后.class文件 命名：外部类$内部类.class
     * 7、非静态内部类无静态成员
     */
    public class Heart {
        int age = 13;
        int temp =22;
        public String beat()
        {
            eat();
            return Person.this.age + "心脏在跳动";
        }
    }
}
}
```

```java
public class PersonTest {
    public static void main(String[] args) {
        Person lili = new Person();
        lili.age = 12;

        //获取内部类对象实例，方式1：new 外部类.new 内部类
        Person.Heart myHeart = new Person().new Heart();
        System.out.println(myHeart.beat());

        //获取内部类对象实例，方式2：new 外部类对象.new 内部类
        myHeart = lili.new Heart();
        System.out.println(myHeart.beat());

        //获取静态内部类对象实例,方式3：外部类对象.获取方法
        myHeart = lili.getHeart();
        System.out.println(myHeart.beat());
    }
}
}
```

### 5.2 静态内部类

```java
//外部类
public class Person {
//    public static int age = 12;
    int age;

    public Heart getHeart()
    {
        return new Heart();
    }

    public static void eat()
    {
        System.out.println("人会吃东西");
    }

    //静态内部类
    /**
     * 1、静态内部类中，只能直接访问外部类的静态成员，如果需要调用非静态成员，可以通过对象实例
     * 2、静态内部类对象实例时，可以不依赖外部类对象
     * 3、可以通过外部类.内部类.静态成员的方式，访问内部类中的静态成员
     * 4、当内部类属性与外部类属性同名时，默认直接调用内部类的成员；
     *    如果需要访问外部类中的静态属性，则可以通过外部类.属性的方式
     *    如果需要访问外部类中的非静态属性，则可以通过 new 外部类().属性的方式
     * 5、外部内可通过内部类.成员的方式访问静态内部类
     */
    public static class Heart
    {
        public static int age = 13;
        public static void say()
        {
            System.out.println("hello");
        }

        public String  beat()
        {
            eat();
//            return Person.age + "岁的心脏在跳动";
            return new Person().age + "岁的心脏在跳动";
        }
    }
}
```

```java
public class PersonTest {
    public static void main(String[] args) {
        Person lili = new Person();
        lili.age = 12;

        //获取静态内部类对象的实例
        Person.Heart myHeart = new Person.Heart();
        System.out.println(myHeart.beat());

        //访问内部类中的静态成员
        Person.Heart.say();

    }
}
```

### 5.3 方法内部类

```java
public class Person {
    public static int age;

    public Object getHeart() {
        //方法内部类
        /**
         * 1、定义在方法内部，作用范围也在方法内
         * 2、和方法内部成员使用规则一样，class前面不可以添加public、private、protected、static
         * 3、类中不能包含静态成员
         * 4、类中可以包含final、abstract修饰的成员
         */
        class Heart {
            public int age = 13;

//            public static void say() {
//                System.out.println("hello");
//            }

            public void eat() {}

            public String beat() {
                new Person().eat();
                return Person.age + "岁的心脏在跳动";
            }
        }
        return new Heart().beat();
    }

    public void eat() {
        System.out.println("人会吃东西");
    }
}
```

```java
public class PersonTest {
    public static void main(String[] args) {
        Person lili = new Person();
        lili.age = 12;

        System.out.println(lili.getHeart());
    }
}
```

### 5.4 匿名内部类

```java
public abstract class Person {
    private String name;

    public Person() {}

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public abstract void read();
}
```

```java
public class PersonTest {
    public void getRead(Person person)
    {
        person.read();
    }

    public static void main(String[] args) {
        PersonTest test = new PersonTest();
        //匿名内部类
        /**
         * 1、匿名内部类没有类型名称、实例对象名称
         * 2、编译后的文件命名：内部类$数字.class
         * 3、无法使用private、public、protected、abstract、static修饰
         * 4、无法编写构造方法，可以添加构造代码块
         * 5、不能出现静态成员
         * 6、匿名内部类可以实现接口也可以继承父类，但是不可兼得
         */
        test.getRead(new Person()
        {
            @Override
            public void read() {
                System.out.println("男生喜欢看科幻类的书籍");
            }
        });
        test.getRead(new Person()
        {

            @Override
            public void read() {
                System.out.println("女生喜欢读言情小说");
            }
        });
    }
}
```
