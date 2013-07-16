---
layout: post
title: "(iOS)url encode / decode"
date: 2013-07-16 21:51
comments: true
categories: [Objective-C, iOS App]
---

網址含有中文時，要將網址先編碼。收到的時候再對應解碼即可。

```ruby
// 編碼
- (NSString *)stringByAddingPercentEscapesUsingEncoding:(NSStringEncoding)enc;
// 解碼
- (NSString *)stringByReplacingPercentEscapesUsingEncoding:(NSStringEncoding)enc;
```

NSStringEncoding 有很多類型，例如常用的：<code>NSUTF8StringEncoding</code>, <code>NSASCIIStringEncoding</code>, ...等。
