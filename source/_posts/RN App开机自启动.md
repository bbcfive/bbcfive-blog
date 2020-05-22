---
title: RN App开机自启动
date: 2019-06-04 21:29:39
tags: 
  - JavaScript 
  - React Native
categories: 移动端
---
RN的最后都逃不开原生...

## 一、目录
![avatar](https://img2018.cnblogs.com/blog/1549437/201906/1549437-20190604205646632-1967275900.png)

## 二、xml配置
``` xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.rnproject"
  android:installLocation="internalOnly" // 限定仅安装在手机内存（而非SD卡）里
  android:permission="android.permission.RECEIVE_BOOT_COMPLETED" // 给到接收系统广播权限
  >

  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
  <uses-permission android:name="android.permission.CAMERA" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>  
  <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />  // 给到接收系统广播权限

  <application
    android:name=".MainApplication"
    android:label="@string/app_name"
    android:icon="@mipmap/ic_launcher"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:allowBackup="false"
    android:theme="@style/AppTheme">
    <activity
      android:name=".MainActivity"
      android:label="@string/app_name"
      android:configChanges="keyboard|keyboardHidden|orientation|screenSize" // 使用键盘时不挤压屏幕
      android:windowSoftInputMode="adjustResize"
      android:noHistory="true" // 退出程序（离开屏幕）时即清除历史
      android:excludeFromRecents="true" // 退出程序（离开屏幕）时即从最近任务列表中移除
      >
      <intent-filter>
          <action android:name="android.intent.action.MAIN" />
          <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>
    <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
    <receiver
      android:name=".BootBroadcastReceiver" 
      android:enabled="true" 
      android:exported="true">
        <intent-filter>
            <!--注册开机广播地址-->
            <action android:name="android.intent.action.BOOT_COMPLETED"/> // 监听系统开机广播
            <action android:name="android.intent.action.ACTION_BOOT_COMPLETED"/>            
            <category android:name="android.intent.category.HOME" />
            <!-- <data android:scheme="file"/> -->
        </intent-filter>  
        <intent-filter>
            <action android:name="android.intent.action.MEDIA_MOUNTED"/> // 监听媒体广播，当检测到有SD卡进入/出时会触发此广播
            <action android:name="android.intent.action.MEDIA_UNMOUNTED"/> // 作用同上
            <data android:scheme="file">
            </data>
        </intent-filter>        
    </receiver>   
  </application>
</manifest>
```

## 三、com.rnproject
BootBroadcastReceiver.java
``` java
package com.rnproject; 

import android.content.BroadcastReceiver;  

import android.content.Context;  

import android.content.Intent;  

import android.util.Log;

public class BootBroadcastReceiver extends BroadcastReceiver {

    // 系统启动完成   
    static final String ACTION = "android.intent.action.BOOT_COMPLETED";
    
    @Override
    public void onReceive(Context context, Intent intent) {
        
        Log.d("BootBroadcastReceiver", intent.getAction()); 

        // 当收听到的事件是“BOOT_COMPLETED”时，就创建并启动相应的Activity和Service     
        if (intent.getAction().equals(ACTION)) {    
            Intent activityIntent = new Intent(context, MainActivity.class);    // 开机启动的Activity
            activityIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            // 启动Activity     
            context.startActivity(activityIntent);
        }    
    }

}
```

## 四、踩坑
查遍了网上的各种方法，配置了所有权限并规避了所有网页已提到问题，仍不能成功自启动....

[为此还贡献了StackOverflow上的首问...](https://stackoverflow.com/questions/56352895/how-to-receive-the-boot-completed-broadcast-in-my-react-native-app)

终于在大神的指导下，发现android8.0以上有些功能不再支持，因此需要降低targetSdkVersion至25以下...

以及，android3.0之后，谷歌为防止垃圾app自启动，做了一些限制，因此app需要在非stopped state情况下才能运行，即打开app，并确定它在你的任务列表里（不要删除进程），它才能开机自启动...