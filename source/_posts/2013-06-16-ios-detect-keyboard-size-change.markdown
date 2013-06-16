---
layout: post
title: "(iOS)偵測鍵盤大小變化"
date: 2013-06-16 13:50
comments: true
categories:  [Objective-C, iOS App]

---

在 iOS 中，輸入時可以選擇英文或中文鍵盤，
例如中文的使用者也可以新增注意鍵盤，
而中文注意鍵盤的大小跟一般英文鍵盤的大小不同，
如果擔心鍵盤的大小會擋到 UI，
可以透過某些方式來得知鍵盤大小並進而調整 UI Layout。

```objc
// 註冊 UIKeyboardDidShowNotification 可以獲得 keyboard 的大小
[[NSNotificationCenter defaultCenter] 
  addObserver:self
  selector:@selector(keyboardDidShow:)
  name:UIKeyboardDidShowNotification 
  object:nil];

- (void)keyboardWasShown:(NSNotification*)notification 
{
  // userInfo提供了一些 keyboard 的訊息
  // UIKeyboardFrameBeginUserInfoKey
  // UIKeyboardFrameEndUserInfoKey
  // UIKeyboardAnimationDurationUserInfoKey
  // UIKeyboardAnimationCurveUserInfoKey
  NSDictionary* info = [notification userInfo];
  CGSize kbSize = [[info objectForKey:UIKeyboardFrameEndUserInfoKey] CGRectValue].size;
}
```
