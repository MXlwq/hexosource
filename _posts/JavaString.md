---
title: 对Java的String类的掌握还是不够
date: 2016-03-16 13:50:53
tags: 
- Java
- String
categories: 语言基础
---

20160318添加
[String类和其他基本数据类型包装类转换](http://liwenquan.top/2016/03/18/String%E7%B1%BB%E5%92%8C%E5%85%B6%E4%BB%96%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E5%8C%85%E8%A3%85%E7%B1%BB%E8%BD%AC%E6%8D%A2/) 
[String运用正则表达式](http://liwenquan.top/2016/03/19/String%E8%BF%90%E7%94%A8%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F/) 

昨晚改FireFight的代码，有用到了处理字符串，这点自己以前没有掌握好，再加上Java JTextArea的文字处理，把这个问题想复杂了，要实现的是只处理JTextArea中的最后一行，并且能够识别输入的换行，并将换行删除，用到了[split方法](http://liwenquan.top/2016/03/02/Java%20String类的方法/#j、split方法)
<!--more-->,之后想这匹配字符串吧，刚开始想用匹配，在网上找到用正则表达式matches，结果被坑了一个多小时(还是因为自己没弄清)。。。没有查文档的习惯，其实这个办法是可以的。而且有很多其他解决方法，比如用charAt(s.length()-1)=="\n"判断，或者其他的。不过contains好像不能用，因为JTextArea有很多换行，这里只判断最后的。
最后附上一张ASCII表![ASCII表](http://7xruee.com1.z0.glb.clouddn.com/001.png)