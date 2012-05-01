---
layout: post
title: Ruby 程式語言摘要
date: 2012-04-24 21:33
comments: true
categories: Ruby
---
### 基本觀念 ###
- load 與 require
  - <code>load</code>: 是一個方法，所以可放在判斷式中，每載入 .rb 一次，就會重新執行 .rb 的內容，可指定相對或絕對路徑
  - <code>require</code>: 使用時可不加上 .rb 副檔名。與 load 不同的是，載入過的檔案，再次 require 並不會重複執行。
  - <code>$:</code> 表示預設路徑 (當不指定路徑或指定相對路徑時，從預設路徑尋找)；必要時，可在啟用 ruby 時加上 -I 指定路徑
```ruby
  load "../core.rb"
  load "C:/workspace/core.rb"
```
---
- 基本輸入輸出
  - <code>ARGV</code>: 使用者輸入的命令列引數，會收集成字串陣列給 ARGV 
  - <code>#{}</code>: 括號內指定變數名稱，直譯器會取得變數值
  - 預設輸出是 STDOUT，標準錯誤輸出是 STDERR，透過 STDERR.class 可知道是屬於 IO Class
  - <code>gets</code>: 此方法取得使用者輸入，含換行字元；如果要拿掉字串前後的空白字元(像是換行字元)，可透過字串物件的chomp
  - print, puts, printf(格式化輸出, 與C類似)
  - sprintf 用來格式化字串
  - 格式化字串的另一種方式：透過%與陣列
  - 透過 to_i, to_f 可轉換字串為整數、浮點數; 也可以透過 Integer, Float 
  - gets 是從標準輸入 STDIN 取得字串，讀取檔案，要用 File.read
  - File.read 會一次讀取檔案中的所有內容，逐行讀取要用 open 
```ruby
  filename = gets.chomp
  open(name, "w") do |file|
    file.print "test"
  end
```
---
### 內建型態與操作 ###
  - 數值型態
    - Numeric 的子類別有 Integer, Float
    - Integer 又包含 Fixnum, Bignum
    - 0b, 0, 0x
    - 如果整數超過 Fixnum 會自動轉為 Bignum
    - 字串、浮點數的 to_i 方法可建立整數；字串的to_i可指定2~36為基底
    - 字串、整數的 to_f 方法可建立浮點數
    - rounding error 可透過 bigdecimal 程式庫來解決
```ruby
1.9.2p290 :037 > "100".to_i
 => 100 
1.9.2p290 :038 > "100".to_i(16)
 => 256 
1.9.2p290 :053 >   0.1 + 0.2 == 0.3
 => false 
```
  - 字串型態
    - Ruby 中的字串，應該都是使用雙引號括住
    - 單引號括住的字串實際上是讓我們不必撰寫\\、\"忽略字元就可以代表\、"
    - 在單引號中，除了\\與\'之外，撰寫其它忽略字元都會被當作原始字串（Raw string）
    - 雙引號字串如果包含#{}，則<code>#{}</code> 中的內容會被當程式解釋
    - <code>%Q</code>: 建立雙引號字串
    - <code>%q</code>: 建立單引號字串
    - 在雙單引號中撰寫字串可直接換行，字串會自動加上\n
    - <code>+</code>: 串接字串
    - <code>*</code>: 重複字串
    - 字串為可變動的，可使用 <code><<</code> 在原字串後面附加字串
    - <code>ord</code>:取得字元編碼
    - <code>chr</code>:將編碼轉為字元
    - <code>[]</code>: 可指定索引取得字元
    - [n..m]: n 起始， m 結束，含 m
    - [n...m]: n 起始，m 結束，不含 m
    - [n, length]: n 起始，取長度 length
    - 字串沒!的方法表示以「新字串」傳回結果；有！的方法表示會「修改原字串」
```ruby
1.9.2p290 :057 > print 'ha\nk'
ha\nk => nil 
1.9.2p290 :058 > 'ha\nk'
 => "ha\\nk" 
1.9.2p290 :059 > print 'ha"nk'
ha"nk => nil 
1.9.2p290 :060 > 'ha"nk'
 => "ha\"nk" 1.9.2p290 :070 >   %Q{this is a book.}
 => "this is a book." 
1.9.2p290 :071 > %Q|this is a pen.|
 => "this is a pen."
1.9.2p290 :072 > "hank".length
 => 4 
1.9.2p290 :073 > "hank".each_char do |c|
1.9.2p290 :074 >     print c, "-"
1.9.2p290 :075?>   end
h-a-n-k- => "hank"
1.9.2p290 :076 > "hank" + "jane"
 => "hankjane" 
1.9.2p290 :077 > "hank" * 5
 => "hankhankhankhankhank" 
1.9.2p290 :081 >   "A".ord
 => 65 
1.9.2p290 :082 > 65.chr
 => "A" 
```
  - 關於編碼








---
References: 1. [良葛格 Ruby Gossip][1]  
[1]: http://caterpillar.onlyfun.net/Gossip/Ruby/index.html "Ruby Gossip"

