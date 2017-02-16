---
title: Java数组小结
date: 2016-03-03 21:37
tags: 
- Java
- 数组
categories: 语言基础
---

1. 两种数组类型数组名的不同

```java
char[] c={'中','国'};
System.out.println(c);
//字符数组输出数组名会是将数组元素依次排列输出  中国
int[] i=new int[]{1,2};
System.out.println(i);
//整形数组输出数组名会是输出 [I@2a139a55  即数组类型+hash码
```
   
2. 快速输出数组内容的方式
```java
//需要import java.util.Arrays;
//可以代替for循环依次输出,以[ , , ]的形式输出
System.out.println(Arrays.toString(c));
```
    
3. 拷贝数组的两种方式
```java
//拷贝数组不同for循环的两种方式
Arrays.copyOf(original,newLength);
//在这里可以用如：
char c=new char[]{};//创建一个空的字符数组
c=Arrays.copyOf(c,c.length);//将数组的长度增加1
//类似可以用arrycopy的形式拷贝数组，但是需要5个参数，分组名，将原数组从srcPos到srcPos+length-1复制到dest数组的destPos到destPos+length-1位置；
System.arraycopy(src, srcPos, dest, destPos, length);
```
    
    