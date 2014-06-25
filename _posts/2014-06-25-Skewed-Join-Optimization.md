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

两张表之间的join是通过转化为mapreduce job执行的，其中map阶段提取出join key 并对其进行排序，然后把特定join key所涉及的所有行数据shuffle到同一个reduce，在reduce阶段，则是对两个join表的join key 进行join，整个join的结果则是所有reduce阶段的结果所组成的。

e.g., Suppose we have table A with a key column, "id" which has values 1, 2, 3 and 4, and table B with a similar column, which has values 1, 2 and 3.
We want to do a join corresponding to the following query

例如：我们有一张表A，其join key为 id ,id有值1,2,3,4.与此同时table B也有类似的属性列，其包含value 1 2 3.现在我们想对这两个表做如下join：

select A.id from A join B on A.id = B.id


A set of Mappers read the tables and gives them to Reducers based on the keys. e.g., rows with key 1 go to Reducer R1, rows with key 2 go to Reducer R2 and so on. These Reducers do a cross product of the values from A and B, and write the output. The Reducer R4 gets rows from A, but will not produce any results.

这个sql查询会转为一个mapreduce job.map阶段，mappers读取表的数据然后依据join key将其传递到相应的reducers。例如：key为1的行shuffle到Reducer R1,key为2的行shuffle到Reducer 2 等等。这些reducers对来自两个表A B 的数据进行join,并写出结果到hdfs。而Reducer R4 仅仅从表A获取行数据，表B没有。

Now let's assume that A was highly skewed in favor of id = 1. Reducers R2 and R3 will complete quickly but R1 will continue for a long time, thus becoming the bottleneck. If the user has information about the skew, the bottleneck can be avoided manually as follows:

问题就来了，如果表A在join key id=1上严重倾斜，而id为2 3 的value比较少，那么RedcuerR2 and Reducer R3将会很快完成，而R1则会运行很长的时间，整个sql查询的时间则受到Reducer1所严重拖累，如果大家对数据倾斜有一定概念的话，可以采用如下方式来避免它：

Do two separate queries
select A.id from A join B on A.id = B.id where A.id <> 1;
select A.id from A join B on A.id = B.id where A.id = 1 and B.id = 1;

把上述sql转化为两个sql查询。

select A.id from A join B on A.id = B.id where A.id <> 1;
select A.id from A join B on A.id = B.id where A.id = 1 and B.id = 1;


The first query will not have any skew, so all the Reducers will finish at roughly the same time. If we assume that B has only few rows with B.id = 1, then it will fit into memory. So the join can be done efficiently by storing the B values in an in-memory hash table. This way, the join can be done by the Mapper itself and the data do not have to go to a Reducer. The partial results of the two queries can then be merged to get the final results.

第一个sql查询不会有任何数据倾斜，将会很快完成。而对于diergesql查询而言，假如表B满足id=1的数据只有少数行，以至于可以填充到内存里。所以第二个sql查询可以转为一个map join，而避免了reduce阶段，能较大降低查询时延。而整个join的结果则是两个sql查询的结果的并集。
这种应对数据倾斜的策略的优缺点如下：
Advantages
If a small number of skewed keys make up for a significant percentage of the data, they will not become bottlenecks.
Disadvantages
The tables A and B have to be read and processed twice.
Because of the partial results, the results also have to be read and written twice.
The user needs to be aware of the skew in the data and manually do the above process.

优点：

把数据倾斜的key的join转化为独立的map join ,减少了时延。

缺点：

表A和表B的数据需要被读取和处理两次

由于需要对结果进行合并，所以每部分的结果也需要被读写两次

需要良好的策略来识别是否发生数据倾斜以及倾斜的join key是哪个

We can improve this further by trying to reduce the processing of skewed keys. First read B and store the rows with key 1 in an in-memory hash table. Now run a set of mappers to read A and do the following:
If it has key 1, then use the hashed version of B to compute the result.
For all other keys, send it to a reducer which does the join. This reducer will get rows of B also from a mapper.

我们可以继续优化来减少对skew倾斜key的处理过程，首先读取表B，并把key为1的行存储在内存哈希表中，并且启动一系列mappers来从表A读书数据，如果读取的key为1就把其和B的id=1 进行map join,，否则的话就把其发送到reducer进行join，reducer同样会从表B的map读取数据。

This way, we end up reading only B twice. The skewed keys in A are only read and processed by the Mapper, and not sent to the reducer. The rest of the keys in A go through only a single Map/Reduce.
The assumption is that B has few rows with keys which are skewed in A. So these rows can be loaded into the memory.
Hive Enhancements
The skew data will be obtained from list bucketing (https://cwiki.apache.org/confluence/display/Hive/ListBucketing). There are no additions to the Hive grammar.
这样子的话，我们只需要读取B表两次，而表A只需要读取一次，表A中倾斜的key仅仅在map阶段被一次读取和处理，不会发送到redcuer端。而表A中剩余的key将会进行一个独立的mapreduce job。

当然，上述优化的前提是假设在B表中只有少数在A表中倾斜的join key 以至于其可以放在内存里进行map join。

