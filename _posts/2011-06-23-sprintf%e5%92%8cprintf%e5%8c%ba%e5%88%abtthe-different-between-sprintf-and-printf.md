---
id: 48
title: sprintf()和printf区别t|the different between sprintf() and printf()
date: 2011-06-23T16:45:54+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=48
permalink: /2011/06/23/sprintf%e5%92%8cprintf%e5%8c%ba%e5%88%abtthe-different-between-sprintf-and-printf/
categories:
  - 未分类
---
sprintf() 格式化输出函数(图形)  
功能:  
函数 sprintf()用来作格式化的输出。  
用法: 此函数调用方式为 int sprintf(char \*string,char \*format,arg_list);  
说明: 函数 sprintf()的用法和 printf()函数一样,只是 sprintf()函数给出  
第一个参数 string(一般为字符数组),然后再调用 outtextxy()函数将串里的  
字符显示在屏幕上。arg_list 为参数表,可有不定个数。通常在绘图方式下输  
出数字时可调用 sprintf()函数将所要输出的格式送到第一个参数,然后显示输  
出。  
函数名: sprintf  
功 能: 送格式化输出到字符串中  
用 法: int sprintf(char \*string, char \*farmat [,argument,...]); .  
程序例:  
#include  
#include  
int main(void)  
{  
char buffer[80];  
sprintf(buffer, "An approximation of Pi is %fn", M_PI);  
puts(buffer);  
return 0;  
}  
sprintf 的作用是将一个格式化的字符串输出到一个目的字符串中,而 printf  
是将一个格式化的字符串输出到屏幕。sprintf 的第一个参数应该是目的字符  
串,如果不指定这个参数,执行过程中出现  
"该程序产生非法操作,即将被  
关闭&#8230;."的提示。  
因为 C 语言在进行字符串操作时不检查字符串的空间是否够大,  
所以可能会出现  
数组越界而导致程序崩溃的问题。即使碰巧,程序没有出错,也不要这么用,因  
为早晚会出错。所以一定要在调用 sprintf 之前分配足够大的空间给 buf。  
php 中同样有这两个函数,对于用法就大同小异了。