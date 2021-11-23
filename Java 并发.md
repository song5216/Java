# Java 并发
<!-- GFM-TOC -->

* [Java 并发](#java-并发)
    * [一、使用线程](#一使用线程)
        * [实现 Runnable 接口](#实现-runnable-接口)
        * [实现 Callable 接口](#实现-callable-接口)
        * [继承 Thread 类](#继承-thread-类)
        * [实现接口 VS 继承 Thread](#实现接口-vs-继承-thread)
    * [二、基础线程机制](#二基础线程机制)
        * [Executor](#executor)
        * [Daemon](#daemon)
        * [sleep()](#sleep)
        * [yield()](#yield)
    * [三、中断](#三中断)
        * [InterruptedException](#interruptedexception)
        * [interrupted()](#interrupted)
        * [Executor 的中断操作](#executor-的中断操作)
    * [四、互斥同步](#四互斥同步)
        * [synchronized](#synchronized)
        * [ReentrantLock](#reentrantlock)
        * [比较](#比较)
        * [使用选择](#使用选择)
    * [五、线程之间的协作](#五线程之间的协作)
        * [join()](#join)
        * [wait() notify() notifyAll()](#wait-notify-notifyall)
        * [await() signal() signalAll()](#await-signal-signalall)
    * [六、线程状态](#六线程状态)
        * [新建（NEW）](#新建new)
        * [可运行（RUNABLE）](#可运行runable)
        * [阻塞（BLOCKED）](#阻塞blocked)
        * [无限期等待（WAITING）](#无限期等待waiting)
        * [限期等待（TIMED_WAITING）](#限期等待timed_waiting)
        * [死亡（TERMINATED）](#死亡terminated)
    * [七、J.U.C - AQS](#七juc---aqs)
        * [CountDownLatch](#countdownlatch)
        * [CyclicBarrier](#cyclicbarrier)
        * [Semaphore](#semaphore)
    * [八、J.U.C - 其它组件](#八juc---其它组件)
        * [FutureTask](#futuretask)
        * [BlockingQueue](#blockingqueue)
        * [ForkJoin](#forkjoin)
    * [九、线程不安全示例](#九线程不安全示例)
    * [十、Java 内存模型](#十java-内存模型)
        * [主内存与工作内存](#主内存与工作内存)
        * [内存间交互操作](#内存间交互操作)
        * [内存模型三大特性](#内存模型三大特性)
        * [先行发生原则](#先行发生原则)
    * [十一、线程安全](#十一线程安全)
        * [不可变](#不可变)
        * [互斥同步](#互斥同步)
        * [非阻塞同步](#非阻塞同步)
        * [无同步方案](#无同步方案)
    * [十二、锁优化](#十二锁优化)
        * [自旋锁](#自旋锁)
        * [锁消除](#锁消除)
        * [锁粗化](#锁粗化)
        * [轻量级锁](#轻量级锁)
        * [偏向锁](#偏向锁)
    * [十三、多线程开发良好的实践](#十三多线程开发良好的实践)
    * [参考资料](#参考资料)
    <!-- GFM-TOC -->



# 概述

## 进程与线程

**进程和线程（Process与Thread）**

- 进程和线程的概念
- 并行和并发的概念
- 线程基本应用

### 进程

**进程**：正在运行的应用程序

1. 程序由指令和**数据**组成

   这些指令要运行，数据要读写，就必须将指令加载至 CPU，数据加载至内存。在指令运行过程中还需要用到**磁盘、网络等设备**。进程就用来**加载指令、管理内存、管理 IO** 的

2. 当一个程序被运行，从磁盘加载这个程序的代码至内存，这时就开启了一个进程。

3. 进程就可以视为程序的一个实例。大部分程序可以同时运行多个实例进程（例如记事本、画图、浏览器等），也有的程序只能启动一个实例进程（例如网易云音乐、360 安全卫士等）

#### 组成

进程包括 :

-   **程序段**
-   **数据段**

-   **PCB**：
    **程序计数器中的值, 指示下一条将运行的指令**
    **一组通用的寄存器的当前值, 堆, 栈**
    **一组系统资源(如打开的文件)**

#### **特点**

- **动态性** : 最基本的特征；可动态地创建, 结果进程;
- **并发性** : 内存中多个进程实体，各进程并发的执行；进程可以被独立调度并占用处理机运行; (并发:一段, 并行:一时刻)（一个CPU只能并发）
- **独立性** : 不同进程的工作不相互影响;(页表是保障措施之一)
- **异步性:** 因访问共享数据, 资源或进程间同步而产生制约；各自并发的执行的进程相互独立，**以不可知顺序进行**
- 结构性

### 线程

#### 概念

**线程**：属于进程的一个执行路径

- 单线程：只有一条执行路径
- 多线程：有多个执行路径

**线程 = 进程 - 共享资源**

#### 线程的属性

1. CPU**调度**的基本单位
2. 有独立的线程线程控制块
3. 也有就绪、阻塞、运行基本状态
4. 几乎不占有系统资源
5. 同一进程的不同线程间共享进程的资源
6. 由于共享内存地址空间，同一进程的线程之间通信 甚至无需系统干预
7. 线程更轻量，线程上下文切换成本一般上要比进程上下文切换低

### **并发和并行**

并发：一个cup交替执行多个线程

并行：多个cpu一起执行多个线程

### 异步调用

以调用方角度来讲，如果
需要等待结果返回，才能继续运行就是同步
**不需要等待结果返回，就能继续运行就是异步**

#### 设计

多线程可以让方法执行变为异步的（即不要巴巴干等着）比如说读取磁盘文件时，假设读取操作花费了 5 秒钟，如果没有线程调度机制，这 5 秒 cpu 什么都做不了，其它代码都得暂停...

#### 结论

1. 比如在项目中，视频文件需要转换格式等操作比较费时，这时开一个新线程处理视频转换，避免阻塞主线程
2. tomcat 的异步 servlet 也是类似的目的，让用户线程处理耗时较长的操作，避免阻塞 tomcat 的工作线程
3. ui 程序中，开线程进行其他操作，避免阻塞 ui 线程

### 提高效率

充分利用多核 cpu 的优势，提高运行效率。想象下面的场景，执行 3 个计算，最后将计算结果汇总。
如果是串行执行，那么总共花费的时间是 10 + 11 + 9 + 1 = 31ms
但如果是四核 cpu，各个核心分别使用线程 1 执行计算 1，线程 2 执行计算 2，线程 3 执行计算 3，那么 3 个
线程是并行的，花费时间只取决于最长的那个线程运行的时间，即 11ms 最后加上汇总时间只会花费 12ms
注意
需要在多核 cpu 才能提高效率，单核仍然时是轮流执行

1. 单核 cpu 下，多线程不能实际提高程序运行效率，只是为了能够在不同的任务之间切换，不同线程轮流使用
cpu ，不至于一个线程总占用 cpu，别的线程没法干活
2. 多核 cpu 可以并行跑多个线程，但能否提高程序运行效率还是要分情况的
有些任务，经过精心设计，将任务拆分，并行执行，当然可以提高程序的运行效率。但不是所有计算任务都能拆分（参考后文的【阿姆达尔定律】）
3. 也不是所有任务都需要拆分，任务的目的如果不同，谈拆分和效率没啥意义
4. IO 操作不占用 cpu，只是我们一般拷贝文件使用的是【阻塞 IO】，这时相当于线程虽然不用 cpu，但需要一直等待 IO 结束，没能充分利用线程。所以才有后面的【非阻塞 IO】和【异步 IO】优化

# 线程

## 实现方式

有三种使用线程的方法：

- 继承 Thread 类；

- 实现 Runnable 接口；
- 实现 Callable 接口；

实现 Runnable 和 Callable 接口的类只能当做一个可以在线程中运行的任务，不是真正意义上的线程，因此最后还需要通过 Thread 来调用。可以理解为任务是通过线程驱动从而执行的。

#### Thread 类

**步骤：**

1. 定义一个MyThread继承Thread类
2. 在MyThread类中重写run()方法
3. 创建MyThread方法
4. 启动线程.start()

同样也是需要实现 run() 方法，因为 Thread 类也实现了 Runable 接口。

当调用 start() 方法启动一个线程时，虚拟机会将该线程放入就绪队列中等待被调度，当一个线程被调度时会执行该线程的 run() 方法。

```java
public class MyThread extends Thread {
    public void run() {
        // ...
    }
}
public static void main(String[] args) {
    MyThread mt = new MyThread();
    //启动线程
    mt.start();
}
```

```java
package MThread;
public class Mythread extends Thread{
    @Override
    public void run() {
        //线程开启后执行的代码
        for (int i = 0; i <50 ; i++) {
            System.out.println("线程开启了"+i);
        }

    }
}

package MThread;
public class Demo1 {
    public static void main(String[] args) {
        Mythread T1 = new Mythread();
        Mythread T2 = new Mythread();
        T1.start();
        T2.start();

    }
}

```

**两个小问题：**

##### **为什么重写run方法：**

run（）是用来分装被线程执行的代码

##### **run和start方法的区别：**

run仅仅表示调用了对象的方法并执行，直接调用与普通方法无差别，没有开启线程
start 是本地方法，用于开启线程，由虚拟机调用线程里面的run（）；

#### Runnable 接口

##### **步骤**

1. 定义一个MyRunnable类实现Runnable接口
2. 在MyRunnable类中重写run（）方法
3. **创建MyRunnable类的对象**
4. 创建Thread类的对象，把MyRunnable对象作为构造方法的参数
5. **启动线程 void start();**

需要实现接口中的 run() 方法。

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        // ...
    }
}
```

使用 Runnable 实例再创建一个 Thread 实例，然后调用 Thread 实例的 start() 方法来启动线程。

Thread 定义了几个构造方法，下面的这个是我们经常使用的：

```
Thread(Runnable threadOb,String threadName);
```

```java
public static void main(String[] args) {
    MyRunnable instance = new MyRunnable();
    Thread thread = new Thread(instance);
    thread.start();
}
```

```java
package MThread;

public class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i <50 ; i++) {
            System.out.println("MyRunnable"+i);
        }
    }
}

package MThread;
public class Demo1 {
    public static void main(String[] args) {
        Mythread T1 = new Mythread();
        Mythread T2 = new Mythread();
        T1.start();
        T2.start();

        MyRunnable mr = new MyRunnable();
        Thread T3 = new Thread(mr);
        T3.start();
    }
}

```

##### Thread 与 Runnable 

分析 Thread 的源码，理清它与 Runnable 的关系

**Thread 内部的run 调用 Runable的 run**

小结

1. 方法1 是把线程和任务合并在了一起，方法2 是把线程和任务分开了
2. 用 Runnable 更容易与线程池等高级 API 配合
3. 用 Runnable 让任务类脱离了 Thread 继承体系，更灵活

####  Callable 接口

**步骤**：

1. 定义一个类MyCallable实现Callable接口
2. 在MyCallable类中重写call（）方法
3. 创建MyCallable类对象
4. 创建Future的实现类FutureTask对象，把MyCallable对象作为构造方法的参数
5. 创建Thread类对象，把Future Task 对象作为构造方法的参数
6. 启动线程
7. 线程启动之后，调用get()方法，就可以获取线程结束之后的结果

与 Runnable 相比，**Callable 可以有返回值，返回值通过 FutureTask 进行封装。**

```java
public class MyCallable implements Callable<Integer> {
    public Integer call() {
        return 123;
    }
}
```

```java
public static void main(String[] args) throws ExecutionException, InterruptedException {
    MyCallable mc = new MyCallable();
    FutureTask<Integer> ft = new FutureTask<>(mc);
    Thread thread = new Thread(ft);
    thread.start();
    System.out.println(ft.get());
}
```





#### 三种方式对比：实现接口 VS 继承 Thread

实现接口会更好一些，因为：

- **Java 不支持多重继承，因此继承了 Thread 类就无法继承其它类，但是可以实现多个接口；**
- **类可能只要求可执行就行，继承整个 Thread 类开销过大。**

![image-20210701180326640](C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210701180326640.png)

## 查看进程线程的方法

##### windows

任务管理器可以查看进程和线程数，也可以用来杀死进程

- tasklist 查看进程
- taskkill 杀死进程

##### linux

- ps -fe 查看所有进程
- ps -fT -p <PID> 查看某个进程（PID）的所有线程
- kill 杀死进程
- top 按大写 H 切换是否显示线程
- top -H -p <PID> 查看某个进程（PID）的所有线程

##### Java

- jps  命令查看所有 Java 进程
- jstack <PID>  查看某个 Java 进程（PID）的所有线程状态
- jconsole  来查看某个 Java 进程中线程的运行情况（图形界面）

##### jconsole 远程监控配置

需要以如下方式运行你的 java 类

```
java -Djava.rmi.server.hostname=`ip地址` -Dcom.sun.management.jmxremote -
Dcom.sun.management.jmxremote.port=`连接端口` -Dcom.sun.management.jmxremote.ssl=是否安全连接 -
Dcom.sun.management.jmxremote.authenticate=是否认证 java类
```

修改 /etc/hosts 文件将 127.0.0.1 映射至主机名

如果要认证访问，还需要做如下步骤

复制 jmxremote.password 文件

修改 jmxremote.password 和 jmxremote.access 文件的权限为 600 即文件所有者可读写

连接时填入 controlRole（用户名），R&D（密码）

## 线程运行

#### 栈与栈帧

Java Virtual Machine Stacks （Java 虚拟机栈）
我们都知道 JVM 中由程序计数器、堆、栈、方法区所组成，其中**栈内存**是给谁用的呢？

- 其实就是线程，每个线程启动后，虚拟机就会为其分配一块栈内存。
- 每个栈由多个栈帧（Frame）组成，对应着每次方法调用时所占用的内存
- 每个线程只能有一个活动栈帧，对应着当前正在执行的那个方法

#### 线程上下文切换

（Thread Context Switch）

##### 切换时机

因为以下一些原因导致 **cpu 不再执行当前的线程**，转而执行另一个线程的代码

1. 线程的 cpu 时间片用完
2. 垃圾回收
3. 有更高优先级的线程需要运行
4. **线程自己调用了 sleep、yield、wait、join、park、synchronized、lock 等方法**

##### 过程

当 Context Switch 发生时，需要由**操作系统保存当前线程的状态**，**并恢复另一个线程的状态**，Java 中对应的概念就是**程序计数器**（Program Counter Register），它的作用是记住下一条 jvm 指令的执行地址，是线程私有的

状态包括：

- 程序计数器
- 虚拟机栈中每个栈帧的信息，如局部变量、操作数栈、返回地址等

Context Switch 频繁发生会影响性能

## 线程的状态

### 操作系统层面

![image-20211110093123164](笔记图库/image-20211110093123164.png)



要想实现多线程，必须在主线程中创建新的线程对象。任何线程一般具有5种状态，即创建，就绪，运行，阻塞，终止。下面分别介绍一下这几种状态：

##### 创建状态 

在程序中用构造方法创建了一个线程对象后，新的线程对象便处于新建状态，此时它已经有了相应的内存空间和其他资源，但还处于不可运行状态，，还未与操作系统线程关联。

新建一个线程对象可采用Thread 类的构造方法来实现，例如 “Thread thread=new Thread()”。

##### 就绪状态 

- 新建线程对象后，**调用该线程的 start() 方法就可以启动线程。**
- **当线程启动时，线程进入就绪状态**。此时，线程将进入线程队列排队，等待 CPU 服务，这表明它已经具备了运行条件。
- 通过调用Thread类的**start()方法来启动一个线程，这时此线程是处于就绪状态，并没有运行。**

##### 运行状态 

- 当就绪状态被调用并获得处理器资源时，线程就进入了运行状态。此时，自动调用该线程对象的 run() 方法。run() 方法定义该线程的操作和功能。
- 当 CPU 时间片用完，会从【运行状态】转换至【就绪状态】，会导致线程的上下文切换

##### 阻塞状态 

- 一个正在执行的线程在某些特殊情况下，如被人为挂起或需要执行耗时的输入/输出操作，这时该线程实际不会用到 CPU，会导致线程上下文切换，进入【阻塞状态】
- 调用sleep(),join(),wait() 等方法，线程都将进入阻塞状态，发生阻塞时线程不能进入排队队列，只有当引起阻塞的原因被消除后，线程才可以转入就绪状态。
- 如果调用了阻塞 API，如 BIO 读写文件，
- 与【就绪状态】的区别是，对【阻塞状态】的线程来说只要它们一直不唤醒，调度器就一直不会考虑
  调度它们

##### 死亡状态 

线程调用 stop() 方法时或 run() 方法执行结束后，即处于死亡状态。处于死亡状态的线程不会再转换为其它状态。

### Java层面

根据 Thread.State 枚举，分为六种状态

#### NEW 

线程刚被创建，但是还没有调用 start() 方法

#### RUNNABLE 

就绪(READY)：当调用了 start() 方法之后，注意，Java API 层面的 RUNNABLE 状态涵盖了 操作系统 层面的
【可运行状态】、【运行状态】和【阻塞状态】（由于 BIO 导致的线程阻塞，在 Java 里无法区分，仍然认为
是可运行）

#### BLOCKED 

请求获取 monitor lock 从而进入 synchronized 函数或者代码块，但是其它线程已经占用了该 monitor lock，没获取到锁的线程就会阻塞在同步队列中（同步队列中放的是尝试获取锁但失败的线程），所以出于阻塞状态。

要结束该状态进入从而 RUNABLE 需要其他线程释放 monitor lock。

####  WAITING 

无限等待

等待其它线程显式地唤醒。

阻塞和等待的区别在于，阻塞是被动的，它是在等待获取 monitor lock。而等待是主动的，通过调用  Object.wait() 等方法进入。

| 进入方法                                   | 退出方法                             |
| ------------------------------------------ | ------------------------------------ |
| 没有设置 Timeout 参数的 Object.wait() 方法 | Object.notify() / Object.notifyAll() |
| 没有设置 Timeout 参数的 Thread.join() 方法 | 被调用的线程执行完毕                 |
| LockSupport.park() 方法                    | LockSupport.unpark(Thread)           |

**以wait()为例**，调用该方法进入等待-WAITING（获得所的队列须要某些资源才能继续运行，就会进入等待队列，**等待进入同步队列**去竞争同步锁）（进入等待队列后，需要notify()唤醒，以进入同步队列）

#### TIMED_WAITING 

wait(time)和sleep(time),是否进入等待队列看调用了哪个方法

| 进入方法                                 | 退出方法                                        |
| ---------------------------------------- | ----------------------------------------------- |
| Thread.sleep() 方法                      | 时间结束                                        |
| 设置了 Timeout 参数的 Object.wait() 方法 | 时间结束 / Object.notify() / Object.notifyAll() |
| 设置了 Timeout 参数的 Thread.join() 方法 | 时间结束 / 被调用的线程执行完毕                 |
| LockSupport.parkNanos() 方法             | LockSupport.unpark(Thread)                      |
| LockSupport.parkUntil() 方法             | LockSupport.unpark(Thread)                      |

调用 Thread.sleep() 方法使线程进入限期等待状态时，常常用“使一个线程睡眠”进行描述。

调用 Object.wait() 方法使线程进入限期等待或者无限期等待时，常常用“挂起一个线程”进行描述。

睡眠和挂起是用来描述行为

而阻塞和等待用来描述状态。

#### TERMINATED 

当线程代码运行结束

### 状态转换

1. **NEW --> RUNNABLE**
   当调用 t.start() 方法时，由 NEW --> RUNNABLE

2. **RUNNABLE --> WAITING**
   t 线程用 **synchronized(obj)** 获取了对象锁后
   调用 obj.wait() 方法时，t 线程从 **RUNNABLE --> WAITING**

3. **WAITING --> BLOCKED**

   调用 **obj.notify() ， obj.notifyAll() ， t.interrupt()** 时
   t 线程从 **WAITING --> BLOCKED** 进入同步队列竞争锁

4. **RUNNABLE** <--> **WAITING**
   当前线程调用 **t.join()** 方法时，当前线程从 **RUNNABLE --> WAITING**

   - 注意是当前线程在t 线程对象的监视器上等待

   t 线程运行结束，或调用了当前线程的 interrupt() 时，当前线程从 **WAITING --> RUNNABLE**

   - 此时的锁对象是 t 线程对象；当前线程不需要竞争锁进入同步队列；直接RUNNABLE

5. RUNNABLE <--> WAITING

   当前线程调用 LockSupport.park() 方法会让当前线程从 RUNNABLE --> WAITING

   调用 LockSupport.unpark(目标线程) 或调用了线程 的 interrupt() ，会让目标线程从 WAITING -->RUNNABLE

#### 总结

等待中的线程被唤醒或者打断 会出现两种情况

1. 当前线程与其他线程存在锁竞争，当前线程进入同步队列阻塞状态竞争锁
2. 不存在锁竞争，进入可执行状态

#### 问题

**在此提出一个问题，Java 程序每次运行至少启动几个线程？**

回答：至少启动两个线程，每当使用 Java 命令执行一个类时，实际上都会启动一个 JVM，每一个JVM实际上就是在操作系统中启动一个线程，Java 本身具备了垃圾的收集机制。所以在 Java 运行时至少会启动两个线程，**一个是 main 线程，另外一个是垃圾收集线程。**

## 线程间的通信

### 同步与锁

在Java中，锁的概念都是基于对象的，所以我们又经常称它为对象锁。线程和锁的关系，一个锁同一时间只能被一个线程持有。

在我们的线程之间，有一个同步的概念：线程同步是线程之间按照**一定的顺序**执行。

**为了达到线程同步，我们可以使用锁来实现它**。

### 等待通知机制

Java多线程的等待/通知机制是基于`Object`类的`wait()`方法和`notify()`, `notifyAll()`方法来实现的

### 信号量

JDK提供了一个类似于“信号量”功能的类`Semaphore`。但本文不是要介绍这个类，而是介绍

**一种基于`volatile`关键字的自己实现的信号量通信。**

后面会有专门的章节介绍`volatile`关键字，这里只是做一个简单的介绍。

> volatile关键字能够保证内存的可见性，如果用volatile关键字声明了一个变量，在一个线程里面改变了这个变量的值，那其它线程是立马可见更改后的值的。

比如我现在有一个需求，我想让线程A输出0，然后线程B输出1，再然后线程A输出2…以此类推。我应该怎样实现呢？

代码：

```java
public class Signal {
    private static volatile int signal = 0;

    static class ThreadA implements Runnable {
        @Override
        public void run() {
            while (signal < 5) {
                if (signal % 2 == 0) {
                    System.out.println("threadA: " + signal);
                    signal++;
                }
            }
        }
    }

    static class ThreadB implements Runnable {
        @Override
        public void run() {
            while (signal < 5) {
                if (signal % 2 == 1) {
                    System.out.println("threadB: " + signal);
                    signal = signal + 1;
                }
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        new Thread(new ThreadA()).start();
        Thread.sleep(1000);
        new Thread(new ThreadB()).start();
    }
}

// 输出：
threadA: 0
threadB: 1
threadA: 2
threadB: 3
threadA: 4
```

我们可以看到，使用了一个`volatile`变量`signal`来实现了“信号量”的模型。这里需要注意的是，`volatile`变量需要进行原子操作。

需要注意的是，`signal++`并不是一个原子操作，所以我们在实际开发中，会根据需要使用`synchronized`给它“上锁”，或者是使用`AtomicInteger`等原子类。并且上面的程序也**并不是线程安全的**，因为执行`while`语句后，可能当前线程就暂停等待时间片了，等线程醒来，可能signal已经大于等于5了。

> 这种实现方式并不一定高效，本例只是演示信号量

**信号量的应用场景**：

假如在一个停车场中，车位是我们的公共资源，线程就如同车辆，而看门的管理员就是起的“信号量”的作用。

因为在这种场景下，多个线程（超过2个）需要相互合作，我们用简单的“锁”和“等待通知机制”就不那么方便了。这个时候就可以用到信号量。

其实JDK中提供的很多多线程通信工具类都是基于信号量模型的。我们会在后面第三篇的文章中介绍一些常用的通信工具类。

### 管道

管道是基于“管道流”的通信方式。JDK提供了`PipedWriter`、 `PipedReader`、 `PipedOutputStream`、 `PipedInputStream`。其中，前面两个是基于字符的，后面两个是基于字节流的。

这里的示例代码使用的是基于字符的：

```java
public class Pipe {
    static class ReaderThread implements Runnable {
        private PipedReader reader;

        public ReaderThread(PipedReader reader) {
            this.reader = reader;
        }

        @Override
        public void run() {
            System.out.println("this is reader");
            int receive = 0;
            try {
                while ((receive = reader.read()) != -1) {
                    System.out.print((char)receive);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    static class WriterThread implements Runnable {

        private PipedWriter writer;

        public WriterThread(PipedWriter writer) {
            this.writer = writer;
        }

        @Override
        public void run() {
            System.out.println("this is writer");
            int receive = 0;
            try {
                writer.write("test");
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                try {
                    writer.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public static void main(String[] args) throws IOException, InterruptedException {
        PipedWriter writer = new PipedWriter();
        PipedReader reader = new PipedReader();
        writer.connect(reader); // 这里注意一定要连接，才能通信

        new Thread(new ReaderThread(reader)).start();
        Thread.sleep(1000);
        new Thread(new WriterThread(writer)).start();
    }
}

// 输出：
this is reader
this is writer
test
```

我们通过线程的构造函数，传入了`PipedWrite`和`PipedReader`对象。可以简单分析一下这个示例代码的执行流程：

1. 线程ReaderThread开始执行，
2. 线程ReaderThread使用管道reader.read()进入”阻塞“，
3. 线程WriterThread开始执行，
4. 线程WriterThread用writer.write("test")往管道写入字符串，
5. 线程WriterThread使用writer.close()结束管道写入，并执行完毕，
6. 线程ReaderThread接受到管道输出的字符串并打印，
7. 线程ReaderThread执行完毕。

#### 应用场景

这个很好理解。使用管道多半与I/O流相关。**当我们一个线程需要先另一个线程发送一个信息（比如字符串）或者文件等等时，就需要使用管道通信了。**

###  其它通信相关

以上介绍了一些线程间通信的基本原理和方法。除此以外，还有一些与线程通信相关的知识点，这里一并介绍。

####  join方法

join()方法是Thread类的一个实例方法。它的作用是让当前线程陷入“等待”状态，等join的这个线程执行完成后，再继续执行当前线程。

有时候，主线程创建并启动了子线程，如果子线程中需要进行大量的耗时运算，主线程往往将早于子线程结束之前结束。

如果主线程想等待子线程执行完毕后，获得子线程中的处理完的某个数据，就要用到join方法了。

示例代码：

```java
public class Join {
    static class ThreadA implements Runnable {

        @Override
        public void run() {
            try {
                System.out.println("我是子线程，我先睡一秒");
                Thread.sleep(1000);
                System.out.println("我是子线程，我睡完了一秒");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(new ThreadA());
        thread.start();
        thread.join();
        System.out.println("如果不加join方法，我会先被打出来，加了就不一样了");
    }
}
```

> 注意join()方法有两个重载方法，一个是join(long)， 一个是join(long, int)。
>
> 实际上，通过源码你会发现，join()方法及其重载方法底层都是利用了wait(long)这个方法。
>
> 对于join(long, int)，通过查看源码(JDK 1.8)发现，底层并没有精确到纳秒，而是对第二个参数做了简单的判断和处理。



#### sleep方法

sleep方法是Thread类的一个静态方法。它的作用是让当前线程睡眠一段时间。它有这样两个方法：

- Thread.sleep(long)
- Thread.sleep(long, int)

> 同样，查看源码(JDK 1.8)发现，第二个方法貌似只对第二个参数做了简单的处理，没有精确到纳秒。实际上还是调用的第一个方法。

这里需要强调一下：**sleep方法是不会释放当前的锁的，而wait方法会。**这也是最常见的一个多线程面试题。

它们还有这些区别：

- **wait可以指定时间，也可以不指定；而sleep必须指定时间。**
- **wait释放cpu资源，同时释放锁；sleep释放cpu资源，但是不释放锁，所以易死锁。**
- **wait必须放在同步块或同步方法中，而sleep可以再任意位置**

#### ThreadLocal类

ThreadLocal是一个本地线程副本变量工具类。内部是一个**弱引用**的Map来维护。这里不详细介绍它的原理，而是只是介绍它的使用，以后有独立章节来介绍ThreadLocal类的原理。

有些朋友称ThreadLocal为**线程本地变量**或**线程本地存储**。严格来说，ThreadLocal类并不属于多线程间的通信，而是让每个线程有自己”独立“的变量，线程之间互不影响。它为每个线程都创建一个**副本**，每个线程可以访问自己内部的副本变量。

ThreadLocal类最常用的就是set方法和get方法。示例代码：

```java
public class ThreadLocalDemo {
    static class ThreadA implements Runnable {
        private ThreadLocal<String> threadLocal;

        public ThreadA(ThreadLocal<String> threadLocal) {
            this.threadLocal = threadLocal;
        }

        @Override
        public void run() {
            threadLocal.set("A");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("ThreadA输出：" + threadLocal.get());
        }

        static class ThreadB implements Runnable {
            private ThreadLocal<String> threadLocal;

            public ThreadB(ThreadLocal<String> threadLocal) {
                this.threadLocal = threadLocal;
            }

            @Override
            public void run() {
                threadLocal.set("B");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("ThreadB输出：" + threadLocal.get());
            }
        }

        public static void main(String[] args) {
            ThreadLocal<String> threadLocal = new ThreadLocal<>();
            new Thread(new ThreadA(threadLocal)).start();
            new Thread(new ThreadB(threadLocal)).start();
        }
    }
}

// 输出：
ThreadA输出：A
ThreadB输出：B
```

可以看到，虽然两个线程使用的同一个ThreadLocal实例（通过构造方法传入），但是它们各自可以存取自己当前线程的一个值。

那ThreadLocal有什么作用呢？如果只是单纯的想要线程隔离，在每个线程中声明一个私有变量就好了呀，为什么要使用ThreadLocal？

如果开发者希望将类的某个静态变量（user ID或者transaction ID）与线程状态关联，则可以考虑使用ThreadLocal。

最常见的ThreadLocal使用场景为用来解决数据库连接、Session管理等。数据库连接和Session管理涉及多个复杂对象的初始化和关闭。如果在每个线程中声明一些私有变量来进行操作，那这个线程就变得不那么“轻量”了，需要频繁的创建和关闭连接。

#### InheritableThreadLocal

InheritableThreadLocal类与ThreadLocal类稍有不同，Inheritable是继承的意思。它不仅仅是当前线程可以存取副本值，而且它的子线程也可以存取这个副本值。

## 线程的调度

线程调度方式：

1. 轮流
2. 根据优先级，抢占CPU；优先级高的抢占的概率大

**Java的调度方法：**
1.对于同优先级的线程组成先进先出队列（先到先服务），使用时间片策略
2.对高优先级，使用优先调度的抢占式策略

**线程的优先级**
等级：
MAX_PRIORITY:10
MIN_PRIORITY:1
NORM_PRIORITY:5 //默认优先级为5

**方法：**
getPriority():返回线程优先级
setPriority(int newPriority):改变线程的优先级

注意！：高优先级的线程要抢占低优先级的线程的cpu的执行权。但是仅是从概率上来说的，高优先级的线程更有可能被执行。并不意味着只有高优先级的线程执行完以后，低优先级的线程才执行。

```java
package MThread.demo4;

import java.util.concurrent.Callable;

public class Mycallable implements Callable {
    @Override
    public Object call() throws Exception {
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName()+""+i);
        }
        return Thread.currentThread().getName();
    }
}




public class PriorityTest {
    public static void main(String[] args) {
        Mycallable mycallable = new Mycallable();
        FutureTask<String> F = new FutureTask<>(mycallable);
        Thread T1 = new Thread(F,"张三");
        T1.setPriority(10);
        T1.start();

        Mycallable mycallable2 = new Mycallable();
        FutureTask<String> F2 = new FutureTask<>(mycallable2);
        new Thread(F2,"李四").start();

    }
}

```

## 线程安全问题

#### **概念**

什么是线程安全问题呢？
线程安全问题是指，多个线程对同一个共享数据进行操作时，线程没来得及更新共享数据，从而导致另外线程没得到最新的数据，从而产生线程安全问题。

上述例子中：创建三个窗口卖票，总票数为100张票
1.卖票过程中，出现了重票（票被反复的卖出，ticket未被减少时就打印出了）错票。
2.问题出现的原因：当某个线程操作车票的过程中，尚未完成操作时，其他线程参与进来，也来操作车票。（将此过程的代码看作一个区域，当有线程进去时，装锁，不让别的线程进去）
生动理解的例子：有一个厕所，有人进去了，但是没有上锁，于是别人不知道你进去了，别人也进去了对厕所也使用造成错误。
3.如何解决：当一个线程在操作ticket时，其他线程不能参与进来，直到此线程的生命周期结束
4.在java中，我们通过同步机制，来解决线程的安全问题。

```java
package MThread.Ticket;

public class Ticket implements  Runnable{
    private int ticket = 100;
    @Override
    public void run() {
        while (true){
            if(ticket==0){
                break;
            }else {
                ticket--;
                System.out.println(Thread.currentThread().getName()+"还剩票数："+ticket);
            }
        }
    }
}


package MThread.Ticket;

public class TicketTest {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();

        Thread t1 = new Thread(ticket,"窗口1");
        Thread t2 = new Thread(ticket,"窗口2");
        Thread t3 = new Thread(ticket,"窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}

```

以上正常，但实际出票需要时间的

```java
public class Ticket implements  Runnable{
    private int ticket = 100;
    @Override
    public void run() {
        while (true){
            if(ticket==0){
                break;
            }else {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                ticket--;
                System.out.println(Thread.currentThread().getName()+"还剩票数："+ticket);
            }
        }
    }
}

```

此时出现重复票，出现负号票

#### **同步代码块**

使用同步监视器（锁/任意对象），**锁必须唯一**
**Synchronized（同步监视器）{**
**//需要被同步的代码**
**}**

```java
package MThread.Ticket;

public class Ticket implements  Runnable{
    private int ticket = 100;
    @Override
    public void run() {
        while (true){
            synchronized (this){
                if(ticket==0){
                    break;
                }else {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    ticket--;
                    System.out.println(Thread.currentThread().getName()+"还剩票数："+ticket);
            }

            }
        }
    }
}
```

说明：

锁默认是打来的，线程进入自动关闭，线程执行完自动打开

操作共享数据的代码（所有线程共享的数据的操作的代码）（视作卫生间区域（所有人共享的厕所）），即为需要共享的代码（同步代码块，在同步代码块中，相当于是一个单线程，效率低）
共享数据：多个线程共同操作的数据，比如公共厕所就类比共享数据
同步监视器（俗称：锁）：任何一个的对象都可以充当锁。（但是为了可读性一般设置英文成lock）当锁住以后只能有一个线程能进去（要求:多个线程必须要共用同一把锁，比如火车上的厕所，同一个标志表示有人）
Runnable天生共享锁，而Thread中需要用static对象或者this关键字或者当前类（window。class）来充当唯一锁

#### 同步方法

使用同步方法，对方法进行synchronized关键字修饰，**其锁对象默认为this**，不能指定
将同步代码块提取出来成为一个方法，用synchronized关键字修饰此方法。

- 对于runnable接口实现多线程，只需要将同步方法用synchronized修饰
- 而对于继承自Thread方式，需要将同步方法用static和synchronized修饰，因为对象不唯一（锁不唯一）

```java
package MThread.Ticket;

public class Ticket implements  Runnable{
    private int ticket = 100;
    @Override
    public void run() {
        while (true){
            if("窗口一".equals(Thread.currentThread().getName())){
              //同步方法一
                boolean result = syn_method();
                if(result){
                    break;
                }
            }
            if("窗口二".equals(Thread.currentThread().getName())){
              //同步方法二
            synchronized (this) {
                if (ticket == 0) {
                    break;
                } else {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    ticket--;
                    System.out.println(Thread.currentThread().getName() + "还剩票数：" + ticket);
                }
            }

            }
        }
    }

    private synchronized boolean syn_method(){
        if(ticket==0){
            return true;
        }else {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            ticket--;
            System.out.println(Thread.currentThread().getName()+"还剩票数："+ticket);
            return false;
        }
    }
}




package MThread.Ticket;

public class TicketTest {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();

        Thread t1 = new Thread(ticket,"窗口一");
        Thread t2 = new Thread(ticket,"窗口二");

        t1.start();
        t2.start();

    }
}

```

**总结：**

1.同步方法仍然涉及到同步监视器，只是不需要我们显示的声明。
2.非静态的同步方法，同步监视器是this
3.静态的同步方法，

- 静态方法中是没有this的
- 因此同步监视器/锁 是**当前类本身**（类名.class）。继承自Thread.class

（注意：静态方法只能访问静态变量）

```java
package MThread.Ticket;

public class Ticket implements  Runnable{
    private static int ticket = 100;
    @Override
    public void run() {
        while (true){
            if("窗口一".equals(Thread.currentThread().getName())){
              //同步方法一
                boolean result = syn_method();
                if(result){
                    break;
                }
            }
            if("窗口二".equals(Thread.currentThread().getName())){
              //同步方法二
            synchronized (Ticket.class) {
                if (ticket == 0) {
                    break;
                } else {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    ticket--;
                    System.out.println(Thread.currentThread().getName() + "还剩票数：" + ticket);
                }
            }

            }
        }
    }

    private static synchronized boolean syn_method(){
        if(ticket==0){
            return true;
        }else {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            ticket--;
            System.out.println(Thread.currentThread().getName()+"还剩票数："+ticket);
            return false;
        }
    }
}

```

##  Lock锁 

便于观察哪里加锁哪里释放锁；JDK5提供了新的锁对象Lock

Lock对象提供了两种方法：

- viod lock（）；
- viod unlock（）；

Lock是接口不能实例化，这里采用它的实现类ReentrantLock来实例化



```java
package MThread.Ticket;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Ticket implements  Runnable{
    private static int ticket = 100;
    private ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true){
              //同步方法二
                try {
                    lock.lock();
                    if (ticket == 0) {
                        break;
                    } else {

                    Thread.sleep(100);
                    ticket--;
                    System.out.println(Thread.currentThread().getName() + "还剩票数：" + ticket);

                    }
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    lock.unlock();
                }
            }
        }

package MThread.Ticket;

public class TicketTest {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();

        Thread t1 = new Thread(ticket,"窗口一");
        Thread t2 = new Thread(ticket,"窗口二");


        t1.start();
        t2.start();

    }
}

```

## API

| **方法名**             |                        **方法描述**                         | 注意                                                         |
| :--------------------- | :---------------------------------------------------------: | ------------------------------------------------------------ |
| start()                |  使该线程开始执行；**Java** 虚拟机调用该线程的 run 方法。   | start 方法只是让线程进入就绪，里面代码不一定立刻运行（CPU 的时间片还没分给它）。每个线程对象的start方法只能调用一次，如果调用了多次会出现IllegalThreadStateException |
| run()                  |                  新线程启动后会调用的方法                   | 如果在构造 Thread 对象时传递了 Runnable 参数，则线程启动后会调用 Runnable 中的 run 方法，否则默认不执行任何操作。但可以创建 Thread 的子类对象，来覆盖默认行为 |
| join()                 |                    等待**线程运行结束**                     |                                                              |
| join(long n)           |                等待线程运行结束,最多等待毫秒                |                                                              |
| getState()             |                        获取线程状态                         | Java 中线程状态是用 6 个 enum 表示，分别为：<br/>NEW, RUNNABLE, BLOCKED, WAITING,<br/>TIMED_WAITING, TERMINATED |
| isInterrupted()        |                       判断是否被打断                        | 不会清除 打断标记                                            |
| interrupt()            |                          打断线程                           | 如果被打断线程正在 sleep，wait，join 会导致被打断的线程抛出 InterruptedException，并清除打断标记；如果打断的正在运行的线程，则会设置 打断标记；park 的线程被打断，也会设置打断标记 |
| interrupted() static   |                   判断当前线程是否被打断                    | 会清除 打断标记                                              |
| currentThread() static |                   获取当前正在执行的线程                    |                                                              |
| sleep(long n) static   | 让当前执行的线程休眠n毫秒，休眠时让出 cpu的时间片给其它线程 |                                                              |
| yield() static         |                  提示线程调度器让出当前线                   | 主要是为了测试和调试                                         |

### run-start

1. 直接调用 run 是在主线程中执行了 run，没有启动新的线程
2. 使用 start 是启动新的线程，通过新的线程间接执行 run 中的代码
3. start()不能多次调用

### sleep-yield

**sleep**

1. 调用 sleep 会让当前线程从 Running 进入 **Timed Waiting** 状态（阻塞）

2. 其它线程可以使用 interrupt 方法打断正在睡眠的线程，这时 sleep 方法会抛出 InterruptedException

   因为异常不能跨线程传播回 main() 中，因此必须在本地进行处理。线程中抛出的其它异常也同样需要在本地进行处理。

3. **睡眠结束后的线程未必会立刻得到执行，需要等待分配时间片**

4. 建议用 TimeUnit 的 sleep 代替 Thread 的 sleep 来获得更好的可读性

5. Thread.sleep(millisec) 方法会休眠当前正在执行的线程，millisec 单位为毫秒。

```
package MThread.demo2;

public class DemoSleep implements Runnable {
    @Override
    public void run() {
        for(int i = 0; i<10; i++){
            try {
                Thread.currentThread().sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+""+i);
        }

    }
}
```

**yield**

1. 调用 yield 会让当前线程从 Running 进入 **Runnable** 就绪状态，然后调度执行其它线程
2. 具体的实现依赖于操作系统的任务调度器
3. 分配到时间片可以立即执行

注意：

**1.sleep会使当前线程睡眠指定时间，不释放锁**

**2.yield会使当前线程重回到可执行状态，等待cpu的调度，不释放锁**

### join

#### 背景

```java
static int r = 0;
public static void main(String[] args) throws InterruptedException {
    test1();
}
private static void test1() throws InterruptedException {
    log.debug("开始");
    Thread t1 = new Thread(() -> {
        log.debug("开始");
        sleep(1);
        log.debug("结束");
        r = 10;
    });
    t1.start();
    log.debug("结果为:{}", r);
    log.debug("结束");
}
```

**分析**

因为主线程和线程 t1 是并行执行的，t1 线程需要 1 秒之后才能算出 r=10
而主线程一开始就要打印 r 的结果，所以只能打印出 r=0
**解决方法**

用 sleep 行不行？为什么？ —时间不好把控

**用 t1.join，加在 t1.start() 之后即可**

#### 作用

实现进程同步

- **需要等待结果返回，才能继续运行就是同步**
- 不需要等待结果返回，就能继续运行就是异步

#### 原理

将线程作为锁，调用者轮询检查该线程 alive 状态

```
t1.join();
```

等价于

```java
synchronized (t1) {
    // 调用者线程进入 t1 的 waitSet 等待, 直到 t1 运行结束
    while (t1.isAlive()) {
        t1.wait(0);//wait(0)表示一直休眠。和wait一样
    }
}
```



### wait-join

wait**会使当前线程进入waiting队列（并非Blocked队列），释放锁**，当被其他线程使用notify，该线程进入Blocked队列等待，唤醒时进入可执行状态

当前线程调用 **某线程.join（）**时会使当前线程等待某线程执行完毕再继续执行，**底层调用了wait，释放锁**

waiting队列 waitSet队列

Blocked队列 entryList队列

### wait-notify

#### **介绍**

1. obj.wait() 让进入 object 监视器的线程到 waitSet 等待
2. obj.notify() 在 object 上正在 waitSet 等待的线程中挑一个唤醒进入同步队列
3. obj.notifyAll() 让 object 上正在 waitSet 等待的线程全部唤醒进入同步队列

wait() 方法会释放对象的锁，进入 WaitSet 等待区，从而让其他线程就机会获取对象的锁。无限制等待，直到
notify 为止

**wait(long n) 有时限的等待, 到 n 毫秒后结束等待，或是被 notify**

#### **注意**

- 下次运行时运行wait之后的代码, 线程的上下文切换
- 它们都是线程之间进行协作的手段，都属于 Object 对象的方法。必须获得此对象的锁，才能调用这几个方法

```java
final static Object obj = new Object();
public static void main(String[] args) {
    new Thread(() -> {
        synchronized (obj) {
            log.debug("执行....");
            try {
                obj.wait(); // 让线程在obj上一直等待下去
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            log.debug("其它代码....");
        }
    }).start();
    new Thread(() -> {
        synchronized (obj) {
            log.debug("执行....");
            try {
                obj.wait(); // 让线程在obj上一直等待下去
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            log.debug("其它代码....");
        }
    }).start();
    // 主线程两秒后执行
    sleep(2);
    log.debug("唤醒 obj 上其它线程");
    synchronized (obj) {
        obj.notify(); // 唤醒obj上一个线程
        // obj.notifyAll(); // 唤醒obj上所有等待线程
    }
}

```

#### **原理**

![image-20211112152149154](笔记图库/image-20211112152149154.png)

1. Owner 线程发现条件不满足，调用 wait 方法，即可进入 WaitSet 变为 WAITING 状态
2. BLOCKED 和 WAITING 的线程都处于阻塞状态，不占用 CPU 时间片
3. BLOCKED 线程会在 Owner 线程释放锁时进入可执行状态
4. WAITING 线程会在 Owner 线程调用 notify 或 notifyAll 时，但唤醒后进入Entrylist重新竞争

#### 使用方法

```java
synchronized(lock) {
    while(条件不成立) {
        lock.wait();
    }
    // 干活
}
//
另一个线程
    synchronized(lock) {
    lock.notifyAll();
}
```

### interrupt

##### **打断阻塞状态的线程**

打断 **sleep，wait，join** 的线程

这几个方法都会让线程进入阻塞状态
打断 sleep 的线程, 会清空打断状态，以 sleep 为例

```java
private static void test1() throws InterruptedException {
	Thread t1 = new Thread(()->{
		sleep(1);
	}, "t1");
	t1.start();
	sleep(0.5);
	t1.interrupt();
	log.debug(" 打断状态: {}", t1.isInterrupted());
}
```

输出

```
java.lang.InterruptedException: sleep interrupted
at java.lang.Thread.sleep(Native Method)
at java.lang.Thread.sleep(Thread.java:340)
at java.util.concurrent.TimeUnit.sleep(TimeUnit.java:386)
at cn.itcast.n2.util.Sleeper.sleep(Sleeper.java:8)
at cn.itcast.n4.TestInterrupt.lambda$test1$3(TestInterrupt.java:59)
at java.lang.Thread.run(Thread.java:745)
21:18:10.374 [main] c.TestInterrupt - 打断状态: false
```

**sleep，wait，join被打断后，打断标记仍然为false**



##### 打断正常运行的线程

```java
private static void test2() throws InterruptedException {
	Thread t2 = new Thread(()->{
		while(true) {
			Thread current = Thread.currentThread();
			boolean interrupted = current.isInterrupted();
			if(interrupted) {
				log.debug(" 打断状态: {}", interrupted);
				break;
			}
		}
	}, "t2");
	t2.start();
	sleep(0.5);
	t2.interrupt();
}
```

输出

```
20:57:37.964 [t2] c.TestInterrupt - 打断状态: true
```

**对于正在运行的线程，interrupt 只会为线程将打断标记置为true，是否被打断由线程自己决定；**

*模式两阶终止*

##### 打断 park 线程

打断 park 线程, 不会清空打断状态

```
private static void test3() throws InterruptedException {
Thread t1 = new Thread(() -> {
log.debug("park...");
LockSupport.park();
log.debug("unpark...");
log.debug("打断状态：{}", Thread.currentThread().isInterrupted());
}, "t1");
t1.start();
sleep(0.5);
t1.interrupt();
```

输出

```
21:11:52.795 [t1] c.TestInterrupt - park...
21:11:53.295 [t1] c.TestInterrupt - unpark...
21:11:53.295 [t1] c.TestInterrupt - 打断状态：true
```

打断状态为true时，park失效

### sleep-wait

#### **来源**

- sleep来自Thread类，和wait来自Object类。

  sleep是Thread的**静态类方法**，谁调用的谁去睡觉

#### **系统资源**

- 最主要是**sleep方法没有释放锁**，而**wait方法释放了锁**，使得其他线程可以使用同步控制块或者方法。

这里需要强调一下：**sleep方法是不会释放当前的锁的，而wait方法会。**这也是最常见的一个多线程面试题。

#### 时限

- **wait可以指定时间，也可以不指定；而sleep必须指定时间。**一般wait不会加时间限制，因为如果wait线程的运行资源不够，再出来也没用，要等待其他线程调用notify/notifyAll唤醒等待池中的所有线程，才会进入同步队列等待OS分配系统资源。

#### **适用范围**

- sleep 不需要强制和 synchronized 配合使用，但 wait 需要和 synchronized 一起用（获得锁对象）

```java
 synchronized(x){ 
      x.notify() 
     //或者wait() 
   }
```

#### 总结

两者都可以暂停线程的执行。

对于sleep()方法，我们首先要知道该方法是属于Thread类中的。而wait()方法，则是属于Object类中的。

Wait 通常被用于线程间交互/通信，sleep 通常被用于暂停执行。

sleep()方法导致了程序暂停执行指定的时间，**让出cpu该其他线程**，但是他的监控状态依然保持者，当指定的时间到了又会自动恢复运行状态。

在调用sleep()方法的过程中，线程不会释放对象锁。

而当调用wait()方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用notify()方法后本线程才进入对象锁定池准备，获取对象锁进入运行状态。线程不会自动苏醒。

什么意思呢？
举个列子说明：

```java
 public static void main(String[] args) {
        new Thread(new Thread1()).start();
        try {
            Thread.sleep(5000);
        } catch (Exception e) {
            e.printStackTrace();
        }
        new Thread(new Thread2()).start();
    }

    private static class Thread1 implements Runnable{
        @Override
        public void run(){
            synchronized (MyTestDemo.class) {
                System.out.println("enter thread1...");
                System.out.println("thread1 is waiting...");
                try {
                    //调用wait()方法，线程会放弃对象锁，进入等待此对象的等待锁定池
                    MyTestDemo.class.wait();
                } catch (Exception e) {
                    e.printStackTrace();
                }
                System.out.println("thread1 is going on ....");
                System.out.println("thread1 is over!!!");
            }
        }
    }

    private static class Thread2 implements Runnable {
        @Override
        public void run() {
            synchronized (MyTestDemo.class) {
                System.out.println("enter thread2....");
                System.out.println("thread2 is sleep....");
                //只有针对此对象调用notify()方法后本线程才进入对象锁定池准备获取对象锁进入运行状态。
                MyTestDemo.class.notify();
                //==================
                //区别
                //如果我们把代码：MyTestDemo.class.notify();给注释掉，MyTestDemo.class调用了wait()方法，但是没有调用notify()
                //方法，则线程永远处于挂起状态。
                try {
                    //sleep()方法导致了程序暂停执行指定的时间，让出cpu该其他线程，
                    //但是他的监控状态依然保持者，当指定的时间到了又会自动恢复运行状态。
                    //在调用sleep()方法的过程中，线程不会释放对象锁。
                    Thread.sleep(5000);
                } catch (Exception e) {
                    e.printStackTrace();
                }
                System.out.println("thread2 is going on....");
                System.out.println("thread2 is over!!!");
            }
        }
    }
```

运行效果：

```csharp
thread1 is waiting...
enter thread2....
thread2 is sleep....
thread2 is going on....
thread2 is over!!!
thread1 is going on ....
thread1 is over!!!
```

如果注释掉代码

```cpp
MyTestDemo.class.notify();
```

运行效果：

```csharp
enter thread1...
thread1 is waiting...
enter thread2....
thread2 is sleep....
thread2 is going on....
thread2 is over!!!
```

程序会一直处于挂起状态。

### park-unpark

#### 概念

LockSupport 的 `park/unpark` 方法本质上是对 Unsafe 的 `park/unpark` 方法的简单封装，而后者是 native 方法，对 Java 程序来说是一个黑箱操作，那么要想了解它的底层实现，就必须深入 Java 虚拟机的源码。

#### 基本使用

它们是 LockSupport 类中的方法

##### 先 park 再 unpark

```java
// 暂停当前线程
LockSupport.park();
// 恢复某个线程的运行
LockSupport.unpark(暂停线程对象)
```

```java
Thread t1 = new Thread(() -> {
    log.debug("start...");
    sleep(1);
    log.debug("park...");
    
    LockSupport.park();
    
    log.debug("resume...");
},"t1");
t1.start();

sleep(2);
log.debug("unpark...");

LockSupport.unpark(t1);
```

输出

```
18:42:52.585 c.TestParkUnpark [t1] - start...
18:42:53.589 c.TestParkUnpark [t1] - park...
18:42:54.583 c.TestParkUnpark [main] - unpark...
18:42:54.583 c.TestParkUnpark [t1] - resume...
```



##### 先 unpark 再 park

```java
Thread t1 = new Thread(() -> {
    log.debug("start...");
    sleep(2);
    log.debug("park...");
    LockSupport.park();
    log.debug("resume...");
}, "t1");

t1.start();
sleep(1);

log.debug("unpark...");
LockSupport.unpark(t1);
```

```
18:43:50.765 c.TestParkUnpark [t1] - start...
18:43:51.764 c.TestParkUnpark [main] - unpark...
18:43:52.769 c.TestParkUnpark [t1] - park...
18:43:52.769 c.TestParkUnpark [t1] - resume...
```

#### 特点

与 Object 的 wait & notify 相比

- wait，notify 和 notifyAll 必须配合 Object Monitor 一起使用，而 park，unpark 不必
- park & unpark 是以线程为单位来**【阻塞】和【唤醒】**线程，而 notify 只能随机唤醒一个等待线程，notifyAll是唤醒所有等待线程，就不那么【精确】
- park & unpark 可以先 unpark，而 wait & notify 不能先 notify

#### 原理

两者的核心操作都是通过委托当前线程所关联的 Parker 对象来完成的（每个线程都会关联一个自己的 Parker 对象），于是，Parker 对象的 `park/unpark` 方法就成为了我们的焦点。

每个线程都会关联一个 Parker 对象，每个 Parker 对象都各自维护了三个角色：计数器、互斥量、条件变量。

**park 操作：**

1. 获取当前线程关联的 Parker 对象。
2. 将计数器置为 0，同时检查计数器的原值是否为 1，如果是则放弃后续操作。
3. 在互斥量上加锁。
4. 在条件变量上阻塞，同时释放锁并等待被其他线程唤醒，当被唤醒后，将重新获取锁。
5. 当线程恢复至运行状态后，将计数器的值再次置为 0。
6. 释放锁。

**unpark 操作：**

1. 获取目标线程关联的 Parker 对象（注意目标线程不是当前线程）。
2. 在互斥量上加锁。
3. 将计数器置为 1。
4. 唤醒在条件变量上等待着的线程。
5. 释放锁。

**注意**：尽管每个线程都关联一个parker（好像是线程私有但不是），但unpark时是其他线程访问 该线程的parker并修改这个线程的 计数器，因此并不是线程私有，计数器的读写需要加锁

```
每个线程都有自己的一个 Parker 对象，由三部分组成 _counter ， _cond 和 _mutex 打个比喻

线程就像一个旅人，Parker 就像他随身携带的背包，条件变量就好比背包中的帐篷。_counter 就好比背包中的备用干粮（0 为耗尽，1 为充足）

调用 park 就是要看需不需要停下来歇息
如果备用干粮耗尽，那么钻进帐篷歇息
如果备用干粮充足，那么不需停留，继续前进

调用 unpark，就好比令干粮充足
如果这时线程还在帐篷，就唤醒让他继续前进
如果这时线程还在运行，那么下次他调用 park 时，仅是消耗掉备用干粮，不需停留继续前进
因为背包空间有限，多次调用 unpark 仅会补充一份备用干粮
```

例一：

1. 当前线程调用LockSupport.park() 方法
2. 检查 _counter ，本情况为 0，这时，获得 _mutex 互斥锁
3. 线程进入 _cond 条件变量阻塞
4. 设置 _counter = 0

例二：

1. 调用 Unsafe.unpark(Thread_0) 方法，设置 _counter 为 1
2. 唤醒 _cond 条件变量中的 Thread_0
3. Thread_0 恢复运行
4. 设置 _counter 为 0

例三：

1. 调用 Unsafe.unpark(Thread_0) 方法，设置 _counter 为 1
2. 当前线程调用 Unsafe.park() 方法
3. 检查 _counter ，本情况为 1，这时线程无需阻塞，继续运行
4. 设置 _counter 为 0

![image-20211116103424269](笔记图库/image-20211116103424269.png)





### 获取和设置线程的名称

使用runnable无法直接使用getname（Thread的方法）

 **获取当前线程对象：Thread.currentThread()**

Thread.currentThread().getName()

```java
public class Myrunnable implements Runnable {
    @Override
    public void run() {
        for(int i = 0; i<10; i++){
            System.out.println(Thread.currentThread().getName()+""+i);
        }
    }
}
```



### 守护线程 

##### **线程的分类**

java中的线程分为两类：1.**守护线程**（如垃圾回收线程，异常处理线程），2.**用户线程**（如主线程）

##### 作用

**当所有非守护线程结束时，程序也就终止，同时会杀死所有守护线程。**

。

##### 使用

**public final void setDaemon(boolean on)** ：将该线程标记为守护线程或用户线程。。

在线程启动之前使用 setDaemon() 方法可以将一个线程设置为守护线程。

##### 问题

问题：**jre 判断程序是否执行结束的标准是**：

**所有主线程是否结束**

main()函数即主函数，是一个主线程，主进程是程序中必须执行完成的，而守护线程则是java中所有主结束后结束，不管有没有完成，守护线程主要用与内存分配等方面。     

##### 主线程和守护线程的区别和联系

1、守护线程不会阻止进程的终止。属于某个进程的所有主线程都终止后，该进程就会被终止。所有剩余的守护线程都会停止且不会完成。
2、可以在任何时候将主线程修改为守护线程，方式是设置Thread.IsBackground 属性。
3、不管是主线程还是守护线程，如果线程内出现了异常，都会导致进程的终止。

4、托管线程池中的线程都是守护线程，使用new Thread方式创建的线程默认都是主线程。

##### 说明  

- main() 属于非守护线程
-  应用程序的主线程以及使用Thread构造的线程都默认为主线程
-  使用Thread建立的线程默认情况下是主线程，在进程中，只要有一个主线程未退出，进程就不会终止。
- 主线程就是一个主线程。
- 守护线程不管线程是否结束，**只要所有的主线程都退出（包括正常退出和异常退出）后，进程就会自动终止。**
- 一般守护线程用于处理时间较短的任务，如在一个Web服务器中可以利用守护线程来处理客户端发过来的请求信息。
- 主线程一般用于处理需要长时间等待的任务，如在Web服务器中的监听客户端请求的程序，或是定时对某些系统资源进行扫描的程序
- 垃圾回收器线程就是一种守护线程
- Tomcat 中的 Acceptor 和 Poller 线程都是守护线程，所以 Tomcat 接收到 shutdown 命令后，不会等
  待它们处理完当前请求

```java
package MThread.demo3;

public class daemonTest {
    public static void main(String[] args) {
        Mythread1 T1 = new Mythread1();
        T1.setName("a");
        Mythread2 T2 = new Mythread2();
        T2.setName("b");
        T2.setDaemon(true);
        T1.start();
        T2.start();

    }
}
```

## 小结

本章的重点在于掌握

**线程创建**

**线程重要 api**

如 start，run，sleep，join，interrupt 等

**线程状态**

**应用方面**

异步调用：主线程执行期间，其它线程异步执行耗时操作
提高效率：并行计算，缩短运算时间
同步等待：join
统筹规划：合理使用线程，得到最优效果

**原理方面**

线程运行流程：栈、栈帧、上下文切换、程序计数器
Thread 两种创建方式 的源码

**模式方面**

终止模式之两阶段终止

# 管程

## 问题

### 临界区 

Critical Section

- 一个程序运行多个线程本身是没有问题的
- 问题出在多个线程访问共享资源
  - 多个线程读共享资源其实也没有问题
  - 在多个线程对共享资源读写操作时发生指令交错，就会出现问题
- 一段代码块内如果存在对共享资源的多线程读写操作，称这段代码块为临界区

```java
static int counter = 0;
static void increment()
// 临界区
{
counter++;
}
static void decrement()
// 临界区
{
counter--;
}
```

### **竞态条件 **

**Race Condition**

多个线程在临界区内执行，由于代码的执行序列不同而导致结果无法预测，称之为发生了竞态条件

## synchronized



为了避免临界区的竞态条件发生，有多种手段可以达到目的。
阻塞式的解决方案：synchronized，Lock
非阻塞式的解决方案：原子变量

本次课使用阻塞式的解决方案：synchronized，来解决上述问题，即俗称的【对象锁】，它采用互斥的方式让同一时刻至多只有一个线程能持有【对象锁】，其它线程再想获取这个【对象锁】时就会阻塞住。这样就能保证拥有锁的线程可以安全的执行临界区内的代码，不用担心线程上下文切换；

**注意**
虽然 java 中互斥和同步都可以采用 synchronized 关键字来完成，但它们还是有区别的：
互斥是保证临界区的竞态条件发生，同一时刻只能有一个线程执行临界区代码
同步是由于线程执行的先后、顺序不同、需要一个线程等待其它线程运行到某个点

### 代码块加锁

#### 语法

代码块

```
synchronized(对象) // 线程1， 线程2(blocked)
{
临界区
}
```

#### 使用

```java
static int counter = 0;
static final Object room = new Object();
public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread(() -> {
        for (int i = 0; i < 5000; i++) {
            
            synchronized (room) {
                counter++;
            }
            
        }
    }, "t1");
    
    Thread t2 = new Thread(() -> {
        for (int i = 0; i < 5000; i++) {
            
            synchronized (room) {
                counter--;
            }
            
        }
    }, "t2");
    t1.start();
    t2.start();
    t1.join();
    t2.join();
    log.debug("{}",counter);
}
```

#### 思考

synchronized 实际是用对象锁保证了临界区内代码的原子性，临界区内的代码对外是不可分割的，不会被线程切
换所打断。
为了加深理解，请思考下面的问题

- 如果把 synchronized(obj) 放在 for 循环的外面，如何理解？-- 原子性
- 如果 t1 synchronized(obj1) 而 t2 synchronized(obj2) 会怎样运作？-- 锁对象
- 如果 t1 synchronized(obj) 而 t2 没有加会怎么样？如何理解？-- 锁对象



#### 面向对象改进

把需要保护的共享对象放入一个类中

```java
class Room {
    int value = 0;
    public void increment() {
        synchronized (this) {
            value++;
        }
    }
    public void decrement() {
        synchronized (this) {
            value--;
        }
    }
    public int get() {
        synchronized (this) {
            return value;
        }
    }
}
@Slf4j
public class Test1 {
    public static void main(String[] args) throws InterruptedException {
        Room room = new Room();
        Thread t1 = new Thread(() -> {
            for (int j = 0; j < 5000; j++) {
                room.increment();
            }
        }, "t1");
        Thread t2 = new Thread(() -> {
            for (int j = 0; j < 5000; j++) {
                room.decrement();
            }
        }, "t2");
        t1.start();
        t2.start();
        t1.join();
        t2.join();
        log.debug("count: {}" , room.get());
    }
}

```

get() 方法中加锁 是为了防止脏读

### 方法上加锁

#### 加在成员方法上

this

```java
class Test{
    public synchronized void test() {
    }
}
等价于
    class Test{
        public void test() {
            synchronized(this) {
            }
        }
    }
```

#### 加在静态方法上

Test.class

```java
class Test{
    public synchronized static void test() {
    }
}
等价于
    class Test{
        public static void test() {
            synchronized(Test.class) {
            }
        }
    }
```

#### 不加 

不加 synchronzied 的方法就好比不遵守规则的人，不去老实排队（好比翻窗户进去的）

## 变量的线程安全

### 成员变量和静态变量

- 如果它们没有共享，则线程安全
- 如果它们被共享了，根据它们的状态是否能够改变，又分两种情况
  - 如果只有读操作，则线程安全
  - 如果有读写操作，则这段代码是临界区，需要考虑线程安全

### 局部变量

- 局部变量是线程安全的

- 但局部变量引用的对象则未必

  - 如果该对象没有逃离方法的作用访问，它是线程安全的

  - 如果该对象逃离方法的作用范围，需要考虑线程安全

  - 子类调用 被重写的父类方法时 ，引入的参数可能产生变量逃逸

    ```java
    public abstract class Test {
        public void bar() {
            // 是否安全
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            foo(sdf);
        }
        public abstract foo(SimpleDateFormat sdf);
        public static void main(String[] args) {
            new Test().bar();
        }
    }
    ```

- 同时也具有相对性

  方法内部调用方法 也可能导致线程不安全

  下面的list就是线程不安全的

  ```java
  class ThreadSafe {
      public final void method1(int loopNumber) {
          ArrayList<String> list = new ArrayList<>();
          for (int i = 0; i < loopNumber; i++) {
              method2(list);
              method3(list);
          }
      }
  
      public void method2(ArrayList<String> list) {
          list.add("1");
      }
  
      private void method3(ArrayList<String> list) {
          System.out.println(1);
          System.out.println(2);
          new Thread(() -> {
              list.remove(0);
          }).start();
      }
  }
  ```

**String 类由final修饰，实际上就是为了防止 继承的子类重写父类方法时出现变量逃逸**



### 常见线程安全类

1. String
2. Integer
3. StringBuffer
4. Random
5. Vector
6. Hashtable
7. java.util.concurrent 包下的类

这里说它们是线程安全的是指，多个线程调用它们同一个实例的某个方法时，是线程安全的。也可以理解为

```java
Hashtable table = new Hashtable();
new Thread(()->{
    table.put("key", "value1");
}).start();
new Thread(()->{
    table.put("key", "value2");
}).start();
```

它们的每个方法是原子的
但注意它们多个方法的组合不是原子的，见后面分析

### 不可变类线程安全性

**String、Integer 等都是不可变类**，因为其内部的状态不可以改变，因此它们的方法都是线程安全的

有同学或许有疑问，String 有 replace，substring 等方法【可以】改变值啊，那么这些方法又是如何保证线程安
全的呢？

- 他们是创建了新的对象

### 习题

测试下面代码是否存在线程安全问题，并尝试改正

```java
public class ExerciseTransfer {
    public static void main(String[] args) throws InterruptedException {
        Account a = new Account(1000);
        Account b = new Account(1000);
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                a.transfer(b, randomAmount());
            }
        }, "t1");
        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                b.transfer(a, randomAmount());
            }
        }, "t2");
        t1.start();
        t2.start();
        t1.join();
        t2.join();
        // 查看转账2000次后的总金额
        log.debug("total:{}",(a.getMoney() + b.getMoney()));
    }
    // Random 为线程安全
    static Random random = new Random();
    // 随机 1~100
    public static int randomAmount() {
        return random.nextInt(100) +1;
    }
}
class Account {
    private int money;
    public Account(int money) {
        this.money = money;
    }
    public int getMoney() {
        return money;
    }
    public void setMoney(int money) {
        this.money = money;
    }
    public void transfer(Account target, int amount) {
        if (this.money > amount) {
            this.setMoney(this.getMoney() - amount);
            target.setMoney(target.getMoney() + amount);
        }
    }
}
```

这样改正行不行，为什么？

```java
public synchronized void transfer(Account target, int amount) {
    if (this.money > amount) {
        this.setMoney(this.getMoney() - amount);
        target.setMoney(target.getMoney() + amount);
    }
}
```

synchronized修饰方法是对this加锁

target也需要上锁

## Monitor

### 对象头

普通对象头

![image-20211111083717038](笔记图库/image-20211111083717038.png)

Klass word : class类型

Markword ：对象自身运行时的数据

![image-20211111083927972](笔记图库/image-20211111083927972.png)

#### 工具

依赖

```xml
   <dependency>
      <groupId>org.openjdk.jol</groupId>
      <artifactId>jol-core</artifactId>
      <version>0.8</version>
    </dependency>
  </dependencies>

```

使用

```java
ClassLayout.parseInstance(object).toPrintable()
```

### 概念

Monitor 被翻译为监视器或管程；**属于操作系统的互斥量**

每个 Java 对象都可以关联**唯一**一个 Monitor 对象

运行时 如果**使用 synchronized 给对象上锁**（重量级）之后，该**对象头的Mark Word 中就被设置指向 Monitor 对象的指针**



### 原理

![image-20211111150919487](笔记图库/image-20211111150919487.png)



1. 一旦出现了竞争，该锁对象就会永久关联一个Monitor（对象头Markword信息记录monitor地址）；
2. 刚开始 Monitor 中 Owner 为 null
3. 当 Thread-2 执行 synchronized(obj) 会根据 object的对象头Mark Word 找到Monitor
4. 将 Monitor 的**所有者 Owner 置为 Thread-2**，Monitor中只能有一个 Owner
5. 在 Thread-2 上锁的过程中，如果 Thread-3，Thread-4，Thread-5 也来执行 synchronized(obj)，就会进入**EntryList BLOCKED**(阻塞队列)
6. Thread-2 执行完同步代码块的内容将owner置为null（执行monitorexit的线程不是与objectref引用的实例关联的监视器的所有者），锁对象Markword不变，然后**唤醒** EntryList 中等待的线程来竞争锁，竞争的时是非公平的
   - 

图中 WaitSet 中的 Thread-0，Thread-1 是之前获得过锁，但条件不满足进入 WAITING 状态的线程，后面讲wait-notify 时会分析

*注意：*
*synchronized 必须是进入同一个对象的 monitor 才有上述的效果*
*不加 synchronized 的对象不会关联监视器，不遵从以上规则*

### 注意事项

- **阻塞和唤醒线程需要操作系统来帮忙，并且涉及到用户态和内核态的转换**

- 操作系统的互斥量调用

  以上**属于十分耗时的重量级操作**

## synchronized原理

仅考虑**重量级锁**

### 原理

#### 加锁

**monitorenter** 通过lock对象 MarkWord 找到 Monitor ，查看owner

1. 若为null，将owner 指向 线程对象 自己，当前线程加锁执行
2. 否则进入阻塞队列，等待唤醒

#### 解锁

**monitorexit** 通过lock对象 MarkWord 找到 Monitor ,将owner重置为null， 唤醒 EntryList

### 互斥

其他线程执行synchronized时，通过lock对象的markword，找到Monitor，**直接**进入阻塞队列

### 同步代码块源码

```java
static final Object lock = new Object();
static int counter = 0;
public static void main(String[] args) {
    synchronized (lock) {
        counter++;
    }
}
```

字节码文件

```java
stack=2, locals=3, args_size=1
0: getstatic #2 // <- lock引用 （synchronized开始）
3: dup
4: astore_1 // lock引用 -> slot 1
5: monitorenter // 将 lock对象 MarkWord 置为 Monitor 指针
6: getstatic #3 // <- i
9: iconst_1 // 准备常数 1
10: iadd // +1
11: putstatic #3 // -> i
14: aload_1 // <- lock引用
15: monitorexit // 通过lock对象找到Monitor将 owner 重置, 唤醒 EntryList
16: goto 24
19: astore_2 // e -> slot 2
20: aload_1 // <- lock引用
21: monitorexit // 将 lock对象 owner 重置, 唤醒 EntryList
22: aload_2 // <- slot 2 (e)
23: athrow // throw e
24: return
Exception table:
from to target type
6 16 19 any
19 22 19 any
LineNumberTable:
line 8: 0
line 9: 6
line 10: 14
line 11: 24
LocalVariableTable:
Start Length Slot Name Signature
0 25 0 args [Ljava/lang/String;
StackMapTable: number_of_entries = 2
frame_type = 255 /* full_frame */
offset_delta = 19
locals = [ class "[Ljava/lang/String;", class java/lang/Object ]
stack = [ class java/lang/Throwable ]
frame_type = 250 /* chop */
offset_delta = 4
```

### 修饰的方法源码

```java
public synchronized void method() {
    int i = 0;
}
```

javap反编译后：

```
  public synchronized void method();
    descriptor: ()V
    flags: ACC_PUBLIC, ACC_SYNCHRONIZED
    Code:
      stack=1, locals=2, args_size=1
         0: iconst_0
         1: istore_1
         2: return
      LineNumberTable:
        line 6: 0
        line 7: 2
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       3     0  this   Lcom/lock/SyncTest;
            2       1     1     i   I
```

对于同步方法，JVM会讲方法设置 ACC_SYNCHRONIZED 标志，调用的时候 JVM 根据这个标志判断是否是同步方法。

## 锁

### 轻量级锁

#### 由来

1. 重量级锁涉及到调用操作系统的互斥量Monitor，比较耗时；
2. 在不涉及到竞争的时间条件下，频繁使用操作系统的互斥量进行加锁和解锁操作在开销上是不合理的；

#### 使用场景

- 一个对象虽然有多线程要加锁，但加锁的时间是错开的（**也就是没有竞争**）
- 有竞争但竞争并不频繁

那么可以使用轻量级锁来优化。

轻量级锁对使用者是透明的，即语法仍然是 synchronized

假设有两个方法同步块，利用同一个对象加锁

![image-20211111083927972](笔记图库/image-20211111083927972.png)

#### 原理



<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/051e436c-0e46-4c59-8f67-52d89d656182.png" width="500"/> </div><br>

轻量级锁是相对于传统的重量级锁而言，它使用 CAS 操作来避免重量级锁使用互斥量的开销。对于绝大部分的锁，在整个同步周期内都是不存在竞争的，因此也就不需要都使用互斥量进行同步，可以先采用 CAS 操作进行同步，如果 CAS 失败了再改用互斥量进行同步。

当尝试获取一个锁对象时，如果锁对象标记为 0 01，说明锁对象的锁未锁定（unlocked）状态。

此时虚拟机在当前线程的虚拟机栈中创建 Lock Record，然后使用 CAS 操作将对象的 Mark Word 更新为 Lock Record 指针。

如果 CAS 操作成功了，那么线程就获取了该对象上的锁，并且对象的 Mark Word 的锁标记变为 00，表示该对象处于轻量级锁状态。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/baaa681f-7c52-4198-a5ae-303b9386cf47.png" width="400"/> </div><br>

如果 CAS 操作失败了

​	虚拟机首先会检查对象的 Mark Word 是否指向当前线程的虚拟机栈

- 如果是的话说明当前线程已经拥有了这个锁对象，那就可以直接进入同步块继续执行
- 否则说明这个锁对象已经被其他线程线程抢占了。如果有两条以上的线程争用同一个锁，自旋，自旋失败，那轻量级锁就不再有效，要膨胀为重量级锁。

**详细原理如下**

##### 加锁步骤一

当 Thread-0 执行 synchronized(obj) **先查看锁对象的markword锁状态标志位**

- 如果为未锁定状态；会创建**锁记录（Lock Record）对象**（每个线程的栈帧都会包含一个锁记录的结构）

![image-20211111165359773](笔记图库/image-20211111165359773.png)

- 让锁记录中 **Object reference（owner） 指向锁对象**，并**尝试**（此时可能出现竞争的情况，或者非原子操作导致对象被提前锁定了）通过cas  用锁记录地址 交换 Object 的 Mark Word（Object对象头数据指向Thread的锁记录），将 Mark Word 的值存入锁记录中
  - 如果尝试失败了跳到步骤二
- **尝试成功**，对象头中存储了**锁记录地址 和 锁对象的markword锁状态标志位 00** ，表示由该线程给对象加锁，这时图示如下

![image-20211111165710196](笔记图库/image-20211111165710196.png)

##### 加锁步骤二

如果为锁定状态，cas 失败，有两种情况

通过对象头信息；查看哪个线程拥有这个轻量级锁

- 如果是其它线程已经持有了该 Object 的轻量级锁，这时表明有竞争，

  **自旋**，自旋过程中，成功获得资源(即之前获的资源的线程执行完成并释放了共享资源)，则整个状态依然处于 轻量级锁的状态，如果自旋失败 进入锁膨胀过程

- 如果是自己执行了 synchronized 锁重入，那么再添加一条 Lock Record 作为重入的计数，直接进入执行即可

  ![image-20211111174427941](笔记图库/image-20211111174427941.png)

##### 解锁步骤三

解锁分两种情况

1. 当退出 synchronized 代码块（解锁时）如果有取值为 null 的锁记录，表示有重入，这时重置锁记录，表示重
   入计数减一
2. 当退出 synchronized 代码块（解锁时）锁记录的值不为 null，这时使用 cas 将 Mark Word 的值恢复给对象头
   - 对象锁标志为轻量级，成功，则解锁成功
   - 失败，说明轻量级锁进行了锁膨胀或已经升级为**重量级锁**，进入重量级锁解锁流程

![img](笔记图库/轻量级锁流程图.png)

### 锁膨胀

如果在尝试加轻量级锁的过程中，CAS 操作无法成功，这时一种情况就是有其它线程为此对象加上了轻量级锁（有竞争），自旋失败，这时需要自旋线程阻塞，进行锁膨胀，将轻量级锁变为重量级锁。

```java
static Object obj = new Object();
public static void method1() {
    synchronized( obj ) {
        // 同步块
    }
}
```

- 当 Thread-1 进行轻量级加锁时，Thread-0 已经对该对象加了轻量级锁

![image-20211112092028090](笔记图库/image-20211112092028090.png)

- 这时 Thread-1 加轻量级锁失败，短暂自旋，自旋失败的话，进入锁膨胀流程
- 即为 Object 对象申请 Monitor 锁，让 Object 指向重量级锁地址，然后自己进入 Monitor 的 EntryList BLOCKED

![image-20211112092126448](笔记图库/image-20211112092126448.png)

- 当 Thread-0 退出同步块解锁时，使用 cas 将 Mark Word 的值恢复给对象头，失败。这时会进入重量级解锁流程，即按照 Monitor 地址找到 Monitor 对象，设置 Owner 为 null，唤醒 EntryList 中 BLOCKED 线程

#### 总结

每一个线程在准备获取共享资源时： 第一步，检查MarkWord里面是不是放的自己的ThreadId ,如果是，表示当前线程是处于 “偏向锁” 。

第二步，如果MarkWord不是自己的ThreadId，锁升级，这时候，用CAS来执行切换，新的线程根据MarkWord里面现有的ThreadId，通知之前线程暂停，之前线程将Markword的内容置为空。

第三步，两个线程都把锁对象的HashCode复制到自己新建的用于存储锁的记录空间，接着开始通过CAS操作， 把锁对象的MarKword的内容修改为自己新建的记录空间的地址的方式竞争MarkWord。

第四步，第三步中成功执行CAS的获得资源，失败的则进入自旋 。

**第五步，自旋的线程在自旋过程中，成功获得资源(即之前获的资源的线程执行完成并释放了共享资源)，则整个状态依然处于 轻量级锁的状态，如果自旋失败 。**

第六步，进入重量级锁的状态，这个时候，自旋的线程进行阻塞，等待之前线程执行完成并唤醒自己。

### 自旋优化

#### 背景

多核cpu，执行同步代码块时间短

竞争导致的线程阻塞，开销较大

#### 原理

重量级锁竞争的时候，还可以使用自旋来进行优化，如果当前线程自旋成功（即这时候持锁线程已经退出了同步
块，释放了锁），这时当前线程就可以避免阻塞。

自选成功

![image-20211112092355547](笔记图库/image-20211112092355547.png)

自旋重试失败的情况

![image-20211112092426744](笔记图库/image-20211112092426744.png)

- 自旋会占用 CPU 时间，单核 CPU 自旋就是浪费，多核 CPU 自旋才能发挥优势。
- 在 Java 6 之后自旋锁是自适应的，比如对象刚刚的一次自旋操作成功过，那么认为这次自旋成功的可能性会高，就多自旋几次；反之，就少自旋甚至不自旋，总之，比较智能。
- Java 7 之后不能控制是否开启自旋功能

### 偏向锁

#### 背景

轻量级锁重入，频繁进行同步操作cas，通过对象头的Markword**检查 锁所有者线程 是否是自己**

**开销较大**

```java
static final Object obj = new Object();
public static void m1() {
    synchronized( obj ) {
        // 同步块 A
        m2();
    }
}
public static void m2() {
    synchronized( obj ) {
        // 同步块 B
        m3();
    }
}
public static void m3() {
    synchronized( obj ) {

        // 同步块 C
    }
}
```

![image-20211112093056841](笔记图库/image-20211112093056841.png)

Hotspot的作者经过以往的研究发现大多数情况下**锁不仅不存在多线程竞争，而且总是由同一线程多次获得**，于是引入了偏向锁。

偏向锁会偏向于第一个访问锁的线程，如果在接下来的运行过程中，该锁没有被其他的线程访问，则持有偏向锁的线程将永远不需要触发同步。也就是说，**偏向锁在资源无竞争情况下消除了同步语句，连CAS操作都不做了，提高了程序的运行性能。**

#### 原理

偏向锁-一种特殊的无锁状态

![image-20211112093617380](笔记图库/image-20211112093617380.png)

**偏向锁的思想是偏向于让第一个获取锁对象的线程，这个线程在之后获取该锁就不再需要进行同步操作，甚至连 CAS 操作也不再需要。**

当锁对象第一次被线程获得的时候，进入偏向状态，标记为 1 01。

同时使用 CAS 操作将线程 ID 记录到 Mark Word 中

如果 CAS 操作成功，这个线程以后每次进入这个锁相关的同步块就不需要再进行任何同步操作。

当有另外一个线程去尝试获取这个锁对象时，偏向状态就宣告结束，此时撤销偏向（Revoke Bias）后恢复到未锁定状态或者轻量级锁状态。



**详细原理**：

一个线程在第一次进入同步块时，**会在对象头和栈帧中的锁记录里存储锁的偏向的线程ID。**

当下次该线程进入这个同步块时，会去检查锁的Mark Word里面是不是放的自己的线程ID。

如果是，表明该线程已经获得了锁，以后该线程在进入和退出同步块时不需要花费CAS操作来加锁和解锁 ；

如果不是，就代表有另一个线程来竞争这个偏向锁。这个时候会尝试使用CAS来替换Mark Word里面的线程ID为新线程的ID，这个时候要分两种情况：

- 成功，表示之前的线程不存在了， Mark Word里面的线程ID为新线程的ID，锁不会升级，仍然为偏向锁；
- 失败，表示之前的线程仍然存在，那么暂停之前的线程，设置偏向锁标识为0，并设置锁标志位为00，升级为轻量级锁，会按照轻量级锁的方式进行竞争锁。

> CAS: Compare and Swap
>
> 比较并设置。用于在硬件层面上提供原子性操作。在 Intel 处理器中，比较并交换通过指令cmpxchg实现。 比较是否和给定的数值一致，如果一致则修改，不一致则不修改。

线程竞争偏向锁的过程如下：

![img](笔记图库/偏向锁2.jpg)

图中涉及到了lock record指针指向当前堆栈中的最近一个lock record，是轻量级锁按照先来先服务的模式进行了轻量级锁的加锁。



![image-20211112093730868](笔记图库/image-20211112093730868.png)



**注意：**

一个对象创建时：

如果开启了偏向锁（默认开启），那么对象创建后，markword 值为 0x05 即最后 3 位为 101，这时它的thread、epoch、age 都为 0

偏向锁是默认是延迟的，不会在程序启动时立即生效，如果想避免延迟，可以加 VM 参数 -
XX:BiasedLockingStartupDelay=0 来禁用延迟

如果没有开启偏向锁，那么对象创建后，markword 值为 0x01 即最后 3 位为 001，这时它的 hashcode、age 都为 0，第一次用到 hashcode 时才会赋值





#### 撤销 

##### hashCode

调用了对象的 hashCode，但偏向锁的对象 MarkWord 中存储的是线程 id，如果调用 hashCode 会导致偏向锁被
撤销

- ​	轻量级锁会在**锁记录**中记录 hashCode
- ​	重量级锁会在 **Monitor** 中记录 hashCode

在调用 hashCode 后使用偏向锁，记得去掉 -XX:-UseBiasedLocking
输出

##### 其它线程使用

特点：无竞争时刻有其他非偏向线程使用偏向锁对象

当有其它线程使用偏向锁对象时，会将偏向锁升级为轻量级锁

```JAVA
private static void test2() throws InterruptedException {
    Dog d = new Dog();
    Thread t1 = new Thread(() -> {
        synchronized (d) {
            log.debug(ClassLayout.parseInstance(d).toPrintableSimple(true));
        }
        synchronized (TestBiased.class) {
            TestBiased.class.notify();
        }
        // 如果不用 wait/notify 使用 join 必须打开下面的注释
        // 因为：t1 线程不能结束，否则底层线程可能被 jvm 重用作为 t2 线程，底层线程 id 是一样的
        /*try {
System.in.read();
} catch (IOException e) {
e.printStackTrace();
}*/
    }, "t1");
    t1.start();
    Thread t2 = new Thread(() -> {
        synchronized (TestBiased.class) {
            try {
                TestBiased.class.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        log.debug(ClassLayout.parseInstance(d).toPrintableSimple(true));
        synchronized (d) {
            log.debug(ClassLayout.parseInstance(d).toPrintableSimple(true));
        }
        log.debug(ClassLayout.parseInstance(d).toPrintableSimple(true));
    }, "t2");
    t2.start();
}
```

偏向锁使用了一种**等到竞争出现才释放锁的机制**，所以当其他线程尝试竞争偏向锁时， 持有偏向锁的线程才会释放锁。

偏向锁升级成轻量级锁时，会暂停拥有偏向锁的线程，重置偏向锁标识，这个过程看起来容易，实则开销还是很大的，大概的过程如下：

1. 在一个安全点（在这个时间点上没有字节码正在执行）停止拥有锁的线程。
2. **遍历线程栈，如果存在锁记录的话，需要修复锁记录和Mark Word，使其变成无锁状态。**？？？
3. 唤醒被停止的线程，将当前锁升级成轻量级锁。

所以，如果应用程序里所有的锁通常处于竞争状态，那么偏向锁就会是一种累赘，对于这种情况，我们可以一开始就把偏向锁这个默认功能给关闭：

```java
-XX:UseBiasedLocking=false。
```

下面这个经典的图总结了偏向锁的获得和撤销：

![img](笔记图库/偏向锁.png)

#### 批量重偏向

由于频繁的偏向的撤销（由偏向锁到轻量级锁）开销较大

如果对象虽然被多个线程访问，但没有竞争，这时偏向了线程 T1 的对象仍有机会重新偏向 T2，重偏向会重置对象的 Thread ID

##### 原理

当撤销偏向锁阈值超过 20 次（20个锁对象对一个线程的偏向 被撤销）后，jvm 会这样觉得，我是不是偏向错了呢，于是会在给这些对象加锁时重新偏向至加锁线程

#### 批量撤销

频繁的重偏向，导致较大开销

当撤销偏向锁阈值超过 40 次后，jvm 会这样觉得，自己确实偏向错了，根本就不该偏向。于是整个类的所有对象
都会变为不可偏向的，新建的对象也是不可偏向的

### 锁消除

编译器运行时，优化锁对象无竞争，没有变量逃逸，锁销除

锁消除主要是通过逃逸分析来支持，如果堆上的共享数据不可能逃逸出去被其它线程访问到，那么就可以把它们当成私有数据对待，也就可以将它们的锁进行消除。

对于一些看起来没有加锁的代码，其实隐式的加了很多锁。例如下面的字符串拼接代码就隐式加了锁：

```java
public static String concatString(String s1, String s2, String s3) {
    return s1 + s2 + s3;
}
```

String 是一个不可变的类，编译器会对 String 的拼接自动优化。在 JDK 1.5 之前，会转化为 StringBuffer 对象的连续 append() 操作：

```java
public static String concatString(String s1, String s2, String s3) {
    StringBuffer sb = new StringBuffer();
    sb.append(s1);
    sb.append(s2);
    sb.append(s3);
    return sb.toString();
}
```

每个 append() 方法中都有一个同步块。虚拟机观察变量 sb，很快就会发现它的动态作用域被限制在 concatString() 方法内部。也就是说，sb 的所有引用永远不会逃逸到 concatString() 方法之外，其他线程无法访问到它，因此可以进行消除。

### 对比

#### 加锁

偏向锁：初始锁对象，无竞争，任何时刻均一个线程使用

轻量级锁：无竞争，分时段使用锁对象

重量级锁：有竞争

#### 解锁

重量级锁：锁对象头（Monitor地址；重量级标志）永久**信息不变**

轻量级锁：重置头信息（锁记录标记；轻量级标志）为未锁定状态

偏向锁：锁对象头信息（线程ID；偏向标志）均不变



| 锁       | 优点                                                         | 缺点                                             | 适用场景                             |
| -------- | ------------------------------------------------------------ | ------------------------------------------------ | ------------------------------------ |
| 偏向锁   | 加锁和解锁不需要额外的消耗，和执行非同步方法比仅存在纳秒级的差距。 | 如果线程间存在锁竞争，会带来额外的锁撤销的消耗。 | 适用于只有一个线程访问同步块场景。   |
| 轻量级锁 | 竞争的线程不会阻塞，提高了程序的响应速度。                   | 如果始终得不到锁竞争的线程使用自旋会消耗CPU。    | 追求响应时间。同步块执行速度非常快。 |
| 重量级锁 | 线程竞争不使用自旋，不会消耗CPU。                            | 线程阻塞，响应时间缓慢。                         | 追求吞吐量。同步块执行时间较长。     |

## 活跃性

为了增强并发度，例如哲学家就餐问题 可以设置就餐行为为一把锁，同一时刻只能允许一个就餐；并发度减低；

但也可以对每个筷子上一把锁，这样就可以多个哲学家就餐；

但也容易产生**死锁**，或者饥饿

### 死锁

#### 概念

有这样的情况：一个线程需要同时获取多把锁，这时就容易发生死锁
t1 线程 获得 A对象 锁，接下来想获取 B对象的锁 t2 线程 获得 B对象 锁，接下来想获取 A对象的锁 ：

#### 定位

检测死锁

- 可以使用 **jconsole**工具，或者
- 使用 jps 定位进程 id，再用 jstack 定位死锁

```
cmd > jps

Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
12320 Jps
22816 KotlinCompileDaemon
33200 TestDeadLock // JVM 进程
11508 Main
28468 Launcher

cmd > jstack 33200
```

避免死锁要注意加锁顺序

另外如果由于某个线程进入了死循环，导致其它线程一直等待

**对于这种情况 linux 下可以通过 top 先定位到CPU 占用高的 Java 进程**，再利用 top -Hp 进程id 来定位是哪个线程，最后再用 jstack 排查

### 活锁

活锁出现在两个线程互相改变对方的结束条件，最后谁也无法结束

### 饥饿

很多教程中把饥饿定义为，一个线程由于优先级太低，始终得不到 CPU 调度执行，也不能够结束

## ReentrantLock

```
可重入锁
```

相对于 synchronized 它具备如下特点

- 可中断
- 可以设置超时时间
- 可以设置为公平锁
- 支持多个条件变量

与 synchronized 一样，都支持可重入
基本语法

```java
// 获取锁
reentrantLock.lock();
try {
// 临界区
} finally {
// 释放锁
reentrantLock.unlock();
}
```

### Lock接口

`ReentrantLock`是实现了`Lock`接口

首先，先看下`Lock`接口提供的方法(篇幅所限，这里将源码注释去掉)，大致可分为三类：获取锁、释放锁、新建条件（可用于高级应用，如等待/唤醒)。


```java
public interface Lock {

    /**
     * 获取锁，若获取失败则进行等待
     */
    void lock();

    /**
     * 可中断锁
     */
    void lockInterruptibly() throws InterruptedException;

    /**
     * 获取锁，立即返回，成功返回true，否则false
     */
    boolean tryLock();

    /**
     * 获取锁，若获取失败则在指定时间内等待，成功返回true，否则false
     */
    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;

    /**
     * 释放锁
     */
    void unlock();

    /**
     * 新建条件，可用与高级应用
     */
    Condition newCondition();
}
```




### 可重入

可重入是指同一个线程如果首次获得了这把锁，那么因为它是这把锁的拥有者，因此有权利再次获取这把锁
如果是**不可重入锁**，那么第二次获得锁时，自己也会被锁挡住

例如

```java
static ReentrantLock lock = new ReentrantLock();
public static void main(String[] args) {
    method1();
}
public static void method1() {
    lock.lock();
    try {
        log.debug("execute method1");
        method2();
    } finally {
        lock.unlock();
    }
}
public static void method2() {
    lock.lock();
    try {
        log.debug("execute method2");
        method3();
    } finally {
        lock.unlock();
    }
}
public static void method3() {
    lock.lock();
    try {
        log.debug("execute method3");
    } finally {
        lock.unlock();
    }
}

```

输出

```
17:59:11.862 [main] c.TestReentrant - execute method1
17:59:11.865 [main] c.TestReentrant - execute method2
17:59:11.865 [main] c.TestReentrant - execute method3
```

### 可打断

示例

```java
ReentrantLock lock = new ReentrantLock();
Thread t1 = new Thread(() -> {
    log.debug("启动...");
    try {
        lock.lockInterruptibly();
    } catch (InterruptedException e) {
        e.printStackTrace();
        log.debug("等锁的过程中被打断");
        return;
    }
    try {
        log.debug("获得了锁");
    } finally {
        lock.unlock();
    }
}, "t1");

lock.lock();
log.debug("获得了锁");
t1.start();
try {
    sleep(1);
    t1.interrupt();
    log.debug("执行打断");
} finally {
    lock.unlock();
}
```

输出

```
18:02:40.520 [main] c.TestInterrupt - 获得了锁
18:02:40.524 [t1] c.TestInterrupt - 启动...
18:02:41.530 [main] c.TestInterrupt - 执行打断
java.lang.InterruptedException
at
java.util.concurrent.locks.AbstractQueuedSynchronizer.doAcquireInterruptibly(AbstractQueuedSynchr
onizer.java:898)
at
java.util.concurrent.locks.AbstractQueuedSynchronizer.acquireInterruptibly(AbstractQueuedSynchron
izer.java:1222)
at java.util.concurrent.locks.ReentrantLock.lockInterruptibly(ReentrantLock.java:335)
at cn.itcast.n4.reentrant.TestInterrupt.lambda$main$0(TestInterrupt.java:17)
at java.lang.Thread.run(Thread.java:748)
18:02:41.532 [t1] c.TestInterrupt - 等锁的过程中被打断
```

注意如果是不可中断模式/没有使用·lock.lockInterruptibly();，那么即使使用了 interrupt 也不会让等待中断

### 锁超时/尝试

非阻塞机制

#### 立刻失败,

```java
ReentrantLock lock = new ReentrantLock();
Thread t1 = new Thread(() -> {
    log.debug("启动...");
    if (!lock.tryLock()) {
        log.debug("获取立刻失败，返回");
        return;
    }
    try {
        log.debug("获得了锁");
    } finally {
        lock.unlock();
    }
}, "t1");
lock.lock();
log.debug("获得了锁");

t1.start();
try {
    sleep(2);
} finally {
    lock.unlock();
}
```

输出

```
18:15:02.918 [main] c.TestTimeout - 获得了锁
18:15:02.921 [t1] c.TestTimeout - 启动...
18:15:02.921 [t1] c.TestTimeout - 获取立刻失败，返回
```

#### 超时失败

```java
ReentrantLock lock = new ReentrantLock();
Thread t1 = new Thread(() -> {
    log.debug("启动...");
    try {
        if (!lock.tryLock(1, TimeUnit.SECONDS)) {
            log.debug("获取等待 1s 后失败，返回");
            return;
        }
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    try {
        log.debug("获得了锁");
    } finally {
        lock.unlock();
    }
}, "t1");
lock.lock();
log.debug("获得了锁");
t1.start();
try {
    sleep(2);
} finally {
    lock.unlock();
}
```

输出

```
18:19:40.537 [main] c.TestTimeout - 获得了锁
18:19:40.544 [t1] c.TestTimeout - 启动...
18:19:41.547 [t1] c.TestTimeout - 获取等待 1s 后失败，返回
```

### 公平锁

ReentrantLock 默认是不公平的

```java
ReentrantLock lock = new ReentrantLock(false);
lock.lock();
for (int i = 0; i < 500; i++) {
    new Thread(() -> {
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + " running...");
        } finally {
            lock.unlock();
        }
    }, "t" + i).start();
}
// 1s 之后去争抢锁
Thread.sleep(1000);

new Thread(() -> {
    System.out.println(Thread.currentThread().getName() + " start...");
    lock.lock();
    try {
        System.out.println(Thread.currentThread().getName() + " running...");
    } finally {
        lock.unlock();
    }
}, "强行插入").start();

lock.unlock();
```

改为公平锁

```java
ReentrantLock lock = new ReentrantLock(true);
```

强行插入，总是在最后输出

公平锁一般没有必要，会降低并发度，后面分析原理时会讲解

### 条件变量

synchronized 中也有条件变量，就是我们讲原理时那个 waitSet 休息室，当条件不满足时进入 waitSet 等待
ReentrantLock 的条件变量比 synchronized 强大之处在于，它是支持多个条件变量的，这就好比

- synchronized 是那些不满足条件的线程都在一间休息室等消息
- 而 ReentrantLock 支持多间休息室，有专门等烟的休息室、专门等早餐的休息室、唤醒时也是按休息室来唤醒

使用要点：

- await 前需要获得锁
- await 执行后，会释放锁，进入 conditionObject 等待
- await 的线程被唤醒（或打断、或超时）取重新竞争 lock 锁
- 竞争 lock 锁成功后，从 await 后继续执行

```java
static ReentrantLock lock = new ReentrantLock();
static Condition waitCigaretteQueue = lock.newCondition();
waitCigaretteQueue.await();
waitCigaretteQueue.signal();
```

例子：

```java
static ReentrantLock lock = new ReentrantLock();
static Condition waitCigaretteQueue = lock.newCondition();
static Condition waitbreakfastQueue = lock.newCondition();
static volatile boolean hasCigrette = false;
static volatile boolean hasBreakfast = false;
public static void main(String[] args) {
    new Thread(() -> {
        try {
            lock.lock();
            while (!hasCigrette) {
                try {
                    waitCigaretteQueue.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            log.debug("等到了它的烟");
        } finally {
            lock.unlock();
        }
    }).start();
    new Thread(() -> {
        try {
            lock.lock();
            while (!hasBreakfast) {
                try {
                    waitbreakfastQueue.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            log.debug("等到了它的早餐");
        } finally {
            lock.unlock();
        }
    }).start();
    sleep(1);
    sendBreakfast();
    sleep(1);
    sendCigarette();
}
private static void sendCigarette() {
    lock.lock();
    try {
        log.debug("送烟来了");
        hasCigrette = true;
        waitCigaretteQueue.signal();
    } finally {
        lock.unlock();
    }
}
private static void sendBreakfast() {
    lock.lock();
    try {
        log.debug("送早餐来了");
        hasBreakfast = true;
        waitbreakfastQueue.signal();
    } finally {
        lock.unlock();
    }
}
```

输出

```
18:52:27.680 [main] c.TestCondition - 送早餐来了
18:52:27.682 [Thread-1] c.TestCondition - 等到了它的早餐
18:52:28.683 [main] c.TestCondition - 送烟来了
18:52:28.683 [Thread-0] c.TestCondition - 等到了它的烟
```



## 本章小结

本章我们需要重点掌握的是

**分析多线程访问共享资源时，哪些代码片段属于临界区**

- 共享变量存在读写

**使用 synchronized 互斥解决临界区的线程安全问题**

- ​	掌握 synchronized 锁对象语法
- ​	掌握 synchronzied 加载成员方法和静态方法语法
- ​	掌握 wait/notify 同步方法

**使用 lock 互斥解决临界区的线程安全问题**

- 掌握 lock 的使用细节：可打断、锁超时、公平锁、条件变量

**学会分析变量的线程安全性、掌握常见线程安全类的使用**

**了解线程活跃性问题：死锁、活锁、饥饿**
应用方面

- **互斥：使用 synchronized 或 Lock 达到共享资源互斥效果**
- **同步：使用 wait/notify 或 Lock 的条件变量来达到线程间通信效果**

**原理方面**

- monitor、synchronized 、wait/notify 原理
- synchronized 进阶原理
- park & unpark 原理

**模式方面**

- 同步模式之保护性暂停
- 异步模式之生产者消费者
- 同步模式之顺序控制



# 内存模型

Java 内存模型试图屏蔽各种硬件和操作系统的内存访问差异，以实现让 Java 程序在各种平台下都能达到一致的内存访问效果。

## 共享内存并发模型

### 通信

**线程之间共享公共的状态**，通过**读写内存中的公共状态实现通信** 如 notify；

### 同步

必须显示的指定某些代码块在线程之间互斥指向，实现同步

## JMM

### 运行时内存的划分

![Java运行时数据区域](笔记图库/Java运行时数据区.png)

对于每一个线程来说，栈都是私有的，而堆是共有的。

也就是说在栈中的变量（局部变量、方法定义参数、异常处理器参数）不会在线程之间共享，也就不会有内存可见性（下文会说到）的问题，也不受内存模型的影响。

**而在堆中的变量是共享的，本文称为共享变量。**

所以，内存可见性是针对的**共享变量**。



### 主内存和工作内存

操作系统中处理器上的寄存器的读写的速度比内存快几个数量级，为了解决这种速度矛盾，在它们之间加入了高速缓存。

**加入高速缓存带来了一个新的问题：缓存一致性。如果多个缓存共享同一块主内存区域，那么多个缓存的数据可能会不一致，需要一些协议来解决这个问题。**

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/942ca0d2-9d5c-45a4-89cb-5fd89b61913f.png" width="600px"> </div><br>

所有的变量都存储在主内存中，每个线程还有自己的工作内存，工作内存存储在高速缓存或者寄存器中，保存了该线程使用的变量的主内存副本拷贝。

线程只能直接操作工作内存中的变量，不同线程之间的变量值传递需要通过主内存来完成。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/15851555-5abc-497d-ad34-efed10f43a6b.png" width="600px"> </div><br>

### JMM定义

JMM 即 Java Memory Model，它定义了主存、工作内存抽象概念，底层对应着 CPU 寄存器、缓存、硬件内存、
CPU 指令优化等。

**JMM 体现在以下几个方面**

原子性 - 保证指令不会受到线程上下文切换的影响
可见性 - 保证指令不会受 cpu 缓存的影响
有序性 - 保证指令不会受 cpu 指令并行优化的影响

Java线程之间的通信由**Java内存模型（简称JMM）**控制，从抽象的角度来说，JMM定义了线程和主内存之间的抽象关系。JMM的抽象示意图如图所示：

![JMM抽象示意图](笔记图库/JMM抽象示意图.jpg)

从图中可以看出：

1. 所有的共享变量都存在主内存中。
2. 每个线程都保存了一份该线程使用到的共享变量的副本。
3. 如果线程A与线程B之间要通信的话，必须经历下面2个步骤：
   1. 线程A将本地内存A中更新过的共享变量刷新到主内存中去。
   2. 线程B到主内存中去读取线程A之前已经更新过的共享变量

**所以，线程A无法直接访问线程B的工作内存，线程间通信必须经过主内存。**

注意，根据JMM的规定，**线程对共享变量的所有操作都必须在自己的本地内存中进行，不能直接从主内存中读取**。

### 内存交互

java 内存模型定义了 8 个操作来完成主内存和工作内存的交互操作。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/8b7ebbad-9604-4375-84e3-f412099d170c.png" width="450px"> </div><br>

- **read**：把一个变量的值从主内存传输到工作内存中
- **load**：在 read 之后执行，把 read 得到的值放入工作内存的变量副本中
- use：把工作内存中一个变量的值传递给执行引擎
- assign：把一个从执行引擎接收到的值赋给工作内存的变量
- **store**：把工作内存的一个变量的值传送到主内存中
- **write**：在 store 之后执行，把 store 得到的值放入主内存的变量中
- lock：作用于主内存的变量
- unlock

所以线程B并不是直接去主内存中读取共享变量的值，而是

1. 先在本地内存B中找到这个共享变量
2. 发现这个共享变量已经被更新了
3. 然后本地内存B去主内存中读取这个共享变量的新值
4. 并拷贝到本地内存B中
5. 最后线程B再读取本地内存B中的新值。

那么怎么知道这个共享变量的被其他线程更新了呢？这就是JMM的功劳了，也是JMM存在的必要性之一。**JMM通过控制主内存与每个线程的本地内存之间的交互，来提供内存可见性保证**。

> Java中的volatile关键字可以保证多线程操作共享变量的可见性以及禁止指令重排序，
>
> synchronized关键字不仅保证可见性，同时也保证了原子性（互斥性）。
>
> 在更底层，JMM通过内存屏障来实现内存的可见性以及禁止重排序。为了程序员的方便理解，提出了happens-before，它更加的简单易懂，从而避免了程序员为了理解内存可见性而去学习复杂的重排序规则以及这些规则的具体实现方法。
>
> 这里涉及到的所有内容后面都会有专门的章节介绍。

### JMM与运行时内存

上面两小节分别提到了JMM和Java运行时内存区域的划分，这两者既有差别又有联系：

- 区别

  两者是不同的概念层次。JMM是抽象的，他是用来描述一组**规则**，通过这个规则来控制各个变量的访问方式，围绕**原子性、有序性、可见性**等展开的。

  而Java运行时内存的划分是具体的，是JVM运行Java程序时，必要的内存划分。

- 联系

  **都存在私有数据区域和共享数据区域**。一般来说，

  JMM中的主内存属于共享数据区域，他是包含了堆和方法区；

  同样，JMM中的本地内存属于私有数据区域，包含了程序计数器、本地方法栈、虚拟机栈

## 内存模型三大特性

###  原子性

#### 概念

原子性是指不中断和干扰的一系列操作；

#### 原子操作

Java 内存模型要求变量的读取操作和写入操作是原子的

Java 内存模型保证了 read、load、use、assign、store、write、lock 和 unlock 操作具有原子性，例如对一个 int 类型的变量**执行 assign 赋值操作，这个操作就是原子性的**。

##### 注意

原子性仅仅保证了对操作的不可中断性不可干扰性

并没有解决**变量的不同原子操作的指令交错问题**；

如当线程没有同步的情况下，可能读取到失效值，因为可能读写操作在不同线程同时进行；

#### 非原子的64位操作

**Java 内存模型允许虚拟机将 没有被 volatile 修饰 的 64 位数据（long，double）的读或写操作划分为两次 32 位的操作来进行，即 load、store、read 和 write 操作可以不具备原子性。**

多线程共享使用 long，double 线程不安全；很可能读取到某一个值得高32位 和 另一个值的低32位

可以通过volatile来声明它们；或者用锁保护起来；

#### 举例

*原子性是指一个操作是不可中断的. 即使是在多个线程一起执行的时候，*

***一个操作一旦开始，就不会被其它线程干扰.** 例如CPU中的一些指令, 属于原子性的,*

*又或者变量直接赋值操作(i = 1), 也是原子性的, 即使有多个线程对i赋值, 相互也不会干扰.*

*而如i++, 则不是原子性的, 因为他实际上i = i + 1, 若存在多个线程操作i, 结果将不可预期.*

![img](笔记图库/1285727-20180506210812001-1855659578.png)

为了方便讨论，将内存间的交互操作简化为 3 个：load、assign、store。



<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/8b7ebbad-9604-4375-84e3-f412099d170c.png" width="450px"> </div><br>



下图演示了两个线程同时对 cnt 进行操作，load、assign、store 这一系列操作整体上看不具备原子性，那么在 T1 修改 cnt 并且还没有将修改后的值写入主内存，**T2 依然可以读入旧值。**

可以看出，这两个线程虽然执行了两次自增运算，但是主内存中 cnt 的值最后为 1 而不是 2。**因此对 int 类型读写操作满足原子性只是说明 load、assign、store 这些单个操作具备原子性**。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/2797a609-68db-4d7b-8701-41ac9a34b14f.jpg" width="300px"> </div><br>

AtomicInteger 能保证多个线程修改的原子性。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/dd563037-fcaa-4bd8-83b6-b39d93a12c77.jpg" width="300px"> </div><br>

#### 原子类和锁对比

使用 AtomicInteger 重写之前线程不安全的代码之后得到以下线程安全实现：

```java
public class AtomicExample {
    private AtomicInteger cnt = new AtomicInteger();

    public void add() {
        cnt.incrementAndGet();
    }

    public int get() {
        return cnt.get();
    }
}
```

```java
public static void main(String[] args) throws InterruptedException {
    final int threadSize = 1000;
    AtomicExample example = new AtomicExample(); // 只修改这条语句
    final CountDownLatch countDownLatch = new CountDownLatch(threadSize);
    ExecutorService executorService = Executors.newCachedThreadPool();
    for (int i = 0; i < threadSize; i++) {
        executorService.execute(() -> {
            example.add();
            countDownLatch.countDown();
        });
    }
    countDownLatch.await();
    executorService.shutdown();
    System.out.println(example.get());
}
```

```html
1000
```

除了使用原子类之外，也可以使用 synchronized 互斥锁来保证操作的原子性。它对应的内存间交互操作为：lock 和 unlock，在虚拟机实现上对应的字节码指令为 monitorenter 和 monitorexit。

```java
public class AtomicSynchronizedExample {
    private int cnt = 0;

    public synchronized void add() {
        cnt++;
    }

    public synchronized int get() {
        return cnt;
    }
}
```

```java
public static void main(String[] args) throws InterruptedException {
    final int threadSize = 1000;
    AtomicSynchronizedExample example = new AtomicSynchronizedExample();
    final CountDownLatch countDownLatch = new CountDownLatch(threadSize);
    ExecutorService executorService = Executors.newCachedThreadPool();
    for (int i = 0; i < threadSize; i++) {
        executorService.execute(() -> {
            example.add();
            countDownLatch.countDown();
        });
    }
    countDownLatch.await();
    executorService.shutdown();
    System.out.println(example.get());
}
```

```html
1000
```

### 可见性

可见性指当一个线程修改了共享变量的值，其它线程能够立即得知这个修改。Java 内存模型是通过在变量修改后将新值同步回主内存，在变量读取前从主内存刷新变量值来实现可见性的。

主要有三种实现可见性的方式：

- volatile
- synchronized，对一个变量执行 unlock 操作之前，必须把变量值同步回主内存。
- final，被 final 关键字修饰的字段在构造器中一旦初始化完成，并且没有发生 this 逃逸（其它线程通过 this 引用访问到初始化了一半的对象），那么其它线程就能看见 final 字段的值。

对前面的线程不安全示例中的 cnt 变量使用 volatile 修饰，不能解决线程不安全问题，因为 volatile 并不能保证操作的原子性。

### 有序性

有序性是指：在本线程内观察，所有操作都是有序的。在一个线程观察另一个线程，所有操作都是无序的，无序是因为发生了指令重排序。在 Java 内存模型中，允许编译器和处理器对指令进行重排序，重排序过程不会影响到单线程程序的执行，却会影响到多线程并发执行的正确性。

#### CPU组成

##### 冯诺依曼机

![image-20211118205925258](笔记图库/image-20211118205925258.png)

##### CPU组成

1.**寄存器**;

2.**控制器**CU（Control Unit）:分析指令、控制信号

​	指令译码器ID 分析指令

​	指令寄存器IR(InstructionRegister) 储指令

​	程序计数器PC(ProgramCounter) 取指令

​	操作控制器OC(OperationController);

3.**ALU**（算数逻辑运算单元），不包括寄存器;

![img](笔记图库/SouthEast.jpeg)

##### 计算机工作流程

![image-20211118211050715](笔记图库/image-20211118211050715.png)

#### 指令重排

计算机在执行程序时，为了提高性能，编译器和处理器常常会对指令做重排。

##### 流水线技术

简单地说

每一个指令都会包含多个步骤

- 取指令
- 指令译码
- 执行指令 
- 内存访问
- 数据写回

根据CPU组成可知 每个步骤使用的硬件是不同的。

因为每次只执行一条指令, 依次执行效率太低了, 假设上述每一个步骤都要消耗一个时钟周期, 那么依次执行的话, 一条指令要5个时钟周期, 两条指令要占用10个时钟周期, 三条指令消耗15个时钟.

因此，**流水线技术**产生了，它的原理是在一个时钟周期执行不同的指令子步骤

指令1还没有执行完，就可以开始执行指令2，而不用等到指令1执行结束之后再执行指令2，这样就大大提高了效率。

![image-20211118211559809](笔记图库/image-20211118211559809.png)



##### 流水线中断问题

但是，流水线技术最害怕**中断**，恢复中断的代价是比较大的，所以我们要想尽办法不让流水线中断。指令重排就是减少中断的一种技术。

我们分析一下下面这个代码的执行情况：

```java
a = b + c;
d = e - f ;
```

先加载b、c（**注意，即有可能先加载b，也有可能先加载c**），但是在**执行add(b,c)的时候，需要等待b、c装载结束才能继续执行，也就是增加了停顿**，那么后面的指令也会依次有停顿,这降低了计算机的执行效率。

![img](笔记图库/1285727-20180503000816520-1448656185.jpg)

##### 指令重排

为了减少这个停顿，我们可以先加载e和f,然后再去加载add(b,c),这样做对程序（串行）是没有影响的,但却减少了停顿。既然add(b,c)需要停顿，那还不如去做一些有意义的事情。

综上所述，**指令重排对于提高CPU处理性能十分必要。虽然由此带来了乱序的问题，但是这点牺牲是值得的。**

指令重排一般分为以下三种：

- **编译器优化重排**

  编译器在**不改变单线程程序语义**的前提下，可以重新安排语句的执行顺序。

- **指令并行重排**

  现代处理器采用了指令级并行技术来将多条指令重叠执行。如果**不存在数据依赖性**(即后一个执行的语句无需依赖前面执行的语句的结果)，处理器可以改变语句对应的机器指令的执行顺序。

- **内存系统重排**

  由于处理器使用缓存和读写缓存冲区，这使得加载(load)和存储(store)操作看上去可能是在乱序执行，因为三级缓存的存在，导致内存与缓存的数据同步存在时间差。

**指令重排可以保证串行语义一致，但是没有义务保证多线程间的语义也一致**。所以在多线程下，指令重排序可能会导致一些问题。

##### **问题产生**

```java
int num = 0;
boolean ready = false;
// 线程1 执行此方法
public void actor1(I_Result r) {
if(ready) {
r.r1 = num + num;
} else {
r.r1 = 1;
}
}
// 线程2 执行此方法
public void actor2(I_Result r) {
num = 2;
ready = true;
}
```

I_Result 是一个对象，有一个属性 r1 用来保存结果，问，可能的结果有几种？

情况1：线程1 先执行，这时 ready = false，所以进入 else 分支结果为 1
情况2：线程2 先执行 num = 2，但没来得及执行 ready = true，线程1 执行，还是进入 else 分支，结果为1
情况3：线程2 执行到 ready = true，线程1 执行，这回进入 if 分支，结果为 4（因为 num 已经执行过了）

但结果还有可能是 0 ，由于指令重排
这种情况下是：**线程2 执行 ready = true，切换到线程1，进入 if 分支，相加为 0，再切回线程2 执行 num = 2**

## volatile

在Java中，volatile关键字有特殊的内存语义。volatile主要有以下两个功能：

- 保证变量的**内存可见性**

- 禁止volatile变量与普通变量**重排序**（JSR133提出，Java 5 开始才有这个“增强的volatile内存语义”）

  volatile 关键字通过添加内存屏障的方式来禁止指令重排，即重排序时不能把后面的指令放到内存屏障之前。（也可以通过 synchronized 来保证有序性，它保证每个时刻只有一个线程执行同步代码，相当于是让线程顺序执行同步代码。）

###  实现可见性

#### 概述

所谓内存可见性，指的是

当一个线程对`volatile`修饰的变量进行**写操作**时，JMM会立即把该线程对应的本地内存中的**共享变量的值**刷新到主内存；

当一个线程对`volatile`修饰的变量进行**读操作**时，JMM会把立即该线程对应的本地内存**共享变量的值**置为无效，从主内存中读取共享变量的值更新到本地内存 在使用。

#### 举例

以一段示例代码开始：

```java
public class VolatileExample {
    int a = 0;
    volatile boolean flag = false;

    public void writer() {
        a = 1; // step 1
        flag = true; // step 2
    }

    public void reader() {
        if (flag) { // step 3
            System.out.println(a); // step 4
        }
    }
}
```

在这段代码里，我们使用`volatile`关键字修饰了一个`boolean`类型的变量`flag`。

所谓内存可见性，指的是当一个线程对`volatile`修饰的变量进行**写操作**（比如step 2）时，JMM会立即把该线程对应的本地内存中的共享变量的值刷新到主内存；当一个线程对`volatile`修饰的变量进行**读操作**（比如step 3）时，JMM会把立即该线程对应的本地内存置为无效，从主内存中读取共享变量的值。

> 在这一点上，volatile与锁具有相同的内存效果，volatile变量的写和锁的释放具有相同的内存语义，volatile变量的读和锁的获取具有相同的内存语义。

假设在时间线上，线程A先执行方法`writer`方法，线程B后执行`reader`方法。那必然会有下图：

![volatile内存示意图](笔记图库/volatile内存示意图.jpg)

而如果`flag`变量**没有**用`volatile`修饰，在step 2，线程A的本地内存里面的变量就不会立即更新到主内存，那随后线程B也同样不会去主内存拿最新的值，仍然使用线程B本地内存缓存的变量的值`a = 0，flag = false`。

#### 底层原理

volatile 的底层实现原理是内存屏障，Memory Barrier（Memory Fence）
对 volatile 变量的写指令后会加入写屏障
对 volatile 变量的读指令前会加入读屏障


写屏障（sfence）保证在该屏障之前的，**对共享变量的改动** （普通num也算），都同步到主存当中

```java
public void actor2(I_Result r) {
num = 2;
ready = true; // ready 是 volatile 赋值带写屏障
// 写屏障
}
```

而读屏障（lfence）保证在该屏障之后，对**共享变量**（num也算）也算的读取，加载的是主存中最新数据

```java
public void actor1(I_Result r) {
// 读屏障
// ready 是 volatile 读取值带读屏障
if(ready) {
r.r1 = num + num;
} else {
r.r1 = 1;
}
}
```

#### JMM对volatile的特殊规则

![img](笔记图库/v2-41656b9fcbcc6cff02900b31462c81d3_720w.jpg)

1. 某一线程对某一变量的assign与 store+write 必须关联在一起，且必须一起连续出现

这条规则使得：主存中的变量为最新值（每次改变的变量都立即写入主存），**这步 和普通变量差不多**

2. 某一线程对某一变量的use与 load+read 必须关联在一起，且必须一起连续出现

这条规则使得：变量为最新值（每次使用都取主存中获取新值），**不管你之前是否加载过，我现在就要最新的**



**尽管如此还是存在一些问题：**load操作的时效性的时效性保证了，每次使用都可以取到主存中获取新值，但也有可能这边T1线程刚载入到最新值，T2 有向主存中写入了新的值，此时T1 线程的值就不是最新的了，为了进一步优化；

happens-before：，如果一个操作A happens-before 另一个操作B，那么操作A的结果是对操作B可见的。**程序员对同一个volatile变量的写操作 先行发生于后面对这个变量的读操作。问题就解决啦**

### 实现有序性

#### 问题来源

```
public class VolatileExample {
    int a = 0;
    volatile boolean flag = false;

    public void writer() {
        a = 1; // step 1
        flag = true; // step 2
    }

    public void reader() {
        if (flag) { // step 3
            System.out.println(a); // step 4
        }
    }
}
```

在JSR-133之前的旧的Java内存模型中，是允许volatile变量与普通变量重排序的。那上面的案例中，可能就会被重排序成下列时序来执行：

1. 线程A写volatile变量，step 2，设置flag为true；
2. 线程B读同一个volatile，step 3，读取到flag为true；
3. 线程B读普通变量，step 4，读取到 a = 0；
4. 线程A修改普通变量，step 1，设置 a = 1；

可见，如果volatile变量与普通变量发生了重排序，虽然volatile变量能保证内存可见性，也可能导致普通变量读取错误。

#### 禁止重排序

所以在旧的内存模型中，volatile的写-读就不能与锁的释放-获取具有相同的内存语义了。为了提供一种比锁更轻量级的**线程间的通信机制**，**JSR-133**专家组决定增强volatile的内存语义：严格限制编译器和处理器对volatile变量与普通变量的重排序。

#### 底层原理

编译器还好说，JVM是怎么还能限制处理器的重排序的呢？它是通过**内存屏障**来实现的。

什么是内存屏障？硬件层面，内存屏障分两种：

读屏障（Load Barrier）和写屏障（Store Barrier）。内存屏障有两个作用：

1. 阻止屏障两侧的指令重排序；
2. （可见性）强制把写缓冲区/高速缓存中的脏数据等写回主内存，或者让缓存中相应的数据失效。

编译器在**生成字节码时**，会在指令序列中插入内存屏障来禁止特定类型的处理器重排序。编译器选择了一个**比较保守的JMM内存屏障插入策略**，这样可以保证在任何处理器平台，任何程序中都能得到正确的volatile内存语义。这个策略是：

写屏障会确保指令重排序时，不会将写屏障之前的代码排在写屏障之后

```java
public void actor2(I_Result r) {
num = 2;
ready = true; // ready 是 volatile 赋值带写屏障
// 写屏障
}
```

读屏障会确保指令重排序时，不会将读屏障之后的代码排在读屏障之前

```java
public void actor1(I_Result r) {
// 读屏障
// ready 是 volatile 读取值带读屏障
if(ready) {
r.r1 = num + num;
} else {
r.r1 = 1;
}
}
```

但如果是下列情况：第一个操作是普通变量读，第二个操作是volatile变量读，那是可以重排序的：读屏障之前的代码随意排

```java
// 声明变量
int a = 0; // 声明普通变量
volatile boolean flag = false; // 声明volatile变量

// 以下两个变量的读操作是可以重排序的
int i = a; // 普通变量读
boolean j = flag; // volatile变量读
```

### 优点

从volatile的内存语义上来看，volatile可以保证内存可见性且禁止重排序。

在保证内存可见性这一点上，volatile有着与锁相同的内存语义，所以可以作为一个“轻量级”的锁来使用。但由于volatile仅仅保证对单个volatile变量的读/写具有原子性，而锁可以保证整个**临界区代码**的执行具有原子性。所以**在功能上，锁比volatile更强大；在性能上，volatile更有优势**。**但解决不了线程安全问题**

在禁止重排序这一点上，volatile也是非常有用的。比如我们**熟悉的单例模式**，其中有一种实现方式是“双重锁检查”，比如这样的代码：

```java
public class Singleton {

    private static Singleton instance; // 不使用volatile关键字

    // 双重锁检验
    public static Singleton getInstance() {
        if (instance == null) { // 第7行
            // 首次访问会同步，而之后的使用没有 synchronized
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton(); // 第10行
                }
            }
        }
        return instance;
    }
}
```

以上的实现特点是：

- 懒惰实例化
- 首次使用 getInstance() 才使用 synchronized 加锁，后续使用时无需加锁
- 有隐含的，但很关键的一点：第一个 if 使用了 INSTANCE 变量，是在同步块之外

但在多线程环境下，上面的代码是有问题的，getInstance 方法对应的字节码为：

```
0: getstatic #2 // Field INSTANCE:Lcn/itcast/n5/Singleton;
3: ifnonnull 37
6: ldc #3 // class cn/itcast/n5/Singleton
8: dup
9: astore_0
10: monitorenter
11: getstatic #2 // Field INSTANCE:Lcn/itcast/n5/Singleton;
14: ifnonnull 27

17: new #3 // class cn/itcast/n5/Singleton
20: dup
21: invokespecial #4 // Method "<init>":()V
24: putstatic #2 // Field INSTANCE:Lcn/itcast/n5/Singleton;

27: aload_0
28: monitorexit
29: goto 37
32: astore_1
33: aload_0
34: monitorexit
35: aload_1
36: athrow
37: getstatic #2 // Field INSTANCE:Lcn/itcast/n5/Singleton;
40: areturn
```

- 17 表示创建对象，将对象**引用入栈** ，产生一份对象引用 // new Singleton
- 20 表示复制一份对象引用 // 引用地址
- 21 表示利用一个对象引用，调用构造方法
- 24 表示利用一个对象引用，赋值给 static INSTANCE

如果这里的变量声明不使用volatile关键字，是可能会发生错误的。它可能会被重排序：

即 先将引用地址 赋值给instance 24，再将执行构造方法21；

先执行 24，再执行 21。如果两个线程 t1，t2 按如下时间序列执行：

1. 关键在于 0: getstatic 这行代码在 monitor 控制之外，它就像之前举例中不守规则的人，可以越过 monitor 读取INSTANCE 变量的值
2. 这时 t1 还未完全将构造方法执行完毕，如果在构造方法中要执行很多初始化操作，那么 t2 拿到的是将是一个未初始化完毕的单例
3. 对 INSTANCE 使用 volatile 修饰即可，可以禁用指令重排，但要注意在 JDK 5 以上的版本的 volatile 才会真正有效



### 小结

**可见性**

写屏障（sfence）保证在该屏障之前的 t1 对共享变量的改动，都同步到主存当中

读屏障（lfence）保证在该屏障之后 t2 对共享变量的读取，加载的是主存中最新数据

**有序性**

写屏障会确保指令重排序时，不会将写屏障之前的代码排在写屏障之后

读屏障会确保指令重排序时，不会将读屏障之后的代码排在读屏障之前

更底层是读写变量时使用 lock 指令来多核 CPU 之间的可见性与有序性

## 先行发生原则

上面提到了可以用 volatile 和 synchronized 来保证有序性。除此之外，JVM 还规定了先行发生原则，让一个操作无需控制就能先于另一个操作完成。

一方面，程序员需要JMM提供一个强的内存模型来编写代码；另一方面，编译器和处理器希望JMM对它们的束缚越少越好，这样它们就可以最可能多的做优化来提高性能，希望的是一个弱的内存模型。

JMM考虑了这两种需求，并且找到了平衡点，对编译器和处理器来说，**只要不改变程序的执行结果（单线程程序和正确同步了的多线程程序），编译器和处理器怎么优化都行。**

而对于程序员，JMM提供了**happens-before规则**（JSR-133规范），满足了程序员的需求——**简单易懂，并且提供了足够强的内存可见性保证。**换言之，**程序员只要遵循happens-before规则**，那他写的程序就能保证在JMM中具有强的内存可见性。



### 1. 单一线程原则

> Single Thread rule

在一个线程内，在程序前面的操作先行发生于后面的操作。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/874b3ff7-7c5c-4e7a-b8ab-a82a3e038d20.png" width="180px"> </div><br>

### 2. 管程锁定规则

> Monitor Lock Rule

一个 unlock 操作先行发生于后面对同一个锁的 lock 操作。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/8996a537-7c4a-4ec8-a3b7-7ef1798eae26.png" width="350px"> </div><br>

### 3. volatile 变量规则

> Volatile Variable Rule

对一个 volatile 变量的写操作先行发生于后面对这个变量的读操作。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/942f33c9-8ad9-4987-836f-007de4c21de0.png" width="400px"> </div><br>

### 4. 线程启动规则

> Thread Start Rule

Thread 对象的 start() 方法调用先行发生于此线程的每一个动作。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/6270c216-7ec0-4db7-94de-0003bce37cd2.png" width="380px"> </div><br>

### 5. 线程加入规则

> Thread Join Rule

Thread 对象的结束先行发生于 join() 方法返回。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/233f8d89-31d7-413f-9c02-042f19c46ba1.png" width="400px"> </div><br>

### 6. 线程中断规则

> Thread Interruption Rule

对线程 interrupt() 方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过 interrupted() 方法检测到是否有中断发生。

### 7. 对象终结规则

> Finalizer Rule

一个对象的初始化完成（构造函数执行结束）先行发生于它的 finalize() 方法的开始。

### 8. 传递性

> Transitivity

如果操作 A 先行发生于操作 B，操作 B 先行发生于操作 C，那么操作 A 先行发生于操作 C。

## 本章小结

本章重点讲解了 JMM 中的

**可见性** - 由 JVM 缓存优化引起

**有序性** - 由 JVM 指令重排序优化引起

**happens-before 规则**

**原理方面**

CPU 指令并行

volatile

**模式方面**

两阶段终止模式的 volatile 改进

同步模式之 balking

# 乐观锁

锁可以从不同的角度分类。其中，乐观锁和悲观锁是一种分类方式。

**悲观锁：**

悲观锁就是我们常说的锁。对于悲观锁来说，它总是认为每次访问共享资源时会发生冲突，所以必须对每次数据操作加上锁，以保证临界区的程序同一时间只能有一个线程在执行。

**乐观锁：**

乐观锁又称为“无锁”，顾名思义，它是乐观派。乐观锁总是假设对共享资源的访问没有冲突，线程可以不停地执行，无需加锁也无需等待。而一旦多个线程发生冲突，乐观锁通常是使用一种称为CAS的技术来保证线程执行的安全性。

由于无锁操作中没有锁的存在，因此不可能出现死锁的情况，也就是说**乐观锁天生免疫死锁**。

乐观锁多用于“读多写少“的环境，避免频繁加锁影响性能；而悲观锁多用于”写多读少“的环境，避免频繁失败和重试影响性能。

## 问题提出

问题提出

有如下需求，保证 account.withdraw 取款方法的线程安全

原有实现并不是线程安全的

### 不安全的线程

```java
package cn.itcast;
import java.util.ArrayList;
import java.util.List;
interface Account {
    // 获取余额
    Integer getBalance();
    // 取款
    void withdraw(Integer amount);
    /**

* 方法内会启动 1000 个线程，每个线程做 -10 元 的操作
* 如果初始余额为 10000 那么正确的结果应当是 0
  */
    static void demo(Account account) {
        List<Thread> ts = new ArrayList<>();
        long start = System.nanoTime();
        for (int i = 0; i < 1000; i++) {
            ts.add(new Thread(() -> {
                account.withdraw(10);
            }));
        }
        ts.forEach(Thread::start);
        ts.forEach(t -> {
            try {
                t.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        long end = System.nanoTime();
        System.out.println(account.getBalance()

                           + " cost: " + (end-start)/1000_000 + " ms");
    }
}
class AccountUnsafe implements Account {
    private Integer balance;
    public AccountUnsafe(Integer balance) {
        this.balance = balance;
    }
    @Override
    public Integer getBalance() {
        return balance;
    }
    @Override
    public void withdraw(Integer amount) {
        balance -= amount;
    }
}
```

执行测试代码

```java
public static void main(String[] args) {
Account.demo(new AccountUnsafe(10000));
}
```

### 解决问题的两种思路

#### 锁机制

首先想到的是给 Account 对象加锁

```java
class AccountUnsafe implements Account {
    private Integer balance;
    public AccountUnsafe(Integer balance) {
        this.balance = balance;
    }
    @Override
    public synchronized Integer getBalance() {
        return balance;
    }
    @Override
    public synchronized void withdraw(Integer amount) {
        balance -= amount;
    }
}
```



#### 无锁机制

```java
class AccountSafe implements Account {
    private AtomicInteger balance;
    public AccountSafe(Integer balance) {
        this.balance = new AtomicInteger(balance);
    }
    @Override
    public Integer getBalance() {
        return balance.get();
    }
    @Override
    public void withdraw(Integer amount) {
        while (true) {
            int prev = balance.get();
            int next = prev - amount;
            //CAS
            if (balance.compareAndSet(prev, next)) {
                break;
            }
        }
        // 可以简化为下面的方法
        // balance.addAndGet(-1 * amount);
    }
}
```

## CAS

### 概念

CAS的全称是：比较并交换（Compare And Swap）。在CAS中，有这样三个值：

- V：要更新的变量(var)
- E：预期值(expected) 
- N：新值(new)

比较并交换的过程如下：

判断V是否等于E，如果等于，将V的值设置为N；如果不等，说明已经有其它线程更新了V，则当前线程放弃更新，什么都不做。

所以这里的**预期值E本质上指的是“旧值”**。

我们以一个简单的例子来解释这个过程：

1. 如果有一个多个线程共享的变量`i`原本等于5，我现在在线程A中，想把它设置为新的值6;
2. 我们使用CAS来做这个事情；
3. 首先我们用i去与5对比，发现它等于5，说明没有被其它线程改过，那我就把它设置为新的值6，此次CAS成功，`i`的值被设置成了6；
4. 如果不等于5，说明`i`被其它线程改过了（比如现在`i`的值为2），那么我就什么也不做，此次CAS失败，`i`的值仍然为2。

在这个例子中，`i`就是V，5就是E，6就是N。

#### 原语CAS

那有没有可能我在判断了`i`为5之后，正准备更新它的新值的时候，被其它线程更改了`i`的值呢？

不会的。**因为CAS是一种原子操作，它是一种系统原语，是一条CPU的原子指令，从CPU层面保证它的原子性**

**当多个线程同时使用CAS操作一个变量时，只有一个会胜出，并成功更新，其余均会失败，但失败的线程并不会被挂起，仅是被告知失败，并且允许再次尝试，当然也允许失败的线程放弃操作。**

### 实现

#### 分析

前面看到的 AtomicInteger 的解决方法，内部并没有用锁来保护共享变量的线程安全。那么它是如何实现的呢？

```java
public void withdraw(Integer amount) {
    while(true) {
        // 需要不断尝试，直到成功为止
        while (true) {
            // 比如拿到了旧值 1000
            int prev = balance.get();
            // 在这个基础上 1000-10 = 990
            int next = prev - amount;
            /*
compareAndSet 正是做这个检查，在 set 前，先比较 prev 与当前值
- 不一致了，next 作废，返回 false 表示失败
比如，别的线程已经做了减法，当前值已经被减成了 990
那么本线程的这次 990 就作废了，进入 while 下次循环重试
- 一致，以 next 设置为新值，返回 true 表示成功
*/
            if (balance.compareAndSet(prev, next)) {
                break;
            }
        }
    }
}
```

其中的关键是 **compareAndSet，它的简称就是 CAS （也有 Compare And Swap 的说法），它必须是原子操作。**

*注意*
*其实 CAS 的底层是 lock cmpxchg 指令（X86 架构），在单核 CPU 和多核 CPU 下都能够保证【比较-交换】的原子性。*

#### 原理

前面提到，CAS是一种原子操作。那么**Java是怎样来使用CAS的**呢？我们知道，在Java中，如果一个方法是native的，那Java就不负责具体实现它，而是交给底层的JVM使用c或者c++去实现。

在Java中，有一个`Unsafe`类，它在`sun.misc`包中。它里面是一些**native**方法，其中就有几个关于CAS的：

```java
boolean compareAndSwapObject(Object o, long offset,Object expected, Object x);
boolean compareAndSwapInt(Object o, long offset,int expected,int x);
boolean compareAndSwapLong(Object o, long offset,long expected,long x);
```

x 表示 next；

当然，他们都是`public native`的。

Unsafe中对CAS的实现是C++写的，它的具体实现和操作系统、CPU都有关系。

Linux的X86下主要是通过`cmpxchgl`这个指令在CPU级完成CAS操作的，但在多处理器情况下必须使用`lock`指令加锁来完成。当然不同的操作系统和处理器的实现会有所不同，大家可以自行了解。

当然，Unsafe类里面还有其它方法用于不同的用途。比如支持线程挂起和恢复的`park`和`unpark`， LockSupport类底层就是调用了这两个方法。还有支持反射操作的`allocateInstance()`方法。

### volatile相关

**获取共享变量时，为了保证该变量的可见性，需要使用 volatile 修饰。**

它可以用来修饰成员变量和静态成员变量，他可以避免线程从自己的工作缓存中查找变量的值，必须到主存中获取它的值，线程操作 volatile 变量都是直接操作主存。即一个线程对 volatile 变量的修改，对另一个线程可见。
注意
volatile 仅仅保证了共享变量的可见性，让其它线程能够看到最新值，但不能解决指令交错问题（不能保证原子性）

**CAS 必须借助 volatile 才能读取到共享变量的最新值来实现【比较并交换】的效果**

### 多核优势

**多核条件下**

- 无锁情况下，即使重试失败，线程始终在高速运行，没有停歇，而 synchronized 会让线程在没有获得锁的时候，发生上下文切换，进入阻塞。
- 打个比喻线程就好像高速跑道上的赛车，高速运行时，速度超快，一旦发生上下文切换，就好比赛车要减速、熄火，等被唤醒又得重新打火、启动、加速... 恢复到高速运行，代价比较大
- 但无锁情况下，因为线程要保持运行，需要额外 CPU 的支持，CPU 在这里就好比高速跑道，没有额外的跑道，线程想高速运行也无从谈起，虽然不会进入阻塞，但由于没有分到时间片，仍然会进入可运行状态，还是会导致上下文切换。

### 对比有锁

结合 **CAS** 和 volatile 可以实现无锁并发，**适用于线程数少、多核 CPU 的场景下**。

- CAS 是基于乐观锁的思想：最乐观的估计，不怕别的线程来修改共享变量，就算改了也没关系，我吃亏点再重试呗。
- synchronized 是基于悲观锁的思想：最悲观的估计，得防着其它线程来修改共享变量，我上了锁你们都别想改，我改完了解开锁，你们才有机会。

CAS 体现的是无锁并发、无阻塞并发，请仔细体会这两句话的意思

- 因为没有使用 synchronized，所以线程不会陷入阻塞，这是效率提升的因素之一
- 但如果竞争激烈，可以想到重试必然频繁发生，反而效率会受影响

## 原子操作

### 原子整数

J.U.C 并发包提供了：
AtomicBoolean
AtomicInteger
AtomicLong

以 AtomicInteger 为例

#### AtomicInteger

##### 方法

源码的诸多功能都是由unsafe提供

```java
AtomicInteger i = new AtomicInteger(0);
// 获取并自增（i = 0, 结果 i = 1, 返回 0），类似于 i++
System.out.println(i.getAndIncrement());
// 自增并获取（i = 1, 结果 i = 2, 返回 2），类似于 ++i
System.out.println(i.incrementAndGet());
// 自减并获取（i = 2, 结果 i = 1, 返回 1），类似于 --i
System.out.println(i.decrementAndGet());
// 获取并自减（i = 1, 结果 i = 0, 返回 1），类似于 i--
System.out.println(i.getAndDecrement());
// 获取并加值（i = 0, 结果 i = 5, 返回 0）
System.out.println(i.getAndAdd(5));
// 加值并获取（i = 5, 结果 i = 0, 返回 0）
System.out.println(i.addAndGet(-5));
// 获取并更新（i = 0, p 为 i 的当前值, 结果 i = -2, 返回 0）
// 其中函数中的操作能保证原子，但函数需要无副作用
System.out.println(i.getAndUpdate(p -> p - 2));
// 更新并获取（i = -2, p 为 i 的当前值, 结果 i = 0, 返回 0）
// 其中函数中的操作能保证原子，但函数需要无副作用
System.out.println(i.updateAndGet(p -> p + 2));
// 获取并计算（i = 0, p 为 i 的当前值, x 为参数1, 结果 i = 10, 返回 0）
// 其中函数中的操作能保证原子，但函数需要无副作用
// getAndUpdate 如果在 lambda 中引用了外部的局部变量，要保证该局部变量是 final 的
// getAndAccumulate 可以通过 参数1 来引用外部的局部变量，但因为其不在 lambda 中因此不必是 final
System.out.println(i.getAndAccumulate(10, (p, x) -> p + x));
// 计算并获取（i = 10, p 为 i 的当前值, x 为参数1, 结果 i = 0, 返回 0）
// 其中函数中的操作能保证原子，但函数需要无副作用
System.out.println(i.accumulateAndGet(-10, (p, x) -> p + x));
```

##### 源码解析

J.U.C 包里面的整数原子类 AtomicInteger 的方法调用了 Unsafe 类的 CAS 操作。

以下代码使用了 AtomicInteger 执行了自增的操作。

```java
private AtomicInteger cnt = new AtomicInteger();

public void add() {
    cnt.incrementAndGet();
}
```

以下代码是 incrementAndGet() 的源码，它调用了 **Unsafe 的 getAndAddInt()** 。

```java
public final int incrementAndGet() {
    return unsafe.getAndAddInt(this, valueOffset, 1) + 1;
}
```

以下代码是 **getAndAddInt() 源码**，var1 指示对象内存地址，var2 指示该字段相对对象内存地址的偏移，var4 指示操作需要加的数值，这里为 1。通过 getIntVolatile(var1, var2) 得到旧的预期值，通过调用 compareAndSwapInt() 来进行 CAS 比较，如果该字段内存地址中的值等于 var5，那么就更新内存地址为 var1+var2 的变量为 var5+var4。

可以看到 getAndAddInt() 在一个循环中进行，发生冲突的做法是不断的进行重试。

```java
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));

    return var5;
}
```

### 原子引用

为什么需要原子引用类型？

AtomicReference

AtomicMarkableReference

AtomicStampedReference

`AtomicReference` 类提供了对象引用的非阻塞原子性读写操作，对比原子更新基本类型的 `AtomicInteger` ，只能更新一个变量，如果要原子更新多个变量，就需要使用这个原子更新引用类型提供的类。

#### ABA问题

所谓ABA问题，就是一个值原来是A，变成了B，又变回了A。这个时候使用CAS是检查不出变化的，但实际上却被更新了两次。

ABA问题的解决思路是在变量前面追加上**版本号或者时间戳**。从JDK 1.5开始，JDK的atomic包里提供了一个类`AtomicStampedReference`类来解决ABA问题。

这个类的`compareAndSet`方法的作用是首先检查当前引用是否等于预期引用，并且检查当前标志是否等于预期标志，如果二者都相等，才使用CAS设置为新的值和标志。

```java
public boolean compareAndSet(V   expectedReference,
                             V   newReference,
                             int expectedStamp,
                             int newStamp) {
    Pair<V> current = pair;
    return
        expectedReference == current.reference &&
        expectedStamp == current.stamp &&
        ((newReference == current.reference &&
          newStamp == current.stamp) ||
         casPair(current, Pair.of(newReference, newStamp)));
}
```

#### AtomicStampedReference

```java
static AtomicStampedReference<String> ref = new AtomicStampedReference<>("A", 0);
public static void main(String[] args) throws InterruptedException {
    log.debug("main start...");
    // 获取值 A
    String prev = ref.getReference();
    // 获取版本号
    int stamp = ref.getStamp();
    log.debug("版本 {}", stamp);
    // 如果中间有其它线程干扰，发生了 ABA 现象
    other();
    sleep(1);
    // 尝试改为 C
    log.debug("change A->C {}", ref.compareAndSet(prev, "C", stamp, stamp + 1));
}
private static void other() {
    new Thread(() -> {
        log.debug("change A->B {}", ref.compareAndSet(ref.getReference(), "B",
                                                      ref.getStamp(), ref.getStamp() + 1));
        log.debug("更新版本为 {}", ref.getStamp());
    }, "t1").start();
    sleep(0.5);
    new Thread(() -> {
        log.debug("change B->A {}", ref.compareAndSet(ref.getReference(), "A",
                                                      ref.getStamp(), ref.getStamp() + 1));
        log.debug("更新版本为 {}", ref.getStamp());
    }, "t2").start();
}
```

输出为

```
15:41:34.891 c.Test36 [main] - main start...
15:41:34.894 c.Test36 [main] - 版本 0
15:41:34.956 c.Test36 [t1] - change A->B true
15:41:34.956 c.Test36 [t1] - 更新版本为 1
15:41:35.457 c.Test36 [t2] - change B->A true
15:41:35.457 c.Test36 [t2] - 更新版本为 2
15:41:36.457 c.Test36 [main] - change A->C false
```



AtomicStampedReference 可以给原子引用加上版本号，追踪原子引用整个的变化过程，如： A -> B -> A ->
C ，通过AtomicStampedReference，我们可以知道，引用变量中途被更改了几次。
但是有时候，并不关心引用变量更改了几次，只是单纯的关心是否更改过，所以就有了

**AtomicMarkableReference**

### 原子数组



Atomic数组，顾名思义，就是能以原子的方式，操作数组中的元素。

JDK提供了三种类型的原子数组：`AtomicIntegerArray`、`AtomicLongArray`、`AtomicReferenceArray`。

#### 原理

这三种类型大同小异：

- `AtomicIntegerArray`对应`AtomicInteger`
- `AtomicLongArray`对应`AtomicLong`
- `AtomicReferenceArray`对应`AtomicReference`

其实阅读源码也可以发现，这些数组原子类与对应的普通原子类相比，只是多了通过索引找到内存中元素地址的操作而已。

> 注意：原子数组并不是说可以让线程以原子方式一次性地操作数组中所有元素的数组。
> 而是指对于数组中的每个元素，可以以原子方式进行操作。

说得简单点，**原子数组类型其实可以看成原子类型组成的数组**。

比如：

```JAVA
AtomicIntegerArray  array = new AtomicIntegerArray(10);
array.getAndIncrement(0);   // 将第0个元素原子地增加1
```

等同于

```
AtomicInteger[]  array = new AtomicInteger[10];
array[0].getAndIncrement();  // 将第0个元素原子地增加1
```

#### 源码

本节将以AtomicIntegerArray为例，介绍下原子数组的原理，AtomicLongArray和AtomicReferenceArray的使用和源码与AtomicIntegerArray大同小异，读者可以自己查看[Oracle官方文档](https://docs.oracle.com/javase/8/docs/api/)和源码。

AtomicIntegerArray其实和其它原子类区别并不大，只不过构造的时候传入的是一个int[]数组，然后底层通过Unsafe类操作数组：

```java
public class AtomicIntegerArray implements java.io.Serializable {
    private static final Unsafe unsafe = Unsafe.getUnsafe();
    private static final int base = unsafe.arrayBaseOffset(int[].class); //获取int类型数组第一个元素的偏移地址
    private static final int shift;
    private final int[] array;

    static {
        int scale = unsafe.arrayIndexScale(int[].class); //获取int类型数组的每个int元素的占用内存大小
        if ((scale & (scale - 1)) != 0)
            throw new Error("data type scale not a power of two");
        //Integer.numberOfLeadingZeros(scale) 表示scale的二进制形式中，从最左边数起，连续0的个数
        shift = 31 - Integer.numberOfLeadingZeros(scale); //计算出每个int元素的比特位数目
    }

    private long checkedByteOffset(int i) {
        if (i < 0 || i >= array.length)
            throw new IndexOutOfBoundsException("index " + i);

        return byteOffset(i);
    }

    /**
     * 将数组索引i转化为对应元素在内存中的偏移地址
     */
    private static long byteOffset(int i) {
        return ((long) i << shift) + base; //i << shift表示当前索引i的元素距离第一个元素的偏移量
    }
    
    public AtomicIntegerArray(int length) {
        array = new int[length];
    }

    
    public AtomicIntegerArray(int[] array) {
        // Visibility guaranteed by final field guarantees
        this.array = array.clone();
    }
}
```

可以看到，AtomicIntegerArray提供了两种构造器，本质都是内部利用`array`变量保存一个`int[]`数组引用。

另外，AtomicIntegerArray利用Unsafe类直接操作`int[]`对象的内存地址，以达到操作数组元素的目的，几个关键的变量解释如下：

```
int base = unsafe.arrayBaseOffset(int[].class);
```

Unsafe类的arraBaseOffset方法：**返回指定类型数组的第一个元素地址**相对于数组起始地址的偏移值。

```
int scale = unsafe.arrayIndexScale(int[].class);
```

Unsafe类的arrayIndexScale方法：返回指定类型数组的元素所占用的字节数。比如int[]数组中的每个int元素占用4个字节，就返回4。

那么，通过`base + i * sacle` 其实就可以知道 索引i的元素在数组中的内存起始地址。

但是，观察AtomicIntegerArray的byteOffset方法，是通过`i << shift + base` 的公式计算元素的起始地址的：i << shift + base = i * 2^{shift} + base

这里 2^{shift}其实就等于scale。

```
shift = 31 - Integer.numberOfLeadingZeros(scale)
```

`Integer.numberOfLeadingZeros(scale)`是将scale转换为2进制，然后从左往右数连续0的个数。比如：

shift = 31 - Integer.numberOfLeadingZeros(4) = 31 - 29 =2

之所以要这么绕一圈，其实是处于性能的考虑，通过移位计算乘法的效率往往更高。

### 原子字段更新器

AtomicReferenceFieldUpdater // 域 字段

AtomicIntegerFieldUpdater

AtomicLongFieldUpdater

利用字段更新器，可以针对对象的某个域（Field）进行原子操作，只能配合 volatile 修饰的字段使用，否则会出现异常

```

Exception in thread "main" java.lang.IllegalArgumentException: Must be volatile type
```

```java

    public class Test5 {
        private volatile int field;
        public static void main(String[] args) {
            AtomicIntegerFieldUpdater fieldUpdater =
                AtomicIntegerFieldUpdater.newUpdater(Test5.class, "field");
            Test5 test5 = new Test5();
            fieldUpdater.compareAndSet(test5, 0, 10);
            // 修改成功 field = 10
            System.out.println(test5.field);
            // 修改成功 field = 20
            fieldUpdater.compareAndSet(test5, 10, 20);
            System.out.println(test5.field);
            // 修改失败 field = 20
            fieldUpdater.compareAndSet(test5, 10, 30);
            System.out.println(test5.field);
        }
    }
```



```java
 AtomicIntegerFieldUpdater fieldUpdater = AtomicIntegerFieldUpdater.newUpdater(Test5.class, "field");
```

为Test5.class,的"field"设置原子更新器

### 原子累加器

**为提升性能而生**

累加器性能比较

```java
private static <T> void demo(Supplier<T> adderSupplier, Consumer<T> action) {
    T adder = adderSupplier.get();
    long start = System.nanoTime();
    List<Thread> ts = new ArrayList<>();
    // 4 个线程，每人累加 50 万
    for (int i = 0; i < 40; i++) {
        ts.add(new Thread(() -> {
            for (int j = 0; j < 500000; j++) {
                action.accept(adder);
            }
        }));
    }
    ts.forEach(t -> t.start());
    ts.forEach(t -> {
        try {
            t.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    });
    long end = System.nanoTime();
    System.out.println(adder + " cost:" + (end - start)/1000_000);
}
```

比较 AtomicLong 与 LongAdder

```java
for (int i = 0; i < 5; i++) {
    demo(() -> new LongAdder(), adder -> adder.increment());
}
for (int i = 0; i < 5; i++) {
    demo(() -> new AtomicLong(), adder -> adder.getAndIncrement());
}
```

#### 原理

性能提升的原因很简单

- 就是在有竞争时，设置多个累加单元，Therad-0 累加 Cell[0]，而 Thread-1 累加Cell[1]... 
- 最后将结果汇总。
- 这样它们在累加时操作的不同的 Cell 变量，因此减少了 CAS 重试失败，从而提高性能。

#### 伪共享

其中 Cell 即为累加单元

```java
// 防止缓存行伪共享
@sun.misc.Contended
    static final class Cell {
        volatile long value;
        Cell(long x) { value = x; }
        // 最重要的方法, 用来 cas 方式进行累加, prev 表示旧值, next 表示新值
        final boolean cas(long prev, long next) {
            return UNSAFE.compareAndSwapLong(this, valueOffset, prev, next);
        }
        // 省略不重要代码
    }
```

主要是防止 线程工作内存（缓存）中缓存两个（多个）Cell单元，但volatile导致线程之间对某一个线程对Cell无效操作，致使开销较大

因此缓存行可以存下 2 个的 Cell 对象。这样问题来了：
Core-0 要修改 Cell[0]
Core-1 要修改 Cell[1]
无论谁修改成功，都会导致对方 Core 的缓存行失效，比如 Core-0 中 Cell[0]=6000, 

Cell[1]=8000 要累加
Cell[0]=6001, Cell[1]=8000 ，**这时会让 Core-1 的缓存行失效**

@sun.misc.Contended 用来解决这个问题，它的原理是在使用此注解的对象或字段的前后各增加 128 字节大小的padding，从而让 CPU 将对象预读至缓存时占用不同的缓存行，这样，不会造成对方缓存行的失效（一个线程只缓存一个Cell）

![image-20211121150547286](笔记图库/image-20211121150547286.png)

## Unsafe

### 获取

Unsafe 对象提供了非常底层的，操作内存、线程的方法，Unsafe 对象不能直接调用，只能通过反射获得

```java
public class UnsafeAccessor {
    static Unsafe unsafe;
    static {
        try {
            Field theUnsafe = Unsafe.class.getDeclaredField("theUnsafe");
            theUnsafe.setAccessible(true);
            unsafe = (Unsafe) theUnsafe.get(null);
        } catch (NoSuchFieldException | IllegalAccessException e) {
            throw new Error(e);
        }
    }
    static Unsafe getUnsafe() {
        return unsafe;
    }
}
```

### CAS 操作

诸多原子类的CAS都源于Unsafe

```java
@Data
class Student {
    volatile int id;
    volatile String name;
}
```

```java
Unsafe unsafe = UnsafeAccessor.getUnsafe();
Field id = Student.class.getDeclaredField("id");
Field name = Student.class.getDeclaredField("name");

// 获得成员变量的偏移量
long idOffset = UnsafeAccessor.unsafe.objectFieldOffset(id);
long nameOffset = UnsafeAccessor.unsafe.objectFieldOffset(name);

Student student = new Student();

// 使用 cas 方法替换成员变量的值
UnsafeAccessor.unsafe.compareAndSwapInt(student, idOffset, 0, 20); // 返回 true
UnsafeAccessor.unsafe.compareAndSwapObject(student, nameOffset, null, "张三"); // 返回 true
System.out.println(student);
```

#### unsafe.objectFieldOffset

一个java对象可以看成是一段内存，各个字段都得按照一定的顺序放在这段内存里，同时考虑到对齐要求，可能这些字段不是连续放置的，

用这个方法能准确地告诉你某个字段相对于对象的起始内存地址的字节偏移量，因为是相对偏移量，所以它其实跟某个具体对象又没什么太大关系，跟class的定义和虚拟机的内存模型的实现细节更相关。

### 自定义类

使用自定义的 AtomicData 实现之前线程安全的原子整数 Account 实现

主要步骤

- 通过反射获取unsafe对象
- 获取对应属性在对象中的**偏移量** 用于 Unsafe 直接访问该属性
- unsafe.compareAndSwapInt(**this**, **DATA_OFFSET**, oldValue, next）

```java
class AtomicData {
    private volatile int data;
    static final Unsafe unsafe;
    static final long DATA_OFFSET;
    
    static {
        unsafe = UnsafeAccessor.getUnsafe();
        try {
            // data 属性在 DataContainer 对象中的偏移量，用于 Unsafe 直接访问该属性
            DATA_OFFSET = unsafe.objectFieldOffset(AtomicData.class.getDeclaredField("data"));
        } catch (NoSuchFieldException e) {
            throw new Error(e);
        }
    }
    
    public AtomicData(int data) {
        this.data = data;
    }
    
    public void decrease(int amount) {
        int oldValue;
        while(true) {
            // 获取共享变量旧值，可以在这一行加入断点，修改 data 调试来加深理解
            oldValue = data;
            // cas 尝试修改 data 为 旧值 + amount，如果期间旧值被别的线程改了，返回 false
            if (unsafe.compareAndSwapInt(this, DATA_OFFSET, oldValue, oldValue - amount)) {
                return;
            }
        }
    }
    public int getData() {
        return data;
    }
}
```

Account 实现

```java
Account.demo(new Account() {
    AtomicData atomicData = new AtomicData(10000);
    @Override
    public Integer getBalance() {
        return atomicData.getData();
    }
    @Override
    public void withdraw(Integer amount) {
        atomicData.decrease(amount);
    }
});
```

## 本章小结

CAS 与 volatile

API

- 原子整数
- 原子引用
- 原子数组
- 字段更新器

原子累加器

Unsafe

原理方面

* LongAdder 源码
* 伪共享

### 劣势

#### 循环时间长开销大

CAS多与自旋结合。如果自旋CAS长时间不成功，会占用大量的CPU资源。

解决思路是让JVM支持处理器提供的**pause指令**。

pause指令能让自旋失败时cpu睡眠一小段时间再继续自旋，从而使得读操作的频率低很多,为解决内存顺序冲突而导致的CPU流水线重排的代价也会小很多。

####  只能保证一个共享变量的原子操作

这个问题你可能已经知道怎么解决了。有两种解决方案：

1. 使用JDK 1.5开始就提供的`AtomicReference`类保证对象之间的原子性，把多个变量放到一个对象里面进行CAS操作；
2. 使用锁。锁内的临界区代码可以保证只有当前线程能操作

# 不可变

## 线程安全问题

如果一个对象在不能够修改其内部状态（属性），那么它就是线程安全的，因为不存在并发修改啊

## 不可变类型

不可变（Immutable）的对象一定是线程安全的，不需要再采取任何的线程安全保障措施。只要一个不可变的对象被正确地构建出来，永远也不会看到它在多个线程之中处于不一致的状态。多线程环境下，应当尽量使对象成为不可变，来满足线程安全。

不可变的类型：

- final 关键字修饰的基本数据类型
- String
- 枚举类型
- Number 部分子类，如 Long 和 Double 等数值包装类型，BigInteger 和 BigDecimal 等大数据类型。
- 但同为 Number 的原子类 AtomicInteger 和 AtomicLong 则是可变的。

对于集合类型，可以使用 Collections.unmodifiableXXX() 方法来获取一个不可变的集合。

```java
public class ImmutableExample {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        Map<String, Integer> unmodifiableMap = Collections.unmodifiableMap(map);
        unmodifiableMap.put("a", 1);
    }
}
```

```html
Exception in thread "main" java.lang.UnsupportedOperationException
    at java.util.Collections$UnmodifiableMap.put(Collections.java:1457)
    at ImmutableExample.main(ImmutableExample.java:9)
```

Collections.unmodifiableXXX() 先对原始的集合进行拷贝，需要对集合进行修改的方法都直接抛出异常。

```java
public V put(K key, V value) {
    throw new UnsupportedOperationException();
}
```

## 不可变设计

另一个大家更为熟悉的 String 类也是不可变的，以它为例，说明一下不可变设计的要素

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
    
    /** Cache the hash code for the string */
    private int hash; // Default to 0
    
    // ...
}
```

## final

### 使用

- 属性用 final 修饰保证了该属性是只读的，不能修改
- 类用 final 修饰保证了该类中的方法不能被覆盖，防止子类无意间破坏不可变性

### 原理

```java
public class TestFinal {
final int a = 20;
}
```

字节码

```
0: aload_0
1: invokespecial #1 // Method java/lang/Object."<init>":()V
4: aload_0
5: bipush 20
7: putfield #2 // Field a:I
<-- 写屏障
10: return
```

final 变量的赋值和volatile也会通过 putfield 指令来完成，同样在这条指令之后也会加入写屏障，保证在其它线程读到
它的值时不会出现为 0 的情况；

### 获取 final 变量的原理

java的常量分为**编译期常量和运行期常量**

编译期常量 如 final int a = 3*2;
运行期常量 如 final int b = new Random(100);
顾名思义，编译期常量的值在编译期就确定了。

# 线程池

## 原因

使用线程池主要有以下三个原因：

1. 创建/销毁线程需要消耗系统资源，线程池可以**复用已创建的线程**。

   比如Thread1执行完毕之后，在传入其他Runnable参数

2. **控制并发的数量**。并发数量过多，可能会导致资源消耗过多，从而造成服务器崩溃。（主要原因）

   比如 Runnable参数（任务）对应一个线程；任务过多并发量太大造成服务器崩溃；

   可以将多余的任务（超出服务器最大并发量的任务）放到任务队列中

   等到任务执行完，在从任务队列中取

3. **可以对线程做统一管理**。

## 自定义线程池

### 任务队列

#### 基本参数

任务队列需要实现：

**添加任务**：

作为生产者，向任务队列中添加任务 / 当任务队列满时应阻塞，需要条件锁

**取出任务：**

作为消费者，从任务队列中取出任务/ 当任务队列空时应该阻塞，需要条件锁

```java
class BlockingQueue<T> {
    // 1. 任务队列
    private Deque<T> queue = new ArrayDeque<>();

    // 2. 锁
    private ReentrantLock lock = new ReentrantLock();

    // 3. 生产者条件变量
    private Condition fullWaitSet = lock.newCondition();

    // 4. 消费者条件变量
    private Condition emptyWaitSet = lock.newCondition();

    // 5. 容量
    private int capcity;

    public BlockingQueue(int capcity) {
        this.capcity = capcity;
    }
}
```

#### 任务获取

##### 基本获取

```java
 // 阻塞获取
    public T take() {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                try {
                    emptyWaitSet.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            T t = queue.removeFirst();
            
            fullWaitSet.signal();
            
            return t;
        } finally {
            lock.unlock();
        }
    }
```

##### 超时获取

有限时间内获取任务，超时返回null

```java
// 带超时阻塞获取
    public T poll(long timeout, TimeUnit unit) {
        lock.lock();
        try {
            // 将 timeout 统一转换为 纳秒
            long nanos = unit.toNanos(timeout);
            while (queue.isEmpty()) {
                try {
                    // 返回值是剩余时间，超出等待时间返回null
                    if (nanos <= 0) {
                        return null;
                    }
                    //队列为空等待一会儿；而非循环访问
                    nanos = emptyWaitSet.awaitNanos(nanos);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            T t = queue.removeFirst();
            
            fullWaitSet.signal();
            
            return t;
        } finally {
            lock.unlock();
        }
    }
```

#### 任务添加

##### 基本任务添加

```java
  public void put(T task) {
        lock.lock();
        try {
            while (queue.size() == capcity) {
                try {
                    log.debug("等待加入任务队列 {} ...", task);
                    fullWaitSet.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            log.debug("加入任务队列 {}", task);
            queue.addLast(task);
            
            emptyWaitSet.signal();
            
        } finally {
            lock.unlock();
        }
    }
```

##### 时限任务添加

有限时间内添加任务，超出时限，返回false

```java
// 带超时时间阻塞添加
public boolean offer(T task, long timeout, TimeUnit timeUnit) {
    lock.lock();
    try {
        long nanos = timeUnit.toNanos(timeout);
        while (queue.size() == capcity) {
            try {
                if(nanos <= 0) {
                    return false;
                }
                log.debug("等待加入任务队列 {} ...", task);
                nanos = fullWaitSet.awaitNanos(nanos);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        log.debug("加入任务队列 {}", task);
        queue.addLast(task);
        emptyWaitSet.signal();
        return true;
    } finally {
        lock.unlock();
    }
}
```

#### 查看任务数

查看任务数时，其他线程不可以添加或者修改线程任务

```java
public int size() {
    lock.lock();
    try {
        return queue.size();
    } finally {
        lock.unlock();
    }
}
```

### 线程池

线程池应实现：

- 通过任务创建线程
- 由线程执行任务

#### 基本参数

任务队列：从任务队列中获取任务并执行

线程池：最大线程数；线程集合

```java
class ThreadPool {
    // 任务队列
    private BlockingQueue<Runnable> taskQueue;

    // 线程集合
    private HashSet<Worker> workers = new HashSet<>();

    // 最大线程数
    private int coreSize;

    // 获取任务时的超时时间
    private long timeout;

    private TimeUnit timeUnit;
}
```

```java
  public ThreadPool(int coreSize, long timeout, TimeUnit timeUnit, int queueCapcity) {
        this.coreSize = coreSize;
        this.timeout = timeout;
        this.timeUnit = timeUnit;
        this.taskQueue = new BlockingQueue<Runnable>(queueCapcity);
        
    }
```



#### 执行任务

线程池已满：将任务放入任务队列

线程池未满：创建线程，执行任务，并将该线程加入到线程池

线程任务结束从任务队列中获取新任务实现 线程复用

```java

    // 执行任务
    public void execute(Runnable task) {
        // 当任务数没有超过 coreSize 时，直接交给 worker 对象执行
        // 如果任务数超过 coreSize 时，加入任务队列暂存
        synchronized (workers) {
            if(workers.size() < coreSize) {
                Worker worker = new Worker(task);
                log.debug("新增 worker{}, {}", worker, task);
                workers.add(worker);
                worker.start();
            } else {
                taskQueue.put(task);
            }
        }
    }

    class Worker extends Thread{
        private Runnable task;

        public Worker(Runnable task) {
            this.task = task;
        }

        @Override
        public void run() {
            // 执行任务
            // 1) 当 task 不为空，执行任务
            // 2) 当 task 执行完毕，再接着从任务队列获取任务并执行
//            while(task != null || (task = taskQueue.take()) != null) {
            while(task != null || (task = taskQueue.poll(timeout, timeUnit)) != null) {
                try {
                    log.debug("正在执行...{}", task);
                    task.run();
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    task = null;
                }
            }
            synchronized (workers) {
                log.debug("worker 被移除{}", this);
                workers.remove(this);
            }
        }
    }
```

### 功能增强

#### 拒绝策略

##### 触发时机

**当任务队列已满**

无法继续任务可以考虑

```
 // 1. 死等
 queue.put(task);
 // 2) 带超时等待
 queue.offer(task, 1500, TimeUnit.MILLISECONDS);
 // 3) 让调用者放弃任务执行
 log.debug("放弃{}", task);
 // 4) 让调用者抛出异常
 throw new RuntimeException("任务执行失败 " + task);
 // 5) 让调用者自己执行任务
 //task.run();
```

可以用一个接口 表示拒绝策略

##### 实现

**其中的参数为实现以上五种拒绝决策必要参数**

```java
interface RejectPolicy<T> {
    void reject(BlockingQueue<T> queue, T task);
}
```

含有一个方法的接口 是 函数式接口 可以使用lamb表达式实现；



#### 具体使用

将该接口作为线程池的一个参数，在进行**任务执行时可以使用**

##### 引入参数

```java
class ThreadPool {
    // 任务队列
    private BlockingQueue<Runnable> taskQueue;

    // 线程集合
    private HashSet<Worker> workers = new HashSet<>();

    // 核心线程数
    private int coreSize;

    // 获取任务时的超时时间
    private long timeout;

    private TimeUnit timeUnit;
    
//拒绝策略接口
    private RejectPolicy<Runnable> rejectPolicy;
    
     public ThreadPool(int coreSize, long timeout, TimeUnit timeUnit, int queueCapcity, RejectPolicy<Runnable> rejectPolicy) {
        this.coreSize = coreSize;
        this.timeout = timeout;
        this.timeUnit = timeUnit;
        this.taskQueue = new BlockingQueue<Runnable>(queueCapcity);
        this.rejectPolicy = rejectPolicy;
    }
}
```

##### 任务执行

```java
public void execute(Runnable task) {
        // 当任务数没有超过 coreSize 时，直接交给 worker 对象执行
        // 如果任务数超过 coreSize 时，加入任务队列暂存
        synchronized (workers) {
            if(workers.size() < coreSize) {
                Worker worker = new Worker(task);
                log.debug("新增 worker{}, {}", worker, task);
                workers.add(worker);
                worker.start();
            } else {
                //当线程池满时，向任务队列中添加任务时才 可能触发拒绝策略
                taskQueue.tryPut(rejectPolicy, task);
            }
        }
    }
```

##### 添加任务改写

任务队列

```java
    public void tryPut(RejectPolicy<T> rejectPolicy, T task) {
        lock.lock();
        try {
            // 判断队列是否满
            if(queue.size() == capcity) {
                //this为任务队列对象，tack为任务
                //reject由lamb表达式实现
                rejectPolicy.reject(this, task);
                
            } else {  // 有空闲
                log.debug("加入任务队列 {}", task);
                queue.addLast(task);
                emptyWaitSet.signal();
            }
        } finally {
            lock.unlock();
        }
    }
```



### 测试

```java
public class TestPool {
    public static void main(String[] args) {
        ThreadPool threadPool = new ThreadPool(1,
                1000, TimeUnit.MILLISECONDS, 1, (queue, task)->{
            // 1. 死等
//            queue.put(task);
            // 2) 带超时等待
//            queue.offer(task, 1500, TimeUnit.MILLISECONDS);
            // 3) 让调用者放弃任务执行
//            log.debug("放弃{}", task);
            // 4) 让调用者抛出异常
//            throw new RuntimeException("任务执行失败 " + task);
            // 5) 让调用者自己执行任务
            task.run();
        });
        for (int i = 0; i < 4; i++) {
            int j = i;
            threadPool.execute(() -> {
                try {
                    Thread.sleep(1000L);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                log.debug("{}", j);
            });
        }
    }
}
```



# 二、基础线程机制

### Executor

Executor 管理多个异步任务的执行，而无需程序员显式地管理线程的生命周期。这里的异步是指多个任务的执行互不干扰，不需要进行同步操作。

主要有三种 Executor：

- CachedThreadPool：一个任务创建一个线程；
- FixedThreadPool：所有任务只能使用固定大小的线程；
- SingleThreadExecutor：相当于大小为 1 的 FixedThreadPool。

```java
public static void main(String[] args) {
    ExecutorService executorService = Executors.newCachedThreadPool();
    for (int i = 0; i < 5; i++) {
        executorService.execute(new MyRunnable());
    }
    executorService.shutdown();
}
```

### Daemon

守护线程是程序运行时在守护提供服务的线程，不属于程序中不可或缺的部分。

当所有非守护线程结束时，程序也就终止，同时会杀死所有守护线程。

main() 属于非守护线程。

在线程启动之前使用 setDaemon() 方法可以将一个线程设置为守护线程。

```java
public static void main(String[] args) {
    Thread thread = new Thread(new MyRunnable());
    thread.setDaemon(true);
}
```

### sleep()

Thread.sleep(millisec) 方法会休眠当前正在执行的线程，millisec 单位为毫秒。

sleep() 可能会抛出 InterruptedException，因为异常不能跨线程传播回 main() 中，因此必须在本地进行处理。线程中抛出的其它异常也同样需要在本地进行处理。

```java
public void run() {
    try {
        Thread.sleep(3000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

### yield()

对静态方法 Thread.yield() 的调用声明了当前线程已经完成了生命周期中最重要的部分，可以切换给其它线程来执行。该方法只是对线程调度器的一个建议，而且也只是建议具有相同优先级的其它线程可以运行。

```java
public void run() {
    Thread.yield();
}
```

# 三、中断

一个线程执行完毕之后会自动结束，如果在运行过程中发生异常也会提前结束。

### InterruptedException

通过调用一个线程的 interrupt() 来中断该线程，如果该线程处于阻塞、限期等待或者无限期等待状态，那么就会抛出 InterruptedException，从而提前结束该线程。但是不能中断 I/O 阻塞和 synchronized 锁阻塞。

对于以下代码，在 main() 中启动一个线程之后再中断它，由于线程中调用了 Thread.sleep() 方法，因此会抛出一个 InterruptedException，从而提前结束线程，不执行之后的语句。

```java
public class InterruptExample {

    private static class MyThread1 extends Thread {
        @Override
        public void run() {
            try {
                Thread.sleep(2000);
                System.out.println("Thread run");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
public static void main(String[] args) throws InterruptedException {
    Thread thread1 = new MyThread1();
    thread1.start();
    thread1.interrupt();
    System.out.println("Main run");
}
```

```html
Main run
java.lang.InterruptedException: sleep interrupted
    at java.lang.Thread.sleep(Native Method)
    at InterruptExample.lambda$main$0(InterruptExample.java:5)
    at InterruptExample$$Lambda$1/713338599.run(Unknown Source)
    at java.lang.Thread.run(Thread.java:745)
```

### interrupted()

如果一个线程的 run() 方法执行一个无限循环，并且没有执行 sleep() 等会抛出 InterruptedException 的操作，那么调用线程的 interrupt() 方法就无法使线程提前结束。

但是调用 interrupt() 方法会设置线程的中断标记，此时调用 interrupted() 方法会返回 true。因此可以在循环体中使用 interrupted() 方法来判断线程是否处于中断状态，从而提前结束线程。

```java
public class InterruptExample {

    private static class MyThread2 extends Thread {
        @Override
        public void run() {
            while (!interrupted()) {
                // ..
            }
            System.out.println("Thread end");
        }
    }
}
```

```java
public static void main(String[] args) throws InterruptedException {
    Thread thread2 = new MyThread2();
    thread2.start();
    thread2.interrupt();
}
```

```html
Thread end
```

### Executor 的中断操作

调用 Executor 的 shutdown() 方法会等待线程都执行完毕之后再关闭，但是如果调用的是 shutdownNow() 方法，则相当于调用每个线程的 interrupt() 方法。

以下使用 Lambda 创建线程，相当于创建了一个匿名内部线程。

```java
public static void main(String[] args) {
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> {
        try {
            Thread.sleep(2000);
            System.out.println("Thread run");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    });
    executorService.shutdownNow();
    System.out.println("Main run");
}
```

```html
Main run
java.lang.InterruptedException: sleep interrupted
    at java.lang.Thread.sleep(Native Method)
    at ExecutorInterruptExample.lambda$main$0(ExecutorInterruptExample.java:9)
    at ExecutorInterruptExample$$Lambda$1/1160460865.run(Unknown Source)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
    at java.lang.Thread.run(Thread.java:745)
```

如果只想中断 Executor 中的一个线程，可以通过使用 submit() 方法来提交一个线程，它会返回一个 Future\<?\> 对象，通过调用该对象的 cancel(true) 方法就可以中断线程。

```java
Future<?> future = executorService.submit(() -> {
    // ..
});
future.cancel(true);
```

# 四、互斥同步

Java 提供了两种锁机制来控制多个线程对共享资源的互斥访问，第一个是 JVM 实现的 synchronized，而另一个是 JDK 实现的 ReentrantLock。

### synchronized

**1. 同步一个代码块**  

```java
public void func() {
    synchronized (this) {
        // ...
    }
}
```

它只作用于同一个对象，如果调用两个对象上的同步代码块，就不会进行同步。

对于以下代码，使用 ExecutorService 执行了两个线程，由于调用的是同一个对象的同步代码块，因此这两个线程会进行同步，当一个线程进入同步语句块时，另一个线程就必须等待。

```java
public class SynchronizedExample {

    public void func1() {
        synchronized (this) {
            for (int i = 0; i < 10; i++) {
                System.out.print(i + " ");
            }
        }
    }
}
```

```java
public static void main(String[] args) {
    SynchronizedExample e1 = new SynchronizedExample();
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> e1.func1());
    executorService.execute(() -> e1.func1());
}
```

```html
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9
```

对于以下代码，两个线程调用了不同对象的同步代码块，因此这两个线程就不需要同步。从输出结果可以看出，两个线程交叉执行。

```java
public static void main(String[] args) {
    SynchronizedExample e1 = new SynchronizedExample();
    SynchronizedExample e2 = new SynchronizedExample();
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> e1.func1());
    executorService.execute(() -> e2.func1());
}
```

```html
0 0 1 1 2 2 3 3 4 4 5 5 6 6 7 7 8 8 9 9
```


**2. 同步一个方法**  

```java
public synchronized void func () {
    // ...
}
```

它和同步代码块一样，作用于同一个对象。

**3. 同步一个类**  

```java
public void func() {
    synchronized (SynchronizedExample.class) {
        // ...
    }
}
```

作用于整个类，也就是说两个线程调用同一个类的不同对象上的这种同步语句，也会进行同步。

```java
public class SynchronizedExample {

    public void func2() {
        synchronized (SynchronizedExample.class) {
            for (int i = 0; i < 10; i++) {
                System.out.print(i + " ");
            }
        }
    }
}
```

```java
public static void main(String[] args) {
    SynchronizedExample e1 = new SynchronizedExample();
    SynchronizedExample e2 = new SynchronizedExample();
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> e1.func2());
    executorService.execute(() -> e2.func2());
}
```

```html
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9
```

**4. 同步一个静态方法**  

```java
public synchronized static void fun() {
    // ...
}
```

作用于整个类。

### ReentrantLock

ReentrantLock 是 java.util.concurrent（J.U.C）包中的锁。

```java
public class LockExample {

    private Lock lock = new ReentrantLock();

    public void func() {
        lock.lock();
        try {
            for (int i = 0; i < 10; i++) {
                System.out.print(i + " ");
            }
        } finally {
            lock.unlock(); // 确保释放锁，从而避免发生死锁。
        }
    }
}
```

```java
public static void main(String[] args) {
    LockExample lockExample = new LockExample();
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> lockExample.func());
    executorService.execute(() -> lockExample.func());
}
```

```html
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9
```


### 比较

**1. 锁的实现**  

synchronized 是 JVM 实现的，而 ReentrantLock 是 JDK 实现的。

**2. 性能**  

新版本 Java 对 synchronized 进行了很多优化，例如自旋锁等，synchronized 与 ReentrantLock 大致相同。

**3. 等待可中断**  

当持有锁的线程长期不释放锁的时候，正在等待的线程可以选择放弃等待，改为处理其他事情。

ReentrantLock 可中断，而 synchronized 不行。

**4. 公平锁**  

公平锁是指多个线程在等待同一个锁时，必须按照申请锁的时间顺序来依次获得锁。

synchronized 中的锁是非公平的，ReentrantLock 默认情况下也是非公平的，但是也可以是公平的。

**5. 锁绑定多个条件**  

一个 ReentrantLock 可以同时绑定多个 Condition 对象。

### 使用选择

除非需要使用 ReentrantLock 的高级功能，否则优先使用 synchronized。这是因为 synchronized 是 JVM 实现的一种锁机制，JVM 原生地支持它，而 ReentrantLock 不是所有的 JDK 版本都支持。并且使用 synchronized 不用担心没有释放锁而导致死锁问题，因为 JVM 会确保锁的释放。

# 五、线程之间的协作

当多个线程可以一起工作去解决某个问题时，如果某些部分必须在其它部分之前完成，那么就需要对线程进行协调。

### join()

在线程中调用另一个线程的 join() 方法，会将当前线程挂起，而不是忙等待，直到目标线程结束。

对于以下代码，虽然 b 线程先启动，但是因为在 b 线程中调用了 a 线程的 join() 方法，b 线程会等待 a 线程结束才继续执行，因此最后能够保证 a 线程的输出先于 b 线程的输出。

```java
public class JoinExample {

    private class A extends Thread {
        @Override
        public void run() {
            System.out.println("A");
        }
    }

    private class B extends Thread {

        private A a;

        B(A a) {
            this.a = a;
        }

        @Override
        public void run() {
            try {
                a.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("B");
        }
    }

    public void test() {
        A a = new A();
        B b = new B(a);
        b.start();
        a.start();
    }
}
```

```java
public static void main(String[] args) {
    JoinExample example = new JoinExample();
    example.test();
}
```

```
A
B
```

### wait() notify() notifyAll()

调用 wait() 使得线程等待某个条件满足，线程在等待时会被挂起，当其他线程的运行使得这个条件满足时，其它线程会调用 notify() 或者 notifyAll() 来唤醒挂起的线程。

它们都属于 Object 的一部分，而不属于 Thread。

只能用在同步方法或者同步控制块中使用，否则会在运行时抛出 IllegalMonitorStateException。

使用 wait() 挂起期间，线程会释放锁。这是因为，如果没有释放锁，那么其它线程就无法进入对象的同步方法或者同步控制块中，那么就无法执行 notify() 或者 notifyAll() 来唤醒挂起的线程，造成死锁。

```java
public class WaitNotifyExample {

    public synchronized void before() {
        System.out.println("before");
        notifyAll();
    }

    public synchronized void after() {
        try {
            wait();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("after");
    }
}
```

```java
public static void main(String[] args) {
    ExecutorService executorService = Executors.newCachedThreadPool();
    WaitNotifyExample example = new WaitNotifyExample();
    executorService.execute(() -> example.after());
    executorService.execute(() -> example.before());
}
```

```html
before
after
```

**wait() 和 sleep() 的区别**  

- wait() 是 Object 的方法，而 sleep() 是 Thread 的静态方法；
- wait() 会释放锁，sleep() 不会。

### await() signal() signalAll()

java.util.concurrent 类库中提供了 Condition 类来实现线程之间的协调，可以在 Condition 上调用 await() 方法使线程等待，其它线程调用 signal() 或 signalAll() 方法唤醒等待的线程。

相比于 wait() 这种等待方式，await() 可以指定等待的条件，因此更加灵活。

使用 Lock 来获取一个 Condition 对象。

```java
public class AwaitSignalExample {

    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void before() {
        lock.lock();
        try {
            System.out.println("before");
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public void after() {
        lock.lock();
        try {
            condition.await();
            System.out.println("after");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

```java
public static void main(String[] args) {
    ExecutorService executorService = Executors.newCachedThreadPool();
    AwaitSignalExample example = new AwaitSignalExample();
    executorService.execute(() -> example.after());
    executorService.execute(() -> example.before());
}
```

```html
before
after
```

# 六、线程状态

一个线程只能处于一种状态，并且这里的线程状态特指 Java 虚拟机的线程状态，不能反映线程在特定操作系统下的状态。

### 新建（NEW）

创建后尚未启动。

### 可运行（RUNABLE）

正在 Java 虚拟机中运行。但是在操作系统层面，它可能处于运行状态，也可能等待资源调度（例如处理器资源），资源调度完成就进入运行状态。所以该状态的可运行是指可以被运行，具体有没有运行要看底层操作系统的资源调度。

### 阻塞（BLOCKED）

请求获取 monitor lock 从而进入 synchronized 函数或者代码块，但是其它线程已经占用了该 monitor lock，所以出于阻塞状态。要结束该状态进入从而 RUNABLE 需要其他线程释放 monitor lock。

### 无限期等待（WAITING）

等待其它线程显式地唤醒。

阻塞和等待的区别在于，阻塞是被动的，它是在等待获取 monitor lock。而等待是主动的，通过调用  Object.wait() 等方法进入。

| 进入方法 | 退出方法 |
| --- | --- |
| 没有设置 Timeout 参数的 Object.wait() 方法 | Object.notify() / Object.notifyAll() |
| 没有设置 Timeout 参数的 Thread.join() 方法 | 被调用的线程执行完毕 |
| LockSupport.park() 方法 | LockSupport.unpark(Thread) |

### 限期等待（TIMED_WAITING）

无需等待其它线程显式地唤醒，在一定时间之后会被系统自动唤醒。

| 进入方法 | 退出方法 |
| --- | --- |
| Thread.sleep() 方法 | 时间结束 |
| 设置了 Timeout 参数的 Object.wait() 方法 | 时间结束 / Object.notify() / Object.notifyAll()  |
| 设置了 Timeout 参数的 Thread.join() 方法 | 时间结束 / 被调用的线程执行完毕 |
| LockSupport.parkNanos() 方法 | LockSupport.unpark(Thread) |
| LockSupport.parkUntil() 方法 | LockSupport.unpark(Thread) |

调用 Thread.sleep() 方法使线程进入限期等待状态时，常常用“使一个线程睡眠”进行描述。调用 Object.wait() 方法使线程进入限期等待或者无限期等待时，常常用“挂起一个线程”进行描述。睡眠和挂起是用来描述行为，而阻塞和等待用来描述状态。

### 死亡（TERMINATED）

可以是线程结束任务之后自己结束，或者产生了异常而结束。

[Java SE 9 Enum Thread.State](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.State.html)

# 七、J.U.C - AQS

java.util.concurrent（J.U.C）大大提高了并发性能，AQS 被认为是 J.U.C 的核心。

### CountDownLatch

用来控制一个或者多个线程等待多个线程。

维护了一个计数器 cnt，每次调用 countDown() 方法会让计数器的值减 1，减到 0 的时候，那些因为调用 await() 方法而在等待的线程就会被唤醒。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/ba078291-791e-4378-b6d1-ece76c2f0b14.png" width="300px"> </div><br>

```java
public class CountdownLatchExample {

    public static void main(String[] args) throws InterruptedException {
        final int totalThread = 10;
        CountDownLatch countDownLatch = new CountDownLatch(totalThread);
        ExecutorService executorService = Executors.newCachedThreadPool();
        for (int i = 0; i < totalThread; i++) {
            executorService.execute(() -> {
                System.out.print("run..");
                countDownLatch.countDown();
            });
        }
        countDownLatch.await();
        System.out.println("end");
        executorService.shutdown();
    }
}
```

```html
run..run..run..run..run..run..run..run..run..run..end
```

### CyclicBarrier

用来控制多个线程互相等待，只有当多个线程都到达时，这些线程才会继续执行。

和 CountdownLatch 相似，都是通过维护计数器来实现的。线程执行 await() 方法之后计数器会减 1，并进行等待，直到计数器为 0，所有调用 await() 方法而在等待的线程才能继续执行。

CyclicBarrier 和 CountdownLatch 的一个区别是，CyclicBarrier 的计数器通过调用 reset() 方法可以循环使用，所以它才叫做循环屏障。

CyclicBarrier 有两个构造函数，其中 parties 指示计数器的初始值，barrierAction 在所有线程都到达屏障的时候会执行一次。

```java
public CyclicBarrier(int parties, Runnable barrierAction) {
    if (parties <= 0) throw new IllegalArgumentException();
    this.parties = parties;
    this.count = parties;
    this.barrierCommand = barrierAction;
}

public CyclicBarrier(int parties) {
    this(parties, null);
}
```

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/f71af66b-0d54-4399-a44b-f47b58321984.png" width="300px"> </div><br>

```java
public class CyclicBarrierExample {

    public static void main(String[] args) {
        final int totalThread = 10;
        CyclicBarrier cyclicBarrier = new CyclicBarrier(totalThread);
        ExecutorService executorService = Executors.newCachedThreadPool();
        for (int i = 0; i < totalThread; i++) {
            executorService.execute(() -> {
                System.out.print("before..");
                try {
                    cyclicBarrier.await();
                } catch (InterruptedException | BrokenBarrierException e) {
                    e.printStackTrace();
                }
                System.out.print("after..");
            });
        }
        executorService.shutdown();
    }
}
```

```html
before..before..before..before..before..before..before..before..before..before..after..after..after..after..after..after..after..after..after..after..
```

### Semaphore

Semaphore 类似于操作系统中的信号量，可以控制对互斥资源的访问线程数。

以下代码模拟了对某个服务的并发请求，每次只能有 3 个客户端同时访问，请求总数为 10。

```java
public class SemaphoreExample {

    public static void main(String[] args) {
        final int clientCount = 3;
        final int totalRequestCount = 10;
        Semaphore semaphore = new Semaphore(clientCount);
        ExecutorService executorService = Executors.newCachedThreadPool();
        for (int i = 0; i < totalRequestCount; i++) {
            executorService.execute(()->{
                try {
                    semaphore.acquire();
                    System.out.print(semaphore.availablePermits() + " ");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release();
                }
            });
        }
        executorService.shutdown();
    }
}
```

```html
2 1 2 2 2 2 2 1 2 2
```

# 八、J.U.C - 其它组件

### FutureTask

在介绍 Callable 时我们知道它可以有返回值，返回值通过 Future\<V\> 进行封装。FutureTask 实现了 RunnableFuture 接口，该接口继承自 Runnable 和 Future\<V\> 接口，这使得 FutureTask 既可以当做一个任务执行，也可以有返回值。

```java
public class FutureTask<V> implements RunnableFuture<V>
```

```java
public interface RunnableFuture<V> extends Runnable, Future<V>
```

FutureTask 可用于异步获取执行结果或取消执行任务的场景。当一个计算任务需要执行很长时间，那么就可以用 FutureTask 来封装这个任务，主线程在完成自己的任务之后再去获取结果。

```java
public class FutureTaskExample {

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        FutureTask<Integer> futureTask = new FutureTask<Integer>(new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                int result = 0;
                for (int i = 0; i < 100; i++) {
                    Thread.sleep(10);
                    result += i;
                }
                return result;
            }
        });

        Thread computeThread = new Thread(futureTask);
        computeThread.start();

        Thread otherThread = new Thread(() -> {
            System.out.println("other task is running...");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        otherThread.start();
        System.out.println(futureTask.get());
    }
}
```

```html
other task is running...
4950
```

### BlockingQueue

java.util.concurrent.BlockingQueue 接口有以下阻塞队列的实现：

-   **FIFO 队列**  ：LinkedBlockingQueue、ArrayBlockingQueue（固定长度）
-   **优先级队列**  ：PriorityBlockingQueue

提供了阻塞的 take() 和 put() 方法：如果队列为空 take() 将阻塞，直到队列中有内容；如果队列为满 put() 将阻塞，直到队列有空闲位置。

**使用 BlockingQueue 实现生产者消费者问题**  

```java
public class ProducerConsumer {

    private static BlockingQueue<String> queue = new ArrayBlockingQueue<>(5);

    private static class Producer extends Thread {
        @Override
        public void run() {
            try {
                queue.put("product");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.print("produce..");
        }
    }

    private static class Consumer extends Thread {

        @Override
        public void run() {
            try {
                String product = queue.take();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.print("consume..");
        }
    }
}
```

```java
public static void main(String[] args) {
    for (int i = 0; i < 2; i++) {
        Producer producer = new Producer();
        producer.start();
    }
    for (int i = 0; i < 5; i++) {
        Consumer consumer = new Consumer();
        consumer.start();
    }
    for (int i = 0; i < 3; i++) {
        Producer producer = new Producer();
        producer.start();
    }
}
```

```html
produce..produce..consume..consume..produce..consume..produce..consume..produce..consume..
```

### ForkJoin

主要用于并行计算中，和 MapReduce 原理类似，都是把大的计算任务拆分成多个小任务并行计算。

```java
public class ForkJoinExample extends RecursiveTask<Integer> {

    private final int threshold = 5;
    private int first;
    private int last;

    public ForkJoinExample(int first, int last) {
        this.first = first;
        this.last = last;
    }

    @Override
    protected Integer compute() {
        int result = 0;
        if (last - first <= threshold) {
            // 任务足够小则直接计算
            for (int i = first; i <= last; i++) {
                result += i;
            }
        } else {
            // 拆分成小任务
            int middle = first + (last - first) / 2;
            ForkJoinExample leftTask = new ForkJoinExample(first, middle);
            ForkJoinExample rightTask = new ForkJoinExample(middle + 1, last);
            leftTask.fork();
            rightTask.fork();
            result = leftTask.join() + rightTask.join();
        }
        return result;
    }
}
```

```java
public static void main(String[] args) throws ExecutionException, InterruptedException {
    ForkJoinExample example = new ForkJoinExample(1, 10000);
    ForkJoinPool forkJoinPool = new ForkJoinPool();
    Future result = forkJoinPool.submit(example);
    System.out.println(result.get());
}
```

ForkJoin 使用 ForkJoinPool 来启动，它是一个特殊的线程池，线程数量取决于 CPU 核数。

```java
public class ForkJoinPool extends AbstractExecutorService
```

ForkJoinPool 实现了工作窃取算法来提高 CPU 的利用率。每个线程都维护了一个双端队列，用来存储需要执行的任务。工作窃取算法允许空闲的线程从其它线程的双端队列中窃取一个任务来执行。窃取的任务必须是最晚的任务，避免和队列所属线程发生竞争。例如下图中，Thread2 从 Thread1 的队列中拿出最晚的 Task1 任务，Thread1 会拿出 Task2 来执行，这样就避免发生竞争。但是如果队列中只有一个任务时还是会发生竞争。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/e42f188f-f4a9-4e6f-88fc-45f4682072fb.png" width="300px"> </div><br>

# 九、线程不安全示例

如果多个线程对同一个共享数据进行访问而不采取同步操作的话，那么操作的结果是不一致的。

以下代码演示了 1000 个线程同时对 cnt 执行自增操作，操作结束之后它的值有可能小于 1000。

```java
public class ThreadUnsafeExample {

    private int cnt = 0;

    public void add() {
        cnt++;
    }

    public int get() {
        return cnt;
    }
}
```

```java
public static void main(String[] args) throws InterruptedException {
    final int threadSize = 1000;
    ThreadUnsafeExample example = new ThreadUnsafeExample();
    final CountDownLatch countDownLatch = new CountDownLatch(threadSize);
    ExecutorService executorService = Executors.newCachedThreadPool();
    for (int i = 0; i < threadSize; i++) {
        executorService.execute(() -> {
            example.add();
            countDownLatch.countDown();
        });
    }
    countDownLatch.await();
    executorService.shutdown();
    System.out.println(example.get());
}
```

```html
997
```



# 十一、线程安全

简单记忆线程安全的集合类： **喂！SHE！  喂是指** **vector，S是指 stack，** **H是指**  **hashtable，E是指：Eenumeration**

多个线程不管以何种方式访问某个类，并且在主调代码中不需要进行同步，都能表现正确的行为。

线程安全有以下几种实现方式：

### 不可变

### 互斥同步

synchronized 和 ReentrantLock。

### 非阻塞同步

互斥同步最主要的问题就是线程阻塞和唤醒所带来的性能问题，因此这种同步也称为阻塞同步。

互斥同步属于一种悲观的并发策略，总是认为只要不去做正确的同步措施，那就肯定会出现问题。无论共享数据是否真的会出现竞争，它都要进行加锁（这里讨论的是概念模型，实际上虚拟机会优化掉很大一部分不必要的加锁）、用户态核心态转换、维护锁计数器和检查是否有被阻塞的线程需要唤醒等操作。

随着硬件指令集的发展，我们可以使用基于冲突检测的乐观并发策略：先进行操作，如果没有其它线程争用共享数据，那操作就成功了，否则采取补偿措施（不断地重试，直到成功为止）。这种乐观的并发策略的许多实现都不需要将线程阻塞，因此这种同步操作称为非阻塞同步。

#### 1. CAS

乐观锁需要操作和冲突检测这两个步骤具备原子性，这里就不能再使用互斥同步来保证了，只能靠硬件来完成。硬件支持的原子性操作最典型的是：比较并交换（Compare-and-Swap，CAS）。CAS 指令需要有 3 个操作数，分别是内存地址 V、旧的预期值 A 和新值 B。当执行操作时，只有当 V 的值等于 A，才将 V 的值更新为 B。

#### 2. AtomicInteger

#### 3. ABA

如果一个变量初次读取的时候是 A 值，它的值被改成了 B，后来又被改回为 A，那 CAS 操作就会误认为它从来没有被改变过。

J.U.C 包提供了一个带有标记的原子引用类 AtomicStampedReference 来解决这个问题，它可以通过控制变量值的版本来保证 CAS 的正确性。大部分情况下 ABA 问题不会影响程序并发的正确性，如果需要解决 ABA 问题，改用传统的互斥同步可能会比原子类更高效。

### 无同步方案

要保证线程安全，并不是一定就要进行同步。如果一个方法本来就不涉及共享数据，那它自然就无须任何同步措施去保证正确性。

#### 1. 栈封闭

多个线程访问同一个方法的局部变量时，不会出现线程安全问题，因为局部变量存储在虚拟机栈中，属于线程私有的。

```java
public class StackClosedExample {
    public void add100() {
        int cnt = 0;
        for (int i = 0; i < 100; i++) {
            cnt++;
        }
        System.out.println(cnt);
    }
}
```

```java
public static void main(String[] args) {
    StackClosedExample example = new StackClosedExample();
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> example.add100());
    executorService.execute(() -> example.add100());
    executorService.shutdown();
}
```

```html
100
100
```

#### 2. 线程本地存储（Thread Local Storage）

如果一段代码中所需要的数据必须与其他代码共享，那就看看这些共享数据的代码是否能保证在同一个线程中执行。如果能保证，我们就可以把共享数据的可见范围限制在同一个线程之内，这样，无须同步也能保证线程之间不出现数据争用的问题。

符合这种特点的应用并不少见，大部分使用消费队列的架构模式（如“生产者-消费者”模式）都会将产品的消费过程尽量在一个线程中消费完。其中最重要的一个应用实例就是经典 Web 交互模型中的“一个请求对应一个服务器线程”（Thread-per-Request）的处理方式，这种处理方式的广泛应用使得很多 Web 服务端应用都可以使用线程本地存储来解决线程安全问题。

可以使用 java.lang.ThreadLocal 类来实现线程本地存储功能。

对于以下代码，thread1 中设置 threadLocal 为 1，而 thread2 设置 threadLocal 为 2。过了一段时间之后，thread1 读取 threadLocal 依然是 1，不受 thread2 的影响。

```java
public class ThreadLocalExample {
    public static void main(String[] args) {
        ThreadLocal threadLocal = new ThreadLocal();
        Thread thread1 = new Thread(() -> {
            threadLocal.set(1);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(threadLocal.get());
            threadLocal.remove();
        });
        Thread thread2 = new Thread(() -> {
            threadLocal.set(2);
            threadLocal.remove();
        });
        thread1.start();
        thread2.start();
    }
}
```

```html
1
```

为了理解 ThreadLocal，先看以下代码：

```java
public class ThreadLocalExample1 {
    public static void main(String[] args) {
        ThreadLocal threadLocal1 = new ThreadLocal();
        ThreadLocal threadLocal2 = new ThreadLocal();
        Thread thread1 = new Thread(() -> {
            threadLocal1.set(1);
            threadLocal2.set(1);
        });
        Thread thread2 = new Thread(() -> {
            threadLocal1.set(2);
            threadLocal2.set(2);
        });
        thread1.start();
        thread2.start();
    }
}
```

它所对应的底层结构图为：

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/6782674c-1bfe-4879-af39-e9d722a95d39.png" width="500px"> </div><br>

每个 Thread 都有一个 ThreadLocal.ThreadLocalMap 对象。

```java
/* ThreadLocal values pertaining to this thread. This map is maintained
 * by the ThreadLocal class. */
ThreadLocal.ThreadLocalMap threadLocals = null;
```

当调用一个 ThreadLocal 的 set(T value) 方法时，先得到当前线程的 ThreadLocalMap 对象，然后将 ThreadLocal-\>value 键值对插入到该 Map 中。

```java
public void set(T value) {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null)
        map.set(this, value);
    else
        createMap(t, value);
}
```

get() 方法类似。

```java
public T get() {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        ThreadLocalMap.Entry e = map.getEntry(this);
        if (e != null) {
            @SuppressWarnings("unchecked")
            T result = (T)e.value;
            return result;
        }
    }
    return setInitialValue();
}
```

ThreadLocal 从理论上讲并不是用来解决多线程并发问题的，因为根本不存在多线程竞争。

在一些场景 (尤其是使用线程池) 下，由于 ThreadLocal.ThreadLocalMap 的底层数据结构导致 ThreadLocal 有内存泄漏的情况，应该尽可能在每次使用 ThreadLocal 后手动调用 remove()，以避免出现 ThreadLocal 经典的内存泄漏甚至是造成自身业务混乱的风险。

#### 3. 可重入代码（Reentrant Code）

这种代码也叫做纯代码（Pure Code），可以在代码执行的任何时刻中断它，转而去执行另外一段代码（包括递归调用它本身），而在控制权返回后，原来的程序不会出现任何错误。

可重入代码有一些共同的特征，例如不依赖存储在堆上的数据和公用的系统资源、用到的状态量都由参数中传入、不调用非可重入的方法等。

# 十二、锁优化

这里的锁优化主要是指 JVM 对 synchronized 的优化。

### 自旋锁

互斥同步进入阻塞状态的开销都很大，应该尽量避免。在许多应用中，共享数据的锁定状态只会持续很短的一段时间。自旋锁的思想是让一个线程在请求一个共享数据的锁时执行忙循环（自旋）一段时间，如果在这段时间内能获得锁，就可以避免进入阻塞状态。

自旋锁虽然能避免进入阻塞状态从而减少开销，但是它需要进行忙循环操作占用 CPU 时间，它只适用于共享数据的锁定状态很短的场景。

在 JDK 1.6 中引入了自适应的自旋锁。自适应意味着自旋的次数不再固定了，而是由前一次在同一个锁上的自旋次数及锁的拥有者的状态来决定。

### 锁消除

锁消除是指对于被检测出不可能存在竞争的共享数据的锁进行消除。

锁消除主要是通过逃逸分析来支持，如果堆上的共享数据不可能逃逸出去被其它线程访问到，那么就可以把它们当成私有数据对待，也就可以将它们的锁进行消除。

对于一些看起来没有加锁的代码，其实隐式的加了很多锁。例如下面的字符串拼接代码就隐式加了锁：

```java
public static String concatString(String s1, String s2, String s3) {
    return s1 + s2 + s3;
}
```

String 是一个不可变的类，编译器会对 String 的拼接自动优化。在 JDK 1.5 之前，会转化为 StringBuffer 对象的连续 append() 操作：

```java
public static String concatString(String s1, String s2, String s3) {
    StringBuffer sb = new StringBuffer();
    sb.append(s1);
    sb.append(s2);
    sb.append(s3);
    return sb.toString();
}
```

每个 append() 方法中都有一个同步块。虚拟机观察变量 sb，很快就会发现它的动态作用域被限制在 concatString() 方法内部。也就是说，sb 的所有引用永远不会逃逸到 concatString() 方法之外，其他线程无法访问到它，因此可以进行消除。

### 锁粗化

如果一系列的连续操作都对同一个对象反复加锁和解锁，频繁的加锁操作就会导致性能损耗。

上一节的示例代码中连续的 append() 方法就属于这类情况。如果虚拟机探测到由这样的一串零碎的操作都对同一个对象加锁，将会把加锁的范围扩展（粗化）到整个操作序列的外部。对于上一节的示例代码就是扩展到第一个 append() 操作之前直至最后一个 append() 操作之后，这样只需要加锁一次就可以了。

### 轻量级锁

JDK 1.6 引入了偏向锁和轻量级锁，从而让锁拥有了四个状态：无锁状态（unlocked）、偏向锁状态（biasble）、轻量级锁状态（lightweight locked）和重量级锁状态（inflated）。

以下是 HotSpot 虚拟机对象头的内存布局，这些数据被称为 Mark Word。其中 tag bits 对应了五个状态，这些状态在右侧的 state 表格中给出。除了 marked for gc 状态，其它四个状态已经在前面介绍过了。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/bb6a49be-00f2-4f27-a0ce-4ed764bc605c.png" width="500"/> </div><br>

下图左侧是一个线程的虚拟机栈，其中有一部分称为 Lock Record 的区域，这是在轻量级锁运行过程创建的，用于存放锁对象的 Mark Word。而右侧就是一个锁对象，包含了 Mark Word 和其它信息。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/051e436c-0e46-4c59-8f67-52d89d656182.png" width="500"/> </div><br>

轻量级锁是相对于传统的重量级锁而言，它使用 CAS 操作来避免重量级锁使用互斥量的开销。对于绝大部分的锁，在整个同步周期内都是不存在竞争的，因此也就不需要都使用互斥量进行同步，可以先采用 CAS 操作进行同步，如果 CAS 失败了再改用互斥量进行同步。

当尝试获取一个锁对象时，如果锁对象标记为 0 01，说明锁对象的锁未锁定（unlocked）状态。此时虚拟机在当前线程的虚拟机栈中创建 Lock Record，然后使用 CAS 操作将对象的 Mark Word 更新为 Lock Record 指针。如果 CAS 操作成功了，那么线程就获取了该对象上的锁，并且对象的 Mark Word 的锁标记变为 00，表示该对象处于轻量级锁状态。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/baaa681f-7c52-4198-a5ae-303b9386cf47.png" width="400"/> </div><br>

如果 CAS 操作失败了，虚拟机首先会检查对象的 Mark Word 是否指向当前线程的虚拟机栈，如果是的话说明当前线程已经拥有了这个锁对象，那就可以直接进入同步块继续执行，否则说明这个锁对象已经被其他线程线程抢占了。如果有两条以上的线程争用同一个锁，那轻量级锁就不再有效，要膨胀为重量级锁。

### 偏向锁

偏向锁的思想是偏向于让第一个获取锁对象的线程，这个线程在之后获取该锁就不再需要进行同步操作，甚至连 CAS 操作也不再需要。

当锁对象第一次被线程获得的时候，进入偏向状态，标记为 1 01。同时使用 CAS 操作将线程 ID 记录到 Mark Word 中，如果 CAS 操作成功，这个线程以后每次进入这个锁相关的同步块就不需要再进行任何同步操作。

当有另外一个线程去尝试获取这个锁对象时，偏向状态就宣告结束，此时撤销偏向（Revoke Bias）后恢复到未锁定状态或者轻量级锁状态。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/390c913b-5f31-444f-bbdb-2b88b688e7ce.jpg" width="600"/> </div><br>

# 十三、多线程开发良好的实践

- 给线程起个有意义的名字，这样可以方便找 Bug。

- 缩小同步范围，从而减少锁争用。例如对于 synchronized，应该尽量使用同步块而不是同步方法。

- 多用同步工具少用 wait() 和 notify()。首先，CountDownLatch, CyclicBarrier, Semaphore 和 Exchanger 这些同步类简化了编码操作，而用 wait() 和 notify() 很难实现复杂控制流；其次，这些同步类是由最好的企业编写和维护，在后续的 JDK 中还会不断优化和完善。

- 使用 BlockingQueue 实现生产者消费者问题。

- 多用并发集合少用同步集合，例如应该使用 ConcurrentHashMap 而不是 Hashtable。

- 使用本地变量和不可变类来保证线程安全。

- 使用线程池而不是直接创建线程，这是因为创建线程代价很高，线程池可以有效地利用有限的线程来启动任务。

## 参考资料

- BruceEckel. Java 编程思想: 第 4 版 [M]. 机械工业出版社, 2007.
- 周志明. 深入理解 Java 虚拟机 [M]. 机械工业出版社, 2011.
- [Threads and Locks](https://docs.oracle.com/javase/specs/jvms/se6/html/Threads.doc.html)
- [线程通信](http://ifeve.com/thread-signaling/#missed_signal)
- [Java 线程面试题 Top 50](http://www.importnew.com/12773.html)
- [BlockingQueue](http://tutorials.jenkov.com/java-util-concurrent/blockingqueue.html)
- [thread state java](https://stackoverflow.com/questions/11265289/thread-state-java)
- [CSC 456 Spring 2012/ch7 MN](http://wiki.expertiza.ncsu.edu/index.php/CSC_456_Spring_2012/ch7_MN)
- [Java - Understanding Happens-before relationship](https://www.logicbig.com/tutorials/core-java-tutorial/java-multi-threading/happens-before.html)
- [6장 Thread Synchronization](https://www.slideshare.net/novathinker/6-thread-synchronization)
- [How is Java's ThreadLocal implemented under the hood?](https://stackoverflow.com/questions/1202444/how-is-javas-threadlocal-implemented-under-the-hood/15653015)
- [Concurrent](https://sites.google.com/site/webdevelopart/21-compile/06-java/javase/concurrent?tmpl=%2Fsystem%2Fapp%2Ftemplates%2Fprint%2F&showPrintDialog=1)
- [JAVA FORK JOIN EXAMPLE](http://www.javacreed.com/java-fork-join-example/ "Java Fork Join Example")
- [聊聊并发（八）——Fork/Join 框架介绍](http://ifeve.com/talk-concurrency-forkjoin/)
- [Eliminating SynchronizationRelated Atomic Operations with Biased Locking and Bulk Rebiasing](http://www.oracle.com/technetwork/java/javase/tech/biasedlocking-oopsla2006-preso-150106.pdf)
