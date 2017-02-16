---
title: Java类集框架
date: 2016-04-18 17:52:32
tags:
- Java
- 类集框架
categories: 语言基础
---

## java集合框架的内容

1. java集合框架的层次结构
2. 使用Collection接口定义的公用方法对集合和线性表操作
3. 使用Iterator接口遍历集合
4. 使用JDK的增强for循环替代迭代Iterator进行集合遍历
5. 熟悉Set接口，了解何时及如何使用HashSet，LinkedHashSet或TreeHashSet来存储元素
6. 使用Comparator接口来比较元素
7. 熟悉List接口，了解何时以及如何使用ArrayList或者LinkedList来存储元素
8. 区分Vector与ArrayList，并了解如何使用Vector和Stack
9. 使用JDK1.5的一般类型来简化程序设计
10. 理解Collection和Map的区别，知道何时及如何使用HashMap，LinkedHashMap，TreeHashMap来存储
11. 使用Collections类中的静态方法
12. 使用Arrays类中的静态方法

----

java集合架构支持3种类型的集合：规则集（Set），线性表（List），图（Map），分别定义在Set，List，Map中。Set实例存储一组互不相同的元素（集合），List实例存储一组顺序排列的元素（表），Map存储一组 Key--Value的映射

总的架构如下，非常重要，包含继承关系，实现的分类，一目了然：

**Collection接口：**

	Set接口：
		HashSet具体类
		LinkedHashSet具体类
		TreeSet具体类

	List接口：　
		ArrayList具体类
		LinkedList具体类
		Vector具体类
		Stack具体类


**Map接口：**

	HashMap类
	LinkedHashMap类
	TreeMap类　　　　　　　　　　　　
　　　　
-----



1. Collection接口，它是处理对象集合的根接口，提供了一些公用方法，size，Iterator，add，remove

2. Set和List接口都扩展自Collection，Set不允许重复。List就像一个表，可以重复，元素在表里有序放着。

3. Set接口的3种实现：
　　HashSet的对象必须实现hashCode方法，javaAPI大多数类实现了hashCode方法。
　　LinkedHashSet实现了对HashSet的扩展，支持规则集内元素的排序，在HashSet中元素是没有顺序的，而在LinkedHashSet中，可以按元素插入集合的顺序进行提取
　　TreeSet保证集中的元素是有序的，有2种方法可以实现对象之间的可比较性：
1. 添加到TreeSet的对象实现了Comparable接口；
2. 给规则集的元素指定一个比较器（Comparator）

<!--more-->
使用方式：

如果希望按照元素插入集合的顺序进行提取元素，用LinkedHashSet，它的元素按添加的顺序存储
如果没有上述需求，应该用HashSet，它的效率比LinkedHashSet高
LinkedHashSet只是按照添加的的先后顺序在存储时保持顺序，要给集合元素添加顺序属性，需要使用TreeSet（集合元素有排序关系）。

4. List的几种实现

最重要的的当然是ArrayList（不同步）和LinkedList，一个使用数组实现的动态扩展容量的list，一个是链式实现的list。还有就是Vector（同步）类，它除了包含访问和修改向量的同步方法之外，跟ArrayList一样。

5. Map

Map是映射，跟前面的Set和List有本质的区别。

散列图HashMap，链式散列图LinkedHashMap，树形图TreeHashMap是映射的3种实现。

HashMap：效率高
LikedHashMap：按照添加顺序存储，可以按添加顺序取出
TreeHashMap：排序性

-----

## Collections类和Arrays类：

Collections类提供了许多静态的方法来管理集合，包括：Sort（排序）、混排（Shuffling）、反转（Reverse）、Copy（复制）

Arrays类：提供了对数组排序，查找，比较，填充元素的各种静态方法。
