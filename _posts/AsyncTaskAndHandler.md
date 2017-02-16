---
title: AsyncTask和Handler
date: 2016-04-15 19:06:12
tags:
- Android
- Intent
categories: 移动开发
---

> 20160502更新，添加代码

## AsyncTask

### 实现的原理
AsyncTask,是android提供的轻量级的异步类,可以直接继承AsyncTask,在类中实现异步操作,并提供接口反馈当前异步执行的程度(可以通过接口实现UI进度更新),最后反馈执行的结果给UI主线程
### 优点:
简单,快捷
过程可控
### 缺点:
在使用多个异步操作和并需要进行Ui变更时,就变得复杂起来.

## Handler异步

### 实现的原理
在Handler 异步实现时,涉及到 Handler, Looper, Message,Thread四个对象，实现异步的流程是主线程启动Thread（子线程）运行并生成Message-Looper获取Message并传递给HandlerHandler逐个获取Looper中的Message，并进行UI变更。

### 优点：
结构清晰，功能定义明确
对于多个后台任务时，简单，清晰

### 缺点：
在单个后台异步处理时，显得代码过多，结构过于复杂（相对性）
 
## AsyncTask介绍

Android的AsyncTask比Handler更轻量级一些（只是代码上轻量一些，而实际上要比handler更耗资源），适用于简单的异步处理。
首先明确Android之所以有Handler和AsyncTask，都是为了不阻塞主线程（UI线程），且UI的更新只能在主线程中完成，因此异步处理是不可避免的。
 
Android为了降低这个开发难度，提供了AsyncTask。AsyncTask就是一个封装过的后台任务类，顾名思义就是异步任务。
AsyncTask直接继承于Object类，位置为android.os.AsyncTask。要使用AsyncTask工作我们要提供三个泛型参数，并重载几个方法(至少重载一个)。
 
AsyncTask定义的三种泛型类型 Params，Progress和Result。
Params 启动任务执行的输入参数，比如HTTP请求的URL。
Progress 后台任务执行的百分比。
Result 后台执行任务最终返回的结果，比如String。
使用过AsyncTask 的同学都知道一个异步加载数据最少要重写以下这两个方法：
> doInBackground(Params…)

后台执行，比较耗时的操作都可以放在这里。注意这里不能直接操作UI。此方法在后台线程执行，完成任务的主要工作，通常需要较长的时间。在执行过程中可以调用publicProgress(Progress…)来更新任务的进度。
> onPostExecute(Result)

相当于Handler 处理UI的方式，在这里面可以使用在doInBackground 得到的结果处理操作UI。 此方法在主线程执行，任务执行的结果作为此方法的参数返回
有必要的话你还得重写以下这三个方法，但不是必须的：
> onProgressUpdate(Progress…)

可以使用进度条增加用户体验度。 此方法在主线程执行，用于显示任务执行的进度。

> onPreExecute()

这里是最终用户调用Excute时的接口，当任务执行之前开始调用此方法，可以在这里显示进度对话框。

> onCancelled()

用户调用取消时，要做的操作

使用AsyncTask类，必须遵守的准则：
1. Task的实例必须在UI thread中创建；
2. execute方法必须在UI thread中调用；
3. 不要手动的调用onPreExecute(), onPostExecute(Result)，doInBackground(Params...), onProgressUpdate(Progress...)这几个方法；
4. 该task只能被执行一次，否则多次调用时将会出现异常；
<!--more-->
## Handler介绍

Handler主要接受子线程发送的数据, 并用此数据配合主线程更新UI。当应用程序启动时，Android首先会开启一个主线程, 主线程为管理界面中的UI控件，进行事件分发,更新UI只能在主线程中更新，子线程中操作是危险的。这个时候，Handler就需要出来解决这个复杂的问题。由于Handler运行在主线程中(UI线程中),它与子线程可以通过Message对象来传递数据, 这个时候，Handler就承担着接受子线程传过来的(子线程用sendMessage()方法传递)Message对象(里面包含数据), 把这些消息放入主线程队列中，配合主线程进行更新UI。

### 实现方法

Hanlder的作用——发送消息和接收消息，Hanlder发送的消息必须被送到指定的MessageQueue。也就是说，如果希望Hanlder正常工作，必须在当前线程中有一个MessageQueue（而MessageQueue由Looper负责管理），也就是说必须在当前线程中有一个Looper对象。
分两种情况：
主线程中，系统已经初始化了一个Looper对象，因此程序直接创建Handler即可，然后可以通过Handler来发送消息、处理消息。
手动创建的新的线程，必须自己创建一个Looper对象，并启动它（Looper.prepare()）;
**线程中使用Handler的步骤**：
1. 调用Looper的**prepare()**方法为当前线程创建Looper对象，创建Looper对象时，它的构造器会创建与之配套的MessageQueue
2. 创建Handler子类的实例，**重写handleMessage()**方法该方法负责处理来自其他线程的消息。
3. **调用Looper.loop()**方法启动Looper。
### Handler的特点
Handler可以分发Message对象和Runnable对象到主线程中, 每个Handler实例,都会绑定到创建他的线程中,
它有两个作用: 
1. 安排消息或Runnable 在某个主线程中某个地方执行 
2. 安排一个动作在不同的线程中执行
Handler中分发消息的一些方法
post(Runnable)
postAtTime(Runnable,long)
postDelayed(Runnable long)
sendEmptyMessage(int)
sendMessage(Message)
sendMessageAtTime(Message,long)
sendMessageDelayed(Message,long)
以上post类方法允许你排列一个Runnable对象到主线程队列中,
sendMessage类方法, 允许你安排一个带数据的Message对象到队列中，等待更新.

### 实例（在子线程中计算质数，并将结果返回给UI线程）

```java

import android.content.Intent;
import android.os.Bundle;
import android.os.Handler;
import android.os.Looper;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    EditText meditText;
    CalThread calThread;
    Handler handler;
    Button mbutton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Toolbar toolbar = (Toolbar) findViewById(R.id.main_toolbar);
        toolbar.setTitleTextColor(getResources().getColor(android.R.color.white));
        setSupportActionBar(toolbar);
        mbutton = (Button) findViewById(R.id.button_cal);
        meditText = (EditText) findViewById(R.id.editText_num);
        //主线程的Hanlder实例，处理消息
        handler = new Handler() {
            @Override
            public void handleMessage(Message msg) {

                if (msg.what == 0x1236) {
                    //更新UI
                    String s = msg.obj.toString();
                    meditText.setText(s);
                }
            }
        };
        calThread = new CalThread(handler);
        calThread.start();
        mbutton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                //发送消息
                Message msg = new Message();
                msg.what = 0x1235;
                Bundle bundle = new Bundle();
                int i;
                try {
                    i = Integer.parseInt(meditText.getText().toString());
                } catch (Exception e) {
                    i = 0;
                }
                bundle.putInt("upper", i);
                msg.setData(bundle);
                calThread.calhandler.sendMessage(msg);
            }
        });
    }


    class CalThread extends Thread {
        public Handler calhandler;
        private Handler handler;

		//传递主线程的hanlder实例给CalThread
        public CalThread(Handler handler) {
            this.handler = handler;
        }

        List<Integer> list;

        @Override
        public void run() {

            Looper.prepare();
            calhandler = new Handler() {
                @Override
                public void handleMessage(Message msg) {

                    if (msg.what == 0x1235) {
                        int upper = msg.getData().getInt("upper");
                        list = new ArrayList<Integer>();
                        outer:
                        for (int i = 2; i < upper; i++) {
                            for (int j = 2; i < Math.sqrt(i); j++) {
                                if (i != 2 && i % j == 0) {
                                    continue outer;
                                }
                            }
                            list.add(i);
                        }
                        //起算结束，开启一个新的线程来给主线程发送消息，将计算结果返回
                        new Thread() {
                            @Override
                            public void run() {
                                Message msg = new Message();
                                msg.what = 0x1236;
                                msg.obj = list.toString();
                                handler.sendMessage(msg);
                            }
                        }.start();
                    }
                }
            };
            Looper.loop();
        }
    }
}

```
## 总结
数据简单使用**AsyncTask**
实现代码简单，数据量多且复杂使用**handler+thread**，相比较AsyncTask来说能更好的利用系统资源且高效