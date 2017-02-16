---
title: 自定义spinner
date: 2016-09-13 19:36:34
tags: 
- View
- Android
categories: 
- 移动开发
---
# 方法
## 描述
* 两个layout view
两个layout view分别负责显示spinner
![](http://7xruee.com1.z0.glb.clouddn.com/spinner01.png)
和点击后显示的item列表布局
![](http://7xruee.com1.z0.glb.clouddn.com/spinner02.png)
* 重写adapter方法，一般是继承baseadapter重写 getView()和getdropdownview这两个方法。getview修改spinner中显示的样式（layout布局），getdropdownview决定了item中显示的样式。

<!--more-->
## 另一种简单的方式
```java
private void configureScaleSpinner() {
    final String[] scaleSpinner = new String[]{"原始比例", "智能全屏", "全屏拉伸"};
    //自定义
    ArrayAdapter<String> adapter = new ArrayAdapter<>(MainActivity.this, R.layout.spinner_item, scaleSpinner);
    adapter.setDropDownViewResource(R.layout.spinner_dropdown_item);
    mScaleSpinner.setAdapter(adapter);
    mScaleSpinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
        @Override
        public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
        	//TODO
        }
        @Override
        public void onNothingSelected(AdapterView<?> parent) {
        }
    });
}
```
**spinner_item**
```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_gravity="center"
    android:background="@drawable/shape_main_button"
    android:gravity="center"
    android:textColor="@color/category_list_item_selected">
</TextView>
```
**spinner_dropdown_item**
```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@android:color/black"
    android:gravity="center"
    android:paddingBottom="@dimen/p_5"
    android:paddingTop="@dimen/p_5"
    android:textColor="@color/umeng_fb_white">
</TextView>
```

# 效果
![](http://7xruee.com1.z0.glb.clouddn.com/videoView.jpg)