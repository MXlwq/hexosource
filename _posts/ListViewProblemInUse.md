---
title: ListView使用中的问题
date: 2016-07-26 18:13:03
tags: 
- Android
categories: 
- 移动开发
thumbnail: http://7xruee.com1.z0.glb.clouddn.com/android.jpg
---


触发Button的点击事件，但是item的点击事件并不会被触发，也就是说，Button控件抢夺了item的焦点事件，使得item不能触发相应的点击事件，那么，如果我们既想触发Button的点击事件，又想触发item的点击事件，我们应该怎么做呢？

这里有三种解决方案

1. 将ListView中的Item布局中的子控件focusable属性设置为false
2. 在getView方法中设置button.setFocusable(false)
3. 设置item的根布局的属性android:descendantFocusability="blocksDescendant"

这三种方法都是为了让Button等控件不能获取焦点，从而使得item可以响应点击事件。

第三种方法使用起来相对方便，因为它是将item布局中的其他所有控件都设置为不能获取焦点。
android:descendantFocusability属性共有三个取值，分别为
beforeDescendants：viewgroup会优先其子类控件而获取到焦点
afterDescendants：viewgroup 只有当其子类控件不需要获取焦点时才获取焦点
blocksDescendants：viewgroup 会覆盖子类控件而直接获得焦点