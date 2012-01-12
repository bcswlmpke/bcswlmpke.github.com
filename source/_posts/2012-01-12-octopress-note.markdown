---
layout: post
title: "How to use Octopress"
date: 2012-01-12 21:53
comments: true
categories: Octopress
---
* 安裝 
請參考[Octopress官網][1]

* 用 Markdown 寫文章
rake new_post[文章名稱]
請參考[Markdown中文文件][2]

* 發佈
git checkout source
git push origin source
rake generate
rake deploy


[1]: http://octopress.org/docs/setup/ "Octopress Setup"
[2]: http://markdown.tw "Markdown Syntax"


