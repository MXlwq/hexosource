---
title: AS Maven转gradle
date: 2016-12-27 10:28:24
tags: 
- Android
- Gradle
categories: 移动开发
---

## Maven
<!-- in the 'repositories' section -->
<repository>
  <id>keytwo.net</id>
  <name>Keytwo.net Repository</name>
  <url>http://audiobox.keytwo.net</url>
</repository>

<!-- in the 'dependencies' section -->
<dependency>
  <groupId>io.socket</groupId>
  <artifactId>socket.io-client</artifactId>
  <version>0.2.1</version> <!-- the desidered version -->
</dependency>

## Gradle
repositories {
    maven { url 'http://audiobox.keytwo.net' }
}
dependencies {
    ...
    compile 'io.socket:socket.io-client:0.2.1'
}