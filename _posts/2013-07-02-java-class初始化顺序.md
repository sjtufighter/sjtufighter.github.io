---
layout: post
title:  "java class初始化顺序"
date:   2013-07-01 13:17:59
categories: 
- Notes 
tags:
- Java
---

ava代码 
class Parent { 
// 静态变量 
public static String p_StaticField = "父类--静态变量"; 
// 变量 
public String p_Field = "父类--变量"; 

// 静态初始化块 
static { 
System.out.println(p_StaticField); 
System.out.println("父类--静态初始化块"); 
} 

// 初始化块 
{ 
System.out.println(p_Field); 
System.out.println("父类--初始化块"); 
} 

// 构造器 
public Parent() { 
System.out.println("父类--构造器"); 
} 
} 

public class SubClass extends Parent { 
// 静态变量 
public static String s_StaticField = "子类--静态变量"; 
// 变量 
public String s_Field = "子类--变量"; 
// 静态初始化块 
static { 
System.out.println(s_StaticField); 
System.out.println("子类--静态初始化块"); 
} 
// 初始化块 
{ 
System.out.println(s_Field); 
System.out.println("子类--初始化块"); 
} 

// 构造器 
public SubClass() { 
System.out.println("子类--构造器"); 
} 

// 程序入口 
public static void main(String[] args) { 
new SubClass(); 
} 
} 

运行一下上面的代码，结果马上呈现在我们的眼前： 

父类--静态变量 
父类--静态初始化块 
子类--静态变量 
子类--静态初始化块 
父类--变量 
父类--初始化块 
父类--构造器 
子类--变量 
子类--初始化块 
子类--构造器 

现在，结果已经不言自明了。大家可能会注意到一点，那就是，并不是父类完全初始化完毕后才进行子类的初始化，实际上子类的静态变量和静态初始化块的初始化是在父类的变量、初始化块和构造器初始化之前就完成了。 

那么对于静态变量和静态初始化块之间、变量和初始化块之间的先后顺序又是怎样呢？是否静态变量总是先于静态初始化块，变量总是先于初始化块就被初始化了呢？实际上这取决于它们在类中出现的先后顺序。我们以静态变量和静态初始化块为例来进行说明。 

同样，我们还是写一个类来进行测试： 
Java代码 
public class TestOrder { 
// 静态变量 
public static TestA a = new TestA(); 

// 静态初始化块 
static { 
System.out.println("静态初始化块"); 
} 

// 静态变量 
public static TestB b = new TestB(); 

public static void main(String[] args) { 
new TestOrder(); 
} 
} 

class TestA { 
public TestA() { 
System.out.println("Test--A"); 
} 
} 

class TestB { 
public TestB() { 
System.out.println("Test--B"); 
} 
} 

运行上面的代码，会得到如下的结果： 

Test--A 
静态初始化块 
Test--B 

大家可以随意改变变量a、变量b以及静态初始化块的前后位置，就会发现输出结果随着它们在类中出现的前后顺序而改变，这就说明静态变量和静态初始化块是依照他们在类中的定义顺序进行初始化的。同样，变量和初始化块也遵循这个规律。

了解了继承情况下类的初始化顺序之后，如何判断最终输出结果就迎刃而解了。

静态块与静态成员的初始化工作与实例化过程无关，实例化必须先执行静态块和静态成员，但并不代表实例化一定会执行静态块和静态成员。只有当实例化的对应的类为加载入虚拟机的时候，才会进行这种操作。有些时候执行静态块或者初始化静态成员不一定就是实例化该类对象才会进行的，例如调研该类的某静态成员或者静态方法，又例如该类的子类被实例化或者调用了静态成员或静态方法等。

还有实例化的实际顺序其实是(省略类初始化过程)
1、进入当前类构造方法。
2、进入父类构造方法递归直到java.lang.Object类构造方法。
3、执行java.lang.Object类构造方法，顺序依次为成员变量初始与初始化块(安装上下文顺序)，对应调用的构造方法体。
4、执行java.lang.Object类的直接子类的构造函数，这个过程递归到当前类。
5、当前类执行顺序与前面java.lang.Object类相同。

构造方法的本质其实就是一个普通的无返回参数的名字叫做<init>的方法，不过虚拟机调用这个方法的指令与其它方法不同而已，它的调用指令与调用private方法的指令相同。
在虚拟机中存在三种方法的调用指令，这三种调用指令在效率上不同。
接口方法的指令调用，这种调用速度最慢。
普通类方法的调用指令，这种调用速度中等。
构造方法与私有方法调用指令，这种调用速度最快。
