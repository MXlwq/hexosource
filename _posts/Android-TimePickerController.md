---
title: Android TimePicker控件
date: 2016-04-01 10:36:48
tags:
- Android
- View
categories:
- 移动开发
---

在Android中使用TimePicker（时间选择器），可以进行时间的快速调整，此类定义如下：

java.lang.Object

   	->android.view.View

    	->android.view.ViewGroup

    		->android.widget.FrameLayout

    			->android.widget.TimePicker.

## 常用方法:

|方法|描述|
|:---|:---|
|public Integer getCurrentHour()|返回当前设置的小时|
|public Integer getCurrentMinute()|返回当前设置的分钟|
|public boolean is24HourView()|判断是否是24小时制|
|public void setCurrentHour(Integer currentHour)|设置当前的小时数|
|public void setCurrentMinute(Integer currentMinute)|设置当前的分钟|
|public void setEnabled(boolean enabled)|设置是否可用|
|public void setIs24HourView(Boolean is24HourView)|设置时间为24小时制|

```java
//将TimePicker时间设置为10点01分
timePicker.setCurrentHour(10);
timePicker.setCurrentMinute(01);
获取TimePicker的时间和分钟数
hour = timePicker.getCurrentHour();
minute = timePicker.getCurrentMinute();
```

