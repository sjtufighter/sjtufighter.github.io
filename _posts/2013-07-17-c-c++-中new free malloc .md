---
layout: post
title:  "c c++ 中new malloc "
date:   2013-07-17
categories: 
- Notes 
tags:
- CC++
---

1.malloc与free是C++/C语言的标准库函数，new/delete是C++的运算符。它们都可用于申请动态内存和释放内存

2.对于非内部数据类型的对象而言，光用maloc/free无法满足动态对象的要求。对象在创建的同时要自动执行构造函数，对象在消亡之前要自动执行析构函数。由malloc/free是库函数而不是运算符，不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加于malloc/free。

3.因此C++语言需要一个能完成动态内存分配和初始化工作的运算符new，以一个能完成清理与释放内存工作的运算符delete。注意new/delete不是库函数。 

4.C++程序经常要调用C函数，而C程序只能用malloc/free管理动态内存。 

5.new可以认为是malloc加构造函数的执行。new出来的指针是直接带类型信息的。而malloc返回的都是void*指针。

new delete在实现上其实调用了malloc,free函数

6.new建立的对象你可以把它当成一个普通的对象,用成员函数访问,不要直接访问它的地址空间;malloc分配的是一块内存区域,就用指针访问好了,而且还可以在里面移动指针.

7.new 建立的是一个对象；alloc分配的是一块内存.

***************************************

**相同点：都可用于申请动态内存和释放内存**

**不同点：** 
(1)操作对象有所不同。 
malloc与free是C++/C 语言的标准库函数，new/delete 是C++的运算符。对于非内部数据类的对象而言，光用maloc/free 无法满足动态对象的要求。对象在创建的同时要自动执行构造函数， 对象消亡之前要自动执行析构函数。由于malloc/free 是库函数而不是运算符，不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加malloc/free。

(2)在用法上也有所不同。 
函数malloc 的原型如下： 
void * malloc(size_t size); 
用malloc 申请一块长度为length 的整数类型的内存，程序如下： 
int *p = (int *) malloc(sizeof(int) * length); 
我们应当把注意力集中在两个要素上：“类型转换”和“sizeof”。 
malloc 返回值的类型是void *，所以在调用malloc 时要显式地进行类型转换，将void * 转换成所需要的指针类型。 
malloc 函数本身并不识别要申请的内存是什么类型，它只关心内存的总字节数。

**函数free 的原型如下** 
void free( void * memblock ); 
为什么free 函数不象malloc 函数那样复杂呢？这是因为指针p 的类型以及它所指的内存的容量事先都是知道的，语句free(p)能正确地释放内存。如果p 是NULL 指针，那么free对p 无论操作多少次都不会出问题。如果p 不是NULL 指针，那么free 对p连续操作两次就会导致程序运行错误。

**new/delete 的使用要点**
运算符new 使用起来要比函数malloc 简单得多，例如： 
int *p1 = (int *)malloc(sizeof(int) * length); 
int *p2 = new int[length]; 
这是因为new 内置了sizeof、类型转换和类型安全检查功能。对于非内部数据类型的对象而言，new 在创建动态对象的同时完成了初始化工作。如果对象有多个构造函数，那么new 的语句也可以有多种形式。

如果用new 创建对象数组，那么只能使用对象的无参数构造函数。例如 
Obj *objects = new Obj[100]; // 创建100 个动态对象 
不能写成 
Obj *objects = new Obj[100](1);// 创建100 个动态对象的同时赋初值1 
在用delete 释放对象数组时，留意不要丢了符号‘[]’。例如 
delete []objects; // 正确的用法 
delete objects; // 错误的用法 
后者相当于delete objects[0]，漏掉了另外99 个对象。
