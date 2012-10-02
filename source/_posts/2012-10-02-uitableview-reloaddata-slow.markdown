---
layout: post
title: "UITableView 顯示上萬筆資料時，reloadData 很慢的問題"
date: 2012-10-02 22:09
comments: true
categories: [Objective-C, iOS App]
---
執行 UITableView reloadData，實作 numberOfRowsInSection 回傳 10000：
```objc 
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
```
但實際可視畫面的範圍可能只有 10 列資料顯示，
如果 heightForRowAtIndexPath 有實作，是會被呼叫 10000 次的。
```objc 
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
```
- 當資料量愈多，UI 實際顯示前的所需時間會愈久，
  - 解決方式為不要去覆寫 heightForRowAtIndexPath，
    而是直接設定 UITableView 身上的 rowHeight property。





