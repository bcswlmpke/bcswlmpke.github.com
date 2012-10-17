---
layout: post
title: "UITableView 如何調整 Scroll Indicator 的位置？"
date: 2012-10-17 21:48
comments: true
categories: [Objective-C, iOS App]
---
嚴格說起來，應該是繼承自 UIScrollView 的都可以調整。
舉例來說，假設要顯示的是垂直方向的 Indicator (預設是開啟的)
```ruby 
_tableView.showsVerticalScrollIndicator = YES;
```
預設位置是在 TableView 的最右邊，假設想放到左邊，或是中間，要怎麼做？
答案是：<code>UIEdgeInsets</code>
```ruby
top, left, bottom, right
```
接下來要怎麼調整，就看個人需求囉！





