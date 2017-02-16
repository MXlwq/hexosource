---
title: Java并发
date: 2016-10-27 18:00:31
tags: 
- Java
- 并发
categories: 语言基础
---

# CountDownLatch
```java
public CountDownLatch(int count);
public void countDown();
public void await() throws InterruptedException
```
构造方法参数count指定了计数的次数
countDown()方法，当前线程调用此方法，则计数减一
await()方法，调用此方法会一直阻塞当前线程，直到计时器的值为0

# new Object()

wait()、notify()、notifyAll()是三个定义在Object类里的方法，可以用来控制线程的状态。

这三个方法最终调用的都是jvm级的native方法。随着jvm运行平台的不同可能有些许差异。
如果对象调用了wait方法就会使持有该对象的线程把该对象的控制权交出去，然后处于等待状态。
如果对象调用了notify方法就会通知某个正在等待这个对象的控制权的线程可以继续运行。
如果对象调用了notifyAll方法就会通知所有等待这个对象控制权的线程继续运行。