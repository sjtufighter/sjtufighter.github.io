---
layout: post
title:  "数据仓库hive中Skewed Join Optimization数据倾斜优化的原理"
date:   2014-06-25
categories: 
- Notes 
tags:
- 分布式系统与计算
---


问题来源：
A join of 2 large data tables is done by a set of MapReduce jobs which first sorts the tables based on the join key and then joins them. The Mapper gives all rows with a particular key to the same Reducer.
