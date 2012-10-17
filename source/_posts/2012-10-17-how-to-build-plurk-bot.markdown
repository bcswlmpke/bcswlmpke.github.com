---
layout: post
title: "如何寫一個噗浪機器人"
date: 2012-10-17 22:02
comments: true
categories: Plurk 
---

- 步驟1：註冊新的噗浪帳號，並取得四項參數，可以參考[噗浪機器人範例程式 - 使用 Plurk API 2.0][1]
  - App Key (Consumer Key)
  - App Secret (Consumer Secret)
  - Access Token (Token Key)
  - Access Token Secret (Token Secret)
- 步驟2：範例測試
  - 參考上面提到的網址，用 Python 來實驗，置換四個參數
  - 下載 Library 
    - (1) [plurk-oauth][2] 
    - (2) [python-oauth2 library][3]
    - (3) [httplib2 client library in Python][4] <code>sudo python setup.py install</code>
  - 將 python-oauth2 的 oauth2 目錄放到與 plurk-oauth 同一層即可。
  - 執行範例，發一則含 <code>hello</code>字樣的噗，機器人會回 <code>world</code>


[1]: http://blog.urdada.net/2011/10/28/426/ "噗浪機器人範例程式 - 使用 Plurk API 2.0"
[2]: https://github.com/clsung/plurk-oauth "plurk-oauth"
[3]: https://github.com/simplegeo/python-oauth2 "python-oauth2"
[4]: http://code.google.com/p/httplib2/ "httplib2"


