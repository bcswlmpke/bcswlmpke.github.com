---
layout: post
title: 將專案轉換為ARC
date: 2012-05-01 20:28
comments: true
categories: [Objective-C, iOS App]
---
### Automatic Reference Counting ###
- ARC
  - XCode 4.2 開始支援
  - Preferences > General > enable Continue building after errors
  - Edit > Refactor > Convert to Objective-C ARC
  - using the Apple LLVM compiler
  - 如果轉換失敗，會提供錯誤訊息，等手動修正後，就可以重新啟動 Refactor ARC-conversion workflow
  ![Convert to ARC dialog][4]
  - 轉換成功後，你可以 review automatic changes，也可以做 snapshot 供之後回復(如果有需要)，然後選擇 Save 
### 將手上的專案轉成 ARC 的一些心得整理 ###
  - 不想要轉換為 ARC 的 source file 可加上 <code>-fno-objc-arc</code>
  - 不需要自己加上 <code>retain</code>、<code>autorelease</code>、<code>release</code>、<code>[super dealloc]</code>
  - Objects in the struct are unretained 
``` objc
typedef struct {
  __unsafe_unretained NSString *name;
} MyStruct;
```
  - 關於 weak
```objc
Becase of 「__weak」 references only work on iOS 5 and above. 
If you have the deployment target set to anything earlier, then you'll get the error.
Basically, if you want to deploy to earlier devices you can't use automated 「__weak」 references. 
The substitute would be 「__unsafe_unretained」.
```
  - NSAutoreleasePool 要修改如下
```objc
  [Before]
    NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];
    [pool drain];
  [After]
    @autoreleasepool {
    }
```
  - cast of C pointer type 'void *' to Objective-C pointer type 'NSObject *' requires a bridged cast <code>__bridge</code>
```objc
  NSObject* myObject = [[NSObject alloc] init];
  void* myObjectPointer = (__bridge void*)myObject;
     
  void* myObjectPointer2 = NULL;
  NSObject* myObject2 = (__bridge NSObject*)myObjectPointer2;
```
  - assign retained object to unsafe_unretained variable, object will be released after assignment
```objc
  請在 @property 加上 strong 
  或是在變數名稱前加上 __strong
```
  - 發生 leaking 怎麼辦？
```objc 
  如果發生類似 *** __NSAutoreleaseNoPool(): Object 0x648ad80 of class NSCFString autoreleased with no pool in place - just leaking 的訊息，
  可以透過 Profile -> Leak Tool 找出有問題的地方
```
  - 如果是 static method 回傳一個物件，要怎麼釋放？(以 C 所產生的物件為例)
```objc 
  CFUUIDRef theUUID = CFUUIDCreate(NULL);
	CFStringRef strUUID = CFUUIDCreateString(NULL, theUUID);
	CFRelease(theUUID);
	
	return (__bridge_transfer NSString *)strUUID; // 釋放原先所有權，將所有權交給 ARC 
```
  - id array member instance under ARC
```objc 
id<ITest> __strong *iArray = nil;
iArray = (id<ITest> __strong *)calloc(sizeof(id<ITest>), Size); 
```
### 一些不錯的參考連結 
  - Property declarations
```objc
「assign」 implies 「__unsafe_unretained」ownership.
「copy」 implies 「__strong」 ownership, as well as the usual behavior of copy semantics on the setter.
「retain」 implies 「__strong」 ownership.
「strong」 implies 「__strong」 ownership.
「unsafe_unretained」 implies 「__unsafe_unretained」 ownership.
weak implies __weak ownership.
```
  - Bridged casts
```objc
A bridged cast is a C-style cast annotated with one of three keywords:

(__bridge T) op casts the operand to the destination type T. If T is a retainable object pointer type, then op must have a non-retainable pointer type. If T is a non-retainable pointer type, then op must have a retainable object pointer type. Otherwise the cast is ill-formed. There is no transfer of ownership, and ARC inserts no retain operations.
(__bridge_retained T) op casts the operand, which must have retainable object pointer type, to the destination type, which must be a non-retainable pointer type. ARC retains the value, subject to the usual optimizations on local values, and the recipient is responsible for balancing that +1.
(__bridge_transfer T) op casts the operand, which must have non-retainable pointer type, to the destination type, which must be a retainable object pointer type. ARC will release the value at the end of the enclosing full-expression, subject to the usual optimizations on local values.

Using a __bridge_retained or __bridge_transfer cast purely to convince ARC to emit an unbalanced retain or release, respectively, is poor form.
```
  - [Automatic Reference Counting][1]
  - [Transitioning to ARC Release Notes][2]
  - [Q&A 2011-09-30: Automatic Reference Counting][3]


[1]: http://clang.llvm.org/docs/AutomaticReferenceCounting.html "Automatic Reference Counting"
[2]: http://developer.apple.com/library/ios/#releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226 "Transitioning to ARC Release Notes"
[3]: http://www.mikeash.com/pyblog/friday-qa-2011-09-30-automatic-reference-counting.html "Q&A 2011-09-30: Automatic Reference Counting"
[4]: http://farm8.staticflickr.com/7238/7131702787_9e4cdfcfe1_z.jpg "Convert to ARC dialog"
