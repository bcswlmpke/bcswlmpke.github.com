---
layout: post
title: "不要相信retainCount，事實上你該怎麼做？"
date: 2012-02-03 21:29
comments: true
categories: Objective-C, iOS
---
在還沒有擁抱 iOS 5 ARC (Automatic Reference Counting) 之前，
我們必須自己管理程式所使用的記憶體，
在開發 iOS 的過程中踩過不少地雷，
整理出一些心得用來提醒自己，
如果有錯誤也請大家幫忙指正或留言一起討論！

* __產生物件者，記得做對應的 release__
  * new (alloc + init)
  * retain 
  * copy
* __addSubView 會將 retainCount +1，有兩種處理的時機__
```objc
    UILabel *label = [UILabel alloc] initWithFrame:self.bounds];
    [self addSubView:label];
    [label release];
```
```objc
    @implementation MyClass
        - (void)dealloc 
        {
            if (label != nil)
            {
                if (label.superview != nil)
                    [label removeFromSuperview];
                [label release];
                label = nil;
            }
            [super dealloc];
        }
    @end
```
* __Layer 的 Animation 或 setFrame 會暫時的增加 retainCount，直到動作完成__
* __如果你要使用 Thread，請記得用 Autorelease Pool 幫 Thread 做記憶體管理__
* __如果實作的類別要用 Delegate 時，Property 請記得用 assgin，如果用 retain 會造成 Circular Reference__
```objc
    @interface MyClass
    {
      id target;
    }
    @property (nonatomic, assgin) id target;
    @end
```
* __如果 Property 的屬性使用 retain，要記得 release 並設成 nil__
```objc
    @interface MyClass
    {
      NSString *name;
    }
    @property (nonatomic, retain) NSString *name;
    @end
    @implementation MyClass
    - (void)dealloc
    {
      self.name = nil; // 等同於 [name release]; name = nil;

      [super dealloc];
    }
    @end
```
> 更詳細的說明可以參考官方文件：[About Memory Management][1]

[1]: https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html "About Memory Management"
