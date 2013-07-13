---
layout: post
title: "Python版本管理工具"
date: 2013-07-13 23:13
comments: true
categories: Python
---
最近開始看 Python，首先就想要找一個類似 Ruby RVM 的工具，
一開始是看[virtualenv][2]，
但後來找到了 [pythonbrew][1]，
這是一個 Python Environment Manager。

安裝方式如下：
<code>curl -kL http://xrl.us/pythonbrewinstall | bash</code>

.bashrc 加入：
<code>[[ -s $HOME/.pythonbrew/etc/bashrc ]] && source $HOME/.pythonbrew/etc/bashrc</code>

* 常用的操作如下：
	* 顯示目前的 pythonbrew 版本 <code>pythonbrew version</code>
	* 更新 pythonbrew 至最新版本 <code>pythonbrew update</code>
	* 列出所有可安裝的 python 版本 <code>pythonbrew list -k</code>
	* 列出目前已安裝的 python 版本 <code>pythonbrew list</code>
	* 安裝指定的 python 版本 <code>pythonbrew install 3.2.3</code>
	* 移除指定的 python 版本 <code>pythonbrew uninstall 2.7.2 3.2.3</code>
	* 在目前 shell 使用某個指定的 python 版本 <code>pythonbrew use 3.2.3</code>
	* 永久切換至某個指定的 python 版本 <code>pythonbrew switch 3.2.3</code>
	* 切回系統的 python 環境 <code>pythonbrew off</code>
* 註：在 Mac 環境下，XCode的 command line tools 要記得安裝，否則 pythonbrew install 會出現錯誤訊息
```ruby
ERROR: Failed to install Python-3.2.3. See /Users/hank/.pythonbrew/log/build.log to see why.
```


[1]: https://github.com/utahta/pythonbrew "pythonbrew"
[2]: http://tech.mozilla.com.tw/posts/2155/python-%E9%96%8B%E7%99%BC%E5%A5%BD%E5%B9%AB%E6%89%8B-virtualenv "virtualenv"