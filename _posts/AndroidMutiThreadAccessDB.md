---
title: Android多线程安全访问数据库
date: 2016-08-12 13:08:41
tags: 
- Android
- 多线程
categories: 移动开发
---
> [原地址](https://github.com/dmytrodanylyk/dmytrodanylyk/blob/gh-pages/articles/Concurrent%20Database%20Access.md)

# 假设你已编写了自己的SQLiteOpenHelper
```java
public class DatabaseHelper extends SQLiteOpenHelper { ... }
```
若想在独立的线程中写数据
例如：
```java
// Thread 1
 Context context = getApplicationContext();
 DatabaseHelper helper = new DatabaseHelper(context);
 SQLiteDatabase database = helper.getWritableDatabase();
 database.insert(…);
 database.close();

 // Thread 2
 Context context = getApplicationContext();
 DatabaseHelper helper = new DatabaseHelper(context);
 SQLiteDatabase database = helper.getWritableDatabase();
 database.insert(…);
 database.close();
```
在logcat中会报如下错误
```
android.database.sqlite.SQLiteDatabaseLockedException: database is locked (code 5)
```
<!--more-->
产生以上错误的原因：
**每创建一个SQLiteOpenHelper对象时，实际上也是在新建一个数据库连接。如果你尝试通过多个连接同时对数据库进行写数据操作，其一定会失败。**

* **结论：多线程使用数据库需要确保我们使用的是同一个数据库connection**

## 解决方案
构造一个单例类DatabaseManager，其持有并返回单一的SQLiteOpenHelper对象。
```
public class DatabaseManager {

    private static DatabaseManager instance;
    private static SQLiteOpenHelper mDatabaseHelper;

    public static synchronized void initializeInstance(SQLiteOpenHelper helper) {
        if (instance == null) {
            instance = new DatabaseManager();
            mDatabaseHelper = helper;
        }
    }

    public static synchronized DatabaseManager getInstance() {
        if (instance == null) {
            throw new IllegalStateException(DatabaseManager.class.getSimpleName() +
                    " is not initialized, call initialize(..) method first.");
        }

        return instance;
    }

    public synchronized SQLiteDatabase getDatabase() {
        return mDatabaseHelper.getWritableDatabase();
    }

}
```
更新数据库操作的方法
```java
// In your application class
 DatabaseManager.initializeInstance(new DatabaseHelper());

 // Thread 1
 DatabaseManager manager = DatabaseManager.getInstance();
 SQLiteDatabase database = manager.getDatabase()
 database.insert(…);
 database.close();

 // Thread 2
 DatabaseManager manager = DatabaseManager.getInstance();
 SQLiteDatabase database = manager.getDatabase()
 database.insert(…);
 database.close();
```

但是使用这种方案会导致**另外的问题**
```java
java.lang.IllegalStateException: attempt to re-open an already-closed object: SQLiteDatabase
```
原因：
因为我们正在使用一个数据连接，对于Thread1和Thread2,getDataBase()方法会返回同一个SQLiteDatabase对象的实例，因此，会有如下的情况发生:Thread1可能关闭了数据库，而Thread2仍然在用这个数据库。这就是会产生IllegalStateException的原因。
## 该怎么做
我们只能在确保数据库没有再被占用的情况下，才去关闭它。在Stackoverflow中有人建议一直不关闭SQLiteDatabase，但是这样做在Logcat中会出现一下信息：
```
Leak foundCaused by: java.lang.IllegalStateException: SQLiteDatabase created and never closed
```
# 最终解决方案
```java
public class DatabaseManager {

    private AtomicInteger mOpenCounter = new AtomicInteger();

    private static DatabaseManager instance;
    private static SQLiteOpenHelper mDatabaseHelper;
    private SQLiteDatabase mDatabase;

    public static synchronized void initializeInstance(SQLiteOpenHelper helper) {
        if (instance == null) {
            instance = new DatabaseManager();
            mDatabaseHelper = helper;
        }
    }

    public static synchronized DatabaseManager getInstance() {
        if (instance == null) {
            throw new IllegalStateException(DatabaseManager.class.getSimpleName() +
                    " is not initialized, call initializeInstance(..) method first.");
        }

        return instance;
    }

    public synchronized SQLiteDatabase openDatabase() {
        if(mOpenCounter.incrementAndGet() == 1) {
            // Opening new database
            mDatabase = mDatabaseHelper.getWritableDatabase();
        }
        return mDatabase;
    }

    public synchronized void closeDatabase() {
        if(mOpenCounter.decrementAndGet() == 0) {
            // Closing database
            mDatabase.close();

        }
    }
}
```
使用
```java
    SQLiteDatabase database = DatabaseManager.getInstance().openDatabase();
    database.insert(...);
    // database.close(); Don't close it directly!
    DatabaseManager.getInstance().closeDatabase(); // correct way
```
每次需要使用数据库时应该调用DatabaseManager的openDatabase()方法，在该方法中有一个计数器，可以标识你当前开启了多少次数据库。如果计数为1，代表我们需要打开一个新的数据库连接，否则，数据库连接已经存在。在方法 closeDatabase() 中，情况也一样。每次我们调用 closeDatabase() 方法，计数器都会递减，直到计数为0，我们就需要关闭数据库连接了。
**使用 AtomicInteger 来处理并发的情况**