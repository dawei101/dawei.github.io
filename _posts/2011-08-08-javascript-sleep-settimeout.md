---
id: 36
title: 'javascript  sleep() setTimeout()'
date: 2011-08-08T11:22:23+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=36
permalink: /2011/08/08/javascript-sleep-settimeout/
categories:
  - 未分类
---
how to do a function like sleep() in javascript  
I have try lots way , this is the best and useful.  
the method is use setTimeout() in two or more functions , and make the functions execute each other. 

<pre>example: 
function doTimer()
{
    setTimeout("write1();", 2000);
}
function write1()
{
    alert("12345");
    //document.write("12345");
    setTimeout("write2();", 2000);
}
function write2()
{
    alert("67890");
    //document.write("67890");
    setTimeout("doTimer();", 2000);
}
doTimer();
</pre>