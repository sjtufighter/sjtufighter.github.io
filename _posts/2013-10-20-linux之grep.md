---
layout: post
title:  "linux之grep 正则"
date:   2013-10-20
categories: 
- Notes 
tags:
- Linux
---

   正则表达式只是字符串的一种描述，只有和支持正则表达式的工具相结合才能进行字符串处理。本文以grep为例来讲解正则表达式。
   
   ##grep命令
功能：输入文件的每一行中查找字符串。

基本用法：
   grep [-acinv] [--color=auto] [-A n] [-B n] '搜寻字符串' 文件名
参数说明：
-a：将二进制文档以文本方式处理
-c：显示匹配次数
-i：忽略大小写差异
-n：在行首显示行号
-A：After的意思，显示匹配字符串后n行的数据
-B：before的意思，显示匹配字符串前n行的数据
-v：显示没有匹配行-A：After的意思，显示匹配部分之后n行-B：before的意思，显示匹配部分之前n行
--color：以特定颜色高亮显示匹配关键字
  
  –color选项是个非常好的选项，可以让你清楚的明白匹配了那些字符。最好在自己的.bashrc或者.bash_profile文件中加入：
  
  alias grep=grep --color=auto
  
   每次grep搜索之后，自动高亮匹配效果了。

     ‘搜寻字符串’是正则表达式，注意为了避免shell的元字符对正则表达式的影响，请用单引号（’’）括起来，千万不要用双引号括起来（"”）或者不括起来。

     正则表达式分为基本正则表达式和扩展正则表达式。下面分别简单总结一下。
     
     基本正则表达式
     正则表达式学习，主要是对正则表达式元数据的学习。正则表达式本身没有什么高深的东西，本文仅仅对基本正则表达式的元数据进行一下总结：

元数据

意义和范例

^word	搜寻以word开头的行。
例如：搜寻以#开头的脚本注释行

grep –n ‘^#’ regular.txt


word$	搜寻以word结束的行
例如，搜寻以‘.’结束的行

grep –n ‘.$’ regular.txt


.	匹配任意一个字符。
例如：grep –n ‘e.e’ regular.txt

匹配e和e之间有任意一个字符，可以匹配eee，eae，eve，但是不匹配ee。


\	转义字符。
例如：搜寻’，’是一个特殊字符，在正则表达式中有特殊含义。必须要先转义。

grep –n ‘\” regular.txt


*	前面的字符重复0到多次。
例如匹配gle，gogle，google，gooogle等等

grep –n ‘go*gle’ regular.txt


[list]	匹配一系列字符中的一个。
例如：匹配gl，gf。

grep –n ‘g[lf]’ regular.txt


[n1-n2]	匹配一个字符范围中的一个字符。
例如：匹配数字字符

grep –n ‘[0-9]’ regular.txt


[^list]	匹配字符集以外的字符
例如：grep –n ‘[^o]‘ regular.txt

匹配非o字符


\{n1,n2\}	前面的字符重复n1，n2次
例如：匹配google，gooogle。

grep –n ‘go\{2,3\}gle’ regular.txt


\<word	单词是的开头。
例如：匹配以g开头的单词

grep –n ‘\<g’ regular.txt


word\>	匹配单词结尾
例如：匹配以tion结尾的单词

grep –n ‘tion\>’ regular.txt


扩展正则表达式
     grep一般情况下支持基本正则表达式，可以通过参数-E支持扩展正则表达式，另外grep单独提供了一个扩展命令叫做egrep用来支持扩展正则表达式，这条命令和grep -E等价。虽然一般情况下，基本正则表达式就够用了。特殊情况下，复杂的扩展表达式，可以简化字符串的匹配。

     扩展正则表达式就是在基本正则表达式的基础上，增加了一些元数据。

元数据

意义和范例

+	重复前面字符1到多次。
例如：匹配god，good，goood等等字符串。

grep –nE go+d’ regular.txt


?	匹配0或1次前面的字符
例如，匹配gd，god

grep –nE ‘go?d’ regular.txt


|	或（or）的方式匹配多个字串  
例如：grep –nE ‘god|good’ regular.txt
匹配god或者good。


()	匹配整个括号内的字符串，原来都是匹配单个字符
例如：搜寻good或者glad

grep –nE ‘g(oo|la)’ regular.txt


()	前面的字符重复0到多次。
例如匹配gle，gogle，google，gooogle等等

grep –nE ‘go*gle’ regular.txt


     Linux下面正则表达式博大精深，上文支持总结了最常用的部分，如果熟练掌握的上面部分的正则表达式基本上可以满足日常使用了。

     另外Linux很多命令支持正则表达式，比如find，sed，awk等等。请在使用的时候参照这些命令的手册使用正则表达式。
     转载自http://www.cnblogs.com/xuxm2007/archive/2011/06/15/2081671.html
