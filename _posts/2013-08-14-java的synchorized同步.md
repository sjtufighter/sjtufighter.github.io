---
layout: post
title:  "java的synchorized同步"
date:   2013-08-14
categories: 
- Notes 
tags:
- Java
---
**据我所致在java中实现线程的方法有两种一是不可变对象，二是用synchronized（ 互斥锁）来进行同步。**
 
  但是由于synchronized包含多种用法，下面将详细介绍synchronized在程序中的使用方式。
**Java 同步关键字（synchronized）*8
**实例方法**
**静态方法**
**实例方法中的同步块**
**静态方法中的同步块**
上述同步块都同步在不同对象上。实际需要那种同步块视具体情况而定。


**实例方法同步**

下面是一个同步的实例方法：
public synchronized void add(int value){
         this.count+= value;
}
注意在方法声明中同步（synchronized  ）关键字
Java 实例方法同步是同步在拥有该方法的对象上。这样，每个实例其方法同步都同步在不同的对象上，即该方法所属的实例。只有一个线程能够在实例方法同步块中运行。如果有多个实例存在，那么一个线程一次可以在一个实例同步块中执行操作。一个实例一个线程。

**静态方法同步**

静态方法同步和实例方法同步方法一样，也使用synchronized  关键字。Java 静态方法同步如下示例：
public static synchronized void add(intvalue){
          count += value;
          }
          
静态方法的同步是指同步在该方法所在的类对象上。因为在**Java 虚拟机中一个类只能对应一个类对象**，所以同时只允许一个线程执行同一个类中的静态同步方法。
对于不同类中的静态同步方法，一个线程可以执行每个类中的静态同步方法而无需等待。不管类中的那个静态同步方法被调用，一个类只能由一个线程同时执行。

**实例方法中的同步块**

有时你不需要同步整个方法，而是同步方法中的一部分。Java 可以对方法的一部分进行同步。
在非同步的Java 方法中的同步块的例子如下所示：
public void add(int value){
             synchronized(this){
                this.count += value;
             }
           }
示例使用Java 同步块构造器来标记一块代码是同步的。该代码在执行时和同步方法一样。
注意Java 同步块构造器用括号将对象括起来。在上例中，**使用了“this”** ，即为调用add 方法的实例本身。在同步构造器中用括号括起来的对象叫做监视器对象。上述代码使用监视器对象同步，同步实例方法使用调用方法本身的实例作为监视器对象。
一次只有一个线程能够在同步于同一个监视器对象的Java 方法内执行。
下面两个例子都同步他们所调用的实例对象上，因此他们在同步的执行效果上是等效的。
public class MyClass {
         
            public synchronized void log1(String msg1,String msg2){
               log.hanshu1(msg1);
               log.hanshu2(msg2);
            }
         
            public void log2(String msg1, String msg2){
               synchronized(this){
                  log.hanshu1(msg1);
                  log.hanshu2(msg2);
               }
            }
          }
在上例中，每次只有一个线程能够在两个同步块中任意一个方法内执行。
如果第二个同步块不是同步在this 实例对象上，那么两个方法可以被线程同时执行。

**静态方法中的同步块**

和上面类似，下面是两个静态方法同步的例子。这些方法同步在该方法所属的类对象上。
public class MyClass {
            public static synchronized void log1(Stringmsg1, String msg2){
                log.writeln(msg1);
                log.writeln(msg2);
             }
         
             public static void log2(String msg1, Stringmsg2){
                synchronized(MyClass.class){
                   log.writeln(msg1);
                   log.writeln(msg2);
                }
             }
           }
这两个方法不允许同时被线程访问。
如果第二个同步块不是同步在MyClass.class 这个对象上。那么这两个方法可以同时被线程访问。

**Java同步实例**

在下面例子中，启动了两个线程，都调用Counter 类同一个实例的add 方法。因为同步在该方法所属的实例上，所以同时只能有一个线程访问该方法。
publicclass Counter{
              long count = 0;
         
              public synchronized void add(long value){
                this.count += value;
              }
           }
           public class CounterThread extends Thread{
         
              protected Counter counter = null;
              public CounterThread(Counter counter){
                 this.counter = counter;
              }
         
              public void run() {
             for(int i=0; i<10; i++){
                    counter.add(i);
                 }
              }
           }
           public class Example {
         
             public static void main(String[] args){
               Counter counter = new Counter();
               Thread threadA = new CounterThread(counter);
               Thread threadB = new CounterThread(counter);
         
               threadA.start();
               threadB.start();
             }
           }
创建了两个线程。他们的构造器引用同一个Counter 实例。Counter.add 方法是同步在实例上，是因为add 方法是实例方法并且被标记上synchronized 关键字。因此每次只允许一个线程调用该方法。另外一个线程必须要等到第一个线程退出add() 方法时，才能继续执行方法。
如果两个线程引用了两个不同的Counter 实例，那么他们可以同时调用add() 方法。这些方法调用了不同的对象，因此这些方法也就同步在不同的对象上。这些方法调用将不会被阻塞。如下面这个例子所示：
            public static voidmain(String[] args){
             Counter counterA = new Counter();
             Counter counterB = new Counter();
             Thread  threadA = newCounterThread(counterA);
             Thread  threadB = newCounterThread(counterB);
         
             threadA.start();
             threadB.start();
           }
          }
注意这两个线程，threadA 和threadB ，不再引用同一个counter 实例。CounterA 和counterB 的add 方法同步在他们所属的对象上。调用counterA 的add 方法将不会阻塞调用counterB 的add 方法.
