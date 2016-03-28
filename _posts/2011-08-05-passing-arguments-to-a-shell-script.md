---
id: 37
title: Passing arguments to a shell script
date: 2011-08-05T10:35:52+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=37
permalink: /2011/08/05/passing-arguments-to-a-shell-script/
categories:
  - 未分类
---
Any shell script you run has access to (inherits) the environment variables accessible to its parent shell. In addition, any arguments you type after the script name on the shell command line are passed to the script as a series of variables.  
The following parameters are recognized:  
$*   
Returns a single string (&#8220;$1, $2 &#8230; $n&#8221;) comprising all of the positional parameters separated by the internal field separator character (defined by the IFS environment variable).  
$@   
Returns a sequence of strings (&#8220;$1&#8221;, &#8220;$2&#8221;, &#8230; &#8220;$n&#8221;) wherein each positional parameter remains separate from the others.  
$1, $2 &#8230; $n   
Refers to a numbered argument to the script, where n is the position of the argument on the command line. In the Korn shell you can refer directly to arguments where n is greater than 9 using braces. For example, to refer to the 57th positional parameter, use the notation ${57}. In the other shells, to refer to parameters with numbers greater than 9, use the shift command; this shifts the parameter list to the left. $1 is lost, while $2 becomes $1, $3 becomes $2, and so on. The inaccessible tenth parameter becomes $9 and can then be referred to.  
$0   
Refers to the name of the script itself.  
$#   
Refers to the number of arguments specified on a command line.  
For example, create the following shell script called mytest:   
echo There are $# arguments to $0: $*   
echo first argument: $1   
echo second argument: $2   
echo third argument: $3   
echo here they are again: $@  
When the file is executed, you will see something like the following:   
$ mytest foo bar quux   
There are 3 arguments to mytest: foo bar quux   
first argument: foo   
second argument: bar   
third argument: quux   
here they are again: foo bar quux  
$# is expanded to the number of arguments to the script, while $* and $@ contain the entire argument list. Individual parameters are accessed via $0, which contains the name of the script, and variables $1 to $3 which contain the arguments to the script (from left to right along the command line).  
Although the output from $@ and $* appears to be the same, it may be handled differently, as $@ lists the positional parameters separately rather than concatenating them into a single string. Add the following to the end of mytest:   
function how_many {   
print "$# arguments were supplied."   
}   
how_many "$*"   
how_many "$@"  
The following appears when you run mytest:   
$ mytest foo bar quux   
There are 3 arguments to mytest: foo bar quux   
first argument: foo   
second argument: bar   
third argument: quux   
here they are again: foo bar quux   
1 arguments were supplied.   
3 arguments were supplied.