---
layout: post
title: "Octopress的目錄結構"
date: 2012-01-14 00:21
comments: true
categories: Octopress
---
* octopress/
    - _config.yml 可以做基本設定修改
* octopress/plugins
    - 放置外掛程式
* octopress/source/_posts
    - 放置你寫的文章
* octopress/source/_includes/asides
    - 修改_config.yml -> rake generate 會在這邊產生html檔案
* octopress/source/new_page
    - rake new_page['new page'] 
* octopress/source/_includes/custom/navigation.html
    - 修改或加入link



