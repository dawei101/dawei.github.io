---
id: 52
title: combo在服务端合并和压缩JavaScript和CSS文件
date: 2011-06-16T14:55:56+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=52
permalink: /2011/06/16/combo%e5%9c%a8%e6%9c%8d%e5%8a%a1%e7%ab%af%e5%90%88%e5%b9%b6%e5%92%8c%e5%8e%8b%e7%bc%a9javascript%e5%92%8ccss%e6%96%87%e4%bb%b6/
categories:
  - 未分类
---
Web性能优化最佳实践中最重要的一条是减少HTTP请求，它也是YSlow中比重最大的一条规则。减少HTTP请求的方案主要有合并JavaScript和CSS文件、CSS Sprites、图像映射（Image Map）和使用Data URI来编码图片。CSS Sprites和图像映射现在已经随处可见了，但由于IE6和IE7不支持Data URI以及性能问题，这项技术尚未大量使用。目前大部分网页中的JavaScript和CSS文件数量和开发时一致，少量的网页会根据实际情况采取本地合并，这些合并中相当多的是有选择地手动完成，每次新的合并都需要重新在本地完成并上传到服务器，比较的随意和繁琐，同样文件的压缩也有类似的情况。而利用服务端的合并和压缩，我们就可以按照开发的逻辑尽可能让文件的颗粒度变小，利用网页中URL的规则来自动实现文件的合并和压缩，这会相当的灵活和高效。  
YUI Combo Handler  
2008年7月YUI Team宣布在YAHOO! CDN上对YUI JavaScript组件提供Combo Handler服务。Combo Handler是Yahoo!开发的一个Apache模块，它实现了开发人员简单方便地通过URL来合并JavaScript和CSS文件，从而大大减少文件请求数。比如在页面上使用YUI2的Rich Text Editor组件需要引入多个JavaScript文件，常用方式如下：
  


> 

而使用Combo Handler服务之后，则上述的代码可以写为：
  


> 
除了代码的可读性稍稍有一点点降低外，使用Combo Handler服务大大的降低了HTTP请求数，同时也减少了URL代码量，这对于Web性能优化来讲至关重要。所以，随后YUI从2.6.0开始，其核心组件YUI Loader内置了Combo Handling功能，即使用YUI Loader时，通过配置combine属性就可以把要加载的多个JavaScript或CSS文件按照使用Combo Handler服务的形式合并起来，这时只要静态文件的服务器支持Combo Handler就行了。在YUI中当combine配置为true时，CDN默认是使用Yahoo! CDN（http://yui.yahooapis.com），所以没有任何问题。这正是YUI最迷人的地方之一。  
遗憾的是http://yui.yahooapis.com在中国的速度并不佳，本来中国雅虎提供http://cn.yui.yahooapis.com/ ，但尚未提供Combo Handler服务，同时因种种原因，其更新在YUI 2.7.0之后就停滞了。更糟糕的是Yahoo!开发的支持Combo Handler的Apache模块虽然据传有计划开源，但至少现在依旧是私有技术，要使用就需要自己实现类似功能，所以国内类似技术的应用并不太多。