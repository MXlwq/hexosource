---
title: Android6.0动态权限
date: 2016-12-12 20:13:13
tags: 
- Android
- Permission
categories: 移动开发
---
# 9组权限
group:android.permission-group.CONTACTS
    permission:android.permission.WRITE_CONTACTS
    permission:android.permission.GET_ACCOUNTS    
    permission:android.permission.READ_CONTACTS

  group:android.permission-group.PHONE
    permission:android.permission.READ_CALL_LOG
    permission:android.permission.READ_PHONE_STATE 
    permission:android.permission.CALL_PHONE
    permission:android.permission.WRITE_CALL_LOG
    permission:android.permission.USE_SIP
    permission:android.permission.PROCESS_OUTGOING_CALLS
    permission:com.android.voicemail.permission.ADD_VOICEMAIL

  group:android.permission-group.CALENDAR
    permission:android.permission.READ_CALENDAR
    permission:android.permission.WRITE_CALENDAR

  group:android.permission-group.CAMERA
    permission:android.permission.CAMERA

  group:android.permission-group.SENSORS
    permission:android.permission.BODY_SENSORS

  group:android.permission-group.LOCATION
    permission:android.permission.ACCESS_FINE_LOCATION
    permission:android.permission.ACCESS_COARSE_LOCATION

  group:android.permission-group.STORAGE
    permission:android.permission.READ_EXTERNAL_STORAGE
    permission:android.permission.WRITE_EXTERNAL_STORAGE

  group:android.permission-group.MICROPHONE
    permission:android.permission.RECORD_AUDIO

  group:android.permission-group.SMS
    permission:android.permission.READ_SMS
    permission:android.permission.RECEIVE_WAP_PUSH
    permission:android.permission.RECEIVE_MMS
    permission:android.permission.RECEIVE_SMS
    permission:android.permission.SEND_SMS
    permission:android.permission.READ_CELL_BROADCASTS

** 每组只要有一个权限申请成功了，就默认整组权限都可以使用了。

以下是普通权限，只需要在AndroidManifest.xml中申请即可
android.permission.ACCESS_LOCATION_EXTRA_COMMANDS
  android.permission.ACCESS_NETWORK_STATE
  android.permission.ACCESS_NOTIFICATION_POLICY
  android.permission.ACCESS_WIFI_STATE
  android.permission.ACCESS_WIMAX_STATE
  android.permission.BLUETOOTH
  android.permission.BLUETOOTH_ADMIN
  android.permission.BROADCAST_STICKY
  android.permission.CHANGE_NETWORK_STATE
  android.permission.CHANGE_WIFI_MULTICAST_STATE
  android.permission.CHANGE_WIFI_STATE
  android.permission.CHANGE_WIMAX_STATE
  android.permission.DISABLE_KEYGUARD
  android.permission.EXPAND_STATUS_BAR
  android.permission.FLASHLIGHT
  android.permission.GET_ACCOUNTS
  android.permission.GET_PACKAGE_SIZE
  android.permission.INTERNET
  android.permission.KILL_BACKGROUND_PROCESSES
  android.permission.MODIFY_AUDIO_SETTINGS
  android.permission.NFC
  android.permission.READ_SYNC_SETTINGS
  android.permission.READ_SYNC_STATS
  android.permission.RECEIVE_BOOT_COMPLETED
  android.permission.REORDER_TASKS
  android.permission.REQUEST_INSTALL_PACKAGES
  android.permission.SET_TIME_ZONE
  android.permission.SET_WALLPAPER
  android.permission.SET_WALLPAPER_HINTS
  android.permission.SUBSCRIBED_FEEDS_READ
  android.permission.TRANSMIT_IR
  android.permission.USE_FINGERPRINT
  android.permission.VIBRATE
  android.permission.WAKE_LOCK
  android.permission.WRITE_SYNC_SETTINGS
  com.android.alarm.permission.SET_ALARM
  com.android.launcher.permission.INSTALL_SHORTCUT
  com.android.launcher.permission.UNINSTALL_SHORTCUT
# 申请步骤
1.将targetSdkVersion设置为23，注意，如果将targetSdkVersion设置为>=23，则必须按照Android谷歌的要求，动态的申请权限，如果你暂时不打算支持动态权限申请，则targetSdkVersion最大只能设置为22.

