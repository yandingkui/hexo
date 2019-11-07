---
title: java并发知识笔记
categories: java基础知识
---
参考书籍：java并发编程之美
#### 线程的创建与运行
 Java 中有三种线程创建方式，分别为:
 1. 实现 Runnable接口的 run方法;
 2. 继承 Thread类 并重写 run 的方法;
 3. 使用 FutureTask方式。
 
#### yield方法
yield : Thread 类中的一个静态方法，当一个线程调用yield方法的时候，当前线程就会让出CPU的使用权，然后处于就绪状态，线程调度器就会从就绪状态的队列中选择一个优先级最高的线程，当然也有可能选到该线程。

*sleep和yield的主要区别：  
  - sleep会被阻塞挂起指定时间，这段时间内线程调度器不会去调用这个线程。
  - yield方法是让出自己的CPU时间，没有被阻塞，变成就绪状态

#### 线程中断
- void interrupt(): 中断线程。设置线程的中断标识，如果线程没有中断则继续执行，如果用了wait,sleep,join被阻塞挂起，就会抛出InterruptedException异常返回；
- boolean isInterrupted(): 获取当前线程的中断状态，如果是返回true,如果不是返回false;
- boolean interrupted(): 检测当前线程是否被中断。该方法如果发现当前线程被中断，就清除中断标识。

#### 线程死锁
死锁满足4个条件：
- 互斥条件。 共享资源只由一个线程占用
- 请求并持有。一个线程已经持有了至少一个资源，但是又提出了新的资源请求
- 不可剥夺。线程获取到的资源在自己使用完之前不能被其它线程抢占，只有自己使用完毕之后才能释放
- 环路等待。 存在线程-资源的环形链  

如何避免死锁: 
至少破坏掉至少一个构造死锁的必要条件，目前只有请求持有和环路等待可以被破坏。
1. 死锁避免
银行家算法
2. 死锁预防
- 破坏请求和保持条件
	协议1:所有进程开始前，必须一次性地申请所需的所有资源，这样运行期间就不会再提出资源要求，破坏了请求条件，即使有一种资源不能满足需求，也不会给它分配正在空闲的资源，这样它就没有资源，就破坏了保持条件，从而预防死锁的发生。
	协议2:允许一个进程只获得初期的资源就开始运行，然后再把运行完的资源释放出来。然后再请求新的资源。
- 破坏不可抢占条件
	当一个已经保持了某种不可抢占资源的进程，提出新资源请求不能被满足时，它必须释放已经保持的所有资源，以后需要时再重新申请。
- 破坏循环等待条件
	对系统中的所有资源类型进行线性排序，然后规定每个进程必须按序列号递增的顺序请求资源。假如进程请求到了一些序列号较高的资源，然后有请求一个序列较低的资源时，必须先释放相同和更高序号的资源后才能申请低序号的资源。多个同类资源必须一起请求。

#### ThreadLocal
- ThreadLocal
ThreadLocal提供了线程本地变量,访问ThreadLocal变量的时候，每个线程都有自己本地的副本，操作的是自己本地内存里面的变量，从而避免了线程安全问题。
ThreadLocal实现原理：
ThreadLocal类图：
![ThreadLocal类图](https://github.com/yandingkui/yandingkui.github.io/blob/master/images/blogimg/ThreadLocal.jpg?raw=true)
Thread类中有一个threadLocals和inheritableThreadLocals,它们都是ThreadLocalMap类型的变量，而ThreadLocalMap是一个定制化的HashMap。在默认情况下，每个线程中的这两个变量都为null,只有当前线程第一次调用ThreadLocal的set或者get方法才回创建它们。本地变量全部是存在threadLocals变量中，也就是线程本地空间中，ThreadLocal类是一个工具，通过set和get方法来存取线程数据。
- InheritableThreadLocal
ThreadLocal的缺点是 子线程无法访问父线程的中的ThreadLocal变量，InheritableThreadLocal的主要作用就是弥补这个缺陷。
InheritableThreadLocal的实现原理：
子线程的构造函数在初始化该线程的时候，利用父线程中的inheritableThreadLocals变量构建了新的ThreadLocalMap变量，然后赋值给了子线程的inheritableThreadLocals变量。从而让子线程获得了父线程的变量。
如果单纯从获得父线程的变量而言，也可以采用传参的方式，不一定要用InheritableThreadLocal的方式。

#### ThreadLocalRandom
- Random类及局限性
 采用全局的种子变量，同步获取
- 在每个线程内保存种子变量

#### 原子类