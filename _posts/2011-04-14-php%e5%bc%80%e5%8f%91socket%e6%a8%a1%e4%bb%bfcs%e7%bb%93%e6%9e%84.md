---
id: 66
title: php开发socket模仿c/s结构
date: 2011-04-14T00:29:18+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=66
permalink: /2011/04/14/php%e5%bc%80%e5%8f%91socket%e6%a8%a1%e4%bb%bfcs%e7%bb%93%e6%9e%84/
categories:
  - 未分类
---
下面是一个 php开发socket的 例子。  
服务器  
// server  
// 设置错误处理  
error\_reporting (e\_all);  
// 设置运行时间  
set\_time\_limit (0);  
// 起用缓冲  
ob\_implicit\_flush ();  
$ip = "127.0.0.1"; // ip地址  
$port = 1000; // 端口号  
$socket = socket\_create (af\_inet, sock\_stream, sol\_tcp); // 创建一个socket  
if ($socket)  
echo "socket_create() successed!n";  
else  
echo "socket\_create() failed:".socket\_strerror ($socket)."n";  
$bind = socket_bind ($socket, $ip, $port); // 绑定一个socket  
if ($bind)  
echo "socket_bind() successed!n";  
else  
echo "socket\_bind() failed:".socket\_strerror ($bind)."n";  
$listen = socket_listen ($socket); // 间听socket  
if ($listen)  
echo "socket_listen() successed!n";  
else  
echo "socket\_listen() failed:".socket\_strerror ($listen)."n";  
while (true)  
{  
$msg = socket_accept ($socket); // 接受一个socket  
if (!$msg)  
{  
echo "socket\_accept() failed:".socket\_strerror ($msg)."n";  
break;  
}  
$welcome = "welcome to php server!n";  
socket_write ($msg, $welcome, strlen ($welcome));  
while (true)  
{  
$command = strtoupper (trim (socket_read ($msg, 1024)));  
if (!$command)  
break;  
switch ($command)  
{  
case "hello":  
$writer = "hello everybody!";  
break;  
case "quit":  
$writer = "bye-bye";  
break;  
case "help":  
$writer = "hellotquitthelp";  
break;  
default:  
$writer = "error command!";  
}  
socket_write ($msg, $writer, strlen ($writer));  
if ($command == "quit")  
break;  
}  
socket_close ($msg);  
}  
socket_close ($socket); // 关闭socket  
?>  
客户端  
// client  
// 设置错误处理  
error\_reporting (e\_all);  
// 设置处理时间  
set\_time\_limit (0);  
$ip = "127.0.0.1"; // ip 地址  
$port = 1000; // 端口号  
$socket = socket\_create (af\_inet, sock\_stream, sol\_tcp); // 创建一个socket  
if ($socket)  
echo "socket_create() successed!n";  
else  
echo "socket\_create() failed:".socket\_strerror ($socket)."n";  
$conn = socket_connect ($socket, $ip, $port); // 建立socket的连接  
if ($conn)  
echo "success to connection![".$ip.":".$port."]n";  
else  
echo "socket\_connect() failed:".socket\_strerror ($conn)."n";  
echo&nbsp;socket_read ($socket, 1024);  
$stdin = fopen (’php://stdin’, ’r’);  
while (true)  
{  
$command = trim (fgets ($stdin, 1024));  
socket_write ($socket, $command, strlen ($command));  
$msg = trim (socket_read ($socket, 1024));  
echo $msg."n";  
if ($msg == "bye-bye")  
break;  
}  
fclose ($stdin);  
socket_close ($socket);  
?>