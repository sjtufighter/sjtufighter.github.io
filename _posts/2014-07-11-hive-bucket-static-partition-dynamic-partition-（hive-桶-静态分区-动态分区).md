---
layout: post
title:  "data warehouse:hive bucket static partition dynamic partition 桶 静态分区 动态分区"
date:   2014-07-11
categories: 
- Notes 
tags:
- 分布式系统与计算
---

hive引入partition和bucket的概念。一般来说，这两个概念都是把数据划分成块，分区是粗粒度的划分桶是细粒度的划分（如果是动态分区的话需要考虑key值的离散情况以及设定最大buckets数目），这样做为了可以让查询发生在小范围的数据上以提高效率。
分区又分为静态分区和动态分区：
Static Partition (SP) columns:静态分区的分区key值在sql编译**COMPILE TIME**的阶段就已近知道了。
Dynamic Partition (DP) columns:动态分区的列值只能在执行阶段**EXECUTION TIME**才能够知道。

**静态分区**

分区的key值不保存在真实数据里面，在加载数据的时候需要显示指定。

Create table wangmeng(id int,name string) partitioned by(datetime string) ;

Load data into Inpath’/user/wangmeng/text/’  into table wangmeng partition(datatime=’2014’)

**动态分区**

样例如下：

Create table dynamic_partition (key int, value string) PARTITIONED BY (part string);

insert  overwrite  table dynamic_partition  PARTITION(part)  select l_orderkey, l_returnflag  from textfilelineitem  ;

**使用动态分区需要设置的参数有**：

set hive.exec.dynamic.partition=true; 

set hive.exec.dynamic.partition.mode=nonstrict;（默认值为严格模式，此模式下不能全部使用动态分区） 

set hive.exec.max.dynamic.partitions.pernode（ 默认100，每一个maper或者reducer能够创建的最大分区数目，如果超过了将会报错）

set hive.exec.max.dynamic.partitions（默认1000，一个query的map或者reducer能够创建的最大分区数目）

set hive.exec.max.created.files（默认100000，一个query的所有maper reducer能够创建的最大文件个数，一个bucket通常会包含多个文件）

用户也可以混合使用动态分区和静态分区数据插入.

默认请情况下，动态分区插入的功能是被禁用的，当被激活后，Hive默认会工作在严格strict模式下。在strict模式下，必须使用静态分区和动态分区混合使用的方式，这主要是避免一些不好的数据查询设计

**动态分区一般来说说转化为两个MRjob，且都只有map阶段，有时候可以利用distributed** **y来把相应的数据分发到reducer之后再进行partition，这样可以使得partition个数不至于太多而超过限制报错（适合于partition** **key中相同值虽然很多但是很分散的情况，即每一个map任务都有各种不同的离散数据**），这样子的话任务就不是两个map阶段而仅是一个mapreduce阶段了,需要说明的是谨慎使用第二种策略，我实测过第一种和第二种分区策略，第二种明显慢很多），使用distributed by的话，reduce不需要用户显视指定。

在动态分区的数据加载过程中，partition key需要放在查询的最后，并且是要按照PARTITIONED BY (ds string, hr int)中的顺序，动态分区和静态分区可以同时使用，但是静态分区需要在动态分区前面，用户可以创建动态分区，只要在加载的时候把某一个动态分区的key值指定为常数即可变为静态分区。

**Wrong:**
insert overwrite  table mixbucket partition(l_returnflag,l_linestatus=50) select l_orderkey , l_returnflag  from  orclineitem  ;

**Right:**

insert overwrite  table mixbucket partition(l_returnflag=50,l_linestatus) select l_orderkey , l_returnflag  from  orclineitem  ;

**Bucket 桶划分**

Create table bucketwangmeng(id int,name string) clustered by (id) into 4 buckets;

Bucket中的cluster by 后面的key必须是新建表中的一个或者多个属性（中间以,隔开）
而静态分区以及动态分区指定分布key不一定要求是新建表里面的属性，动态分区需要要求分区key是source中的属性即可。由于hive在load数据的时候不能检查数据文件的格式与桶的定义是否匹配，如果不匹配在查询的时候就会报错，所以最好还是让hive来帮你生成数据，简单来说就是利用现有的表的数据导入到新定义的带有桶的表中。一般来说，动态分区是只有map阶段的两个MRjob,bucket则是一个完整mapreduce job ,指定为多少个bucket就只有多少个reduce任务，最终生成reduce个数的结果。而分区的个数则取决于分区key值组合的个数，且一个分区下面的文件很可能是多个。
需要注意，要set hive.enforce.bucketing=true，由于默认值是false,不设置的话不会划分为桶，但却**不抛出异常**，而是启动多少个map就生成多少个文件而省去了分桶所需要的reduce阶段。
