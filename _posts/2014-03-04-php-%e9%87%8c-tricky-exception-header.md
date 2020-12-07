---
id: 211
title: 'PHP 里, tricky Exception &#038;&#038; header'
date: 2014-03-04T16:43:28+00:00
author: dawei
layout: post
permalink: /2014/03/04/php-%e9%87%8c-tricky-exception-header/
categories:
  - Tech

---
Php 的header 方法是可以指定response的输出的, 在自定义header时, 如果设备不支持自定义header的话, 就需要在服务端response header 里添加自定义项到Access-Control-Allow-Headers.

最近在写项目的时候就发现一个非常非常tricky的算做是bug吧.

如果我throw exception 的话, 是应该扔出status是500 或400系列的response.

问题就来了, 假如:

if (没获得自定义header的值):

throw 一个 404 的exception.

无论怎么做, 这一段都会抛出异常的.原因就是, 假如是404的异常, 是不会接收到自定义的header的, 而没有自定义的header就会抛出异常.

逆向来看, 在throw exception语句的时候(至少是在此语句, 怀疑所有的header语句都可能生效), 不管运行是否有问题, 都会编译并搜寻header的信息, 并作为首要的输出.

实际上php作为一个脚本语言, 完全嵌入html的, 发展到现在,变的对象化. header应该是放在最最开始输出的.

如果你也碰到了相同的问题, 欢迎交流.
