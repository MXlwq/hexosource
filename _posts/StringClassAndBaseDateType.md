---
title: String类和其他基本数据类型包装类转换
date: 2016-03-18 18:14:33
tags: 
- Java
categories: 语言基础
---

Java语言是一个面向对象的语言，但是Java中的基本数据类型却是不面向对象的，这在实际使用时存在很多的不便，为了解决这个不足，在设计类时为每个基本数据类型设计了一个对应的类进行代表，这样八个和基本数据类型对应的类统称为包装类(Wrapper Class)，有些地方也翻译为外覆类或数据类型类。包装类均位于java.lang包，包装类和基本数据类型的对应关系如下表所示：
**包装类对应表**

| 基本数据类型 | 包装类|
| --------- |:--------:|
| byte | Byte |
| boolean | Boolean |
| short | Short |
| char | Character |
| byte | Byte |
| int | Integer |
| long | Long |
| double | Double |

<!--more-->
## 介绍
在这八个类名中，除了Integer和Character类以后，其它六个类的类名和基本数据类型一直，只是类名的第一个字母大写即可。
对于包装类说，这些类的用途主要包含两种：
a、作为和基本数据类型对应的类类型存在，方便涉及到对象的操作。
b、包含每种基本数据类型的相关属性如最大值、最小值等，以及相关的操作方法。
由于八个包装类的使用比较类似，下面以最常用的Integer类为例子介绍包装类的实际使用。
## 实现int和Integer类之间的转换
在实际转换时，使用Integer类的构造方法和Integer类内部的intValue方法实现这些类型之间的相互转换，实现的代码如下：
```java
int n = 10;
Integer in = new Integer(100);
//将int类型转换为Integer类型
Integer in1 = new Integer(n);
//将Integer类型的对象转换为int类型
int m = in.intValue();
```
**装箱**
把基本数据类型用相对于的引用类型包起来，使他们可以具有对象的特质。
**拆箱**
将Integer等这样的引用类型的对象重新简化为值类型的数据。

## Integer类内部的常用方法
在Integer类内部包含了一些和int操作有关的方法，下面介绍一些比较常用的方法：
## **将String转变为基本数据类型**
parseInt方法
```java
public static int parseInt(String s)
//该方法的作用是将数字字符串转换为int数值。
String s = "123";
int n = Integer.parseInt(s);
```
则int变量n的值是123，该方法实际上实现了字符串和int之间的转换，如果字符串都包含的不是都是数字字符，则程序执行将出现异常。
另外一个parseInt方法：
```java
public static int parseInt(String s, int radix)
则实现将字符串按照参数radix指定的进制转换为int，使用示例如下：
//将字符串"20"按照十进制转换为int，则结果为120
int n = Integer.parseInt("120",10);
//将字符串”12”按照十六进制转换为int，则结果为18
int n = Integer.parseInt("12",16);
//将字符串”ff”按照十六进制转换为int，则结果为255
int n = Integer.parseInt("ff",16);
```
这样可以实现更灵活的转换。

### **将基本数据类型转变为String类型**
#### String.valueOf()方法
```java
int x=100;
Stirng str=String.value(x);
System.out.println(x);
```
#### 任何数据类型遇见String之后自动变成字符串。
```java
int x=100;
String str=x+"";
System.out.println(str);
```

## 新特性
JDK自从1.5(5.0)版本以后，就引入了自动拆装箱的语法，也就是在进行基本数据类型和对应的包装类转换时，系统将自动进行，这将大大方便程序员的代码书写。使用示例代码如下：
```java
//int类型会自动转换为Integer类型
int m = 12;
Integer in = m;
//Integer类型会自动转换为int类型
int n = in;
所以在实际使用时的类型转换将变得很简单，系统将自动实现对应的转换。
```