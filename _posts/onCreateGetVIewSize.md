---
title: onCreate中获取控件大小
date: 2016-09-07 20:38:23
tags: 
- View
- Android
categories: 移动开发
---
# 问题
## 现象
在onCreate里面，无法直接通过View.getHeight()或者View.getWith()获得控件的长宽值的，始终为0。
## 原因
在onCreate中，控件并没有画好，换句话说，等onCreate方法执行完了，控件才会被度量(onMeasure)

# 解决方案
## 手动测量
```java
    int w = View.MeasureSpec.makeMeasureSpec(0,
            View.MeasureSpec.UNSPECIFIED);
    int h = View.MeasureSpec.makeMeasureSpec(0,
            View.MeasureSpec.UNSPECIFIED);
    view.measure(w, h);
    int height = view.getMeasuredHeight();
    int width = view.getMeasuredWidth();
```
## 注册一个ViewTreeObserver的监听回调
在每次监听前remove前一次的监听，避免重复监听。
```java
    ViewTreeObserver vto = imageView.getViewTreeObserver();
    vto.addOnPreDrawListener(new ViewTreeObserver.OnPreDrawListener() {
        public boolean onPreDraw() {
            vto.removeOnPreDrawListener(this);
            int height = imageView.getMeasuredHeight();
            int width = imageView.getMeasuredWidth();
            return true;
        }
    });
```
<!--more-->
## 全局的布局改变监听器
```java
    ViewTreeObserver vto = mProgressBar.getViewTreeObserver();
    vto.addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
        @Override
        public void onGlobalLayout() {
            mProgressBar.getViewTreeObserver().removeGlobalOnLayoutListener(this);
            imageView.getHeight();
            imageView.getWidth();
        }
    });
```
# 修改控件的大小

## 方法
使用getLayoutParams() 和setLayoutParams()方法
## 示例
```java
LinearLayout.LayoutParams linearParams = (LinearLayout.LayoutParams) aaa.getLayoutParams(); 
// 取控件aaa当前的布局参数
linearParams.height = 256; // 当控件的高强制设成256象素
view.setLayoutParams(linearParams); // 使设置好的布局参数应用到控件view
```
## 原理
* getLayoutParams()和setLayoutParams()都是控件基类view的public方法，在外部也可以直接调用。
* 由于LayoutParams一般是在加入容器中设置的，所以容易混淆所指定的布局属性究竟是保存在容器中，还是控件本身的属性，答案是控件本身。但是在设置时还是要注意布局属性与容器种类密切相关。
```

# 补充
如果控件没有绘制完成，是无法获取到起宽和高，并监听事件响应的，因此可以通过post来做
mProgressBar.post(new Runnable() {
    @Override
    public void run() {
        Log.e(TAG, mProgressBar.getWidth() + ";" + mProgressBar.getHeight());
    }
});
