---
id: 17
title: OBJECTIVE-C 添加弹出UIView 背景锁定
date: 2012-04-12T17:03:04+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=17
permalink: /2012/04/12/objective-c-%e6%b7%bb%e5%8a%a0%e5%bc%b9%e5%87%bauiview-%e8%83%8c%e6%99%af%e9%94%81%e5%ae%9a/
categories:
  - 未分类
---
在   UIWindow 的 keyWindow 上 加上一层 透明的背景图， 或者删除背景图。使用就自己看下吧， 原理很简单。
  
副： 最近想试下游戏的开发玩， 不知道能不能有机会做。

<div class="wp_syntax">
  <table>
    <tr>
      <td class="code">
        <pre class="objc" style="font-family:monospace;">&nbsp;
<span style="color: #11740a; font-style: italic;">//</span>
<span style="color: #11740a; font-style: italic;">//  UIView+Extend.h</span>
<span style="color: #11740a; font-style: italic;">//  VIPStore</span>
<span style="color: #11740a; font-style: italic;">//</span>
<span style="color: #11740a; font-style: italic;">//  Created by dawei li on 12-4-11.</span>
<span style="color: #11740a; font-style: italic;">//  Copyright (c) 2012年 vipstore. All rights reserved.</span>
<span style="color: #11740a; font-style: italic;">//</span>
<span style="color: #6e371a;">#import 'UIKit/UIKit.h';</span>
<span style="color: #a61390;">@interface</span> UIView <span style="color: #002200;">&#40;</span>Extend<span style="color: #002200;">&#41;</span>
<span style="color: #002200;">-</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">void</span><span style="color: #002200;">&#41;</span> addSubviewWithBackgroundBlock<span style="color: #002200;">:</span><span style="color: #002200;">&#40;</span>UIView <span style="color: #002200;">*</span><span style="color: #002200;">&#41;</span> subview;
<span style="color: #002200;">-</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">void</span><span style="color: #002200;">&#41;</span> hiddenViewWithAnimation <span style="color: #002200;">:</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">float</span><span style="color: #002200;">&#41;</span> duration ;
<span style="color: #002200;">-</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">void</span><span style="color: #002200;">&#41;</span> hiddenViewWithAnimation<span style="color: #002200;">:</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">float</span><span style="color: #002200;">&#41;</span> duration andUnblockBackground<span style="color: #002200;">:</span><span style="color: #002200;">&#40;</span><span style="color: #a61390;">BOOL</span><span style="color: #002200;">&#41;</span> back;
<span style="color: #002200;">-</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">void</span><span style="color: #002200;">&#41;</span> hiddenViewAndUnblockBackground ;
<span style="color: #a61390;">@end</span>
<span style="color: #11740a; font-style: italic;">//</span>
<span style="color: #11740a; font-style: italic;">//  UIView+Extend.m</span>
<span style="color: #11740a; font-style: italic;">//  VIPStore</span>
<span style="color: #11740a; font-style: italic;">//</span>
<span style="color: #11740a; font-style: italic;">//  Created by dawei li on 12-4-11.</span>
<span style="color: #11740a; font-style: italic;">//  Copyright (c) 2012年 vipstore. All rights reserved.</span>
<span style="color: #11740a; font-style: italic;">//</span>
<span style="color: #6e371a;">#import "UIView+Extend.h"</span>
<span style="color: #a61390;">@implementation</span> UIView <span style="color: #002200;">&#40;</span>Extend<span style="color: #002200;">&#41;</span>
<span style="color: #002200;">-</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">void</span><span style="color: #002200;">&#41;</span> addSubviewWithBackgroundBlock<span style="color: #002200;">:</span><span style="color: #002200;">&#40;</span>UIView <span style="color: #002200;">*</span><span style="color: #002200;">&#41;</span> view
<span style="color: #002200;">&#123;</span>
  UIWindow <span style="color: #002200;">*</span>keyWindow <span style="color: #002200;">=</span> <span style="color: #002200;">&#91;</span><span style="color: #002200;">&#91;</span>UIApplication sharedApplication<span style="color: #002200;">&#93;</span> keyWindow<span style="color: #002200;">&#93;</span>;
  UIView <span style="color: #002200;">*</span> background<span style="color: #002200;">=</span><span style="color: #002200;">&#91;</span><span style="color: #002200;">&#91;</span><span style="color: #002200;">&#91;</span>UIView alloc<span style="color: #002200;">&#93;</span> initWithFrame<span style="color: #002200;">:</span>CGRectMake<span style="color: #002200;">&#40;</span><span style="color: #2400d9;"></span>,<span style="color: #2400d9;"></span>, keyWindow.frame.size.width,keyWindow.frame.size.height<span style="color: #002200;">&#41;</span><span style="color: #002200;">&#93;</span> autorelease<span style="color: #002200;">&#93;</span>;
  <span style="color: #002200;">&#91;</span>background setBackgroundColor<span style="color: #002200;">:</span><span style="color: #002200;">&#91;</span>UIColor colorWithWhite<span style="color: #002200;">:</span><span style="color: #2400d9;">0.6</span> alpha<span style="color: #002200;">:</span><span style="color: #2400d9;">0.4</span><span style="color: #002200;">&#93;</span><span style="color: #002200;">&#93;</span>;
  <span style="color: #002200;">&#91;</span>view setFrame<span style="color: #002200;">:</span>
CGRectMake<span style="color: #002200;">&#40;</span>view.frame.origin.x ,view.frame.origin.y<span style="color: #002200;">+</span>keyWindow.frame.size.height <span style="color: #002200;">-</span>self.frame.size.height , view.frame.size.width, view.frame.size.height<span style="color: #002200;">&#41;</span><span style="color: #002200;">&#93;</span>;
  <span style="color: #002200;">&#91;</span>background addSubview<span style="color: #002200;">:</span>view<span style="color: #002200;">&#93;</span>;
  <span style="color: #002200;">&#91;</span>keyWindow addSubview<span style="color: #002200;">:</span>background<span style="color: #002200;">&#93;</span>;
<span style="color: #002200;">&#125;</span>
<span style="color: #002200;">-</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">void</span><span style="color: #002200;">&#41;</span> hiddenViewWithAnimation<span style="color: #002200;">:</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">float</span><span style="color: #002200;">&#41;</span> duration
<span style="color: #002200;">&#123;</span>
  <span style="color: #002200;">&#91;</span>UIView beginAnimations<span style="color: #002200;">:</span><span style="color: #a61390;">nil</span> context<span style="color: #002200;">:</span><span style="color: #a61390;">nil</span><span style="color: #002200;">&#93;</span>;
  <span style="color: #002200;">&#91;</span>self setFrame<span style="color: #002200;">:</span>CGRectMake<span style="color: #002200;">&#40;</span><span style="color: #002200;">&#40;</span><span style="color: #2400d9;"></span><span style="color: #002200;">-</span>self.frame.origin.x<span style="color: #002200;">&#41;</span> , <span style="color: #002200;">&#40;</span><span style="color: #2400d9;"></span><span style="color: #002200;">-</span>self.frame.origin.y<span style="color: #002200;">&#41;</span> , self.frame.size.height,   self.frame.size.width<span style="color: #002200;">&#41;</span> <span style="color: #002200;">&#93;</span>;
  <span style="color: #002200;">&#91;</span>UIView setAnimationDuration<span style="color: #002200;">:</span>duration<span style="color: #002200;">&#93;</span>;
  <span style="color: #002200;">&#91;</span>UIView commitAnimations<span style="color: #002200;">&#93;</span>;
<span style="color: #002200;">&#125;</span>
<span style="color: #002200;">-</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">void</span><span style="color: #002200;">&#41;</span> hiddenViewWithAnimation<span style="color: #002200;">:</span> <span style="color: #002200;">&#40;</span><span style="color: #a61390;">float</span><span style="color: #002200;">&#41;</span> delay andUnblockBackground<span style="color: #002200;">:</span><span style="color: #002200;">&#40;</span><span style="color: #a61390;">BOOL</span><span style="color: #002200;">&#41;</span> back
<span style="color: #002200;">&#123;</span>
  <span style="color: #002200;">&#91;</span>self hiddenViewWithAnimation<span style="color: #002200;">:</span>0.1f<span style="color: #002200;">&#93;</span>;
  <span style="color: #a61390;">if</span> <span style="color: #002200;">&#40;</span>back<span style="color: #002200;">&#41;</span><span style="color: #002200;">&#123;</span>
    UIView <span style="color: #002200;">*</span> background <span style="color: #002200;">=</span> <span style="color: #002200;">&#91;</span>self superview<span style="color: #002200;">&#93;</span>;
    <span style="color: #002200;">&#91;</span>background performSelector<span style="color: #002200;">:</span><span style="color: #a61390;">@selector</span><span style="color: #002200;">&#40;</span>removeFromSuperview<span style="color: #002200;">&#41;</span> withObject<span style="color: #002200;">:</span><span style="color: #a61390;">nil</span> afterDelay<span style="color: #002200;">:</span>delay<span style="color: #002200;">&#93;</span>;
  <span style="color: #002200;">&#125;</span>
<span style="color: #002200;">&#125;</span>
<span style="color: #002200;">-</span><span style="color: #002200;">&#40;</span><span style="color: #a61390;">void</span><span style="color: #002200;">&#41;</span> hiddenViewAndUnblockBackground
<span style="color: #002200;">&#123;</span>
  <span style="color: #002200;">&#91;</span>self hiddenViewWithAnimation<span style="color: #002200;">:</span>0.0f andUnblockBackground<span style="color: #002200;">:</span><span style="color: #a61390;">YES</span><span style="color: #002200;">&#93;</span>;
<span style="color: #002200;">&#125;</span>
<span style="color: #a61390;">@end</span></pre>
      </td>
    </tr>
  </table>
</div>

\___\___\___\___\___\___\___
  
keyWindow 是不好做旋转的，如果要做旋转可以用 navgationConller.view 来添加弹出层。
  
当然还有一个更简单的办法， 可以直接 继承UIAlert 来实现，非常好用。
  
如果想禁用某控件的事件， 可以将 userInterfaceEnable （大约是，记不清）设置为NO就可以了。