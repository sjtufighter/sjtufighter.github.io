---
layout: post
title:  "malloc reallloc calloc"
date:   2013-07-20
categories: 
- Notes 
tags:
- CC++
---


三个函数的申明分别是: 
void* realloc(void* ptr, unsigned newsize); 
void* malloc(unsigned size); 
void* calloc(size_t numElements, size_t sizeOfElement); 
都在stdlib.h函数库内
它们的返回值都是请求系统分配的地址,如果请求失败就返回NULL 

malloc用于申请一段新的地址,参数size为需要内存空间的长度,如: 
char* p; 
p=(char*)malloc(20);

calloc与malloc相似,参数sizeOfElement为申请地址的单位元素长度,numElements为元素个数,如: 
char* p; 
p=(char*)calloc(20,sizeof(char)); 
这个例子与上一个效果相同

realloc是给一个已经分配了地址的指针重新分配空间,参数ptr为原有的空间地址,newsize是重新申请的地址长度 
如: 
char* p; 
p=(char*)malloc(sizeof(char)*20); 
p=(char*)realloc(p,sizeof(char)*40);

注意，这里的空间长度都是以字节为单位。 

C语言的标准内存分配函数：malloc，calloc，realloc，free等。 
malloc与calloc的区别为1块与n块的区别： 
malloc调用形式为(类型*)malloc(size)：在内存的动态存储区中分配一块长度为“size”字节的连续区域，返回该区域的首地址。 
calloc调用形式为(类型*)calloc(n，size)：在内存的动态存储区中分配n块长度为“size”字节的连续区域，返回首地址。 
realloc调用形式为(类型*)realloc(*ptr，size)：将ptr内存大小增大到size。 


free的调用形式为free(void*ptr)：释放ptr所指向的一块内存空间。 
C++中为new/delete函数。




转载自http://www.cnblogs.com/BlueTzar/articles/1136549.html
