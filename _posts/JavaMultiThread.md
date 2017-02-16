---
title: Java多线程
date: 2016-05-01 22:55:42
tags:
- Java
- 多线程
categories: 语言基础
---

> 20160505修改部分代码
> 20160506添加进程同步

![多线程](http://7xruee.com1.z0.glb.clouddn.com/Thread.jpg)
## 继承Thread类
```java
class Thread1 extends Thread {
	public void run() {
		int ticket = 5;
		while (ticket > 0)
			System.out.println(Thread.currentThread().getName() + "剩余"
					+ (ticket--));
		
	}
}

public class Main {

	public static void main(String[] args) {
		Thread1 mTh1 = new Thread1();
		Thread1 mTh2 = new Thread1();
		mTh1.start();
		mTh2.start();

	}

}
```
运行结果：
Thread-1剩余5
Thread-1剩余4
Thread-1剩余3
Thread-1剩余2
Thread-0剩余5
Thread-1剩余1
Thread-0剩余4
Thread-0剩余3
Thread-0剩余2
Thread-0剩余1
---
**一个继承了Thread的类的实例对象，无论调用多少次start方法，结果都只有一个线程在运行**
<!--more-->
说明：
从程序运行的结果可以发现，多线程程序是乱序执行。因此，只有乱序执行的代码才有必要设计为多线程。
Thread.sleep()方法调用目的是不让当前线程独自霸占该进程所获取的CPU资源，以留出一定时间给其他线程执行的机会。

## 实现java.lang.Runnable接口
```java
class Thread2 implements Runnable {
	int ticket = 5;

	public void run() {

		while (ticket > 0) {
			try {
				Thread.sleep(100);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			System.out.println(Thread.currentThread().getName() + "剩余"
					+ (ticket--));
		}

	}

}

public class Main {
	public static void main(String[] args) {
		Thread2 thrad = new Thread2();
		new Thread(thrad).start();
		new Thread(thrad).start();
		new Thread(thrad).start();
		new Thread(thrad).start();
	}

}
```
运行结果：
Thread-2剩余5
Thread-0剩余4
Thread-1剩余2
Thread-3剩余3
Thread-3剩余1
Thread-0剩余-2
Thread-2剩余0
Thread-1剩余-1
**Thread结果出现负数在后面会说明**
---
Runnable接口是没有start方法的，所以，一个类即使实现了Runnable接口，也需要Thread类中的start方法来启动线程
**实际上所有的多线程代码都是通过运行Thread的start()方法来运行的。因此，不管是继承Thread类还是实现Runnable接口来实现多线程，最终都是通过Thread的对象的API来控制线程。**
<!--more-->
## Thread和Runnable的区别

如果一个类继承Thread，则不适合资源共享。但是如果实现了Runable接口的话，则很容易的实现资源共享（代码例子很明显）。

## 线程操作的一些方法
**thread是Thread的实例**
```java
Thread.currentThread();//方法获取当前线程
thread.getName();//获取线程名
thread.isALive();//判断线程是否启动
thread.setDaemon();//将thread设置为守护线程
thread.join();//线程联合，强制运行完thread线程后，再运行后面的程序。
Thread.interrupt();//中断该线程（只对调用wait、join、sleep等方法受阻的线程产生影响）

```
### 详解
**守护线程**
JVM中线程成分为用户线程和守护线程。用户线程也称之为前台线程，守护线程也称之为后台线程。顾名思义，守护线程是守护其他线程的线程，它是指用户程序在运行时后台提供的一种通用服务的线程。当线程中只剩下守护线程时，JVM就会退出，反之如果还有用户线程在，JVM就不会退出。
**线程的联合**
一个线程A在占有CPU资源期间，可以让其他线程调用join()和本线程联合。如果一旦线程A在占有CPU资源期间联合B线程，那么A线程将立刻被立刻挂起(suspend)，直到它所联合的线程B执行完毕，A线程再重新排队等待CPU资源，以便恢复执行。
**线程的同步**
在前面代码中，如果线程数比较多，可能会出现同一个票号码被打印两次，可或者出现票号为0或者负数的情况。这些问题出现的原因是下面这部分代码
```java

while (ticket > 0) {
	System.out.println(Thread.currentThread().getName() + "剩余"
					+ (ticket--));
	}
```
假设ticket的值为1的时候，线程1刚执行完ticket>0的判断，此时操作系统恰好将CPU换到了线程2执行，而此时线程1还没来的及进行--操作，使得最终出现负数的结果。
解决办法————**添加同步代码块**
synchronized(对象){
	同步代码块；
}
如将上述代码修改为：
```java
class Thread2 implements Runnable {
	int ticket = 5;

	public void run() {

		while (true) {
			synchronized (this) {

				if (ticket <= 0)
					break;
				System.out.println(Thread.currentThread().getName() + "剩余"
						+ (ticket));
				// 模拟耗时操作
				try {
					Thread.sleep(100);
				} catch (InterruptedException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
				ticket--;
			}
		}
	}
}

public class Main {
	public static void main(String[] args) {
		Thread2 thrad = new Thread2();
		new Thread(thrad).start();
		new Thread(thrad).start();
		new Thread(thrad).start();
		new Thread(thrad).start();
	}

}
```
## start()和run()
1. start()方法
它的作用是启动一个新的线程，有了它的调用，才能真正实现多线程运行，这时无需等待run()方法执行完毕，而是直接继续执行start()其后的代码。可以理解为，**start()调用后，是的主线程"兵分两路"——创建了一个新线程，并是的这个新的线程进入"就绪状态"(Runnable),一旦新的线程获得CPU时间片，就开始执行run()方法**，一旦run()方法执行完毕，此线程就终止了。
2. run()方法
run()方法只是类的一个普通方法而已，如果直接调用run()方法，程序依然只有主线程一个线程，并没有达到多线程并发执行的目的。
## 总结

实现Runnable接口比继承Thread类所具有的优势：

1. 适合多个相同的程序代码的线程去处理同一个资源

2. 可以避免java中的单继承的限制

3. 增加程序的健壮性，代码可以被多个线程共享，代码和数据独立

备注：java每次程序执行至少启动2个线程（MAIN线程和GC线程）