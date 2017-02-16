---
title: Fragment KeyEvent监听
date: 2016-12-19 20:56:10
tags: 
- Android
- Fragment
categories: 移动开发
---

# 接口回调

Fragment中
```java
protected BackHandlerInterface backHandlerInterface;  
    public interface BackHandlerInterface {  
        public void setSelectedFragment(Fragment3 backHandledFragment);  
}  
     
@Override  
public void onCreate(Bundle savedInstanceState) {  
    super.onCreate(savedInstanceState);  
    //回调函数赋值  
    if(!(getActivity()  instanceof BackHandlerInterface)) {  
        throw new ClassCastException("Hosting activity must implement BackHandlerInterface");  
    } else {  
        backHandlerInterface = (BackHandlerInterface) getActivity();  
    }  
}  
  
@Override  
public void onStart() {  
    super.onStart();  
    //将自己的实例传出去  
    backHandlerInterface.setSelectedFragment(this);  
}  

```
<!--more-->
Activity中回掉

```

    @Override  
    public void setSelectedFragment(Fragment3 backHandledFragment) {  
        this.selectedFragment = backHandledFragment;  
    }  
  
    @Override  
    public void onBackPressed() {  
        if(selectedFragment == null || !selectedFragment.onBackPressed()) {  
            super.onBackPressed();  
        }  
    } 
```
# To be continued