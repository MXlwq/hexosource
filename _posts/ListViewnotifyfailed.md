---
title: ListViewnotify无效
date: 2017-01-07 22:15:27
tags:
- LisView
- Android
categories: 
- 移动开发
---
可能的三个原因：
数据源没有更新；
数据源更新了，但是它指向新的引用；
数据源更新了，但是adpter没有收到消息通知；

原因2示例：
```java

list = new String[]{"listView item"};
adapter = new ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,list);
listView.setAdapter(adapter);
list = new String[]{"new listView item"};
adapter.notifyDataSetChanged();

```
<!--more-->
Arrayadapter相关源码：
```java
public ArrayAdapter(Context context, int textViewResourceId, T[] objects) {
    init(context, textViewResourceId, 0, Arrays.asList(objects));
}
Arrays：
public static <T> List<T> asList(T... array) {
    return new ArrayList<T>(array);//注意这里的ArrayList不是常见的那个ArrayList，而是Arrays的一个内部类。。
}
```