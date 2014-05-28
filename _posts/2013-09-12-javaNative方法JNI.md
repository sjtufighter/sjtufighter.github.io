---
layout: post
title:  "javaNative方法JNI"
date:   2013-09-12
categories: 
- Notes 
tags:
- Java
---

**native方法出现在很多第放，比如说线程的start方法，使用native有两个目的，一是表明这个方法所执行的操作是平台相关的，涉及到OS以及cpu等资源等，二是使用native方法可以提升一定的性能。**

**native关键字用法**

native是与C++联合开发的时候用的！使用native关键字说明这个方法是原生函数，也就是这个方法是用C/C++语言实现的，并且被编译成了DLL，由java去调用。 这些函数的实现体在DLL中，JDK的源代码中并不包含，你应该是看不到的。对于不同的平台它们也是不同的。这也是java的底层机制，实际上java就是在不同的平台上调用不同的native方法实现对操作系统的访问的。总而言之：
native 是用做java 和其他语言（如c++）进行协作时使用的，也就是native 后的函数的实现不是用java写的。

native的意思就是通知操作系统， 这个函数你必须给我实现，因为我要使用。 所以native关键字的函数都是操作系统实现的， java只能调用。
**java是跨平台的语言，既然是跨了平台，所付出的代价就是牺牲一些对底层的控制，而java要实现对底层的控制，就需要一些其他语言的帮助，这个就是native的作用了**



native方法是通过java中的JNI实现的。JNI是Java Native Interface的 缩写。从Java 1.1开始，Java Native Interface (JNI)标准成为java平台的一部分，它允许Java代码和其他语言写的代码进行交互。JNI一开始是为了本地已编译语言，尤其是C和C++而设计 的，但是它并不妨碍你使用其他语言，只要调用约定受支持就可以了。使用java与本地已编译的代码交互，通常会丧失平台可移植性。但是，有些情况下这样做是可以接受的，甚至是必须的，比如，使用一些旧的库，与硬件、操作系统进行交互，或者为了提高程序的性能。JNI标准至少保证本地代码能工作在任何Java 虚拟机实现下。
目前java与dll交互的技术主要有3种：jni，jawin和jacob。Jni（Java Native Interface）是sun提供的java与系统中的原生方法交互的技术（在windows\linux系统中，实现java与native method互调）。目前只能由c/c++实现。后两个都是sourceforge上的开源项目，同时也都是基于jni技术的windows系统上的一个应用库。Jacob（Java-Com Bridge）提供了java程序调用microsoft的com对象中的方法的能力。而除了com对象外，jawin（Java/Win32 integration project）还可以win32-dll动态链接库中的方法
