---
title: AndroidProxyTest
date: 2016-10-10 10:30:24
tags: 
- Android
categories: 
- 移动开发
---

# 检测Android代理设置
```java 
private boolean isWifiProxy() {
    final boolean IS_ICS_OR_LATER = Build.VERSION.SDK_INT >= Build.VERSION_CODES.ICE_CREAM_SANDWICH;
    String proxyAddress;
    int proxyPort;
    if (IS_ICS_OR_LATER) {
        proxyAddress = System.getProperty("http.proxyHost");
        String portStr = System.getProperty("http.proxyPort");
        proxyPort = Integer.parseInt((portStr != null ? portStr : "-1"));
    } else {
        proxyAddress = android.net.Proxy.getHost(getApplicationContext());
        proxyPort = android.net.Proxy.getPort(getApplicationContext());
    }
    return (!TextUtils.isEmpty(proxyAddress)) && (proxyPort != -1);
}
```