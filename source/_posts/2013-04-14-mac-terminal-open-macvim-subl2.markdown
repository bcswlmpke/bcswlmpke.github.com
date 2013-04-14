---
layout: post
title: "Mac Terminal 開啟 MacVim, Sublime Text 2"
date: 2013-04-14 21:02
comments: true
categories: [Vim, Subl2]
---
```ruby
sudo ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/bin/subl
```

* Sublime Text 2
	* 編輯文件:<code>subl file</code>
	* 打開目錄:<code>subl ..</code>

```ruby
#!/bin/sh

open -a /Applications/MacVim.app/Contents/MacOS/MacVim $@
```

* MacVim
	* 將 mvim script 放到 /usr/local/bin/
	* 編輯文件:<code>mvim file</code>


