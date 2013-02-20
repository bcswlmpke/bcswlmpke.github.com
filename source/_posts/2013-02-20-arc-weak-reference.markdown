---
layout: post
title: "iOS 5 以下不支援 weak reference 遇到的問題"
date: 2013-02-20 20:26
comments: true
categories: [Objective-C, iOS App]
---
假設 Application 必須支援 iOS 4.3 的版本，
且是在使用 ARC 的情況下，
由於 iOS 5 才開始支援 weak reference，
那麼在 iOS 5 以下的版本，
可能會遇到下面這段程式有可能 crash 的問題：
```ruby
dispatch_async(dispatch_get_main_queue(), ^{
	NSUInteger count = [self.dataSource numberOfItems:self];
});
```
首先，
因為 weak 屬性在 iOS 4.3 不支援，
所以 dataSource 是一個屬性設為 assign 的 property，
numberOfItems 是一個 delegate 由其它物件進行實作，
如果在開始執行 dispatch_async 這個 block 內的動作之前，
dataSource 已經被釋放了，
這時候輪到執行 block 內的動作時，
會去 retain dataSource，
但因為 dataSource 已經被釋放，所以這個 retain 的動作會造成 crash。

如果 dataSource 這個 property 的屬性為 weak，
那個在 dataSource 被釋放時，
這個 property 就會被設成 nil。
此時應該不會造成 crash 才對。(不曉得這樣的理解有沒有錯)

但因為 main thread 可能處於忙碌狀態，
所以 <code>dispatch_async(dispatch_get_main_queue()</code>的動作
未必會馬上被執行，就有機會遇到上述的問題。

在 iOS 4.3 除了 [MAZeroingWeakRef][1] 這個方式外，
不曉得在實作上，
大家是用什麼方式來避免上述的情況發生？

[1]: https://github.com/mikeash/MAZeroingWeakRef "MAZeroingWeakRef"


