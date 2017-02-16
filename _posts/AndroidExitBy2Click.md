---
title: Android两次点击返回键退出
date: 2016-09-06 19:04:23
tags: 
- 模板
- Android
categories: 移动开发
---

# 需求
之前又需求要双击点击返回键退出应用，现在总结一下
实现方法：
onKeyDown方法监听中做exitBy2Click()方法；
exitBy2Click()方法中用Timer做定时操作，每两秒后将退出状态恢复为false，只有在两秒内再次点击(此时状态为true，则finish改Activity)
# 代码
<!--more-->
```java
    /**
     * 菜单、返回键响应
     */
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        // TODO Auto-generated method stub
        if (keyCode == KeyEvent.KEYCODE_BACK) {
            exitBy2Click(); 
        }
        return false;
    }

    private void exitBy2Click() {
        Timer tExit = null;
        if (isExit == false) {
            isExit = true; // 准备退出
            Toast.makeText(this, "再按一次", Toast.LENGTH_SHORT).show();
            tExit = new Timer();
            tExit.schedule(new TimerTask() {
                @Override
                public void run() {
                    isExit = false; // 取消退出
                }
            }, 2000); // 如果2秒钟内没有按下返回键，则启动定时器取消掉刚才执行的任务

        } else {
            finish();
        }
    }
```