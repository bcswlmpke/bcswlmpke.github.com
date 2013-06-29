---
layout: post
title: "如何取消 git reset"
date: 2013-06-29 10:13
comments: true
categories: Git
---
假設原本有兩個 commit 如下

```ruby
$ git log
commit 98357430a2953262d75063d2711f6597dbdeb8bb
Author: Hank Chen
Date:   Sat Jun 29 11:03:27 2013 +0800

    edit file a

commit 8a50f7fb56411210cff2141c92a7a5bc2007adaa
Author: Hank Chen
Date:   Sat Jun 29 11:02:50 2013 +0800

    add file a
```

後來執行了 <code>git reset</code>, 且 <code>git log</code> 的確只剩下一個 commit 記錄

```ruby
$ git reset --soft 8a50f7fb56411210cff2141c92a7a5bc2007adaa

$ git log
commit 8a50f7fb56411210cff2141c92a7a5bc2007adaa
Author: Hank Chen
Date:   Sat Jun 29 11:02:50 2013 +0800

    add file a
```

但想要反悔剛剛的 <code>git reset</code>, 回到先前的 commit, 可以怎麼做？

先透過 git reflog 列出目前 branch 被更新的記錄

```ruby
$ git reflog
8a50f7f HEAD@{0}: reset: moving to 8a50f7fb56411210cff2141c92a7a5bc2007adaa
9835743 HEAD@{1}: commit: edit file a
8a50f7f HEAD@{2}: commit (initial): add file a
```

然後再透過 <code>git reset</code> 選擇回到前一個 commit

```ruby
$ git reset --hard 9835743
```

看 <code>git log</code> 的結果如下：

```ruby
$ git log
commit 98357430a2953262d75063d2711f6597dbdeb8bb
Author: Hank Chen
Date:   Sat Jun 29 11:03:27 2013 +0800

    edit file a

commit 8a50f7fb56411210cff2141c92a7a5bc2007adaa
Author: Hank Chen
Date:   Sat Jun 29 11:02:50 2013 +0800

    add file a
```