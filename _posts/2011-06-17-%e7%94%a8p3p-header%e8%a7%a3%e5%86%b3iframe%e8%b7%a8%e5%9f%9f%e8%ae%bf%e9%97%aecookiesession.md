---
id: 50
title: 用P3P header解决iframe跨域访问cookie/session
date: 2011-06-17T15:32:02+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=50
permalink: /2011/06/17/%e7%94%a8p3p-header%e8%a7%a3%e5%86%b3iframe%e8%b7%a8%e5%9f%9f%e8%ae%bf%e9%97%aecookiesession/
categories:
  - 未分类
---
理论很简单,而且模式也和大多请求返回状态的SSO差不多.但是有几个地方是要注意一下的.  
1.页面里的COOKIE不能是浏览器进程的COOKIE(包括验证票和不设置超时时间的COOKIE),否则跨域会取不到.这点做跨域COOKIE的人比较少提到.不过实际上留意下几家大学做的方案,有细微的提到他们的验证模块里的COOKIE是有设置超时时间的.  
2.当利用IFRAME时,记得要在相应的动态页的页头添加一下P3P的信息,否则IE会自觉的把IFRAME框里的COOKIE给阻止掉,产生问题.本身不保存自然就取不到了.这个其实是FRAMESET和COOKIE的问题,用FRAME或者IFRAME都会遇到.  
3.测试时输出TRACE,会减少很多测试的工作量.  
只需要设置 P3P HTTP Header，在隐含 iframe 里面跨域设置 cookie 就可以成功。他们所用的内容是：  
P3P: CP=&#8217;CURa ADMa DEVa PSAo PSDo OUR BUS UNI PUR INT DEM STA PRE COM NAV OTC NOI DSP COR&#8217;  
php如下写法：  
header(&#8216;P3P: CP=CAO PSA OUR&#8217;);