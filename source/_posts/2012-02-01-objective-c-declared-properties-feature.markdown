---
layout: post
title: "About Objective-C Declared Properties Feature"
date: 2012-02-01 21:54
comments: true
categories: Objective-C
---
剛開始接觸 iOS 應用程式開發時，
曾經對 @synthesize window = _window 所表達的意義有疑問，
查過資料卻沒記下來，
今天剛好同事問到，
所以這次一定要做一下筆記。

* __Property Declaratoin__
      @property (attributes) type name;

      @interface MyClass : NSObject
      @property float value;
      @end
@property float value; 相等於
      - (float)value;
      - (void)setValue:(float)newValue;
而 property 的宣告也可以放在 class extensions (也就是 anonymous categories)
      @interface MyClass : NSObject
      @property (retain, readonly) float value;
      @end

      @interface MyClass() // Private extension
      @property (retain, readwrite) float value;
      @end
* __Property Declaratoin Attributes__
      @property (attribute, [, attribute2, ...])
* __Accessor Method Names__
      getter=getterName
      setter=setterName
* __Writability__
  * readwrite
  * readonly
* __Setter Semantics__
  * strong
  * weak (OS X v10.6 and iOS 4 not support)
  * copy
  * assign (Default)
  * retain
* __Atomicity__
  * nonatomic
  * atomic (Default)
* __Property Implementation Directives__

用 property=ivar 可以指定特別的 instance variable 來讓 property 使用。
不指定的情況如下，@synthesize value; 其實等同於 @synthesize value = value;
      @interface MyClass : NSObject
      @property (copy, readwrite) NSString *value;
      @end

      @implementation MyClass
      @synthesize value;
      @end
有指定的情況如下：
      @implementation MyClass
      @synthesize firstName, lastName, age=yearsOld;
      @end
age 是用 yearsOld 這個 instance variable 來表示。
所以在 MyClass 內部使用時，可以用 yearsOld 來進行操作，
而其它類別如果使用 MyClass，則可以用 age 這個 property 來進行操作。

>如果要看更詳細的說明，可以參考官方文件[Declared Properties][1]


[1]: http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Chapters/ocProperties.html "ocProperties"


