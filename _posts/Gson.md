---
title: Gson
date: 2016-07-12 18:24:09
tags:
- Android
- json
categories: 语言基础
thumbnail: https://avatars0.githubusercontent.com/u/1342004?v=3&s=200---
---
# Json
> [json官网](http://www.json.org/json-zh.html)上面还有很多各种语言的相关链接

## JSON(JavaScript Object Notation)
一种轻量级的数据交换格式，具有良好的可读和便于快速编写的特性。可在不同平台之间进行数据交换。JSON采用兼容性很高的、完全独立于语言文本格式，同时也具备类似于C语言的习惯(包括C, C++, C#, Java, JavaScript, Perl, Python等)体系的行为。这些特性使JSON成为理想的数据交换语言。
JSON基于JavaScript Programming Language , Standard ECMA-262 3rd Edition - December 1999 的一个子集。
<!--more-->s

## JSON优缺点
* 优点：
1. 数据格式比较简单，易于读写，格式都是压缩的，占用带宽小；
2. 易于解析，客户端JavaScript可以简单的通过eval()进行JSON数据的读取；
3. 支持多种语言，包括ActionScript, C, C#, ColdFusion, Java, JavaScript, Perl, PHP, Python, Ruby等服务器端语言，便于服务器端的解析；
4. 在PHP世界，已经有PHP-JSON和JSON-PHP出现了，偏于PHP序列化后的程序直接调用，PHP服务器端的对象、数组等能直接生成JSON格式，便于客户端的访问提取；
5. 因为JSON格式能直接为服务器端代码使用，大大简化了服务器端和客户端的代码开发量，且完成任务不变，并且易于维护。
* 缺点
1. 没有XML格式这么推广的深入人心和喜用广泛，没有XML那么通用性；
2. JSON格式目前在Web Service中推广还属于初级阶段。

## 格式

**对象**是一个无序的“‘名称/值’对”集合。一个对象以“{”开始，“}”结束。每个“名称”后跟一个“:”；“键值对”之间使用“,”分隔。JSON数组: 以"["开始, 以"]"结束
![json](http://www.json.org/object.gif)

# 解析
## BTW
Chrome的一个插件——JSONView,很好用,省去了在线转换格式的麻烦
## JSONArray和JSONObject
* JSONObject
json对象，就是一个键对应一个值，使用的是大括号{ }，如：{key:value}

* JSONArray
json数组，使用中括号[ ],数组里面的项也是json键值对格式的

**JSONObject中添加的是键值对，JSONArray中添加的是JSONObject**
> Show me the code

构造json文件
* 用map、list和Employee对象构造
```java
		// JSON格式数据解析对象
        JSONObject jo = new JSONObject();

        // 下面构造两个map、一个list和一个Employee对象
        Map<String, String> map1 = new HashMap<String, String>();
        map1.put("name", "Alexia");
        map1.put("sex", "female");
        map1.put("age", "23");

        Map<String, String> map2 = new HashMap<String, String>();
        map2.put("name", "Edward");
        map2.put("sex", "male");
        map2.put("age", "24");

        List<Map> list = new ArrayList<Map>();
        list.add(map1);
        list.add(map2);

        Employee employee = new Employee();
        employee.setName("wjl");
        employee.setSex("female");
        employee.setAge(24);

        // 将Map转换为JSONArray数据
        JSONArray ja1 = JSONArray.fromObject(map1);
        // 将List转换为JSONArray数据
        JSONArray ja2 = JSONArray.fromObject(list);
        // 将Bean转换为JSONArray数据
        JSONArray ja3 = JSONArray.fromObject(employee);
```
* 直接用JSONObject和JSONArray
```json
JSONObject jsonObject=new JSONObject();  
JSONArray jsonArray = new JSONArray();  
JSONObject jsonObject1 = new JSONObject();  
jsonObject1.put("loginname", "张三");  
jsonObject1.put("password", "123456");
jsonArray.put(jsonObject1);

JSONObject jsonObject2 = new JSONObject();  
jsonObject2.put("loginname", "李四");  
jsonObject2.put("password", "654321");
jsonArray.put(jsonObject2);
jsonObject.put("users", jsonMembers);  
```
## GSON
google在[Github上开源的项目](https://github.com/google/gson)
### 使用流程
今天用GSON解析一个结构稍微复杂一些的Json数据，主要是为了实现软件和后台的交互，结合上一篇的Volley实现软件升级。
上面的开源项目的地址上面写的还算详细，下面只是做一个总结。

Gson能实现Java对象和JSON之间的相互转换。甚至都不需要注释。Gson的特点：
* 提供简单的toJson()方法和fromJson()去实现相互转换。
* 可以从JSON中转换出之前存在的不可改变的对象。
* 扩展提供了Java泛型。
* 支持任意复杂的对象。

**以上面的例子为例**
1. 内部嵌套的类必须是static的，要不然解析会出错；
2. 类里面的属性名必须跟Json字段里面的Key是一模一样的；
3. 内部嵌套的用[]括起来的部分是一个List，所以定义为 public List<B> b，而只用{}嵌套的就定义为 public C c

真实的例子，解析的数据如下
```json
{
        actions: [
        {
            action: "get_selfupdate_info",
                    version: 1468207331,
                data: {
            englishMsg: "增加热门点播内容 喜欢的剧集不用再等 可以随时看啦",
                    fileMd5: "3d4c24caf58216c6a5a31b5d30d8e9b7",
                    fileSize: 12007328,
                    forceUpdate: "true",
                    message: "增加热门点播内容 喜欢的剧集不用再等 可以随时看啦",
                    url: "http://download.dianshijia.cn/tvlive/apk/2.8.5/dianshijia_official_default_2.8.5_release.apk",
                    version: "2.8.5",
                    versionCode: 95
        }
        }
        ],
        errcode: 0,
                msg: "成功",
            timestamp: 1468207331
    }

```

```java
        Gson gson = new Gson();
        UpdateMsg updatemsg = gson.fromJson(json, UpdateMsg.class);
        List<Actions> actions = updatemsg.getActions();
        mVersion = actions.get(0).getData().getVersion();
        String updateMessage = actions.get(0).getData().getMessage();
        String updateURL = actions.get(0).getData().getUrl();

```
UpdateMsg类
json中带[]的需要用List<T>
```java
public class UpdateMsg {
    private List<Actions> actions;
    private int errcode;
    private String msg;
    private String timestamp;

    @Override
    public String toString() {

        return "errorcode:"+errcode+",msg:"+msg+",timestamp:"+timestamp;
    }

    public List<Actions> getActions() {
        return actions;
    }

    public void setActions(List<Actions> actions) {
        this.actions = actions;
    }

    public int getErrcode() {
        return errcode;
    }

    public void setErrcode(int errcode) {
        this.errcode = errcode;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public String getTimestamp() {
        return timestamp;
    }

    public void setTimestamp(String timestamp) {
        this.timestamp = timestamp;
    }

}

```

```Data类
public class Data {
    private String englishMsg;
    private String fileMd5;
    private int fileSize;
    @Override
    public String toString() {

        return "englishMsg:"+englishMsg+",fileMd5:"+fileMd5+",fileSize:"+fileSize;
    }
    public String getEnglishMsg() {
        return englishMsg;
    }

    public void setEnglishMsg(String englishMsg) {
        this.englishMsg = englishMsg;
    }

    public String getFileMd5() {
        return fileMd5;
    }

    public void setFileMd5(String fileMd5) {
        this.fileMd5 = fileMd5;
    }

    public int getFileSize() {
        return fileSize;
    }

    public void setFileSize(int fileSize) {
        this.fileSize = fileSize;
    }

    public boolean isForceUpdate() {
        return forceUpdate;
    }

    public void setForceUpdate(boolean forceUpdate) {
        this.forceUpdate = forceUpdate;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getVersion() {
        return version;
    }

    public void setVersion(String version) {
        this.version = version;
    }

    public int getVersionCode() {
        return versionCode;
    }

    public void setVersionCode(int versionCode) {
        this.versionCode = versionCode;
    }

    private boolean forceUpdate;
    private String message;
    private String url;
    private String version;
    private int versionCode;
}
```

Actions类
```java
public class Actions {
    public Data getData() {
        return data;
    }

    public void setData(Data data) {
        this.data = data;
    }

    public String getAction() {
        return action;
    }

    public void setAction(String action) {
        this.action = action;
    }

    public String getVersion() {
        return version;
    }

    public void setVersion(String version) {
        this.version = version;
    }

    private String action;
    private String version;
    private Data data;
    @Override
    public String toString() {

        return "action:"+action+",version:"+version+",data:"+data;
    }
}


```
## fastjson
Alibaba在[Github上开源的项目](https://github.com/alibaba/fastjson)
* 下载最新的jar包，在[这里](https://search.maven.org/remote_content?g=com.alibaba&a=fastjson&v=LATEST)
### 待补充