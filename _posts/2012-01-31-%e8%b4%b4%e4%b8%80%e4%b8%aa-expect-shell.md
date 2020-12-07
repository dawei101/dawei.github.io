---
id: 24
title: 贴一个 expect shell
date: 2012-01-31T20:37:25+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=24
permalink: /2012/01/31/%e8%b4%b4%e4%b8%80%e4%b8%aa-expect-shell/
categories:
  - Tech

---
之前做的fleet 向远程服务器 上传文件的项目用到的一个shell。
  
shell有时候需要登录远程服务器进行操作， 这时候输入密码 就比较麻烦了。 当然你可以用其他办法处理。比如……………………
  
用 expect 可以很简单的处理这个问题。
  
本地每次 sudo的时候 输入是不是特别麻烦？ 改装一下自己写个shell 比如 autosu 可以直接切换为 root 用户 ，是不是很爽。 其他实现办法也很多。 大家参考下就好。

<pre>#!/usr/bin/expect #
proc do_command {command} {
  set accum {}
  send "$commandn"
  expect {
    -regexp {..*}
    {
      set accum "${accum}$expect_out(0,string)"
      set timeout 2
      exp_continue
    }
  }
}
set ip [lindex $argv 0]
set user [lindex $argv 1]
set passwd [lindex $argv 2]
set cmd [lindex $argv 3]
spawn ssh -l $user $ip
sleep 1
expect "*yes/n*" {
send "yesr"
expect "*password:"
send "$passwdr"} "*passwor*" { send "$passwdr" }
sleep 1
do_command ${cmd}
send "exitr"
</pre>

当然你也可以把用户加到sudoer里, 但这个需要运维操作, 很麻烦, 我就先这样用了.
