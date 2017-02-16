---
title: Java I/O操作
date: 2016-04-21 22:25:35
tags: 
- Java
- I/O
categories: 语言基础
---

Java的IO类操作主要包括如下几类

字节操作流：OutputStream、InputStream
字符操作流：Reader、Writer
对象序列化：serializable

## 字节/字符操作流
 1. 字节输出流OutputStream
 ![pic02](http://7xruee.com1.z0.glb.clouddn.com/pic02.jpg)
 2. 字节输入流InputStream
 ![pic03](http://7xruee.com1.z0.glb.clouddn.com/pic03.jpg)
 3. 字符输出流Write
 ![pic04](http://7xruee.com1.z0.glb.clouddn.com/pic04.jpg)
 4. 字符输入流Reader
 ![pic05](http://7xruee.com1.z0.glb.clouddn.com/pic05.jpg)
 5. 字节流和字符流的区别（重点）
	字节流没有缓冲区，是直接输出的，而字符流是输出到缓冲区的。因此在输出时，字节流不调用colse()方法时，信息已经输出了，而字符流只有在调用close()方法关闭缓冲区时，信息才输出。要想字符流在未关闭时输出信息，则需要手动调用flush()方法。
 6. 转换流：在IO中还存在一类是转换流，将字节流转换为字符流，同时可以将字符流转化为字节流。
 
 ```java
OutputStreamWriter(OutStream out);//将字节流以字符流输出。
InputStreamReader(InputStream in);//将字节流以字符流输入。
```

 7. 打印流 PrintStream
	在操作中要求输出信息时，可以采用PrintStream进行输出，它包括PrintWrite和PrintReader

<!--more-->

## File类
```java
public class File extends Object implements Serizliable Comparable<File> 
```

从定义看，File类是Object的直接子类，同时它继承了Comparable接口可以进行数组的排序。

File类的操作包括文件的创建、删除、重命名、得到路径、创建时间等，以下是文件操作常用的函数。
![pic01](http://7xruee.com1.z0.glb.clouddn.com/pic01.jpg)
File类的操作：

 1. 创建文件，注意File.separator可以解决跨操作系统的问题。
 2. 文件的类型函数
       file.isFile(); //判断是不是文件
       file.isDirectory();//判断是不是目录
 3. 列出目录的内容
 ```java
public String[] list();//列出所有文件名和目录名
public File[] listFiles();//列出所有文件和目录
```

## 对象序列化

对象序列化是指将一个对象可以转化为二进制的byte流，可以以文件的方式进行保存。
将对象保存在文件的操作叫做对象的序列化操作。
将对象从文件中恢复的操作叫做反序列化操作。
一个对象如果要能序列化，它必须继承Serizliable。在实现序列化是则需要ObjectOurputStream完成，而需要反序列化时则采用ObjectInputStream。
![pic06](http://7xruee.com1.z0.glb.clouddn.com/pic06.jpg)
## 内存流

在项目的开发过程中，有时希望只产生临时文件，将信息输出的内存中，此时会用到内存流，内存流基本方法如下：
![pic07](http://7xruee.com1.z0.glb.clouddn.com/pic07.jpg)

## System类对IO的支持
![pic08](http://7xruee.com1.z0.glb.clouddn.com/pic08.jpg)

## 缓存读取
![pic09](http://7xruee.com1.z0.glb.clouddn.com/pic09.jpg)