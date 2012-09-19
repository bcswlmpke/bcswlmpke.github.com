---
layout: post
title: "Tap手勢的處理"
date: 2012-09-19 23:11
comments: true
categories: [Objective-C, iOS App]
---
要處理 Single Tap 與 Double Tap 的時候, 
除了下面這行要設定以外
```objc 
  [_singleTap requireGestureRecognizerToFail:_doubleTap];
```
還必須設定 
```objc 
  [_singleTap setDelaysTouchesBegan:YES];
  [_doubleTap setDelaysTouchesBegan:YES];
```
否則當 Double Tap trigger 時，也會觸發 Single Tap.


