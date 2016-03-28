---
id: 42
title: mysql 存储过程|mysql procedure
date: 2011-07-07T10:49:29+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=42
permalink: /2011/07/07/mysql-%e5%ad%98%e5%82%a8%e8%bf%87%e7%a8%8bmysql-procedure/
categories:
  - 未分类
---
mysql存储过程的创建，删除，调用及其他常用命令.

mysql存储过程的创建，删除，调用及其他常用命令

# mysql 5.0存储过程学习总结

<span>一.创建存储过程</span>

1.基本语法：  
create procedure sp_name()  
begin  
………  
end 

2.参数传递

<span>二.调用存储过程</span>

1.基本语法：call sp_name()  
注意：存储过程名称后面必须加括号，哪怕该存储过程没有参数传递 

<span>三.删除存储过程</span>

1.基本语法：  
drop procedure sp_name//  
2.注意事项  
(1)不能在一个存储过程中删除另一个存储过程，只能调用另一个存储过程 

<span>四.区块，条件，循环<br /></span>

1.区块定义，常用  
begin  
……  
end;  
也可以给区块起别名，如：  
lable:begin  
………..  
end lable;  
可以用leave lable;跳出区块，执行区块以后的代码  
2.条件语句

<img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><span>if</span><span>&nbsp;条件&nbsp;</span><span>then</span><span><br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" />statement<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /></span><span>else</span><span><br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" />statement<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /></span><span>end</span><span>&nbsp;</span><span>if</span><span>;</span>

3.循环语句  
(1).while循环

<img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><span>[</span><span>label:</span><span>]</span><span>&nbsp;</span><span>WHILE</span><span>&nbsp;expression&nbsp;DO<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" />statements<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /></span><span>END</span><span>&nbsp;</span><span>WHILE</span><span>&nbsp;</span><span>[</span><span>label</span><span>]</span><span>&nbsp;;<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /></span>

(2).loop循环

<img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><span>[</span><span>label:</span><span>]</span><span>&nbsp;LOOP<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" />statements<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /></span><span>END</span><span>&nbsp;LOOP&nbsp;</span><span>[</span><span>label</span><span>]</span><span>;</span>

(3).repeat until循环

<img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><span>[</span><span>label:</span><span>]</span><span>&nbsp;REPEAT<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" />statements<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" />UNTIL&nbsp;expression<br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /><br /><img id="" src="http://m2.img.libdd.com/farm2/166/100D6EAF9536B88382D126CE89322FA6_11_16.GIF" /></span><span>END</span><span>&nbsp;REPEAT&nbsp;</span><span>[</span><span>label</span><span>]</span><span>&nbsp;;</span>

<span>五.其他常用命令</span>

1.show procedure status  
显示数据库中所有存储的存储过程基本信息，包括所属数据库，存储过程名称，创建时间等  
2.show create procedure sp_name  
显示某一个存储过程的详细信息

<span></span>

mysql存储过程中要用到的运算符

## mysql存储过程学习总结－操作符

**算术运算符**

+&nbsp;&nbsp;&nbsp;&nbsp; 加&nbsp;&nbsp; SET var1=2+2;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 4  
-&nbsp;&nbsp;&nbsp;&nbsp; 减&nbsp;&nbsp; SET var2=3-2;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1  
\*&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;乘&nbsp;&nbsp; SET var3=3\*2;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 6  
/&nbsp;&nbsp;&nbsp;&nbsp; 除&nbsp;&nbsp; SET var4=10/3;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3.3333  
DIV&nbsp;&nbsp; 整除&nbsp;SET var5=10 DIV 3;&nbsp; 3  
%&nbsp;&nbsp;&nbsp;&nbsp; 取模&nbsp;SET var6=10%3 ;&nbsp;&nbsp;&nbsp;&nbsp; 1

**比较运算符**

>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 大于&nbsp;1>2&nbsp;False  
<&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 小于&nbsp;2<1&nbsp;False  
<=&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 小于等于&nbsp;2<=2&nbsp;True  
>=&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 大于等于&nbsp;3>=2&nbsp;True  
BETWEEN&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 在两值之间&nbsp;5 BETWEEN 1 AND 10&nbsp;True  
NOT BETWEEN&nbsp; 不在两值之间&nbsp;5 NOT BETWEEN 1 AND 10&nbsp;False  
IN&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 在集合中&nbsp;5 IN (1,2,3,4)&nbsp;False  
NOT IN&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 不在集合中&nbsp;5 NOT IN (1,2,3,4)&nbsp;True  
=&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;等于&nbsp;2=3&nbsp;False  
<>, !=&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 不等于&nbsp;2<>3&nbsp;False  
<=>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 严格比较两个NULL值是否相等&nbsp;NULL<=>NULL&nbsp;True  
LIKE&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;简单模式匹配&nbsp;"Guy Harrison" LIKE "Guy%"&nbsp;True  
REGEXP&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 正则式匹配&nbsp;"Guy Harrison" REGEXP "[Gg]reg"&nbsp;False  
IS NULL&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 为空&nbsp;0 IS NULL&nbsp;False  
IS NOT NULL&nbsp; 不为空&nbsp;0 IS NOT NULL&nbsp;True 

**逻辑运算符**

**与**(AND)



AND

TRUE

FALSE

NULL

TRUE

TRUE

FALSE

NULL

FALSE

FALSE

FALSE

NULL

NULL

NULL

NULL

NULL

**或(OR)** 



OR

TRUE

FALSE

NULL

TRUE

TRUE

TRUE

TRUE

FALSE

TRUE

FALSE

NULL

NULL

TRUE

NULL

NULL

**异或(XOR)** 



XOR

TRUE

FALSE

NULL

TRUE

FALSE

TRUE

NULL

FALSE

TRUE

FALSE

NULL

NULL

NULL

NULL

NULL

**位运算符**

|&nbsp;&nbsp; 位或  
&&nbsp;&nbsp; 位与  
<<&nbsp; 左移位  
>>&nbsp; 右移位  
~&nbsp;&nbsp; 位非(单目运算，按位取反)

&nbsp;

mysq存储过程中常用的函数，字符串类型操作，数学类，日期时间类。

# mysql存储过程基本函数

<span>一.字符串类</span>&nbsp;

CHARSET(str) //返回字串字符集  
CONCAT (string2&nbsp; [,... ]) //连接字串  
INSTR (string ,substring ) //返回substring首次在string中出现的位置,不存在返回0  
LCASE (string2 ) //转换成小写  
LEFT (string2 ,length ) //从string2中的左边起取length个字符  
LENGTH (string ) //string长度  
LOAD\_FILE (file\_name ) //从文件读取内容  
LOCATE (substring , string&nbsp; [,start_position ] ) 同INSTR,但可指定开始位置  
LPAD (string2 ,length ,pad ) //重复用pad加在string开头,直到字串长度为length  
LTRIM (string2 ) //去除前端空格  
REPEAT (string2 ,count ) //重复count次  
REPLACE (str ,search\_str ,replace\_str ) //在str中用replace\_str替换search\_str  
RPAD (string2 ,length ,pad) //在str后用pad补充,直到长度为length  
RTRIM (string2 ) //去除后端空格  
STRCMP (string1 ,string2 ) //逐字符比较两字串大小,  
SUBSTRING (str , position&nbsp; [,length ]) //从str的position开始,取length个字符,  
<span>注：mysql中处理字符串时，默认第一个字符下标为1</span>，即参数position必须大于等于1  
mysql> select substring(‘abcd’,0,2);  
+———————–+  
| substring(‘abcd’,0,2) |  
+———————–+  
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |  
+———————–+  
1 row in set (0.00 sec)

mysql> select substring(‘abcd’,1,2);  
+———————–+  
| substring(‘abcd’,1,2) |  
+———————–+  
| ab&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |  
+———————–+  
1 row in set (0.02 sec) 

TRIM(\[[BOTH|LEADING|TRAILING\] \[padding\] FROM]string2) //去除指定位置的指定字符  
UCASE (string2 ) //转换成大写  
RIGHT(string2,length) //取string2最后length个字符  
SPACE(count) //生成count个空格&nbsp;

<span>二.数学类</span>

ABS (number2 ) //绝对值  
BIN (decimal_number ) //十进制转二进制  
CEILING (number2 ) //向上取整  
CONV(number2,from\_base,to\_base) //进制转换  
FLOOR (number2 ) //向下取整  
FORMAT (number,decimal_places ) //保留小数位数  
HEX (DecimalNumber ) //转十六进制  
注：HEX()中可传入字符串，则返回其ASC-11码，如HEX(‘DEF’)返回4142143  
也可以传入十进制整数，返回其十六进制编码，如HEX(25)返回19  
LEAST (number , number2&nbsp; [,..]) //求最小值  
MOD (numerator ,denominator ) //求余  
POWER (number ,power ) //求指数  
RAND([seed]) //随机数  
ROUND (number&nbsp; [,decimals ]) //四舍五入,decimals为小数位数]  
注：返回类型并非均为整数，如：  
(1)默认变为整形值  
mysql> select round(1.23);  
+————-+  
| round(1.23) |  
+————-+  
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1 |  
+————-+  
1 row in set (0.00 sec) 

mysql> select round(1.56);  
+————-+  
| round(1.56) |  
+————-+  
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2 |  
+————-+  
1 row in set (0.00 sec)

<span>(2)可以设定小数位数，返回浮点型数据</span>  
mysql> select round(1.567,2);  
+—————-+  
| round(1.567,2) |  
+—————-+  
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1.57 |  
+—————-+  
1 row in set (0.00 sec)

SIGN (number2 ) //返回符号,正负或0  
SQRT(number2) //开平方

&nbsp;  
<span>三.日期时间类</span>  
&nbsp;

ADDTIME (date2 ,time\_interval ) //将time\_interval加到date2  
CONVERT_TZ (datetime2 ,fromTZ ,toTZ ) //转换时区  
CURRENT_DATE (&nbsp; ) //当前日期  
CURRENT_TIME (&nbsp; ) //当前时间  
CURRENT_TIMESTAMP (&nbsp; ) //当前时间戳  
DATE (datetime ) //返回datetime的日期部分  
DATE\_ADD (date2 , INTERVAL d\_value d_type ) //在date2中加上日期或时间  
DATE_FORMAT (datetime ,FormatCodes ) //使用formatcodes格式显示datetime  
DATE\_SUB (date2 , INTERVAL d\_value d_type ) //在date2上减去一个时间  
DATEDIFF (date1 ,date2 ) //两个日期差  
DAY (date ) //返回日期的天  
DAYNAME (date ) //英文星期  
DAYOFWEEK (date ) //星期(1-7) ,1为星期天  
DAYOFYEAR (date ) //一年中的第几天  
EXTRACT (interval_name&nbsp; FROM date ) //从date中提取日期的指定部分  
MAKEDATE (year ,day ) //给出年及年中的第几天,生成日期串  
MAKETIME (hour ,minute ,second ) //生成时间串  
MONTHNAME (date ) //英文月份名  
NOW (&nbsp; ) //当前时间  
SEC\_TO\_TIME (seconds ) //秒数转成时间  
STR\_TO\_DATE (string ,format ) //字串转成时间,以format格式显示  
TIMEDIFF (datetime1 ,datetime2 ) //两个时间差  
TIME\_TO\_SEC (time ) //时间转秒数]  
WEEK (date\_time [,start\_of_week ]) //第几周  
YEAR (datetime ) //年份  
DAYOFMONTH(datetime) //月的第几天  
HOUR(datetime) //小时  
LAST_DAY(date) //date的月的最后日期  
MICROSECOND(datetime) //微秒  
MONTH(datetime) //月  
MINUTE(datetime) //分 

&nbsp;

附:可用在INTERVAL中的类型  
DAY ,DAY\_HOUR ,DAY\_MINUTE ,DAY\_SECOND ,HOUR ,HOUR\_MINUTE ,HOUR\_SECOND ,MINUTE ,MINUTE\_SECOND,MONTH ,SECOND ,YEAR&nbsp; 

&nbsp;