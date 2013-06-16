---
layout: post
title: "(iOS)FacebookSDK armv7s slice error"
date: 2013-06-16 13:52
comments: true
categories:  [Objective-C, iOS App]
---

最近在研究 FacebookSDK 的功能，
發現在朋友的手機 iPhone3GS iOS 6.0 會出現下面這樣的錯誤：
```objc
ld: file is universal (3 clices) but doest not contain a(n) armv7s slice
```

有可能是跟 FacebookSDK 的版本有關，
但目前先透過下面的方式可以解決。
```objc
TARGETS > Build Settings > Valid Architectures 只留下 armv7 
```
