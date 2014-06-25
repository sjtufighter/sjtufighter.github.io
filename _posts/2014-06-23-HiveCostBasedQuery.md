---
layout: post
title:  "hive cost based query"
date:   2014-06-23
categories: 
- Notes 
tags:
- 分布式系统与计算
---
apache hive是建立在hadoop之上的一个数据分析仓库。用户提交sql 查询到hive后被转换为物理查询算子后交给底层的mapreduce计算框架进行执行（TEZ是一个新的用于进行sql计算的需要依赖yarn的计算框架，这里不予讨论）

当前存在的hive 查询优化大都是关于最小化shuffling代价。当前，用户提交的join查询可以在优化之后以最佳的join顺序进行执行。hive当前逻辑上的查询优化仅仅局限于fliter push down机制，分区裁剪等。cost based的逻辑查询优化可以显著提高hive的查询效率，并且更加易用。

