---
id: 31
title: 'lftp Access failed error| tell you lftp&#8217; secrect'
date: 2011-09-09T18:27:10+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=31
permalink: /2011/09/09/lftp-access-failed-error-tell-you-lftp-secrect/
categories:
  - 未分类
---
I wrote a linux shell which can help me to upload files to the webserver these days .
  
I choose lftp beacuse it has one command &#8220;mirror&#8221; .
  
Access failed \****
  
lftp -u user,password host<command
  
&#8230;
  
&#8230;
  
bye
  
EOF
  
But I disturbed about the &#8220;Access failed&#8221; error ..
  
if you have the same problem ,
  
1check the lftp &#8216;s the path right especially sftp-home-path ?
  
2do you have permission to deal this file?
  
3 the lftp did not have enough command , like &#8220;mv&#8221; ,&#8221;rm&#8221;,&#8221;mrm&#8221; . if something was wrong, it would die. so you can do like this.

<pre>lftp -u user,password host&lt;command
bye
EOF
lftp -u user,password host&lt;notsure command{like - rm tmpfilename:it may not exist.}
bye
EOF
lftp -u user,password host&lt;command
bye
EOF

</pre>

ok that's all , Good luck.