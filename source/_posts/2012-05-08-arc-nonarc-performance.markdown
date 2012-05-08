---
layout: post
title: "ARC 與 非ARC 在大量資料的情況下的執行速度相差數倍"
date: 2012-05-08 21:49
comments: true
categories: [Objective-C, iOS App]
---
將手邊的 iOS 專案轉換為 ARC，
讓 XCode 於 Compile 時期安插必要的記憶體管理程式碼，
但是它所產生的程式碼是否與我們所想的一樣，
這個就未必了。

我們應該盡可能讓 XCode 幫我們產生正確的記憶體管理程式碼，
但必要時，我們也可以自己對程式碼進行改善！

下面舉一個例子，同樣的程式碼，只是在「ARC」與「非ARC」的情況下編譯執行，
但是兩者所需要的時間是相差數倍的！

```objc
  int n = 600000, m = 10000000;
    
    arr = [NSMutableArray new];
    
    for (int i = 0; i < n; i++)
        [arr addObject:[NSNumber numberWithInt:1]];
    
    CFAbsoluteTime start = CFAbsoluteTimeGetCurrent();
    
    id obj = nil;
    for (int j = 0; j < m; j++)
    {
        obj = [arr objectAtIndex:n - 1];
    }

    CFAbsoluteTime end = CFAbsoluteTimeGetCurrent();

    NSLog(@"end:%lf, start:%lf, diff:%lf", end, start, end - start);

    [arr release]; // -> 這一行是「非ARC」需要加上的，但「ARC」沒有這行
```

執行結果：
```objc
非ARC:  end:358178433.846184, start:358178433.753032, diff:0.093152
ARC:    end:358178929.480108, start:358178928.841418, diff:0.638690
可以看出「非ARC」的版本比「ARC」的版本快了近7倍！
```

但實際上程式是慢在哪呢？
-> 慢在 <code>obj = [arr objectAtIndex:n - 1];</code> 這邊，
XCode 在 Compile 的時候，幫我們安插了類似下面的程式，
```objc
  obj = [[arr objectAtIndex:n - 1] retain];
  [obj autorelease];

如果把「非ARC」的版本修改為上述的程式碼，
則執行結果：
非ARC:  end:358179022.496308, start:358179021.894909, diff:0.601399
是不是就變慢了！
```

因此，XCode 在 Compile 的時候，我想它對於程式的記憶體管理是採取較保守的態度，
如此看來，iOS 5 預設 property 為 strong 也就不意外了！

那麼上面所舉的例子要怎麼解決呢？
-> 我們可以透過 <code>Toll-Free Bridged Types</code> 來解決！
來看一下，我們將 ARC 的版本的程式碼改成下面這個樣子：
將<code>NSArray</code>改成使用<code>CFArrayRef</code>，
這是 Foundation class -> Core Foundation type 的轉換，
這樣的轉換是 Toll-Free 的！
```objc 
  __unsafe_unretained id obj;
  for (int j = 0; j < m; j++)
  {
      obj = (__bridge __unsafe_unretained id)CFArrayGetValueAtIndex((__bridge CFArrayRef)arr, n - 1);
  }
如果把「ARC」的版本修改為上述的程式碼，
則執行結果：
非ARC:  end:358179460.237259, start:358179460.004701, diff:0.232558
是不是就變快了！(但還沒有辦法跟原本的「非ARC」版本一樣快！)
```

所以其實寫程式的時候要多想一下有沒有其它作法，
因為不同的寫法雖然可能達成的結果相同，
但所需要的時間是不同的，
在使用 ARC 時，
如果能清楚的知道自己所創建的物件是被 retain 的狀態，
那麼在傳遞的過程中就可以視需求決定接收此物件是要 retain 或只是 assign，
這樣可以讓 XCode 在 Compile 的時候，
依照我們給它的指示去產生記憶體管理的程式碼，
避免不必要或多餘的效能損失！

