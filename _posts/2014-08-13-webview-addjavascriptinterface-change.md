---
layout: post
title:  "Android 4.2中对WebView中addJavascriptInterface方法的变更"
date:   2014-08-13 12:05:56
categories: JavaScript WebView Android
---
如果在WebView中JavaScript调用Java中的函数，则在Java会用到WebView中得addJavascriptInterface方法。  
但是在Android4.2(如果应用的android:targetSdkVersion数值为17+)开始，因为安全问题在JavaScript中只能访问带有@JavascriptInterface注解的Java函数，这样就可以增强安全性。  
Android文档中是这样写的：
 > If your app uses WebView, Android 4.2 adds an additional layer of security so you can more safely bind JavaScript to your Android code. If you set your targetSdkVersion to 17 or higher, you must now add the @JavascriptInterface annotation to any method that you want available to your JavaScript (the method must also be public). If you do not provide the annotation, the method is not accessible by a web page in your WebView when running on Android 4.2 or higher. If you set the targetSdkVersion to 16 or lower, the annotation is not required, but we recommend that you update your target version and add the annotation for additional security.

扩展阅读：[Android 4.2 APIs](http://developer.android.com/about/versions/android-4.2.html)