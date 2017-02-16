---
title: Android造成内存泄漏的几种可能
date: 2017-01-11 22:14:19
tags: 
- Android
- 内存泄漏
categories: 移动开发
---

# 分类
* 资源未关闭造成的内存泄漏
* 非静态内部类创建静态实例造成的内存泄漏
	1. Handler造成的内存泄漏
	2. 线程造成的内存泄漏
* 单例造成的内存泄漏


## 资源未关闭造成的内存泄漏
对于使用了BraodcastReceiver，ContentObserver，File，Cursor，Stream，Bitmap等资源的使用，应该在Activity销毁时及时关闭或者注销，否则这些资源将不会被回收，造成内存泄漏。

## 非静态内部类创建静态实例造成的内存泄漏
在Activity内部创建了一个非静态内部类的单例，每次启动Activity时都会使用该单例的数据，这样虽然避免了资源的重复创建，不过这种写法却会造成内存泄漏，因为非静态内部类默认会持有外部类的引用，而又使用了该非静态内部类创建了一个静态的实例，该实例的生命周期和应用的一样长，这就导致了该静态实例一直会持有该Activity的引用，导致Activity的内存资源不能正常回收。正确的做法为：
**将该内部类设为静态内部类或将该内部类抽取出来封装成一个单例，如果需要使用Context，使用ApplicationContext**

### Handler造成的内存泄漏
```java
public class MainActivity extends AppCompatActivity {

    private Handler mHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            //...
        }

    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        loadData();
    }
    private void loadData(){
        //...request
        Message message = Message.obtain();
        mHandler.sendMessage(message);
    }
}
```
这种创建Handler的方式会造成内存泄漏，由于mHandler是Handler的非静态匿名内部类的实例，所以它持有外部类Activity的引用，我们知道消息队列是在一个Looper线程中不断轮询处理消息，那么当这个Activity退出时消息队列中还有未处理的消息或者正在处理消息，而消息队列中的Message持有mHandler实例的引用，mHandler又持有Activity的引用，所以导致该Activity的内存资源无法及时回收，引发内存泄漏.
正确的写法
```java
public class MainActivity extends AppCompatActivity {

    private MyHandler mHandler = new MyHandler(this);
    private TextView mTextView ;
    private static class MyHandler extends Handler {
    	//使用弱引用
        private WeakReference<Context> reference;
        public MyHandler(Context context) {
            reference = new WeakReference<>(context);
        }

        @Override
        public void handleMessage(Message msg) {
            MainActivity activity = (MainActivity) reference.get();
            if(activity != null){
                activity.mTextView.setText("");
            }
        }
    }

  
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mTextView = (TextView)findViewById(R.id.textview);
        loadData();
    }

    private void loadData() {
        //...request
        Message message = Message.obtain();
        mHandler.sendMessage(message);
    }
}
```
### 线程造成的内存泄漏
例如
```java
new AsyncTask<Void, Void, Void>() {
    @Override
    protected Void doInBackground(Void... params) {
        SystemClock.sleep(10000);
        return null;
    }
}.execute();
new Thread(new Runnable() {
    @Override
    public void run() {
        SystemClock.sleep(10000);
    }
}).start();
```
上面的异步任务和Runnable都是一个匿名内部类，因此它们对当前Activity都有一个隐式引用。如果Activity在销毁之前，任务还未完成， 那么将导致Activity的内存资源无法回收，造成内存泄漏。
## 单例造成的内存泄漏
Android的单例模式非常受开发者喜爱，不过使用不恰当的话也会造成内存泄漏。因为单例的静态特性使得单例的生命周期和应用的生命周期一样长，这就说明了如果一个对象已经不需要使用了，而单例对象还持有该对象的引用，那么这个对象将不能被正常回收，这就导致了内存泄漏。
例如：
```java
public class AppManager {

    private static AppManager instance;
    private Context mContext;
    private AppManager(Context context) {
        mContext = context;
    }

    public static AppManager getInstance(Context context) {
        if (instance != null) {
            instance = new AppManager(context);
        }
        return instance;
    }
}
```
这是一个普通的单例模式，当创建这个单例的时候，由于需要传入一个Context，所以这个Context的生命周期的长短至关重要：

1、传入的是Application的Context：这将没有任何问题，因为单例的生命周期和Application的一样长 ;

2、传入的是Activity的Context：当这个Context所对应的Activity退出时，由于该Context和Activity的生命周期一样长(Activity间接继承于Context)，所以当前Activity退出时它的内存并不会被回收，因为单例对象持有该Activity的引用。
正确写法：
```java
public class AppManager {

    private static AppManager instance;

    private Context context;

    private AppManager(Context context) {

        this.context = context.getApplicationContext();

    }

    public static AppManager getInstance(Context context) {

        if (instance != null) {

            instance = new AppManager(context);

        }

        return instance;

    }

}
```