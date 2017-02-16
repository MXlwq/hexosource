---
title: Inflate的三个参数理解
date: 2017-02-12 16:45:09
tags: 
- Android
- Inflate
categories: 移动开发
---
[原文链接](http://blog.csdn.net/u012702547/article/details/52628453)

# 使用
方式1. LayoutInflater layoutInflater = LayoutInflater.from(context);  
方式2. LayoutInflater layoutInflater = (LayoutInflater) context  
        .getSystemService(Context.LAYOUT_INFLATER_SERVICE);

        layoutInflater.inflate(resourceId, root);  
# 参数
```java
inflate(int resourceId,ViewGroup root);
inflate(int resource, ViewGroup root, boolean attachToRoot)  
```
## 规则
1. 如果root为null，attachToRoot将失去作用，设置任何值都没有意义。
2. 如果root不为null，attachToRoot设为true，则会给加载的布局文件的指定一个父布局，即root。
3. 如果root不为null，attachToRoot设为false，则会将布局文件最外层的所有layout属性进行设置，当该view被添加到父view当中时，这些layout属性会自动生效。
4. 在不设置attachToRoot参数的情况下，如果root不为null，attachToRoot参数默认为true。

# 理解

对于规则1的情况，不管怎么调整加载的控件的layout_width和layout_height都不生效。
但是事实上我们经常使用layout_width和layout_height来设置View的大小，并且一直都能正常工作，就好像这两个属性确实是用于设置View的大小的。而实际上不是，它们其实是用于设置View在布局中的大小的，也就是说，首先View必须存在于一个布局中，之后如果将layout_width设置成match_parent表示让View的宽度填充满布局，如果设置成wrap_content表示让View的宽度刚好可以包含其内容，如果设置成具体的数值则View的宽度会变成相应的数值。这也是为什么这两个属性叫作layout_width和layout_height，而不是width和height。
看到这些可能有些人会很奇怪，为什么经常在Activity的setContentView的根布局属性设置大小是可以的呢？其实
Android会自动在布局文件的最外层再嵌套一个FrameLayout，所以layout_width和layout_height属性才会有效果，这一点使用Android 提供的HierarchyViewer工具就能看到，或者通过findViewById()方法拿到了Activity布局中最外层的对象，然后调用getParent()打印出来也能看到啦。