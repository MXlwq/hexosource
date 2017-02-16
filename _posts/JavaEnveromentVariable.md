---
title: Java环境变量配置
date: 2015-08-20 18:09:17
tags: Java
categories: 语言基础
---

## JDK安装目录说明

| 目录名 | 说明|
|:---:|:----:|
| bin | 一些命令行工具，包括Javac命令 |
| demo | 一些Java的示例 |
| include | 编写JNI等需要的C头文件 |
| jre | Java运行时环境 |
| lib | 出去jre中包含的类库，JDK额外的一些类库 |
| src.zip | 部分JDK的源码 |

\* *JNI*：Java Native INterface
<!--more-->

## 配置环境变量
![添加环境变量JAVA_HOME](http://7xruee.com1.z0.glb.clouddn.com/0001.png)
### 添加环境变量JAVA_HOME
将JAVA的安装目录加入（这样做的目的只是为了更加了灵活，以后在移动的Java安装文件的位置时只需要修改JAVA_HOME即可）
如下图：
![添加环境变量JAVA_HOME](http://7xruee.com1.z0.glb.clouddn.com/0002.png)
### 修改环境变量PATH
用%JAVA_HOME%表示JAVA_HOME环境变量的目录位置，将%JAVA_HOME%\bin加入（注意：**Windows中的路径符都是反斜杠\ ，Linux的是/**）
如下图：
![修改环境变量PATH](http://7xruee.com1.z0.glb.clouddn.com/0003.png)
### 设置环境变量CLASSPATH
首先要有表示当前目录的**.**，其次是两个JRE中的jar包，分别是%JAVA_HOME%\lib\dt.jar和%JAVA_HOME%\lib\tools.jar，如果是win7及以下的系统，主要要用英文分号;将三个量隔开，同时在末尾加上英文分号;以隔开系统的环境变量，避免出错
如：
> .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;

如下图：
![设置环境变量CLASSPATHE](http://7xruee.com1.z0.glb.clouddn.com/0004.png)