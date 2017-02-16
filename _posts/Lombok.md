---
title: Lombok
date: 2016-08-20 20:38:30
tags: 
- Android
- 插件
categories: 移动开发
---

# 下载安装
lombok plugin插件
![](http://7xruee.com1.z0.glb.clouddn.com/lombok1.png)
# build.gradle文件
添加依赖
![](http://7xruee.com1.z0.glb.clouddn.com/lombok2.png)
```groovy
compile 'org.glassfish:javax.annotation:10.0-b28'
compile 'org.projectlombok:lombok:1.16.8'
```
# model类
使用@Data
如图，get、set、equals、hashCode方法以及全部自动生成
![](http://7xruee.com1.z0.glb.clouddn.com/lombok3.png)

使用@Getter  @Setter
<!--more-->
# 小记
今天和谢总查Json解析的bug，单步调试发现获取的数据是对的，但是就解析之后那个字段总是null，fastJson反序列化应该注意的地方也注意了（字段要和接口中的一致），但是还是不行，最终问题找到了，是该字段的setter方法写错了。。。这点神坑。。极其小的bug导致了效率低下，耗时费力。所以用Lombok，至少可以保证马虎出错，lombok化，代码的确会精简很多，将思路集中在业务上，这也算是一次修行吧，谨记。
