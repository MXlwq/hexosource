---
title: Intent传递对象的两种方法(Serializable,Parcelable)
date: 2016-04-16 23:20:30
tags:
- Intent
- Android
categories: 移动开发
---

Intent在不同的组件中传递对象数据的应用非常普遍。下面介绍两种通过Intent传递对象的方法。
1、实现Serializable接口
2、实现Parcelable接口
 
为什么要将对象序列化？
 1、永久性保存对象，保存对象的字节序列到本地文件中；
 2、用过序列化对象在网络中传递对象；
 3、通过序列化对象在进程间传递对象。
 
1、实现Serializable接口
    Serializable的作用是将数据对象存入字节流当中，在需要时重新生成对象，主要应用是利用外部存储设备保存对象状态，以及通过网络传输对象等。
    implements Serializable接口的的作用就是给对象打了一个标记，系统会自动将其序列化。
    
    案例1：
 1)User.java  (implements Serializable )
 2)MainActivity.java  
     User user = new User();
     Intent intent = new Intent(this,Second.class);  
     intent.putExtra("user",user);
 3)Second.java
     Intent intent = getIntent();
     User user = intent.getSerializableExtra("user");
 
2、实现Parcelable接口
     1)为什么要实现Parfcelable接口来实现在Intent中传递对象？
      a、在使用内存的时候，Parcelable比Serializable性能高，所以推荐使用Parcelable类。
      b、Serializable在序列化的时候会产生大量的临时变量，从而引起频繁的GC。
 
     注意：Parcelable不能使用在将数据存储在磁盘上的情况，因为Parcelable不能很好的保存数据的持续性在外界有变化的情况下。因此在这种情况下，建议使用Serializable
      
     2) Android中的新的序列化机制
     在Android系统中，针对内存受限的移动设备，因此对性能要求更高，Android系统采用了新的IPC(进程间通信)机制，要求使用性能更出色的对象传输方式。因此Parcel类被设计出来，其定位就是轻量级的高效的对象序列化和反序列化机制。
     Parcel的序列化和反序列化的读写全是在内存中进行，所以效率比JAVA序列化中使用外部存储器会高很多。
 
Parcel类
     就应用程序而言，在常使用Parcel类的场景就是在Activity间传递数据。在Activity间使用Intent传递数据的时候，可以通过Parcelable机制传递复杂的对象。
     Parcel机制：本质上把它当成一个Serialize就可以了。只是Parcel的对象实在内存中完成的序列化和反序列化，利用的是连续的内存空间，因此更加高效。
    
案例：
    步骤1:自定义实体类，实现Parcelable接口，重写其两个方法。
    步骤2：该实体类必须添加一个常量CREATOR（名字大小写都不能使其他的），该常量必须实现Parcelable的内部接口：Parcelable.Creator，并实现该接口中的两个方法。
    User.java如下：
```java
package com.example.intent_object;  
  
import android.os.Parcel;  
import android.os.Parcelable;  
  
public class User implements Parcelable {  
    public String name;  
    public int age;  
  
    // 必须要创建一个名叫CREATOR的常量。  
    public static final Parcelable.Creator<User> CREATOR = new Parcelable.Creator<User>() {  
        @Override  
        public User createFromParcel(Parcel source) {  
            return new User(source);  
        }  
        //重写createFromParcel方法，创建并返回一个获得了数据的user对象  
        @Override  
        public User[] newArray(int size) {  
            return new User[size];  
        }  
    };  
  
    @Override  
    public String toString() {  
        return name + ":" + age;  
    }  
  
    // 无参数构造器方法，供外界创建类的实例时调用  
    public User() {  
    }  
  
    // 带参构造器方法私用化，本构造器仅供类的方法createFromParcel调用  
    private User(Parcel source) {  
        name = source.readString();  
        age = source.readInt();  
    }  
  
    @Override  
    public int describeContents() {  
        return 0;  
    }  
  
    // 将对象中的属性保存至目标对象dest中  
    @Override  
    public void writeToParcel(Parcel dest, int flags) {  
        dest.writeString(name);  
        dest.writeInt(age);  
    }  
  
  //省略getter/setter }  
 
```
其他代码：
```java
Bundle bundle = new Bundle();  
                    bundle.putParcelable("user", user);  
                    Intent intent = new Intent(MainActivity.this,  
                            SecondActivity.class);  
                    intent.putExtras(bundle);  
```
  
```java
Intent intent = getIntent();  
        Bundle bun = intent.getExtras();  
        User user = bun.getParcelable("user");  
        System.out.println(user);  
```