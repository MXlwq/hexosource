---
title: Android Dialog汇总
date: 2016-04-03 23:50:35
tags: 
- Android
- View
categories: 移动开发
---

## Dialog
在Android开发中，经常会需要在Android界面上弹出一些对话框。
分类：
1. 普通的对话框
2. 多按钮对话框
3. 列表对话框
4. 选择对话框（单选和多选）
5. 进度条对话框
6. 读取对话框

## 上代码

> MainActivity.class

<!--more-->
```java
package com.liwenquan.dailogdemo;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.DialogInterface;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.PopupWindow;
import android.widget.Toast;

import java.util.ArrayList;

public class MainActivity extends Activity implements Runnable {
    public final static int MAX_READPROCESS = 100;
    /*单项选择对话框*/
    int yourChose = -1;
    ArrayList<Integer> myChose = new ArrayList<Integer>();
    //进度读取框需要模拟读取
    ProgressDialog mReadProcessDia = null;
    private Button btn_diaNormal;
    private Button btn_diaMulti;
    private Button btn_diaList;
    private Button btn_diaSinChos;
    private Button btn_diaMultiChos;
    private Button btn_diaProcess;
    private Button btn_diaReadProcess;
    private PopupWindow window = null;
    private Button.OnClickListener btnListener = new Button.OnClickListener() {
        public void onClick(View v) {
            if (v instanceof Button) {
                int btnId = v.getId();
                switch (btnId) {
                    case R.id.btn_diaNormal:
                        showNormalDia();
                        break;
                    case R.id.btn_diaMulti:
                        showMultiDia();
                        break;
                    case R.id.btn_diaList:
                        showListDia();
                        break;
                    case R.id.btn_diaSigChos:
                        showSinChosDia();
                        break;
                    case R.id.btn_diaMultiChos:
                        showMultiChosDia();
                        break;
                    case R.id.btn_diaReadProcess:
                        showReadProcess();
                        break;
                    case R.id.btn_diaProcess:
                        showProcessDia();
                        break;
                    default:
                        break;
                }
            }
        }
    };

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        getView();
        setListener();
    }

    private void getView() {
        btn_diaNormal = (Button) findViewById(R.id.btn_diaNormal);
        btn_diaMulti = (Button) findViewById(R.id.btn_diaMulti);
        btn_diaList = (Button) findViewById(R.id.btn_diaList);
        btn_diaSinChos = (Button) findViewById(R.id.btn_diaSigChos);
        btn_diaMultiChos = (Button) findViewById(R.id.btn_diaMultiChos);
        btn_diaProcess = (Button) findViewById(R.id.btn_diaProcess);
        btn_diaReadProcess = (Button) findViewById(R.id.btn_diaReadProcess);

    }

    private void setListener() {
        btn_diaNormal.setOnClickListener(btnListener);
        btn_diaMulti.setOnClickListener(btnListener);
        btn_diaList.setOnClickListener(btnListener);
        btn_diaSinChos.setOnClickListener(btnListener);
        btn_diaMultiChos.setOnClickListener(btnListener);
        btn_diaProcess.setOnClickListener(btnListener);
        btn_diaReadProcess.setOnClickListener(btnListener);
    }

    /*普通的对话框*/
    private void showNormalDia() {
        //AlertDialog.Builder normalDialog=new AlertDialog.Builder(getApplicationContext());
        AlertDialog.Builder normalDia = new AlertDialog.Builder(MainActivity.this);
        //normalDia.setIcon(R.drawable.ic_launcher);
        normalDia.setTitle("普通的对话框");
        normalDia.setMessage("普通对话框的message内容");

        normalDia.setPositiveButton("确定", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                // TODO Auto-generated method stub
                showClickMessage("确定");
            }
        });
        normalDia.setNegativeButton("取消", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                // TODO Auto-generated method stub
                showClickMessage("取消");
            }
        });
        normalDia.create().show();
    }

    /*多按钮对话框*/
    private void showMultiDia() {
        AlertDialog.Builder multiDia = new AlertDialog.Builder(MainActivity.this);
        multiDia.setTitle("多选项对话框");
        multiDia.setPositiveButton("按钮一", new DialogInterface.OnClickListener() {

            @Override
            public void onClick(DialogInterface dialog, int which) {
                // TODO Auto-generated method stub
                showClickMessage("按钮一");
            }
        });
        multiDia.setNeutralButton("按钮二", new DialogInterface.OnClickListener() {

            @Override
            public void onClick(DialogInterface dialog, int which) {
                // TODO Auto-generated method stub
                showClickMessage("按钮二");
            }
        });
        multiDia.setNegativeButton("按钮三", new DialogInterface.OnClickListener() {

            @Override
            public void onClick(DialogInterface dialog, int which) {
                // TODO Auto-generated method stub
                showClickMessage("按钮三");
            }
        });
        multiDia.create().show();
    }

    /*列表对话框*/
    private void showListDia() {
        final String[] mList = {"选项1", "选项2", "选项3"};
        AlertDialog.Builder listDia = new AlertDialog.Builder(MainActivity.this);
        listDia.setTitle("列表对话框");
        listDia.setItems(mList, new DialogInterface.OnClickListener() {

            @Override
            public void onClick(DialogInterface dialog, int which) {
                // TODO Auto-generated method stub
                /*下标是从0开始的*/
                showClickMessage(mList[which]);
            }
        });
        listDia.create().show();
    }

    private void showSinChosDia() {
        final String[] mList = {"选项1", "选项2", "选项3"};
        yourChose = -1;
        AlertDialog.Builder sinChosDia = new AlertDialog.Builder(MainActivity.this);
        sinChosDia.setTitle("单项选择对话框");
        sinChosDia.setSingleChoiceItems(mList, 0, new DialogInterface.OnClickListener() {

            @Override
            public void onClick(DialogInterface dialog, int which) {
                // TODO Auto-generated method stub
                yourChose = which;

            }
        });
        sinChosDia.setPositiveButton("确定", new DialogInterface.OnClickListener() {

            @Override
            public void onClick(DialogInterface dialog, int which) {
                // TODO Auto-generated method stub
                if (yourChose != -1) {
                    showClickMessage(mList[yourChose]);
                }
            }
        });
        sinChosDia.create().show();
    }

    private void showMultiChosDia() {
        final String[] mList = {"选项1", "选项2", "选项3"};
        final boolean mChoseSts[] = {false, false, false};
        myChose.clear();
        AlertDialog.Builder multiChosDia = new AlertDialog.Builder(MainActivity.this);
        multiChosDia.setTitle("多项选择对话框");
        multiChosDia.setMultiChoiceItems(mList, mChoseSts, new DialogInterface.OnMultiChoiceClickListener() {

            @Override
            public void onClick(DialogInterface dialog, int which, boolean isChecked) {
                // TODO Auto-generated method stub
                if (isChecked) {
                    myChose.add(which);
                } else {
                    myChose.remove(which);
                }
            }
        });
        multiChosDia.setPositiveButton("确定", new DialogInterface.OnClickListener() {

            @Override
            public void onClick(DialogInterface dialog, int which) {
                // TODO Auto-generated method stub
                int size = myChose.size();
                String str = "";
                for (int i = 0; i < size; i++) {
                    str += mList[myChose.get(i)];
                }
                showClickMessage(str);
            }
        });
        multiChosDia.create().show();
    }

    private void showReadProcess() {
        mReadProcessDia = new ProgressDialog(MainActivity.this);
        mReadProcessDia.setProgress(0);
        mReadProcessDia.setTitle("进度条窗口");
        mReadProcessDia.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
        mReadProcessDia.setMax(MAX_READPROCESS);
        mReadProcessDia.show();
        new Thread(this).start();
    }

    //新开启一个线程，循环的累加，一直到100然后在停止
    @Override
    public void run() {
        int Progress = 0;
        while (Progress < MAX_READPROCESS) {
            try {
                Thread.sleep(100);
                Progress++;
                mReadProcessDia.incrementProgressBy(1);
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        //读取完了以后窗口自消失
        mReadProcessDia.cancel();
    }

    /*读取中的对话框*/
    private void showProcessDia() {
        ProgressDialog processDia = new ProgressDialog(MainActivity.this);
        processDia.setTitle("进度条框");
        processDia.setMessage("内容读取中...");
        processDia.setIndeterminate(true);
        processDia.setCancelable(true);
        processDia.show();
    }


    /*显示点击的内容*/
    private void showClickMessage(String message) {
        Toast.makeText(MainActivity.this, "你选择的是: " + message, Toast.LENGTH_SHORT).show();
    }
}


```

> activity_main.xml

```xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="各种Dialog合集" />

    <Button
        android:id="@+id/btn_diaNormal"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="普通Dialog" />

    <Button
        android:id="@+id/btn_diaMulti"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="多按钮Dialog" />

    <Button
        android:id="@+id/btn_diaList"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="列表Dialog" />

    <Button
        android:id="@+id/btn_diaSigChos"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="单项选择Dialog" />

    <Button
        android:id="@+id/btn_diaMultiChos"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="多项选择Dialog" />

    <Button
        android:id="@+id/btn_diaReadProcess"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="进度条Dialog" />

    <Button
        android:id="@+id/btn_diaProcess"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="读取中Dialog" />

</LinearLayout>


```