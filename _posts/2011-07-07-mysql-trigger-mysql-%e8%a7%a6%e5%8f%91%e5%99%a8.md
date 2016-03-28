---
id: 43
title: Mysql trigger | Mysql 触发器
date: 2011-07-07T09:56:09+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=43
permalink: /2011/07/07/mysql-trigger-mysql-%e8%a7%a6%e5%8f%91%e5%99%a8/
categories:
  - 未分类
---
本示例实现如下效果：  
0.test数据库有userinfo用户信息表 和userinfolog用户信息日志表  
1.建立一个userinfo表新增记录时的触发器 将新增日志加入到userinfolog  
2.建立一个向userinfo表新增记录的存储过程  
3.根据userinfo表的出生日期字段 我们将建立一个简单算得年龄的自定义函数  
4.创建一个userinfo的视图 调用年龄函数  
－－－－－－－－－－－－－  
0.准备相关表  
mysql> use test;  
mysql> create table userinfo(userid int,username varchar(10),userbirthday date);  
mysql> create table userinfolog(logtime datetime,loginfo varchar(100));  
mysql> describe userinfo;  
1.触发器  
mysql> delimiter |  
mysql> create trigger beforeinsertuserinfo   
-> before insert on userinfo   
-> for each row begin   
-> insert into userinfolog values(now(),CONCAT(new.userid,new.username));   
-> end;   
-> |  
mysql> delimiter ;  
mysql> show triggers;  
2.存储过程  
mysql> delimiter //  
mysql> create procedure spinsertuserinfo(   
-> puserid int,pusername varchar(10)   
-> ,puserbirthday date   
-> )   
-> begin   
-> insert into userinfo values(puserid,pusername,puserbirthday);   
-> end;   
-> //  
mysql> show procedure status like &#8216;spinsertuserinfo&#8217;;  
mysql> call spinsertuserinfo(1,&#8217;zhangsan&#8217;,current_date);  
mysql> select * from userinfo;  
3.自定义函数  
mysql> update userinfo   
-> set userbirthday=&#8217;2000.01.01&#8242;   
-> where userid=&#8217;1&#8242;;  
mysql> drop function if exists fngetage;  
mysql> delimiter //  
mysql> create function fngetage(pbirthday date)   
-> returns integer   
-> begin   
-> return year(now()) &#8211; year(pbirthday);   
-> end   
-> //  
4.视图  
mysql> create view viewuserinfo   
-> as select * ,fngetage(userbirthday) as userage from userinfo;  
mysql> select * from viewuserinfo;  
清除日志记录  
mysql> truncate table userinfolog;  
mysql> delete from userinfolog;