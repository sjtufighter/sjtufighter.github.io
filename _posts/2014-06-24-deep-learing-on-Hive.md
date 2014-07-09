---
layout: post
title:  "Hive Wiki:deep learning on Hive Architecture ,Sql analysis and Storage"
date:   2014-06-24
categories: 
- Notes 
tags:
- 分布式系统与计算
---

本文将对Hive的架构，Hive如何把一个输入的sql语句转化为mapreduce job，和关键词join group by  distinct等做一个较为详细的介绍，下面是以前做的slides，所以就贴在这里了，看起来可能会不太好理解，请见谅。

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


[image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片28.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片29.JPG)

![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片30.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片31.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片32.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片33.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片34.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片35.JPG)

![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片36.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片37.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片38.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片39.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片40.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片41.JPG)

[image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片42.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片43.JPG)

![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片44.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片45.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片46.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片47.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片48.JPG)
![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片49.JPG)

![image](https://raw.githubusercontent.com/sjtufighter/sjtufighter.github.io/master/images/hiveWiki1/幻灯片50.JPG)
