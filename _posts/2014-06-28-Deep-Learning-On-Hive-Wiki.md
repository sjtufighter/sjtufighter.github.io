---
layout: post
title:  "Deep  Learning On Hive(Hive  Wiki)：Hive Architecture&&Hive Sql"
date:   2014-06-11
categories: 
- Notes 
tags:
- 分布式系统与计算
---

本文将对Hive的架构，Hive如何把一个输入的sql语句转化为mapreduce job做一个较为详细的介绍，下面是以前做的slides，所以就贴在这里了。

![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片1.JPG)

![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片2.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片3.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片4.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片5.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片6.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片8.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片9.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片10.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片11.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片12.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片13.JPG)

![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片14.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片15.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片16.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片17.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片18.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片19.JPG)

![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片20.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片21.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片22.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片23.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片24.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片25.JPG)

![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片26.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片27.JPG)


好了，下一篇文章我将对Hive的存储结构的发展演化做一个分析。
