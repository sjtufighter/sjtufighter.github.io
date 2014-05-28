---
layout: post
title:  "HavingOrWhere?"
date:   2014-04-29
categories: 
- Notes 
tags:
- SqlHive
---
在select 语句中可以使用group by 子句将行划分成较小的组，然后，使用聚组函数返回每一个组的汇总信息，另外，可以使用**having子句限制返回的结果集。group by 子句可以将查询结果分组，并返回行的汇总信息Oracle 按照group by 子句中指定的表达式的值分组查询结果。
   在带有group by 子句的查询语句中**，在**select 列表中指定的列要么是group by 子句中指定的列，要么包含聚组函数**
   
   select max(sal),job emp group by job; 
   

   查询语句的select 和group by ,having 子句是聚组函数唯一出现的地方，**在where 子句中不能使用聚组函数**。 select deptno,sum(sal) from emp where sal>1200 group by deptno having sum(sal)>8500 order by deptno;
    
   当在gropu by 子句中使用having 子句时，查询结果中只返回满足having条件的组。**在一个sql语句中可以有where子句和having子句**。having 与where 子句类似，均用于设置限定条件 **where 子句的作用是在对查询结果进行分组前**，将不符合where条件的行去掉，即在分组之前过滤数据，**条件中不能包含聚组函数**，使用where条件显示特定的行。
  **having 子句的作用是筛选满足条件的组，即在分组之后过滤数据**，条件中经常**包含聚组函数**，使用having 条件显示特定的组，也可以使用多个分组标准进行分组。
  查询每个部门的每种职位的雇员数 
  select deptno,job,count(*) from emp group by deptno,job;
 
　　总之，  

　　**WHERE语句在GROUP BY语句之前；SQL会在分组之前计算WHERE语句。**   

　　**HAVING语句在GROUP BY语句之后；SQL会在分组之后计算HAVING语句。**
