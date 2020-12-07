---
id: 23
title: 技术细节| weibo与douban
date: 2012-02-15T23:20:50+00:00
author: dawei
layout: post
permalink: /2012/02/15/%e6%8a%80%e6%9c%af%e7%bb%86%e8%8a%82-weibo%e4%b8%8edouban/
categories:
  - Tech

---
大家可以看下在所有的网站退出的链接
  
小小的技巧，谈不上漏洞，但可以商业上利用。
  
以weibo为例：http://weibo.com/logout.php?backurl=/&topnav=1
  
这是某个退出的链接，也就是说 只要打开http://weibo.com/logout.php 链接就可以退出
  
再以豆瓣为例：http://www.douban.com/accounts/logout?ck=eAPQ
  
这个链接有个ck的参数，检查是不是我生成的链接，也就是退出的时候判断一次验证，所以打开http://www.douban.com/accounts/logout 是无法退出的。
  
如果在不是weibo的页面里加上 或这有对这个链接的弹窗，会怎么样呢？

> 在Appannie技术分享的时候了解到, 2013年黑帽大会上, 爆出的料: 只要你打开了网页, 所有的操作(比如银联支付)都可以被这个网页所操纵, 比如我可以把内容放到一个屏幕以外的位置, 然后模拟鼠标事件去操作.
