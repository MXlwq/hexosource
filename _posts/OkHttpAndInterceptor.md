---
title: OkHttp使用及拦截器
date: 2016-07-31 19:58:26
tags: 
- Android
- 网络
categories: 
- 移动开发
thumbnail: http://7xruee.com1.z0.glb.clouddn.com/okhttp.png
---

# 准备

使用前，对于Android Studio，选择在Gradle添加:
```
compile 'com.squareup.okhttp3:okhttp:(insert latest version)'
```

# 使用

## GET A URL
```java
	//某个控件的点击监听器
	private View.OnClickListener mDownloadClickListener = new View.OnClickListener() {
        @Override
        public void onClick(View view) {

            OkHttpClient okHttpClient = new OkHttpClient();
            Request request = new Request.Builder()
                    .url("http://liwenquan.top")
                    .build();
            okHttpClient.newCall(request).enqueue(callback);
        }
    };
    //请求后的回调接口
    private Callback callback = new Callback() {
        @Override
        public void onFailure(Call call, IOException e) {
            Log.e("LWQ", e.toString());
        }

        @Override
        public void onResponse(Call call, Response response) throws IOException {
        	//onResponse|onFailure都不是在UI线程中执行,因此进行UI相关的操作，需要在UI线程中进行，这里借助了Handler
            Message msg = new Message();
            msg.what = 0x123;
            msg.obj = response.body().string();
            mHandler.sendMessage(msg);
        }
    };
}
```

## POST TO A SERVER
<!--more-->
```java

public static final MediaType JSON
    = MediaType.parse("application/json; charset=utf-8");

OkHttpClient client = new OkHttpClient();

String post(String url, String json) throws IOException {
  RequestBody body = RequestBody.create(JSON, json);
  Request request = new Request.Builder()
      .url(url)
      .post(body)
      .build();
  Response response = client.newCall(request).execute();
  return response.body().string();
}

```