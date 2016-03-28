---
id: 227
title: 原生css根据宽度定义高度
date: 2014-03-05T19:42:56+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=227
permalink: /2014/03/05/%e5%8e%9f%e7%94%9fcss%e6%a0%b9%e6%8d%ae%e5%ae%bd%e5%ba%a6%e5%ae%9a%e4%b9%89%e9%ab%98%e5%ba%a6/
categories:
  - 未分类
---
最近做phonegap应用, 为了让应用精细到极致, 必然要把尺寸弄的非常细, 才能达到native的效果.

手机设备的宽度我们肯定要做成100%比例适应.但这么多设备的宽高比例不同, 而且像素也不同. 高度与宽度的单位就坚决不能用px了. 但高度怎么与宽度联系起来?除了javascript难道css不能解决?

这个问题貌似除了javascript没有其他的解决办法了.

直接上干货.

<pre><div class="box">
  <div class="content">
    1:1
  </div>
  
</div>
</pre>

原理上很狡猾.

<pre>.box{
	position: relative;
	width: 50%;		/* 你想要的 width */
}
.box:before{
	content: "";
	display: block;
	padding-top: 100%; 	/* 你想要的比例 1:1*/
}</pre>

box的height=0,在这里其实就是padding-top的高度是以宽度算的.也可以用margin-bottom来做.
  
如果想在content里写内容, 加上

<pre>.content{
         position: absolute;
         top: 0;
         bottom:0;
         left:0;
         right:0;
}
</pre>

效果:

<div class="t-box">
  <div class="t-content">
    1:1
  </div></p>
</div>