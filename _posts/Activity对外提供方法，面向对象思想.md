---
title: Activity对外提供方法，面向对象思想
date: 2017-01-08 22:46:06
tags:
---
```java
public static void goTo(Context context) {
    Logger.i(TAG, "Go to the main activity");

    Intent intent = new Intent(context, MainActivity.class);

    if (context instanceof Activity) {
        context.startActivity(intent);
    } else {
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        context.startActivity(intent);
    }
}
```