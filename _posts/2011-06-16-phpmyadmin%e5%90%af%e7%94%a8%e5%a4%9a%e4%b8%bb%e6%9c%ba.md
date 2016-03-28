---
id: 53
title: phpMyAdmin启用多主机
date: 2011-06-16T13:58:24+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=53
permalink: /2011/06/16/phpmyadmin%e5%90%af%e7%94%a8%e5%a4%9a%e4%b8%bb%e6%9c%ba/
categories:
  - 未分类
---
phpMyAdmin启用多主机，phpmyadmin 自定义填写主机：  
之前做过很多，比如把模板里加上一些东西，cookie里设置用户名等等。  
但其实phpmyadmin里自带有此功能的。  
找到 phpmyadmin目录下的 libraries/config.default.php
  


> \# /\* AllowArbitraryServer \*/  
> \# $cfg['AllowArbitraryServer'] = true</p>
即可