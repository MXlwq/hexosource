---
title: AndroidTextViewMarquee
date: 2016-12-02 09:49:24
tags:
---

# 跑马灯效果

```xml
android:ellipsize="marquee"
android:focusable="true" <!--获取焦点-->
android:focusableInTouchMode="true" <!--TouchMode(手机设备)获取焦点-->
android:marqueeRepeatLimit="marquee_forever"
android:singleLine="true" <!--不能用maxLine="1"代替-->
```

如果想要实现同时出现过个TextView跑马灯，可以重写TextView
<!--more-->
```java
public class FocusTextView extends TextView {
    public FocusTextView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }

    public FocusTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public FocusTextView(Context context) {
        super(context);
    }

    public void setTextSize(float textSize) {
        this.setTextSize(0, textSize);
    }

    @Override
    public boolean isFocused() {
        return true;
    }
}

```