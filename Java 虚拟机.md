# Java 虚拟机
<!-- GFM-TOC -->
* [Java 虚拟机](#java-虚拟机)
    * [一、运行时数据区域](#一运行时数据区域)
        * [程序计数器](#程序计数器)
        * [Java 虚拟机栈](#java-虚拟机栈)
        * [本地方法栈](#本地方法栈)
        * [堆](#堆)
        * [方法区](#方法区)
        * [运行时常量池](#运行时常量池)
        * [直接内存](#直接内存)
    * [二、垃圾收集](#二垃圾收集)
        * [判断一个对象是否可被回收](#判断一个对象是否可被回收)
        * [引用类型](#引用类型)
        * [垃圾收集算法](#垃圾收集算法)
        * [垃圾收集器](#垃圾收集器)
    * [三、内存分配与回收策略](#三内存分配与回收策略)
        * [Minor GC 和 Full GC](#minor-gc-和-full-gc)
        * [内存分配策略](#内存分配策略)
        * [Full GC 的触发条件](#full-gc-的触发条件)
    * [四、类加载机制](#四类加载机制)
        * [类的生命周期](#类的生命周期)
        * [类加载过程](#类加载过程)
        * [类初始化时机](#类初始化时机)
        * [类与类加载器](#类与类加载器)
        * [类加载器分类](#类加载器分类)
        * [双亲委派模型](#双亲委派模型)
        * [自定义类加载器实现](#自定义类加载器实现)
    * [参考资料](#参考资料)
    <!-- GFM-TOC -->

本文大部分内容参考   **周志明《深入理解 Java 虚拟机》**  ，想要深入学习的话请看原书。

## 引言

![image-20210609090714160](C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210609090714160.png)

**定义：**

Java Virtual Machine - java程序的运行环境

**好处：**

- 一次编写，到处运行（）
- 内存字自处理，垃圾回收机制
- 数组越界检查
- 多态

**比较：![20200608150422](D:\桌面\学习资料\程序猿\image\20200608150422.png)**

- JVM
- JRE（JVM+基础类库（集合类，IO类，日期类））
- JDK（JVM+基础类库+编译工具）
- 开发JAVASE程序（JDK+IDE）

**JVM用处：**

- 底层实现原理
- 中高程序员进阶技能

# 运行时数据区域

同一进程内的

**线程共享** 1代码段 2数据段 3打开文件列表 

**线程私有** 1线程id 2寄存器 3工作栈

![第02章_JVM架构-英](D:\桌面\学习资料\程序猿\JVM上篇配图\第02章_JVM架构-英.jpg)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/5778d113-8e13-4c53-b5bf-801e58080b97.png" width="400px"> </div><br>

大多数 JVM 将内存区域划分为 

- **Method Area（Non-Heap）（方法区）** 
- **Heap（堆）** 
- **Program Counter Register（程序计数器）** 
-  **VM Stack（虚拟机栈，也有翻译成JAVA 方法栈的）**
- **Native Method Stack** （ 本地方法栈 ）

**其中Method Area和 Heap 是线程共享的 **

**VM Stack，Native Method Stack 和Program Counter Register 是非线程共享的。**

```
为什么分为 线程共享和非线程共享的呢?请继续往下看。
```

首先我们熟悉一下一个一般性的 Java 程序的工作过程。一个 Java 源程序文件，会被编译为字节码文件（以 class 为扩展名），每个java程序都需要运行在自己的JVM上，然后告知 JVM 程序的运行入口，再被 JVM 通过字节码解释器加载运行。那么程序开始运行后，都是如何涉及到各内存区域的呢？

1. 概括地说来，**JVM初始运行的时候都会分配好** **Method Area（方法区）** 和**Heap（堆）** 
2. 而JVM 每遇到一个线程，就为其分配一个 **Program Counter Register（程序计数器）** ,  **VM Stack（虚拟机栈）和Native Method Stack （本地方法栈）**
3.  当线程终止时，三者（虚拟机栈，本地方法栈和程序计数器）所占用的内存空间也会被释放掉。
4. 这也是为什么我把内存区域分为线程共享和非线程共享的原因，非线程共享的那三个区域的生命周期与所属线程相同，
5. 而线程共享的区域与JAVA程序运行的生命周期相同，所以这也是系统垃圾回收的场所只发生在线程共享的区域（实际上对大部分虚拟机来说知发生在Heap上）的原因。





同一进程内的

线程共享 1代码段 2数据段 3打开文件列表 

线程私有 1线程id 2寄存器 3工作栈

## 程序计数器

记录正在执行的虚拟机字节码指令的地址（如果正在执行的是本地方法则为空）。

**作用**：**记住下一条JVM（字节码）指令的执行地址**

**特点**：

- 线程**私有**：每个线程都有自己的程序计数器
- 不会存在内存溢出

## 虚拟机栈

### **概念**

线程**私有**

#### **栈**

![img](https://bkimg.cdn.bcebos.com/pic/a686c9177f3e67098c4504c03fc79f3df8dc5543?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U5Mg==,g_7,xp_5,yp_5/format,f_auto)

**栈** ：线程运行需要的内存空间，每个线程有多个栈帧；但每个线程只能有一个活动栈帧（栈顶的栈帧）；

#### 栈帧

**栈帧**：每个方法运行时需要的内存；栈帧用于存储局部变量表、操作数栈、常量池引用等信息

**程序运行流程：**

每次运行一个方法，都将方法（栈帧）压入栈，**方法结束后出栈，释放资源**；

### **栈内存**

- 垃圾回收**不涉及**栈内存
  方法执行完毕后，对应的栈帧会被自动弹出，**无需垃圾回收**

- 栈内存分配不是越大越好；
  
  资源浪费，不过是可以多几次递归调用；物理空间是一定的，栈内存过大，可用的线程数就会变少
  
- 方法内的局部变量安全么？

  主要是看这个局部变量，是否被其他线程共用？

  如果方法内局部变量没有逃离方法的作用范围，不会，局部变量是线程私有的，每个线程运行时会开辟一个新栈，**除非是static的，否则安全**；

  如果局部变量引用了对象并逃离了方法的作用域，则不安全，其他线程可以访问到这个变量。

每个 Java 方法在执行的同时会**创建一个栈帧用于存储局部变量表、操作数栈、常量池引用等信息**。从方法调用直至执行完成的过程，对应着一个栈帧在 Java 虚拟机栈中入栈和出栈的过程。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/8442519f-0b4d-48f4-8229-56f984363c69.png" width="400px"> </div><br>

### **栈溢出**

- 栈帧过多（递归调用，没有设置终止条件）
- 栈帧过大，这种情况比较少

可以通过 -Xss 这个虚拟机参数来指定每个线程的 Java 虚拟机栈内存大小，在 JDK 1.4 中默认为 256K，而在 JDK 1.5+ 默认为 1M：

```java
java -Xss2M HackTheJava
```

该区域可能抛出以下异常：

- 当线程请求的栈深度超过最大值，会抛出 StackOverflowError 异常；
- 栈进行动态扩展时如果无法申请到足够内存，会抛出 OutOfMemoryError 异常。

### 线程运行诊断

案例一：cpu占用过高

```Linux
top//定位哪个进程对CPU占用过高
ps H -eo pid,tid,%cpu|grep 进程id //进一步定位线程占用， -eo：感兴趣区域：显示进程PID，线程和cpu占用率，H：打印线程数
jstack 进程id
	根据线程id找到有问题的线程，进一步定位到问题代码的源码行号
```

案例二：线程死锁

```
jstack 进程id
	根据最后的提醒找到死锁
```



## 本地方法栈

**私有**

本地方法栈与 Java 虚拟机栈类似，它们之间的区别只不过是**本地方法栈为本地方法服务。**

**本地方法一般是用其它语言（C、C++ 或汇编语言等）编写的**，并且被编译为基于本机硬件和操作系统的程序，对待这些方法需要特别处理。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/66a6899d-c6b0-4a47-8569-9d08f0baf86c.png" width="300px"> </div><br>



## Heap 堆

#### 1.4.1**特点：**

- 通过new关键字，创建的对象都会使用堆内存
- 线程**共享**，要考虑线程安全问题
- 有垃圾回收机制

所有对象都在这里分配内存，是**垃圾收集的主要区域**（"GC 堆"）。

现代的垃圾收集器基本都是采用分代收集算法，其主要的思想是针对不同类型的对象采取不同的垃圾回收算法。可以将堆分成两块：

- 新生代（Young Generation）
- 老年代（Old Generation）

堆不需要连续内存，并且可以动态增加其内存，增加失败会抛出 OutOfMemoryError 异常。

**可以通过 -Xms 和 -Xmx 这两个虚拟机参数来指定一个程序的堆内存大小**，第一个参数设置初始值，第二个参数设置最大值。

```java
java -Xms1M -Xmx2M HackTheJava
```

#### 1.4.2 堆内存溢出

OutofMemoryError

比如new一个对象，无限增加这个对象的数据量

#### 1.4.2**堆内存诊断**

1. jps 工具
   查看当前系统中有哪些 java 进程

2. jmap 工具
   查看堆内存占用情况： jmap - heap 进程id

3. jconsole 工具
   图形界面的，多功能的监测工具，可以连续监测

4. jvisualvm 工具

   可视化的方式展示虚拟机的内容

## 方法区

### 定义

类似于进程地址空间的 代码段和数据段

Java 虚拟机有一个在所有 **Java 虚拟机线程之间   共享   的方法区域**。

方法区域类似于用于传统语言的编译代码的存储区域，或者类似于操作系统进程中的“文本”段。

**它存储每个类的结构**，例如

1. **运行时常量池**
2. **字段**
3. **方法数据，方法和构造函数的代码，包括特殊方法，用于类和实例初始化以及接口初始化**。

### 特点

- 方法区域是在虚拟机启动时创建的。
- 尽管**方法区域在逻辑上是堆的一部分**，**但简单的实现可能不会选择垃圾收集或压缩它**。**此规范不强制指定方法区的位置或用于管理已编译代码的策略**。
- 方法区域可以具有固定的大小，或者可以根据计算的需要进行扩展，并且如果不需要更大的方法区域，则可以收缩。方法区域的内存不需要是连续的！

- 用于存**放已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。**


- 和堆一样不需要连续的内存，并且可以动态扩展，动态扩展失败一样会抛出 OutOfMemoryError 异常。


- 对这块区域进行垃圾回收的主要目标是对常量池的回收和对类的卸载，但是一般比较难实现。

### 变化

**1.6版本用永久代实现方法区**；HotSpot 虚拟机把它当成永久代来进行垃圾回收。但很难确定永久代的大小，因为它受到很多因素影响，并且每次 Full GC 之后永久代的大小都会改变，所以经常会抛出 OutOfMemoryError 异常。

为了更容易管理方法区，从 **JDK 1.8 开始，移除永久代，并把方法区移至元空间，它位于本地内存中，而不是虚拟机内存中**。

![img](https://img-blog.csdnimg.cn/20210208112903305.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

方法区是一个 JVM 规范，永久代与元空间都是其一种实现方式。在 JDK 1.8 之后，原来永久代的数据被分到了堆和元空间中。**元空间存储类的元信息，静态变量和常量池等放入堆中。**

### 方法区内存溢出

- 1.8 之前会导致永久代内存溢出

  - 使用 -XX:MaxPermSize=8m 指定永久代内存大小

- 1.8 之后会导致元空间内存溢出

  - 使用 -XX:MaxMetaspaceSize=8m 指定元空间大小

  ![image-20210609095749733](C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210609095749733.png)

**场景**：

- spring
- mybatis

### 运行时常量池

运行程序时，先要编译成二进制字节码【类的基本信息（版本、父类、接口等），常量池（**编译器生成的字面量和符号引用**），类方法定义（包括虚拟机指令）】

首先看看常量池是什么，编译如下代码：

```java
public class Test {

    public static void main(String[] args) {
        System.out.println("Hello World!");
    }

}
```

然后使用 javap -v Test.class 命令反编译查看结果。

#### **常量池**

就是一张表，虚拟机指令根据这张常量表找到要执行的**类名、方法名、参数类型、字面量信息**

![img](https://img-blog.csdnimg.cn/20210208124448238.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

**每条指令都会对应常量池表中一个地址，常量池表中的地址可能对应着一个类名、方法名、参数类型等信息。**

![img](https://img-blog.csdnimg.cn/20210208124525875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

#### **运行时常量池**

- **运行时常量池是方法区的一部分**。常量池是 *.class 文件中的，**当该类被加载到虚拟机以后，它的常量池信息就会放入运行时常量池**，并把里面的符号地址变为真实地址
- Class 文件中的常量池（编译器生成的字面量和符号引用）会在类加载后被放入这个区域。

- 除了在编译期生成的常量，还允许动态生成，例如 String 类的 intern()。


### StringTable

#### **特征**：

- 常量池中的字符串仅是符号，只有在被用到时才会转化为对象 
- 利用串池的机制，来避免重复创建字符串对象 
- **字符串 变量 拼接的原理**是StringBuilder 
- **字符串 常量 拼接的原理**是编译器优化 
- 可以**使用intern方法，主动将串池中还没有的字符串对象放入串池中** 
- 注意：无论是串池还是堆里面的字符串，都是对象

案例一：

```java
public class StringTableStudy { 
    public static void main(String[] args) { 	
    String a = "a";  	
    String b = "b"; 	
    String ab = "ab"; } }
```

常量池中的信息，**都会被加载到运行时常量池中**，但这是a b ab 仅是常量池中的符号，**还没有成为java字符串**；

```
0: ldc           #2                  // String a
2: astore_1
3: ldc           #3                  // String b
5: astore_2
6: ldc           #4                  // String ab
8: astore_3
9: return
```

1. 当执行到 ldc #2 时，会把符号 a 变为 “a” 字符串对象，**并放入串池中**（hashtable结构 不可扩容）
2. 当执行到 ldc #3 时，会把符号 b 变为 “b” 字符串对象，并放入串池中
3. 当执行到 ldc #4 时，会把符号 ab 变为 “ab” 字符串对象，并放入串池中

最终**StringTable [“a”, “b”, “ab”]**

**注意**：字符串对象的创建都是**懒惰的**，只有当运行到那一行字符串且在串池中不存在的时候（如 ldc #2）时，该字符串才会被创建并放入**串池**中。



案例二：使用拼接**字符串变量对象**创建字符串的过程

```java
public class StringTableStudy {
	public static void main(String[] args) {
		String a = "a";
		String b = "b";
		String ab = "ab";
		//拼接字符串对象来创建新的字符串
		String ab2 = a+b; 
	}
}
```

```
	 Code:
      stack=2, locals=5, args_size=1
         0: ldc           #2                  // String a
         2: astore_1
         3: ldc           #3                  // String b
         5: astore_2
         6: ldc           #4                  // String ab
         8: astore_3
         9: new           #5                  // class java/lang/StringBuilder
        12: dup
        13: invokespecial #6                  // Method java/lang/StringBuilder."<init>":()V
        16: aload_1
        17: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String
;)Ljava/lang/StringBuilder;
        20: aload_2
        21: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String
;)Ljava/lang/StringBuilder;
        24: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/Str
ing;
        27: astore        4
        29: return
```

通过拼接的方式来创建字符串的**过程**是：StringBuilder().append(“a”).append(“b”).toString()

最后的toString方法的返回值是一个**新的字符串**，虽然字符串的**值**和拼接的字符串一致，**但是两个不同的字符串**，**一个存在于串池之中，一个存在于堆内存之中**

```java
String ab = "ab";
String ab2 = a+b;
//结果为false,因为ab是存在于串池之中，ab2是由StringBuffer的toString方法所返回的一个对象，存在于堆内存之中
System.out.println(ab == ab2);
```



案例三：使用**拼接字符串常量对象**的方法创建字符串

```java
public class StringTableStudy {
	public static void main(String[] args) {
		String a = "a";
		String b = "b";
		String ab = "ab";
		String ab2 = a+b;
		//使用拼接字符串的方法创建字符串
		String ab3 = "a" + "b";
	}
}
```

```
 	  Code:
      stack=2, locals=6, args_size=1
         0: ldc           #2                  // String a
         2: astore_1
         3: ldc           #3                  // String b
         5: astore_2
         6: ldc           #4                  // String ab
         8: astore_3
         9: new           #5                  // class java/lang/StringBuilder
        12: dup
        13: invokespecial #6                  // Method java/lang/StringBuilder."<init>":()V
        16: aload_1
        17: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String
;)Ljava/lang/StringBuilder;
        20: aload_2
        21: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String
;)Ljava/lang/StringBuilder;
        24: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/Str
ing;
        27: astore        4
        //ab3初始化时直接从串池中获取字符串
        29: ldc           #4                  // String ab
        31: astore        5
        33: return
```

- 使用**拼接字符串常量**的方法来创建新的字符串时，因为**内容是常量，javac在编译期会进行优化，结果已在编译期确定为ab**，而创建ab的时候已经在串池中放入了“ab”，所以ab3直接从串池中获取值，所以进行的操作和 ab = “ab” 一致。
- 使用**拼接字符串变量**的方法来创建新的字符串时，因为内容是变量，只能**在运行期确定它的值，所以需要使用StringBuilder来创建**（编译器的优化）

```
下面这条语句一共创建了多少个对象：String s="welcome"+"to"+360;

由于编译器优化，最后只创建一个字符对象
```

案例四：**直接new创建字符串 在堆内存中**

#### intern方法 1.8

调用字符串对象的intern方法，会将该字符串对象尝试放入到串池中

- 如果串池中没有该字符串对象，则放入成功
- 如果有该字符串对象，则放入失败

无论放入是否成功，都会返回**串池中**的字符串对象

**注意**：此时如果调用intern方法成功，堆内存与串池中的字符串对象是同一个对象；如果失败，则不是同一个对象；

例一：

```java
//调用intern方法成功
public class Main {
	public static void main(String[] args) {
		//"a" "b" 被放入串池中，str则存在于堆内存之中
		String str = new String("a") + new String("b");
		//调用str的intern方法，这时串池中没有"ab"，则会将该字符串对象放入到串池中，此时堆内存与串池中的"ab"是同一个对象
		String st2 = str.intern();
		//给str3赋值，因为此时串池中已有"ab"，则直接将串池中的内容返回
		String str3 = "ab";
		//因为堆内存与串池中的"ab"是同一个对象，所以以下两条语句打印的都为true
		System.out.println(str == st2);
		System.out.println(str == str3);
	}
}
```

例二

```java
//调用intern方法失败
public class Main {
	public static void main(String[] args) {
        //此处创建字符串对象"ab"，因为串池中还没有"ab"，所以将其放入串池中
		String str3 = "ab";
        //"a" "b" 被放入串池中，str则存在于堆内存之中
		String str = new String("a") + new String("b");
        //此时因为在创建str3时，"ab"已存在与串池中，所以放入失败，但是会返回串池中的"ab"
		String str2 = str.intern();
        //false
		System.out.println(str == str2);
        //false
		System.out.println(str == str3);
        //true
		System.out.println(str2 == str3);
	}
}
```

#### intern方法 1.6

调用字符串对象的intern方法，会将该字符串对象尝试放入到串池中

- 如果串池中没有该字符串对象，会将该字符串对象复制一份，再放入到串池中
- 如果有该字符串对象，则放入失败

无论放入是否成功，都会返回**串池中**的字符串对象

**注意**：此时无论调用intern方法成功与否，串池中的字符串对象和堆内存中的字符串对象**都不是同一个对象**

**面试题**

![image-20210609152519891](C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210609152519891.png)

#### StringTable的位置

- jdk1.6 StringTable 位置是在永久代中
- 1.7 1.8 StringTable 位置是在堆中

**原因**：String常量经常使用，而永久代的垃圾回收效率不够高；堆的垃圾回收效率高；

![20200608150547](D:\桌面\学习资料\程序猿\image\20200608150547.png)



####  StringTable 垃圾回收

StringTable在内存紧张时，会发生垃圾回收

-Xmx10m 指定堆内存大小
-XX:+PrintStringTableStatistics 打印字符串常量池信息
-XX:+PrintGCDetails
-verbose:gc 打印 gc 的次数，耗费时间等信息

```java
/**
 * 演示 StringTable 垃圾回收
 * -Xmx10m -XX:+PrintStringTableStatistics -XX:+PrintGCDetails -verbose:gc
 */
public class Code_05_StringTableTest {

    public static void main(String[] args) {
        int i = 0;
        try {
            for(int j = 0; j < 10000; j++) { // j = 100, j = 10000
                String.valueOf(j).intern();
                i++;
            }
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            System.out.println(i);
        }
    }

}
```

#### StringTable 性能调优

因为StringTable是由HashTable实现的，所以**可以适当增加HashTable桶的个数**，减少冲突，来减少字符串放入串池所需要的时间
-XX:StringTableSize=桶个数（最少设置为 1009 以上）
考虑是否需要将字符串对象入池
可以通过 intern 方法减少重复入池

**哈希表（散列表）**：

- 概念：快速查找对象的位置

- 散列查找法的两个基本工作：
  - 计算位置：构造散列函数确定关键词存储位置
  - 解决冲突：应用某种策略解决多个关键词位置相同的问题
- 时间复杂度几乎为常量

**散列函数的构造方法：**

- 数字关键字：
  - 直接定址法：线性函数
  - 取余法：除数求余法
  - 数值分析法：分析数字关键字在各位上的变化情况，取比较随机的位作为散列地址
  - 折叠发法：把关键词分割成位数相同的几部分，然后折叠

**为什么用StringTable**：

- StringTable 中没有重复的string，大大减少了内存占用
- 可以通过将堆内存的String，**放入池中（intern），减少字符串对象的个数**，减少内存的使用；

## 直接内存

#### 1.6.1 特点

- 属于操作系统，常见于NIO操作时，**用于数据缓冲区**
- 分配回收成本较高，但读写性能高
- 不受JVM内存回收管理

**文件读写流程**

系统内存 java无法直接访问，需要先读入java堆内存；这种方式文件读写两次，耗时

![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150715.png)

**使用了DirectBuffer**

直接内存是操作系统和Java代码**都可以访问的一块区域**，无需将代码从系统内存复制到Java堆内存，从而提高了效率

#### 释放原理

![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150736.png)

在 JDK 1.4 中新引入了 NIO 类，它可以使用 Native 函数库直接分配堆外内存，然后通过 Java 堆里的 DirectByteBuffer 对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，**因为避免了在堆内存和堆外内存来回拷贝数据。**

#### 1.6.2 直接内存溢出和释放原理

直接内存的回收**不是**通过JVM的垃圾回收来释放的，而是通过**unsafe.freeMemory**来手动释放

```
//通过ByteBuffer申请1M的直接内存
ByteBuffer byteBuffer = ByteBuffer.allocateDirect(_1M);
```

申请直接内存，但JVM并不能回收直接内存中的内容，它是如何实现回收的呢？

**allocateDirect的实现**

```java
public static ByteBuffer allocateDirect(int capacity) {
    return new DirectByteBuffer(capacity);
}
```

DirectByteBuffer类

```java
DirectByteBuffer(int cap) {   // package-private
   
    super(-1, 0, cap, cap);
    boolean pa = VM.isDirectMemoryPageAligned();
    int ps = Bits.pageSize();
    long size = Math.max(1L, (long)cap + (pa ? ps : 0));
    Bits.reserveMemory(size, cap);

    long base = 0;
    try {
        base = unsafe.allocateMemory(size); //申请内存
    } catch (OutOfMemoryError x) {
        Bits.unreserveMemory(size, cap);
        throw x;
    }
    unsafe.setMemory(base, size, (byte) 0);
    if (pa && (base % ps != 0)) {
        // Round up to page boundary
        address = base + ps - (base & (ps - 1));
    } else {
        address = base;
    }
    cleaner = Cleaner.create(this, new Deallocator(base, size, cap)); //通过虚引用，来实现直接内存的释放，this为虚引用的实际对象
    att = null;
}Copy
```

这里调用了一个Cleaner的create方法，且后台线程还会对虚引用的对象监测，**如果虚引用的实际对象（这里是DirectByteBuffer）被回收以后，就会调用Cleaner的clean方法，来清除直接内存中占用的内存**

```java
public void clean() {
       if (remove(this)) {
           try {
               this.thunk.run(); //调用run方法
           } catch (final Throwable var2) {
               AccessController.doPrivileged(new PrivilegedAction<Void>() {
                   public Void run() {
                       if (System.err != null) {
                           (new Error("Cleaner terminated abnormally", var2)).printStackTrace();
                       }

                       System.exit(1);
                       return null;
                   }
               });
           }Copy
```

对应对象的run方法

```java
public void run() {
    if (address == 0) {
        // Paranoia
        return;
    }
    unsafe.freeMemory(address); //释放直接内存中占用的内存
    address = 0;
    Bits.unreserveMemory(size, capacity);
}Copy
```

##### 直接内存的回收机制总结

- 使用了Unsafe类来完成直接内存的分配回收，回收需要主动调用freeMemory方法
- ByteBuffer的实现内部使用了Cleaner（虚引用）来检测ByteBuffer。**一旦ByteBuffer被垃圾回收，那么会由ReferenceHandler来调用Cleaner的clean方法调用freeMemory来释放内存**



# 垃圾回收

垃圾收集主要是针对**堆和方法区**进行。

**程序计数器、虚拟机栈和本地方法栈**这三个区域属于线程私有的，只存在于线程的生命周期内，线程结束之后就会消失    

*方法调用时，会创建栈帧在栈中，调用完是程序自动出栈释放，而不是gc释放*

因此**不需要对这三个区域进行垃圾回收**。



**1、在java中，对象的内存在哪个时刻回收，取决于垃圾回收器何时运行。**

**2、**一旦垃圾回收器准备好释放对象占用的存储空间，将首先调用其finalize()方法， 并且在**下一次**垃圾回收动作发生时，才会**真正的**回收对象占用的内存（《java 编程思想》）

### 2.1判断一个对象是否可被回收

在java语言中，判断一块内存空间是否符合垃圾收集器收集标准的标准只有两个：

1.给对象赋值为null，以下没有调用过。

2.给对象赋了新的值，重新分配了内存空间。

#### 引用计数算法

为对象添加一个引用计数器，当对象增加一个引用时计数器加 1，引用失效时计数器减 1。引用计数为 0 的对象可被回收。

**在两个对象出现循环引用的情况下**，此时引用计数器永远不为 0，导致无法对它们进行回收。正是因为循环引用的存在，因此 **Java 虚拟机不使用引用计数算法。**

![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150750.png)

```java
public class Test {

    public Object instance = null;

    public static void main(String[] args) {
        Test a = new Test();
        Test b = new Test();
        a.instance = b;
        b.instance = a;
        a = null;
        b = null;
        doSomething();
    }
}
```

在上述代码中，a 与 b 引用的对象实例互相持有了对象的引用，因此当我们把对 a 对象与 b 对象的引用去除之后，由于两个对象还存在互相之间的引用，导致两个 Test 对象无法被回收。

#### 可达性分析算法

以 **GC Roots 为起始点进行搜索，可达的对象都是存活的，不可达的对象可被回收。**

Java 虚拟机使用该算法来判断对象是否可被回收，**GC Roots 一般包含以下内容**：

- 虚拟机栈（方法**栈帧中的本地变量表**）中引用的对象。
- 方法区中**类静态属性**引用的对象
- 方法区中**常量**引用的对象
- 本地方法栈中 JNI（即一般说的**Native方法**）引用的对象

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/83d909d2-3858-4fe1-8ff4-16471db0b180.png" width="350px"> </div><br>


#### 方法区的回收

因为方法区主要存放永久代对象，而永久代对象的回收率比新生代低很多，所以在方法区上进行回收性价比不高。

主要是对常量池的回收和对类的卸载。

为了避免内存溢出，在大量使用反射和动态代理的场景都需要虚拟机具备类卸载功能。

类的卸载条件很多，需要满足以下三个条件，并且满足了条件也不一定会被卸载：

- 该类所有的实例都已经被回收，此时堆中不存在该类的任何实例。
- 加载该类的 ClassLoader 已经被回收。
- 该类对应的 Class 对象没有在任何地方被引用，也就无法在任何地方通过反射访问该类方法。

#### finalize()

是垃圾回收器操作的运行机制中的一部分，**进行垃圾回收器操作时会调用finalize方法**，因为finalize方法是object的方法，所以每个类都有这个方法并且可以重写这个方法，在这个方法里实现释放系统资源及其他清理工作，**JVM不保证此方法总被调用。**

类似 C++ 的析构函数，用于关闭外部资源。但是 try-finally 等方式可以做得更好，并且该方法运行代价很高，不确定性大，无法保证各个对象的调用顺序，因此最好不要使用。

当一个对象**可被回收时**，如果需要执行该对象的 finalize() 方法，那么**就有可能在该方法中让对象重新被引用，从而实现自救**。自救只能进行一次，如果回收的对象之前调用了 finalize() 方法自救，后面回收时不会再调用该方法。

### 2.2引用类型

无论是通过引用计数算法判断对象的引用数量，还是通过可达性分析算法判断对象是否可达，判定对象是否可被回收都与引用有关。

Java 提供了四种强度不同的引用类型。

![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150800.png)

#### 1. 强引用

被强引用关联的对象A1不会被回收。只有GC Root**都不引用**该对象时，才会回收**强引用**对象

- 如上图B、C对象都不引用A1对象时，A1对象才会被回收

使用 new 一个新对象的方式来创建强引用。

```java
Object obj = new Object();
```

#### 2. 软引用

被软引用关联的对象，发生垃圾回收，**只有在内存不够的情况下**才会被回收。

使用 SoftReference 类来创建软引用。

```java
/**
 * 演示 软引用
 * -Xmx20m -XX:+PrintGCDetails -verbose:gc
 */
public class Code_08_SoftReferenceTest {

    public static int _4MB = 4 * 1024 * 1024;

    public static void main(String[] args) throws IOException {
        method2();
    }

    // 设置 -Xmx20m , 演示堆内存不足,
    public static void method1() throws IOException {
        ArrayList<byte[]> list = new ArrayList<>();

        for(int i = 0; i < 5; i++) {
            list.add(new byte[_4MB]);
        }
        System.in.read();
    }

    // 演示 软引用
    public static void method2() throws IOException {
        ArrayList<SoftReference<byte[]>> list = new ArrayList<>();
        for(int i = 0; i < 5; i++) {
            //使用软引用对象 list和SoftReference是强引用，而SoftReference和byte数组则是软引用
            SoftReference<byte[]> ref = new SoftReference<>(new byte[_4MB]);
            System.out.println(ref.get());
            list.add(ref);
            System.out.println(list.size());
        }
        System.out.println("循环结束：" + list.size());
        for(SoftReference<byte[]> ref : list) {
            System.out.println(ref.get());
        }
    }
}


```

method1 方法解析：
首先会设置一个堆内存的大小为 20m，然后运行 mehtod1 方法，会抛异常，堆内存不足，因为 mehtod1 中的 **list 都是强引用**。

![img](https://img-blog.csdnimg.cn/20210209125537878.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

method2 方法解析：
在 list 集合中存放了 软引用对象，当内存不足时，会触发 full gc，将软引用的对象回收。细节如图：

![img](https://img-blog.csdnimg.cn/20210209130334776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

上面的代码中，当软引用引用的对象被回收了，但是软引用（null占内存空间）还存在，所以，一般软引用需要搭配一个引用队列一起使用。
修改 method2 如下：如果想要**清理软引用**，需要使**用引用队列**

```java
// 演示 软引用 搭配引用队列
    public static void method3() throws IOException {
        ArrayList<SoftReference<byte[]>> list = new ArrayList<>();
        // 引用队列
        ReferenceQueue<byte[]> queue = new ReferenceQueue<>();

        for(int i = 0; i < 5; i++) {
            // 关联了引用队列，当软引用所关联的 byte[] 被回收时，软引用自己会加入到 queue 中去
            SoftReference<byte[]> ref = new SoftReference<>(new byte[_4MB], queue);
            System.out.println(ref.get());
            list.add(ref);
            System.out.println(list.size());
        }

        // 从队列中获取无用的 软引用对象，并移除
        Reference<? extends byte[]> poll = queue.poll();
        while(poll != null) {
            list.remove(poll);
            poll = queue.poll();
        }

        System.out.println("=====================");
        for(SoftReference<byte[]> ref : list) {
            System.out.println(ref.get());
        }
    }

```

最后只有一个byte，其他的null（软引用对象）没有了；

![img](https://img-blog.csdnimg.cn/20210209140627985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

**大概思路为：**查看引用队列中有无软引用，如果有，则将该软引用从存放它的集合中移除（这里为一个list集合）

#### 3. 弱引用

被弱引用关联的对象，**只要发生垃圾回收（Full GC时会全部回收），一定会被回收**，也就是说它只能存活到下一次垃圾回收发生之前。

使用 WeakReference 类来创建弱引用。

```java
public class Code_09_WeakReferenceTest {

    public static void main(String[] args) {
//        method1();
        method2();
    }

    public static int _4MB = 4 * 1024 *1024;

    // 演示 弱引用
    public static void method1() {
        List<WeakReference<byte[]>> list = new ArrayList<>();
        for(int i = 0; i < 10; i++) {
            WeakReference<byte[]> weakReference = new WeakReference<>(new byte[_4MB]);
            list.add(weakReference);

            for(WeakReference<byte[]> wake : list) {
                System.out.print(wake.get() + ",");
            }
            System.out.println();
        }
    }

    // 演示 弱引用搭配 引用队列
    public static void method2() {
        List<WeakReference<byte[]>> list = new ArrayList<>();
        ReferenceQueue<byte[]> queue = new ReferenceQueue<>();

        for(int i = 0; i < 9; i++) {
            WeakReference<byte[]> weakReference = new WeakReference<>(new byte[_4MB], queue);
            list.add(weakReference);
            for(WeakReference<byte[]> wake : list) {
                System.out.print(wake.get() + ",");
            }
            System.out.println();
        }
        System.out.println("===========================================");
        Reference<? extends byte[]> poll = queue.poll();
        while (poll != null) {
            list.remove(poll);
            poll = queue.poll();
        }
        for(WeakReference<byte[]> wake : list) {
            System.out.print(wake.get() + ",");
        }
    }

}

```

#### 4. 虚引用

**必须配合引用队列使用**，主要配合 ByteBuffer 使用，被引用对象回收时，会将虚引用入队，
由 Reference Handler 线程调用虚引用相关方法释放直接内存

当虚引用对象所引用的对象被回收以后，**虚引用对象就会被放入引用队列中，调用虚引用的方法**

- 虚引用的一个体现是**释放直接内存所分配的内存**，当引用的对象ByteBuffer被垃圾回收以后，**虚引用对象Cleaner就会被放入引用队列中**，然后调用Cleaner的clean方法来释放直接内存
- 如上图，B对象不再引用ByteBuffer对象，ByteBuffer就会被回收。但是直接内存中的内存还未被回收。这时需要将虚引用对象Cleaner放入引用队列中，然后调用它的clean方法来释放直接内存。

使用 PhantomReference 来创建虚引用。

```java
Object obj = new Object();
PhantomReference<Object> pf = new PhantomReference<Object>(obj, null);
obj = null;
```

#### 5. 终结器引用

所有的类都继承自Object类，Object类有一个finalize方法。当某个对象不再被其他的对象所引用时，会**先**将终结器引用对象放入引用队列中，**然后**根据终结器引用对象找到它所引用的对象，**然后调用该对象的finalize方法。**调用以后，该对象就可以被垃圾回收了。

无需手动编码，但其内部配合引用队列使用，在垃圾回收时，终结器引用入队（被引用对象暂时没有被回收），再由 Finalizer 线程通过终结器引用找到被引用对象并调用它的 finalize 方法，第二次 GC 时才能回收被引用对象。

#### 6.引用队列

- 软引用和弱引用**可以配合**引用队列
  - 在**弱引用**和**虚引用**所引用的对象被回收以后，会将这些引用放入引用队列中，方便一起回收这些软/弱引用对象
- 虚引用和终结器引用**必须配合**引用队列
  - 虚引用和终结器引用在使用时会关联一个引用队列

### 2.3 垃圾收集算法

#### 1. 标记 - 清除

![img](https://img-blog.csdnimg.cn/20210209142921425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/005b481b-502b-4e3f-985d-d043c2b330aa.png" width="400px"> </div><br>

在标记阶段，程序会检查每个对象是否为活动对象，如果是活动对象，则程序会在对象头部打上标记。

**在清除阶段，会进行对象回收并取消标志位**，另外，还会判断回收后的分块与前一个空闲分块是否连续，若连续，会合并这两个分块。回收对象就是把对象作为分块，连接到被称为 “空闲链表” 的单向链表，之后进行分配时只需要遍历这个空闲链表，就可以找到分块。

在分配时，程序会搜索空闲链表寻找空间大于等于新对象大小 size 的块 block。如果它找到的块等于 size，会直接返回这个分块；如果找到的块大于 size，会将块分割成大小为 size 与 (block - size) 的两部分，返回大小为 size 的分块，并把大小为 (block - size) 的块返回给空闲链表。

**不足：**

- 标记和清除过程效率都不高；
- **会产生大量不连续的内存碎片，导致无法给大对象分配内存。**

#### 2. 标记 - 整理

![img](https://img-blog.csdnimg.cn/20210209143504936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/ccd773a5-ad38-4022-895c-7ac318f31437.png" width="400px"> </div><br>

让所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存。

优点:

- 不会产生内存碎片

不足:

- **需要移动大量对象，处理效率比较低。**

#### 3. 复制

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/b2b77b9e-958c-4016-8ae5-9c6edd83871e.png" width="400px"> </div><br>

![img](https://img-blog.csdnimg.cn/20210209144026784.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

将内存划分为大小相等的两块，每次只使用其中一块，当这一块内存用完了就将还存活的对象复制到另一块上面，然后再把使用过的内存空间进行一次清理。

主要不足是只使用了内存的一半。

现在的商业虚拟机都采用这种收集算法回收新生代，但是并不是划分为大小相等的两块，而是一块较大的 Eden 空间和两块较小的 Survivor 空间，每次使用 Eden 和其中一块 Survivor。在回收时，将 Eden 和 Survivor 中还存活着的对象全部复制到另一块 Survivor 上，最后清理 Eden 和使用过的那一块 Survivor。

HotSpot 虚拟机的 Eden 和 Survivor 大小比例默认为 8:1，保证了内存的利用率达到 90%。如果每次回收有多于 10% 的对象存活，那么一块 Survivor 就不够用了，此时需要依赖于老年代进行空间分配担保，也就是借用老年代的空间存储放不下的对象。

- 不会有内存碎片
- 需要占用两倍内存空间

#### 4. 分代收集

![img](https://img-blog.csdnimg.cn/20210209161407621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

现在的商业虚拟机采用分代收集算法，它根据对象存活周期将内存划分为几块，不同块采用适当的收集算法。

一般将堆分为新生代和老年代。

- 新生代使用：复制算法（）
- 老年代使用：标记 - 清除 或者 标记 - 整理 算法

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150931.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150931.png)

**回收流程**

1. 新创建的对象都被放在了**新生代的伊甸园**中

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150939.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150939.png)

2. 新生代空间不足时（当伊甸园中的内存不足时），就会进行一次垃圾回收，这时的回收叫做 **Minor GC**（引发Stop world, 暂停其他用户线程，等垃圾回收结束，用户才恢复运行）

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150946.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150946.png)

3. Minor GC 会将**伊甸园和幸存区FROM**存活的对象**先**复制到 **幸存区 TO**中， 并让其**寿命加1**，再**交换两个幸存区**

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150955.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150955.png)

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151002.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151002.png)

4. 再次创建对象，若新生代的伊甸园又满了，则会**再次触发 Minor GC**（会触发 **stop the world**， 暂停其他用户线程，只让垃圾回收线程工作），这时不仅会回收伊甸园中的垃圾，**还会回收幸存区中的垃圾**，再将活跃对象复制到幸存区TO中。回收以后会交换两个幸存区，并让幸存区中的对象**寿命加1**

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151010.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151010.png)

5. 如果幸存区中的对象的**寿命超过某个阈值**（最大为15，4bit），就会被**放入老年代**中

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151018.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151018.png)

6. 如果新生代老年代中的内存都满了，就会先触发Minor GC，再触发**Full GC**，扫描**新生代和老年代中**所有不再使用的对象并回收

##### **相关 JVM 参数**

含义	参数
堆初始大小	-Xms
堆最大大小	-Xmx 或 -XX:MaxHeapSize=size
新生代大小	-Xmn 或 (-XX:NewSize=size + -XX:MaxNewSize=size )
幸存区比例（动态）	-XX:InitialSurvivorRatio=ratio 和 -XX:+UseAdaptiveSizePolicy
幸存区比例	-XX:SurvivorRatio=ratio
晋升阈值	-XX:MaxTenuringThreshold=threshold
晋升详情	-XX:+PrintTenuringDistribution
GC详情	-XX:+PrintGCDetails -verbose:gc
FullGC 前 MinorGC	-XX:+ScavengeBeforeFullGC

#### GC分析

    public class Code_10_GCTest {
    
        private static final int _512KB = 512 * 1024;
        private static final int _1MB = 1024 * 1024;
        private static final int _6MB = 6 * 1024 * 1024;
        private static final int _7MB = 7 * 1024 * 1024;
        private static final int _8MB = 8 * 1024 * 1024;
    
        // -Xms20m -Xmx20m -Xmn10m -XX:+UseSerialGC -XX:+PrintGCDetails -verbose:gc
        public static void main(String[] args) {
            List<byte[]> list = new ArrayList<>();
            list.add(new byte[_6MB]);
            list.add(new byte[_512KB]);
            list.add(new byte[_6MB]);
            list.add(new byte[_512KB]);
            list.add(new byte[_6MB]);
        }
    
    }


​    

通过上面的代码，给 list 分配内存，来观察 新生代和老年代的情况，什么时候触发 minor gc，什么时候触发 full gc 等情况，使用前需要设置 jvm 参数。

### 2.4 垃圾收集器

**相关概念**

**并行收集**：指多条垃圾收集线程并行工作，但此时**用户线程仍处于等待状态**。

**并发收集**：指用户线程与垃圾收集线程**同时工作**（不一定是并行的可能会交替执行）。**用户程序在继续运行**，而垃圾收集程序运行在另一个CPU上

**吞吐量**：即CPU用于**运行用户代码的时间**与CPU**总消耗时间**的比值（吞吐量 = 运行用户代码时间 / ( 运行用户代码时间 + 垃圾收集时间 )），也就是。例如：虚拟机共运行100分钟，垃圾收集器花掉1分钟，那么吞吐量就是99%

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/c625baa0-dde6-449e-93df-c3a67f2f430f.jpg" width=""/> </div><br>

以上是 HotSpot 虚拟机中的 7 个垃圾收集器，连线表示垃圾收集器可以配合使用。

- 单线程与多线程：单线程指的是垃圾收集器只使用一个线程，而多线程使用多个线程；
- 串行与并行：串行指的是垃圾收集器与用户程序交替执行，这意味着在执行垃圾收集的时候需要停顿用户程序；并行指的是垃圾收集器和用户程序同时执行。除了 CMS 和 G1 之外，其它垃圾收集器都是以串行的方式执行。

#### 2.4.0 串行

- 单线程
- 内存较小，个人电脑（CPU核数较少）

##### 1. Serial 收集器（新生代：复制算法）

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/22fda4ae-4dd5-489d-ab10-9ebfdad22ae0.jpg" width=""/> </div><br>

![img](https://img-blog.csdnimg.cn/20210210092812153.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

Serial 翻译为串行，也就是说它以串行的方式执行。

它是**单线程的收集器，只会使用一个线程进行垃圾收集工作。**

它的优点是简单高效，在**单个 CPU** 环境下，由于没有线程交互的开销，因此拥有最高的单线程收集效率。

它是 Client 场景下的默认新生代收集器，因为在该场景下内存一般来说不会很大。它收集一两百兆垃圾的停顿时间可以控制在一百多毫秒以内，只要不是太频繁，这点停顿时间是可以接受的。

##### 2. Serial Old 收集器（老年代：标记 整理算法）

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/08f32fd3-f736-4a67-81ca-295b2a7972f2.jpg" width=""/> </div><br>

是 Serial 收集器的老年代版本，也是给 Client 场景下的虚拟机使用。如果用在 Server 场景下，它有两大用途：

- 在 JDK 1.5 以及之前版本（Parallel Old 诞生以前）中与 Parallel Scavenge 收集器搭配使用。
- 作为 CMS 收集器的后备预案，在并发收集发生 Concurrent Mode Failure 时使用。

[0.028s][info][gc] Using Serial   //使用serial + serial old
[0.028s][info][gc,heap,coops] Heap address: 0x00000000fec00000, size: 20 MB, Compressed Oops mode: 32-bit

****************
```
垃圾收集 1：

[0.154s][info][gc,start     ] GC(0) Pause Young (Allocation Failure)
                                    //在新生代进行垃圾收集
[0.156s][info][gc,heap      ] GC(0) DefNew: 1890K->640K(6144K)
                                    //defnew 新生代使用serial，回收前使用空间1890k，回收后使用640k，新生代空间共6m
[0.156s][info][gc,heap      ] GC(0) Tenured: 0K->292K(13696K)
                                    //tenures 老年代使用serial old，回收后部分新生代存活对象进入老年代，从0变为292k，老年代空间大小为13696k
[0.156s][info][gc,metaspace ] GC(0) Metaspace: 614K->614K(1056768K)
                                    //元空间使用614k，共有1032m
[0.156s][info][gc           ] GC(0) Pause Young (Allocation Failure) 1M->0M(19M) 2.653ms
                                    //新生代进行垃圾收集，堆内存19m，时间2.653毫秒
[0.156s][info][gc,cpu       ] GC(0) User=0.00s Sys=0.00s Real=0.00s


****************

垃圾收集 2：

[0.159s][info][gc,start     ] GC(1) Pause Young (Allocation Failure)
[0.163s][info][gc,heap      ] GC(1) DefNew: 4846K->12K(6144K)
[0.163s][info][gc,heap      ] GC(1) Tenured: 292K->5028K(13696K)
[0.163s][info][gc,metaspace ] GC(1) Metaspace: 698K->698K(1056768K)
[0.163s][info][gc           ] GC(1) Pause Young (Allocation Failure) 5M->4M(19M) 4.133ms
[0.163s][info][gc,cpu       ] GC(1) User=0.02s Sys=0.00s Real=0.00s
[0.163s][info][gc,start     ] GC(2) Pause Young (Allocation Failure)
[0.164s][info][gc,heap      ] GC(2) DefNew: 2116K->13K(6144K)
[0.164s][info][gc,heap      ] GC(2) Tenured: 5028K->5028K(13696K)
[0.164s][info][gc,metaspace ] GC(2) Metaspace: 701K->701K(1056768K)
[0.164s][info][gc           ] GC(2) Pause Young (Allocation Failure) 6M->4M(19M) 0.260ms
[0.164s][info][gc,cpu       ] GC(2) User=0.00s Sys=0.00s Real=0.00s


****************

垃圾收集 3：

[0.161s][info][gc,start     ] GC(3) Pause Full (System.gc())
                             //System.gc()触发full gc，新生代使用复制算法，老年代使用整理算法

新生代进行垃圾回收，老年代没有进行垃圾回收
[0.161s][info][gc,phases,start] GC(3) Phase 1: Mark live objects         //标记存活对象
[0.163s][info][gc,phases      ] GC(3) Phase 1: Mark live objects 1.907ms //标记存活对象花费时间1.907毫秒

[0.163s][info][gc,phases,start] GC(3) Phase 2: Compute new object addresses //计算新对象地址
[0.163s][info][gc,phases      ] GC(3) Phase 2: Compute new object addresses 0.314ms //计算新对象地址花费时间0.314毫秒

[0.163s][info][gc,phases,start] GC(3) Phase 3: Adjust pointers          //调整指针
[0.164s][info][gc,phases      ] GC(3) Phase 3: Adjust pointers 0.805ms  //调整指针花费时间0.805毫秒

[0.164s][info][gc,phases,start] GC(3) Phase 4: Move objects             //移动对象
[0.166s][info][gc,phases      ] GC(3) Phase 4: Move objects 2.322ms     //移动对象花费时间2.322毫秒

[0.166s][info][gc,heap        ] GC(3) DefNew: 4155K->0K(6144K)       //新生代回收4155k
[0.166s][info][gc,heap        ] GC(3) Tenured: 11181K->11191K(13696K)
[0.166s][info][gc,metaspace   ] GC(3) Metaspace: 777K->777K(1056768K)
[0.166s][info][gc             ] GC(3) Pause Full (System.gc()) 14M->10M(19M) 5.730ms
[0.166s][info][gc,cpu         ] GC(3) User=0.00s Sys=0.00s Real=0.01s


****************

垃圾收集后堆内存空间

[0.167s][info][gc,heap,exit   ] Heap       //堆内存分布
[0.167s][info][gc,heap,exit   ]  def new generation   total 6144K, used 256K [0x00000000fec00000, 0x00000000ff2a0000, 0x00000000ff2a0000)
                                 //新生代，总共6m，使用了256k
[0.167s][info][gc,heap,exit   ]   eden space 5504K,   4% used [0x00000000fec00000, 0x00000000fec402a0, 0x00000000ff160000)
                                 //eden总共5504k，使用了4%
[0.167s][info][gc,heap,exit   ]   from space 640K,   0% used [0x00000000ff200000, 
0x00000000ff200000, 0x00000000ff2a0000)
                                 //from survivor空间640k，没使用
[0.167s][info][gc,heap,exit   ]   to   space 640K,   0% used [0x00000000ff160000, 0x00000000ff160000, 0x00000000ff200000)
                                 //to survivor空间640k，没使用

[0.167s][info][gc,heap,exit   ]  tenured generation   total 13696K, used 11191K [0x00000000ff2a0000, 0x0000000100000000, 0x0000000100000000)
                                 //老年代空间大小：13696k，使用了11191k
[0.167s][info][gc,heap,exit   ]    the space 13696K,  81% used [0x00000000ff2a0000, 0x00000000ffd8dfd8, 0x00000000ffd8e000, 0x0000000100000000)
                                 //老年代使用了81%

[0.167s][info][gc,heap,exit   ]  Metaspace       used 780K, capacity 4531K, committed 4864K, reserved 1056768K
                                 //元空间使用了780k，剩余空间1056768k
[0.167s][info][gc,heap,exit   ]   class space    used 75K, capacity 402K, committed 512K, reserved 1048576K
```

#### 2.4.1吞吐量优先

- 多线程
- 堆内存较大，多核 cpu
- 让单位时间内，STW 的时间最短 0.2 0.2 = 0.4

##### 1.Parallel Scavenge 收集器

与吞吐量关系密切，故也称为吞吐量优先收集器
特点：属于新生代收集器也是采用复制算法的收集器（用到了新生代的幸存区），又是并行的多线程收集器（与 ParNew 收集器类似）

该收集器的目标是达到一个可控制的吞吐量。还有一个值得关注的点是：GC自适应调节策略（与 ParNew 收集器最重要的一个区别）

GC自适应调节策略：
Parallel Scavenge 收集器可设置 -XX:+UseAdptiveSizePolicy 参数。
当开关打开时不需要手动指定新生代的大小（-Xmn）、Eden 与 Survivor 区的比例（-XX:SurvivorRation）、
晋升老年代的对象年龄（-XX:PretenureSizeThreshold）等，虚拟机会根据系统的运行状况收集性能监控信息，动态设置这些参数以提供最优的停顿时间和最高的吞吐量，这种调节方式称为 GC 的自适应调节策略。

![img](https://img-blog.csdnimg.cn/20210210094915306.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

```
-XX:+UseParallelGC ~ -XX:+UsePrallerOldGC
-XX:+UseAdaptiveSizePolicy//自适应调整大小的比例（新生代中）会动态调整 Eden、两个Survivor 的大小
-XX:GCTimeRatio=ratio // 调整吞吐量 1/(1+radio)
//假设-XX:GCTimeRatio=19 ，则垃圾收集时间为1/(1+99),默认值为99，即1%时间用于垃圾收集。
-XX:MaxGCPauseMillis=ms // 最大暂停时间（垃圾回收） 200ms
-XX:ParallelGCThreads=n //并行GC线程数

```

与 ParNew 一样是多线程收集器。

其它收集器目标是尽可能缩短垃圾收集时用户线程的停顿时间，而它的目标是达到一个可控制的吞吐量，因此它被称为“吞吐量优先”收集器。这里的吞吐量指 CPU 用于运行用户程序的时间占总时间的比值。

停顿时间越短就越适合需要与用户交互的程序，良好的响应速度能提升用户体验。而高吞吐量则可以高效率地利用 CPU 时间，尽快完成程序的运算任务，适合在后台运算而不需要太多交互的任务。

缩短停顿时间是以牺牲吞吐量和新生代空间来换取的：新生代空间变小，垃圾回收变得频繁，导致吞吐量下降。

可以通过一个开关参数打开 GC 自适应的调节策略（GC Ergonomics），就不需要手工指定新生代的大小（-Xmn）、Eden 和 Survivor 区的比例、晋升老年代对象年龄等细节参数了。虚拟机会根据当前系统的运行情况收集性能监控信息，动态调整这些参数以提供最合适的停顿时间或者最大的吞吐量。

**Parallel Scavenge 收集器使用两个参数控制吞吐量：**

XX:MaxGCPauseMillis=ms 控制最大的垃圾收集停顿时间（默认200ms）
XX:GCTimeRatio=rario 直接设置吞吐量的大小

##### 2 Parallel Old 收集器

#### 

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/278fe431-af88-4a95-a895-9c3b80117de3.jpg" width=""/> </div><br>

是 Parallel Scavenge 收集器的老年代版本。

在注重吞吐量以及 CPU 资源敏感的场合，都可以优先考虑 Parallel Scavenge 加 Parallel Old 收集器。
特点：多线程，采用标记-整理算法（老年代没有幸存区）

#### 2.4.2 响应时间优先

- 多线程
- 堆内存较大，多核CPU
- 尽可能让单次STW时间变短（尽量不影响其他线程运行）

![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151052.png)

```
-XX:+UseConcMarkSweepGC ~ -XX:+UseParNewGC ~ SerialOld
-XX:ParallelGCThreads=n ~ -XX:ConcGCThreads=threads
-XX:CMSInitiatingOccupancyFraction=percent//老年代内存垃圾回收 垃圾占比阈值
-XX:+CMSScavengeBeforeRemark
```

##### CMS 收集器

CMS（Concurrent Mark Sweep）Concurrent Mark Sweep，一种以获取**最短回收停顿时间**为目标的老年代收集器，Mark Sweep 指的是标记 - 清除算法。

**应用场景**：适用于注重服务的响应速度，希望系统停顿时间最短，给用户带来更好的体验等场景下。如 web 程序、b/s 服务

**分为以下四个流程**：

- 初始标记：仅仅只是标记一下 GC Roots 能直接关联到的对象，速度很快，需要**停顿**。
- 并发标记：进行 GC Roots Tracing 的过程，它在整个回收过程中耗时最长，不需要停顿。
- 重新标记：为了修正并发标记期间因用户程序继续运作而导致标记产生变动的那一部分对象的标记记录，需要**停顿。**
- 并发清除：不需要停顿。对标记的对象进行清除回收，清除的过程中，可能任然会有新的垃圾产生，这些垃圾就叫浮动垃圾，**如果当用户需要存入一个很大的对象时，新生代放不下去，老年代由于浮动垃圾过多，就会退化为 serial Old 收集器，将老年代垃圾进行标记-整理，当然这也是很耗费时间的！**

在整个过程中耗时最长的并发标记和并发清除过程中，收集器线程都可以与用户线程一起工作，不需要进行停顿。

**具有以下缺点**：

- 吞吐量低：低停顿时间是以牺牲吞吐量为代价的，导致 CPU 利用率不够高。
- 无法处理浮动垃圾，可能出现 Concurrent Mode Failure。**浮动垃圾是指并发清除阶段由于用户线程继续运行而产生的垃圾，这部分垃圾只能到下一次 GC 时才能进行回收。**由于浮动垃圾的存在，因此需要预留出一部分内存，意味着 CMS 收集不能像其它收集器那样等待老年代快满的时候再回收。**如果预留的内存不够存放浮动垃圾，就会出现 Concurrent Mode Failure，这时虚拟机将临时启用 Serial Old 来替代 CMS。**
- 标记 - 清除算法导致的空间碎片，往往出现老年代空间剩余，但无法找到足够大连续空间来分配当前对象，不得不提前触发一次 Full GC。

**特点**：

1. 基于标记-清除算法实现。并发收集、低停顿，但是会产生内存碎片
2. 在**初次标记，重新标志的时候，要求我们暂停其它应用程序**，那么这两个阶段用户线程是不会参与的

CMS 收集器的内存回收过程是与用户线程一起并发执行的，可以搭配 ParNew 收集器（多线程，新生代，复制算法）与 Serial Old 收集器（单线程，老年代，标记-整理算法）使用。




##### ParNew 收集器

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/81538cd5-1bcf-4e31-86e5-e198df1e013b.jpg" width=""/> </div><br>

它是 Serial 收集器的多线程版本。

它是 Server 场景下默认的新生代收集器，除了性能原因外，主要是因为除了 Serial 收集器，只有它能与 CMS 收集器配合使用。

#### 2.4.5 G1 收集器

**适用场景：**

- 同时注重吞吐量和低延迟（响应时间）
- 超大堆内存（内存大的），会将堆内存划分为多个大小相等的区域
- 整体上是标记-整理算法，两个区域之间是复制算法

G1（Garbage-First），它是一款面向服务端应用的垃圾收集器，在多 CPU 和大内存的场景下有很好的性能。HotSpot 开发团队赋予它的使命是未来可以替换掉 CMS 收集器。

堆被分为新生代和老年代，其它收集器进行收集的范围都是整个新生代或者老年代，而 G1 可以直接对新生代和老年代一起回收。

##### 1.G1l垃圾回收阶段

![img](https://img-blog.csdnimg.cn/20210210114932887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

Young Collection：对新生代垃圾收集
Young Collection + Concurrent Mark：如果老年代内存到达一定的阈值了，新生代垃圾收集同时会执行一些并发的标记。
Mixed Collection：会对新生代 + 老年代 + 幸存区等进行混合收集，然后收集结束，会重新进入新生代收集。

**相关参数：**
JDK8 并不是默认开启的，所需要参数开启

```java
-XX:+UseG1GC
-XX:G1HeapRegionSize=size
-XX:MaxGCPauseMillis=time
```

###### **Young Collection**

**新生代存在 STW：**
分代是按对象的生命周期划分，分区则是将堆空间划分连续几个不同小区间，每一个小区间独立回收，可以控制一次回收多少个小区间，方便控制 GC 产生的停顿时间！
E：eden，S：幸存区，O：老年代
新生代收集会产生 STW ！

![img](https://img-blog.csdnimg.cn/20210210122339138.gif)

###### **Young Collection + CM**

在 Young GC 时会进行 GC Root 的初始化标记
老年代占用堆空间比例达到**阈值时，进行并发标记（不会STW）**，由下面的 JVM 参数决定 -XX:InitiatingHeapOccupancyPercent=percent （默认45%）

![img](https://img-blog.csdnimg.cn/20210210122601873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

###### **Mixed Collection**

会对 E S O 进行全面的回收

最终标记会 STW
拷贝存活会 STW
-XX:MaxGCPauseMills=xxms 用于指定最长的停顿时间！
问：为什么有的老年代被拷贝了，有的没拷贝？
**因为指定了最大停顿时间**，如果对所有老年代都进行回收，耗时可能过高。为了保证时间不超过设定的停顿时间，**会回收最有价值的老年代（回收后，能够得到更多内存）**

![img](https://img-blog.csdnimg.cn/20210210144216170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

##### 2.Full GC

###### **Full GC 的触发条件**

对于 Minor GC，其触发条件非常简单，当 Eden 空间满时，就将触发一次 Minor GC。而 Full GC 则相对复杂，有以下条件：

**1. 调用 System.gc()**

只是建议虚拟机执行 Full GC，但是虚拟机不一定真正去执行。不建议使用这种方式，而是让虚拟机管理内存。

**2. 老年代空间不足**

老年代空间不足的常见场景为前文所讲的大对象直接进入老年代、长期存活的对象进入老年代等。

为了避免以上原因引起的 Full GC，应当尽量不要创建过大的对象以及数组。除此之外，可以通过 -Xmn 虚拟机参数调大新生代的大小，让对象尽量在新生代被回收掉，不进入老年代。还可以通过 -XX:MaxTenuringThreshold 调大对象进入老年代的年龄，让对象在新生代多存活一段时间。

**3. 空间分配担保失败**

使用复制算法的 Minor GC 需要老年代的内存空间作担保，如果担保失败会执行一次 Full GC。具体内容请参考上面的第 5 小节。

**4. JDK 1.7 及以前的永久代空间不足**

在 JDK 1.7 及以前，HotSpot 虚拟机中的方法区是用永久代实现的，永久代中存放的为一些 Class 的信息、常量、静态变量等数据。

当系统中要加载的类、反射的类和调用的方法较多时，永久代可能会被占满，在未配置为采用 CMS GC 的情况下也会执行 Full GC。如果经过 Full GC 仍然回收不了，那么虚拟机会抛出 java.lang.OutOfMemoryError。

为避免以上原因引起的 Full GC，可采用的方法为增大永久代空间或转为使用 CMS GC。

**5. Concurrent Mode Failure**

执行 CMS GC 的过程中同时有对象要放入老年代，而此时老年代空间不足（可能是 GC 过程中浮动垃圾过多导致暂时性的空间不足），便会报 Concurrent Mode Failure 错误，并触发 Full GC。

###### **Minor GC 和 Full GC**

1. Minor GC：回收新生代，因为新生代对象存活时间很短，因此 Minor GC 会频繁执行，执行的速度一般也会比较快。
2. Full GC：回收老年代和新生代，老年代对象其存活时间长，因此 Full GC 很少执行，执行速度会比 Minor GC 慢很多。

- SerailGC
  - 新生代：minor gc
  - 老年代：full gc
- ParallelGC
  - 新生代：minor gc
  - 老年代：full gc
- CMS
  - 新生代：minor
  - 老年代：尽管达到阈值了，但如果垃圾回收速度大于产生速度，不叫full gc；小于时才叫full gc（并发失败）
- GI
  - 新生代：minor
  - 老年代：尽管达到阈值了，但如果垃圾回收速度大于产生速度，不叫full gc；小于时才叫full gc（并发失败）

G1 在老年代内存不足时（老年代所占内存超过阈值）
如果垃圾产生速度慢于垃圾回收速度，不会触发 Full GC，还是并发地进行清理
如果垃圾产生速度快于垃圾回收速度，便会触发 Full GC，然后退化成 serial Old 收集器串行的收集，就会导致停顿的时候长。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/f99ee771-c56f-47fb-9148-c0036695b5fe.jpg" width=""/> </div><br>

##### 3.细节分析

###### Young Collection 跨代引用

新生代垃圾回收时，需要找root对象，root对象可能在老年代中，但每次都遍历老年代的所有对象，比较耗时。

- 新生代回收的跨代引用（**老年代引用新生代**）问题

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151211.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151211.png)

- 卡表与Remembered Set
  - **Remembered Set 存在于E中，用于保存新生代对象对应的脏卡（老年代中被新生代引用的对象）**
    - 脏卡：O被划分为多个区域（一个区域512K），**如果该区域引用了新生代对象，则该区域被称为脏卡**
- 在引用变更时通过post-write barried + dirty card queue
- concurrent refinement threads 更新 Remembered Set

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151222.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151222.png)

如果不计算维护 Remembered Set 的操作，G1 收集器的运作大致可划分为以下几个步骤：

- 初始标记
- 并发标记
- 最终标记：为了修正在并发标记期间因用户程序继续运作而导致标记产生变动的那一部分标记记录，虚拟机将这段时间对象变化记录在线程的 Remembered Set Logs 里面，最终标记阶段需要把 Remembered Set Logs 的数据合并到 Remembered Set 中。这阶段需要停顿线程，但是可并行执行。
- 筛选回收：首先对各个 Region 中的回收价值和成本进行排序，根据用户所期望的 GC 停顿时间来制定回收计划。此阶段其实也可以做到与用户程序一起并发执行，但是因为只回收一部分 Region，时间是用户可控制的，而且停顿用户线程将大幅度提高收集效率。

具备如下特点：

- 空间整合：整体来看是基于“标记 - 整理”算法实现的收集器，从局部（两个 Region 之间）上来看是基于“复制”算法实现的，这意味着运行期间不会产生内存空间碎片。
- 可预测的停顿：能让使用者明确指定在一个长度为 M 毫秒的时间片段内，消耗在 GC 上的时间不得超过 N 毫秒。



###### Remark

重新标记阶段

在垃圾回收时，收集器处理对象的过程中

黑色：已被处理，需要保留的 灰色：正在处理中的 白色：还未处理的

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151229.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151229.png)

但是在**并发标记过程中**，有可能A被处理了以后未引用C，但该处理过程还未结束，在处理过程结束之前A引用了C，这时就会用到remark

过程如下

- 之前C未被引用，这时A引用了C，就会给C加一个写屏障，写屏障的指令会被执行，将C放入一个队列当中，并将C变为 处理中 状态

- ###### 在**并发标记**阶段结束以后，重新标记阶段会STW，然后将放在该队列中的对象重新处理，发现有强引用引用它，就会处理它

[![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151239.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151239.png)

###### JDK 8u20 字符串去重

**过程：**

将所有新分配的字符串（底层是 char[] ）放入一个队列
当新生代回收时，G1 并发检查是否有重复的字符串
如果字符串的值一样，就让他们引用同一个字符串对象
**注意，其与 String.intern() 的区别**

- String.intern() 关注的是字符串对象
- 字符串去重关注的是 char[]
- 在 JVM 内部，使用了不同的字符串标

**优点缺点：**

- 节省了大量内存
- 新生代回收时间略微增加，导致略微多占用 CPU
  -XX:+UseStringDeduplication
- JDK 8u40 并发标记类卸载
- 在并发标记阶段结束以后，就能知道哪些类不再被使用。如果一个类加载器的所有类都不在使用，则卸载它所加载的所有类

###### JDK 8u60 回收巨型对象

- 一个对象大于region的一半时，就称为巨型对象
- G1不会对巨型对象进行拷贝
- 回收时被优先考虑
- G1会跟踪老年代所有incoming引用（脏卡），如果老年代incoming引用为0的巨型对象就可以在新生代垃圾回收时处理掉

###### JDK 9 并发标记起始时间的调整

并发标记必须在堆空间占满前完成，否则退化为 FulGC

- JDK 9 之前需要使用 -XX:InitiatingHeapOccupancyPercent
  JDK 9 可以动态调整
  -XX:InitiatingHeapOccupancyPercent 用来设置初始值
- 进行数据采样并动态调整
- 总会添加一个安全的空挡空间
- 内存分配与回收策略

### 3 垃圾回收调优

预备知识
掌握 GC 相关的 VM 参数，会基本的空间调整
掌握相关工具
明白一点：调优跟应用、环境有关，没有放之四海而皆准的法则

查看虚拟机参数命令

```
"F:\JAVA\JDK8.0\bin\java" -XX:+PrintFlagsFinal -version | findstr "GC"
```

可以根据参数去查询具体的信息
如

```
ConGCThreads=0:CMS回收的并发线程数
GCTimeRatio=99:并行GC,与吞吐量相关的GC时间占领
MaxGCPauseMills:最大的GC停顿时间的目标
```



#### 3.1 调优领域

- 内存
- 锁竞争
- cpu 占用
- io
- gc

#### 3.2确定目标

吞吐量优先还是响应时间优先

低延迟/高吞吐量？ 选择合适的GC

- CMS G1 ZGC
- ParallelGC
- Zing

#### 3.3 最快的GC是不发生GC

首先排除减少因为自身编写的代码而引发的内存问题

查看 Full GC 前后的内存占用，考虑以下几个问题

- 数据是不是太多？
  resultSet = statement.executeQuery(“select * from 大表 limit n”)加载了不必要的数据到内存，内存的数据太多，导致堆内存的压力过大
  比如如下代码：resultSet=statement.query(“select * from 大表”);
  写完了再去遍历结果集。如果执行这样一条查询，它就会把所有的数据读入到java内存，这样的话内存再大，它也顶不住这么多语句同时查询，导致堆内存过大。我们可以加上limit n

- 数据表示是否太臃肿
  比如查询用户，不需要把用户所有信息都查询出来。有可能只是用到其中的一部分。
  
- 对象图
  对象大小 16 Integer 24 int 4
  java的最小一个对象都要占用16字节。Integer包装头也要用16字节。能用基本类型，就不用包装类型。

- 是否存在内存泄漏
  自制缓存 static Map map= 静态变量不断增加导致占有内存激增

  - 解决方法：

    软引用
    弱引用
    **第三方**

#### 3.4 新生调优

##### 3.4.1新生代的特点

- 所有的 new 操作分配内存都是非常廉价的

  - TLAB thread-lcoal allocation buffer.当new一个对象时，这个对象首先会在Eden伊甸园区分配，分配速度非常快。每个线程都会在伊甸园中分配一个私有区域TLAB thread-local allocation buffer，线程局部私有缓冲区。当new一个对象的时候，首先会减产TLAB中有没有可用内存，如果有，会在这块区域进行内存分配。TLAB 它的作用就是让每个线程用自己私有的伊甸园区来进行内存分配，这样多个线程即使同时创建对象，也不会有并发问题带来的干扰。

- 死亡对象回收零代价

  注意：**以前介绍的所有垃圾回收器新生代都是采用复制算法**。即把伊甸园和幸存区From都复制到幸存区To中去，复制完了之后，它们的内存都被释放出来了，因此，死亡对象回收代价就是0

- 大部分对象用过即死（朝生夕死）

- Minor GC 所用时间远小于 Full GC
  从而minor GC时间短，所以新生代优化空间更大一些

##### 3.4.2 新生代内存分配策略

**新生代内存越大越好么？**
不是

新生代内存**太小**：频繁触发Minor GC，会STW，会使得吞吐量下降
新生代内存**太大**：老年代内存占比有所降低，会更频繁地触发Full GC。而且触发Minor GC时，清理新生代所花费的时间会更长

**Oracle建议新生代占整个堆的1/4以上1/2以上的标准。**但是总的原则，我们还是要将新生代调的尽可能大，因为新生代垃圾回收用的是复制算法，复制算法分为两个阶段，1:标记，2.复制。复制花的时间更多一点。复制牵扯到对象占有内存的移动以及更新其它对象的内存地址。而新生代大部分对象都是朝生夕死的，只有少量的对象能够存活下来，那么复制所占用的时间较短，而标记时间相对复制时间来讲，就不是那么重要了。

**设置多大合适：**

新生代内存设置为内容纳**[并发量(请求-响应)]**的数据为宜*
**即一次请求和响应中所产生的对象的大小乘以并发量。**
比如一次请求和响应过程中会创建很多新的对象，这些新的对象占512K的内存，并发量大概是1000，即同一时刻有1000个用户来访问，那么新生代比较理想的内存就是每一个请求响应乘以并发量。即512K1000约为512M。**因为这一次请求响应的过程以后大部分对象都会被回收，而这一次请求并发量所占用的内存不超过新生代的内存，就较少的触发新生代的垃圾回收。这样就可以大约估算出新生代内存占用大概划分为多少合理。**

##### 3.4.3 幸存区调优

**幸存区需要能够保存 当前活跃对象+需要晋升的对象**
幸存区中可以看到两类对象

- 一类对象生命周期较短，很快就会被回收掉，但是现在还在使用它，暂时不能回收。所以就会留在幸存区出。
- 另一类是将来将会晋升到老年代，但是因为年龄还不够，所以暂存在幸存区中。所以幸存区的大小就需要大到能够把这两类对象都能容纳

**幸存区大小分配**

注意：如果幸存区比较小，它就会由JVM动态的调整晋升阈值，也许有些对象寿命还不够，但它存活时间并不是很长，由于幸存区内存太少，导致会提前把一些对象晋升到老年代中去。如果存活时间短的对象被晋升到老年代之后，那么它就要等老年代内存不足触发Full GC才能把它当成垃圾进行回收，这就间接的延长了对象的存活时间。我们要保证真正需要长时间存活的对象才把它晋升到老年代。

**晋升阈值配置得当，让长时间存活的对象尽快晋升**

因为长时间存活的对象，一直留在幸存区，只能够耗费幸存区的内存。**并且新生代的垃圾回收算法主要耗费时间就在对象复制上**，如果有大量的长时间存活的对象不能及时的进程，它们就会被留在幸存区复制来复值去，这样就会降低性能。**所以我们应该设置晋升阈值，将晋升阈值调的比较小**。将长时间存活的对象尽快调整到老年区
-XX:MaxTenuringThreshold=threshold
调整最大晋升阈值
-XX:+PrintTenuringDistribution
打印晋升的详细信息，以便判断晋升阈值是否更合适。

```
Desired survivor size 48286924 bytes, new threshold 10 (max 10) 
- age 1: 28992024 bytes, 28992024 total 
- - age 2: 1366864 bytes, 30358888 total 
- - age 3: 1425912 bytes, 31784800 total ...
```

通过打印幸存区中不同年龄的对象打印，可以更细致的决定的把最大阈值调整到多少合适

##### 3.4.4 老年代调优

以 CMS 为例
**CMS 的老年代内存越大越好**
CMS垃圾回收器，是一种低响应时间的，并且是一个并发的垃圾回收器，即垃圾回收线程在工作的同时，其他的用户线程也能够并发的执行，但是这样有一个缺点，因为垃圾回收的同时，其他的用户线程也在运行，那么又会产生新的垃圾，如果浮动垃圾产生又导致内存不足，就会导致CMS并发失败，此时CMS垃圾回收器就不能正常工作了，因此它就会退化SerialOld串行垃圾回收器，然后就Stop the world.响应时间就会变得特别长。所以规划内存的时候就尽量大一点，这样就可以预留更多的空间，避免浮动垃圾引起的并发失败。

**先尝试不做调优，如果没有 Full GC 那么已经…，否则先尝试调优新生代**
如果没有引起Full GC,那么说明老年代的空间比较充裕，这种没有Full GC,说明系统工作的很OK了。即便发生了FullGC,也从新生代开始调试。如果还是经常发生Full GC,那么就开始老年代调优

**观察发生 Full GC 时老年代内存占用，将老年代内存预设调大 1/4 ~ 1/3**
-XX:CMSInitiatingOccupancyFraction=percent
控制老年代内存被占用多少了之后，使用CMS进行垃圾回收。这个值越低，老年代垃圾回收触发的时机就越早。

##### 案例

案例1：Full GC 和 Minor GC 频繁 新生代
案例2：请求高峰期发生 Full GC，单次暂停时间特别长（CMS）标记时间
案例3：老年代充裕情况下，发生 Full GC（jdk1.7）永久代不足

### 4 内存分配策略

掌握GC相关的VM参数,会基本的空间调整

掌握相关的工具

#### 1. 对象优先在 Eden 分配

大多数情况下，对象在新生代 Eden 上分配，当 Eden 空间不够时，发起 Minor GC。

#### 2. 大对象直接进入老年代

大对象是指需要连续内存空间的对象，最典型的大对象是那种很长的字符串以及数组。

经常出现大对象会提前触发垃圾收集以获取足够的连续空间分配给大对象。

-XX:PretenureSizeThreshold，大于此值的对象直接在老年代分配，避免在 Eden 和 Survivor 之间的大量内存复制。

#### 3. 长期存活的对象进入老年代

为对象定义年龄计数器，对象在 Eden 出生并经过 Minor GC 依然存活，将移动到 Survivor 中，年龄就增加 1 岁，增加到一定年龄则移动到老年代中。

-XX:MaxTenuringThreshold 用来定义年龄的阈值。

#### 4. 动态对象年龄判定

虚拟机并不是永远要求对象的年龄必须达到 MaxTenuringThreshold 才能晋升老年代，如果在 Survivor 中相同年龄所有对象大小的总和大于 Survivor 空间的一半，则年龄大于或等于该年龄的对象可以直接进入老年代，无需等到 MaxTenuringThreshold 中要求的年龄。

#### 5. 空间分配担保

在发生 Minor GC 之前，虚拟机先检查老年代最大可用的连续空间是否大于新生代所有对象总空间，如果条件成立的话，那么 Minor GC 可以确认是安全的。

如果不成立的话虚拟机会查看 HandlePromotionFailure 的值是否允许担保失败，如果允许那么就会继续检查老年代最大可用的连续空间是否大于历次晋升到老年代对象的平均大小，如果大于，将尝试着进行一次 Minor GC；如果小于，或者 HandlePromotionFailure 的值不允许冒险，那么就要进行一次 Full GC。



# 类的字节码文件

**Java类分为三个阶段**

- Java类的编译、加载和执行。

本章讲解类的编译产生的在字节码

**什么是编译：**

- 编译是由 **.java源码文件转为 .class二进制字节码文件的过程**。
- 我们编写好的源代码，就是 .java文件。使用“javac test.java”就可以编译test.java文件。

**编译**过程主要有三步：

- 词法分析和输入到符号表
- 注解处理
- 语义分析和**生成字节码**

##  类的文件结构

**(字节码文件)（类的字节码文件）**

字节码（Bytecode）是一种包含执行程序、由一序列 op 代码/数据对 组成的**二进制文件**。**字节码是一种中间码**，它比机器码更抽象，需要直译器转译后才能成为机器码的中间代码。

通常情况下**它是已经经过编译**，但与特定机器码无关。字节码通常不像源码一样可以让人阅读，而是**编码后的数值常量、引用、指令等构成的序列**。

字节码主要为了实现特定软件运行和软件环境、与硬件环境无关。字节码的实现方式是通过编译器和虚拟机器。**编译器将源码编译成字节码**，特定平台上的虚拟机器将字节码转译为可以直接执行的指令。字节码的典型应用为Java bytecode。

字节码在运行时通过JVM（JAVA虚拟机）做一次转换生成机器指令，因此能够更好的跨平台运行。

**总结：**字节码是一种中间状态（中间码）的二进制代码（文件）。需要直译器转译后才能成为机器码



### 类加载与字节码技术

类加载与字节码技术包括以下内容
1.类文件结构
2.字节码指令
3.编译期处理
4.类加载阶段
5.类加载器
6.运行期优化
本篇博文仅讲解类文件结构，余下的可以参看[JVM系列（未完待续）](https://blog.csdn.net/qq_39736597/article/details/113482047)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210202132517805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)



### 类文件结构

一个简单的helloworld

```
package cn.yj.jvm;
//HelloWorld示例
public class HelloWorld {
    public static void main(String[] args){
        System.out.println("hello world");
    }
}
```

执行javac -parameters -d .HelloWorld.java或

```
F:\IDEA\projects\jvm>javac F:\IDEA\projects\jvm\untitled\src\cn\yj\jvm\HelloWorld.java
```

编译为HelloWorld.class之后是这个样子：

```
[root@localhost ~]# od -t xC HelloWorld.class 
0000000 ca fe ba be 00 00 00 34 00 23 0a 00 06 00 15 09 
0000020 00 16 00 17 08 00 18 0a 00 19 00 1a 07 00 1b 07 
0000040 00 1c 01 00 06 3c 69 6e 69 74 3e 01 00 03 28 29 
0000060 56 01 00 04 43 6f 64 65 01 00 0f 4c 69 6e 65 4e 
0000100 75 6d 62 65 72 54 61 62 6c 65 01 00 12 4c 6f 63 
0000120 61 6c 56 61 72 69 61 62 6c 65 54 61 62 6c 65 01 
0000140 00 04 74 68 69 73 01 00 1d 4c 63 6e 2f 69 74 63 
0000160 61 73 74 2f 6a 76 6d 2f 74 35 2f 48 65 6c 6c 6f 
0000200 57 6f 72 6c 64 3b 01 00 04 6d 61 69 6e 01 00 16 
0000220 28 5b 4c 6a 61 76 61 2f 6c 61 6e 67 2f 53 74 72 
0000240 69 6e 67 3b 29 56 01 00 04 61 72 67 73 01 00 13 
0000260 5b 4c 6a 61 76 61 2f 6c 61 6e 67 2f 53 74 72 69 
0000300 6e 67 3b 01 00 10 4d 65 74 68 6f 64 50 61 72 61 
0000320 6d 65 74 65 72 73 01 00 0a 53 6f 75 72 63 65 46 
0000340 69 6c 65 01 00 0f 48 65 6c 6c 6f 57 6f 72 6c 64 
0000360 2e 6a 61 76 61 0c 00 07 00 08 07 00 1d 0c 00 1e 
0000400 00 1f 01 00 0b 68 65 6c 6c 6f 20 77 6f 72 6c 
...............
```

根据 JVM 规范，**类文件结构**如下：

```
ClassFile{
	u4			   magic;				//魔数
	u2             minor_version;    	//版本
	u2             major_version;    
	u2             constant_pool_count;    //常量池
	cp_info        constant_pool[constant_pool_count-1];    
	u2             access_flags;    //访问修饰符
	u2             this_class;    	//类自己的包名
	u2             super_class;     //父类信息
	u2             interfaces_count;    //接口信息
	u2             interfaces[interfaces_count];    
	u2             fields_count;    //类中的变量字段信息
	field_info     fields[fields_count];    
	u2             methods_count;    //类中的方法信息
	method_info    methods[methods_count];    
	u2             attributes_count;    //类附加的属性信息
	attribute_info attributes[attributes_count]; 
}
```

u4代表的是字节数，比如u4代表前4个字节，u2代表接下来两个字节

- 魔数
  0~3 字节，表示它是否是【class】类型的文件

  ```
  0000000 ca fe ba be 00 00 00 34 00 23 0a 00 06 00 15 09
  //ca fe ba be是代表魔数信息的4个字节，所有的文件都有自己的特定类型。比如class文件，jpg文件，png文件。不同的文件有不同的类型。ca fe ba be是java的class文件编码。
  ```

- **版本**
  **4~7 字节00 00 00 34，表示类的版本 00 34（52） 表示是 Java 8**

  ```
  0000000 ca fe ba be 00 00 00 34 00 23 0a 00 06 00 15 09
  ```

- **常量池**
  常量池里记录了java类中的各种信息，包括它的**类的信息，方法的信息，成员变量的信息，还有方法的属性等等**

  ![img](https://img-blog.csdnimg.cn/20210202163647606.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

  
  
- 访问标识和继承信息21 表示该 class 是一个类，公共的

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021020217093899.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)
  
- filed 信息

  表示方法数量，本类为 2
  **一个方法由 访问修饰符，名称，参数描述，方法属性数量，方法属性组成**
  红色代表访问修饰符（本类中是 public）
  蓝色代表引用了常量池 #07 项作为方法名称
  绿色代表引用了常量池 #08 项作为方法参数描述
  黄色代表方法属性数量，本方法是 1
  红色代表方法属性

00 09 表示引用了常量池 #09 项，发现是【Code】属性

00 00 00 2f 表示此属性的长度是

**47 00 01 表示【操作数栈】大深度**

**00 01 表示【局部变量表】大槽（slot）数**

2a b7 00 01 b1 是字节码指令

00 00 00 02 表示方法细节属性数量，本例是 2

00 0a 表示引用了常量池 #10 项，发现是【LineNumberTable】属性

00 00 00 06 表示此属性的总长度，
  本例是 6 00 01 表示【LineNumberTable】长度
  00 00 表示【字节码】行号 00 04 表示【java 源码】行号

00 0b 表示引用了常量池 #11 项，发现是【LocalVariableTable】属性

## 字节码指令

java虚拟机内部有解释器，解释器会识别这些平台无关的字节码指令，把它们最终解释为机器码，然后执行。

如**public cn.itcast.jvm.t5.HelloWorld(); 构造方法的字节码指令**

```java
2a b7 00 01 b1
```

那么怎么知道机器码对应的字节码指令呢。
请参考
https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-6.html#jvms-6.5

![img](https://img-blog.csdnimg.cn/20210202181711149.png)

- 2a => **aload_0 加载 slot 0 的局部变量，即 this，做为下面的 invokespecial 构造方法调用的参数 （把局部变量表中0号槽位的变量加载到操作数栈上）**
- b7 => invokespecial 预备调用构造方法，哪个方法呢？（准备进行方法的调用）
- 00 01 引用常量池中 #1 项，即【 Method java/lang/Object." ": () V 】
- b1 表示返回

### javap 工具

自己分析类文件结构太麻烦了，Oracle 提供了 javap 工具来反编译 class 文件
使用IDEA反编译

```
F:\IDEA\projects\jvm>javap -v F:\IDEA\projects\jvm\out\production\untitled\cn\yj\jvm\HelloWorld.class
Classfile /F:/IDEA/projects/jvm/out/production/untitled/cn/yj/jvm/HelloWorld.class
  Last modified 2021-2-2; size 553 bytes
  MD5 checksum 6b7033e0eab7845f9c8aa7b8e1f2d44f
  Compiled from "HelloWorld.java"
public class cn.yj.jvm.HelloWorld
  minor version: 0
  major version: 52 //jdk8
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #6.#20         // 方法引用java/lang/Object."<init>":()V
   #2 = Fieldref           #21.#22        // java/lang/System.out:Ljava/io/PrintStream;
   #3 = String             #23            // hello world
   #4 = Methodref          #24.#25        // java/io/PrintStream.println:(Ljava/lang/String;)V
   #5 = Class              #26            // cn/yj/jvm/HelloWorld
   #6 = Class              #27            // java/lang/Object
   #7 = Utf8               <init>
   #8 = Utf8               ()V
   #9 = Utf8               Code
  #10 = Utf8               LineNumberTable
  #11 = Utf8               LocalVariableTable
  #12 = Utf8               this
  #13 = Utf8               Lcn/yj/jvm/HelloWorld;
  #14 = Utf8               main
  #15 = Utf8               ([Ljava/lang/String;)V
  #16 = Utf8               args
  #17 = Utf8               [Ljava/lang/String;
  #18 = Utf8               SourceFile
  #19 = Utf8               HelloWorld.java
  #20 = NameAndType        #7:#8          // "<init>":()V
  #21 = Class              #28            // java/lang/System
  #22 = NameAndType        #29:#30        // out:Ljava/io/PrintStream;
  #23 = Utf8               hello world
  #24 = Class              #31            // java/io/PrintStream
  #25 = NameAndType        #32:#33        // println:(Ljava/lang/String;)V
  #26 = Utf8               cn/yj/jvm/HelloWorld
  #27 = Utf8               java/lang/Object
  #28 = Utf8               java/lang/System
  #29 = Utf8               out
  #30 = Utf8               Ljava/io/PrintStream;
  #31 = Utf8               java/io/PrintStream
  #32 = Utf8               println
  #33 = Utf8               (Ljava/lang/String;)V
{
  public cn.yj.jvm.HelloWorld(); //构造方法
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1 zhan
         0: aload_0//aload_0把this装载到了操作数栈中aload_0是一组格式为aload_的操作码中的一个
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 2: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   Lcn/yj/jvm/HelloWorld;

  public static void main(java.lang.String[]); // main 方法
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #3                  // 加载常量池的常量String hello world
         5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 4: 0
        line 5: 8
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       9     0  args   [Ljava/lang/String;
}
SourceFile: "HelloWorld.java"
```

###  字节码指令的执行

1. ##### 原始java代码（.java）

```java
package cn.yj.jvm;

/** * 演示 字节码指令 和 操作数栈、常量池的关系 */
public class Demo3_1 {
    public static void main(String[] args)
    {
        int a = 10;
        int b = Short.MAX_VALUE + 1;
        int c = a + b;
        System.out.println(c);
    }
}
```

2. ##### 编译后的字节码文件

   ```
   F:\IDEA\projects\jvm>javap -v F:\IDEA\projects\jvm\out\production\untitled\cn\yj\jvm\Demo3_1.class
   Classfile /F:/IDEA/projects/jvm/out/production/untitled/cn/yj/jvm/Demo3_1.class
     Last modified 2021-2-2; size 603 bytes
     MD5 checksum 9bdbe178a29e07556915f368dbf7def1
     Compiled from "Demo3_1.java"
   public class cn.yj.jvm.Demo3_1
     minor version: 0
     major version: 52
     flags: ACC_PUBLIC, ACC_SUPER
   Constant pool:
      #1 = Methodref          #7.#25         // java/lang/Object."<init>":()V
      #2 = Class              #26            // java/lang/Short
      #3 = Integer            32768
      #4 = Fieldref           #27.#28        // java/lang/System.out:Ljava/io/PrintStream;
      #5 = Methodref          #29.#30        // java/io/PrintStream.println:(I)V
      #6 = Class              #31            // cn/yj/jvm/Demo3_1
      #7 = Class              #32            // java/lang/Object
      #8 = Utf8               <init>
      #9 = Utf8               ()V
     #10 = Utf8               Code
     #11 = Utf8               LineNumberTable
     #12 = Utf8               LocalVariableTable
     #13 = Utf8               this
     #14 = Utf8               Lcn/yj/jvm/Demo3_1;
     #15 = Utf8               main
     #16 = Utf8               ([Ljava/lang/String;)V
     #17 = Utf8               args
     #18 = Utf8               [Ljava/lang/String;
     #19 = Utf8               a
     #20 = Utf8               I
     #21 = Utf8               b
     #22 = Utf8               c
     #23 = Utf8               SourceFile
     #24 = Utf8               Demo3_1.java
     #25 = NameAndType        #8:#9          // "<init>":()V
     #26 = Utf8               java/lang/Short
     #27 = Class              #33            // java/lang/System
     #28 = NameAndType        #34:#35        // out:Ljava/io/PrintStream;
     #29 = Class              #36            // java/io/PrintStream
     #30 = NameAndType        #37:#38        // println:(I)V
     #31 = Utf8               cn/yj/jvm/Demo3_1
     #32 = Utf8               java/lang/Object
     #33 = Utf8               java/lang/System
     #34 = Utf8               out
     #35 = Utf8               Ljava/io/PrintStream;
     #36 = Utf8               java/io/PrintStream
     #37 = Utf8               println
     #38 = Utf8               (I)V
   {
     public cn.yj.jvm.Demo3_1();
       descriptor: ()V
       flags: ACC_PUBLIC
       Code:
         stack=1, locals=1, args_size=1
            0: aload_0
            1: invokespecial #1                  // Method java/lang/Object."<init>":()V
            4: return
         LineNumberTable:
           line 4: 0
         LocalVariableTable:
           Start  Length  Slot  Name   Signature
               0       5     0  this   Lcn/yj/jvm/Demo3_1;
   
     public static void main(java.lang.String[]);
       descriptor: ([Ljava/lang/String;)V
       flags: ACC_PUBLIC, ACC_STATIC
       Code:
         stack=2, locals=4, args_size=1
            0: bipush        10
            2: istore_1
            3: ldc           #3                  // int 32768
            5: istore_2
            6: iload_1
            7: iload_2
            8: iadd
            9: istore_3
           10: getstatic     #4                  // Field java/lang/System.out:Ljava/io/PrintStream;
           13: iload_3
           14: invokevirtual #5                  // Method java/io/PrintStream.println:(I)V
           17: return
         LineNumberTable:
           line 7: 0
           line 8: 3
           line 9: 6
           line 10: 10
           line 11: 17
         LocalVariableTable:
           Start  Length  Slot  Name   Signature
               0      18     0  args   [Ljava/lang/String;
               3      15     1     a   I
               6      12     2     b   I
              10       8     3     c   I
   }
   SourceFile: "Demo3_1.java"
   ```

3. ##### 常量池载入到运行时常量池
   
   当我们java代码被执行时：
   
   - 它会由java虚拟机的类加载器把我们**main方法所在的类**进行**类加载的操作**（类加载实际上把这些字节的class的数据读取到内存里）
   - 常量池的数据被放入运行时常量池（运行时常量池属于方法区的组成部分只是因为相对比较特殊，其实就是把class文件中的数据存入到运行时常量池的地方。将来找其中一些常量池信息就到运行时常量池中找）
   
   如图只列出了3,4,5几项，第3项是原码中的int b = Short.MAX_VALUE + 1;而int a=10;**比较小的数字与方法字节码存储在一起，不存在常量池中。一旦超过了Short整数的最大值的范围，就存到常量池中。**
   **常量池也属于方法区，只不过这里单独提出来了**
   
   分析class文件，int取值0~5时JVM采用**iconst_0**、iconst_1、iconst_2、iconst_3、iconst_4、iconst_5指令将常量压入栈中，取值-1时采用iconst_m1指令将常量压入栈中。
   
   **bipush**：当int取值**-128~127**时，JVM采用**bipush**指令将常量压入栈中。
   
   
   
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021020310301626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)
   
4. ##### 方法字节码载入到方法区

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203103721203.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)



5. ##### main 线程开始运行，分配栈帧内存

（stack=2，locals=4） 对应操作数栈有2个空间（每个空间4个字节），局部变量表中有4个槽位

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203103833206.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)



6. ##### 开始执行字节码文件

**bipush 10**

**将一个 byte 压入操作数栈（其长度会补齐 4 个字节），类似的指令还有：**

- sipush 将一个 short 压入操作数栈（其长度会补齐 4 个字节）
- ldc 将一个 int 压入操作数栈
- ldc2_w 将一个 long 压入操作数栈（分两次压入，因为 long 是 8 个字节）
- 这里小的数字都是和字节码指令存在一起，超过 short 范围的数字存入了常量池

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203104118222.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**istore 1**

**将操作数栈栈顶元素弹出，放入局部变量表的slot 1中**0

**对应代码中的**

```java
a = 10
```



![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203104348188.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**ldc #3**
**读取运行时常量池中#3，即32768(超过short最大值范围的数会被放到运行时常量池中)，将其加载到操作数栈中**

注意 Short.MAX_VALUE 是 32767，所以 32768 = Short.MAX_VALUE + 1 实际是在编译期间计算好的

![img](https://img-blog.csdnimg.cn/20210203105536353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**istore 2**
**将操作数栈中的元素弹出，放到局部变量表的2号位置![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203105636650.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)**

**iload_1 iload_2**
**将局部变量表中1号位置和2号位置的元素放入操作数栈中**

**因为只能在操作数栈中执行运算操作**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203105758348.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**iadd**

**将操作数栈中的两个元素弹出栈并相加，结果在压入操作数栈中**



![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203105942839.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203105953685.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**istore 3**
**将操作数栈中的元素弹出，放入局部变量表的3号位置**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203110032707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**getstatic #4**

在运行时常量池中找到#4，**发现是一个对象**

在**堆内存中找到该对象**，**并将其引用**放入操作数栈中

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203110145475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203110207826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**iload 3**
将局部变量表中3号位置的元素压入操作数栈中

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203110229966.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**invokevirtual 5**

**找到常量池 #5 项，定位到方法区 java/io/PrintStream.println:(I)V 方法**

**生成新的栈帧（分配 locals、stack等）**

**传递参数，执行新栈帧中的字节码![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203110342973.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)**

**执行完毕，弹出栈帧**
**清除 main 操作数栈内容**

**return**
**完成 main 方法调用，弹出 main 栈帧**
**程序结束**

**练习 - 分析 i++**

目的：从字节码角度分析 a++ 相关题目
源码：

```
package cn.yj.jvm;

/** * 从字节码角度分析 a++  相关题目 */ public class Demo3_2 {

    public static void main(String[] args) {
        int a = 10;
        int b = a++ + ++a + a--;
        System.out.println(a);
        System.out.println(b);
    }
}
```

字节码：

```
F:\IDEA\projects\jvm>javap -v F:\IDEA\projects\jvm\out\production\untitled\cn\yj\jvm\Demo3_2.class
Classfile /F:/IDEA/projects/jvm/out/production/untitled/cn/yj/jvm/Demo3_2.class
  Last modified 2021-2-3; size 578 bytes
  MD5 checksum c5e9d3ebbd57d36a03305a1c3a5d9b4c
  Compiled from "Demo3_2.java"
public class cn.yj.jvm.Demo3_2
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #5.#22         // java/lang/Object."<init>":()V
   #2 = Fieldref           #23.#24        // java/lang/System.out:Ljava/io/PrintStream;
   #3 = Methodref          #25.#26        // java/io/PrintStream.println:(I)V
   #4 = Class              #27            // cn/yj/jvm/Demo3_2
   #5 = Class              #28            // java/lang/Object
   #6 = Utf8               <init>
   #7 = Utf8               ()V
   #8 = Utf8               Code
   #9 = Utf8               LineNumberTable
  #10 = Utf8               LocalVariableTable
  #11 = Utf8               this
  #12 = Utf8               Lcn/yj/jvm/Demo3_2;
  #13 = Utf8               main
  #14 = Utf8               ([Ljava/lang/String;)V
  #15 = Utf8               args
  #16 = Utf8               [Ljava/lang/String;
  #17 = Utf8               a
  #18 = Utf8               I
  #19 = Utf8               b
  #20 = Utf8               SourceFile
  #21 = Utf8               Demo3_2.java
  #22 = NameAndType        #6:#7          // "<init>":()V
  #23 = Class              #29            // java/lang/System
  #24 = NameAndType        #30:#31        // out:Ljava/io/PrintStream;
  #25 = Class              #32            // java/io/PrintStream
  #26 = NameAndType        #33:#34        // println:(I)V
  #27 = Utf8               cn/yj/jvm/Demo3_2
  #28 = Utf8               java/lang/Object
  #29 = Utf8               java/lang/System
  #30 = Utf8               out
  #31 = Utf8               Ljava/io/PrintStream;
  #32 = Utf8               java/io/PrintStream
  #33 = Utf8               println
  #34 = Utf8               (I)V
{
  public cn.yj.jvm.Demo3_2();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 3: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   Lcn/yj/jvm/Demo3_2;

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=3, args_size=1
         0: bipush        10
         2: istore_1
         3: iload_1
         4: iinc          1, 1
         7: iinc          1, 1
        10: iload_1
        11: iadd
        12: iload_1
        13: iinc          1, -1
        16: iadd
        17: istore_2
        18: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
        21: iload_1
        22: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
        25: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
        28: iload_2
        29: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
        32: return
      LineNumberTable:
        line 6: 0
        line 7: 3
        line 8: 18
        line 9: 25
        line 10: 32
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      33     0  args   [Ljava/lang/String;
            3      30     1     a   I
           18      15     2     b   I
}
SourceFile: "Demo3_2.java"
```

分析：
**注意 iinc 指令是直接在局部变量 slot 上进行运算**
**a++ 和 ++a 的区别是先执行 iload 还是 先执行 iinc**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203112110302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)



### 构造方法

####  < cinit> ()V

```java
public class Demo3_8_1 {
	static int i = 10;
    static {
        i = 20;
    }
    static {
        i = 30;
    }
    public static void main(String[] args) {
        System.out.println(Demo3_8_1.i);
    }
}

```

**编译器会按从上至下的顺序，收集所有 static 静态代码块和静态成员赋值的代码，合并为一个特殊的方法 < cinit> ()V ：**

```
stack=1, locals=0, args_size=0
         0: bipush        10
         2: putstatic     #2                  // Field i:I
         5: bipush        20
         7: putstatic     #2                  // Field i:I
        10: bipush        30
        12: putstatic     #2                  // Field i:I
        15: return
```

**最后赋值的是30，所以结果是30**

#### **init()V**

```java
public class Demo4 {
	private String a = "s1";

	{
		b = 20;
	}

	private int b = 10;

	{
		a = "s2";
	}

	public Demo4(String a, int b) {
		this.a = a;
		this.b = b;
	}

	public static void main(String[] args) {
		Demo4 d = new Demo4("s3", 30);
		System.out.println(d.a);
		System.out.println(d.b);
	}
}

```

编译器会按从上至下的顺序，**收集所有 {} 代码块和成员变量赋值的代码，形成新的构造方法**，但**原始构造方法内的代码总是在后**

```
Code:
     stack=2, locals=3, args_size=3
        0: aload_0 //aload_0把this装载到了操作数栈中aload_0是一组格式为aload_的操作码中的一个
        1: invokespecial #1                  // Method java/lang/Object."<init>":()V
        4: aload_0
        5: ldc           #2                  // String s1
        7: putfield      #3                  // Field a:Ljava/lang/String;
       10: aload_0
       11: bipush        20
       13: putfield      #4                  // Field b:I
       16: aload_0
       17: bipush        10
       19: putfield      #4                  // Field b:I
       22: aload_0
       23: ldc           #5                  // String s2
       25: putfield      #3                  // Field a:Ljava/lang/String;
       //原始构造方法在最后执行
       28: aload_0
       29: aload_1
       30: putfield      #3                  // Field a:Ljava/lang/String;
       33: aload_0
       34: iload_2
       35: putfield      #4                  // Field b:I
       38: return

```

**最终输出 S3 30**

**执行顺序：静态代码块->非静态代码块->类的构造方法**

**类中静态代码块仅在类加载时被执行一次**

**如果有父类**：

父类静态代码块-》子类静态代码块-》父类构造代码块-》父类构造函数-》子类构造代码块-》子类构造函数

**执行顺序**

（1）父类静态成员和静态初始化块，按在代码中出现的顺序依次执行。

（2）子类静态成员和静态初始化块，按在代码中出现的顺序依次执行。

（3）父类实例成员和实例初始化块，按在代码中出现的顺序依次执行。

（4）执行父类构造方法。

（5）子类实例成员和实例初始化块，按在代码中出现的顺序依次执行。

（6）执行子类构造方法

###  方法的调用

```java
public class Demo5 {
	public Demo5() {

	}

	private void test1() {

	}

	private final void test2() {

	}

	public void test3() {

	}

	public static void test4() {

	}

	public static void main(String[] args) {
		Demo5 demo5 = new Demo5();
		demo5.test1();
		demo5.test2();
		demo5.test3();
		Demo5.test4();
	}
}

```

不同方法在调用时，对应的虚拟机指令有所区别：

- **私有、构造、被final修饰的方法，在调用时都使用invokespecial指令**
- **public普通成员方法在调用时，使用invokevirtual指令。因为编译期间无法确定该方法的内容，只有在运行期间才能确定**
- **静态方法在调用时使用invokestatic指令**
- **invokevirtual在编译时时无法确定是调用哪个对象的方法，也许是父类的，也许是子类的。invokevirtual称之为动态绑定，在运行的时候确定调用哪个对象的方法。**
- **invokestatic静态绑定直接就能找到方法的入口地址了。**

```
Code:
      stack=2, locals=2, args_size=1
         0: new           #2                  // class com/nyima/JVM/day5/Demo5 
         3: dup
         4: invokespecial #3                  // Method "<init>":()V
         7: astore_1
         8: aload_1
         9: invokespecial #4                  // Method test1:()V
        12: aload_1
        13: invokespecial #5                  // Method test2:()V
        16: aload_1
        17: invokevirtual #6                  // Method test3:()V
        20: invokestatic  #7                  // Method test4:()V
        23: return

```

注意：**new 是创建【对象】，给对象分配堆内存，执行成功会将【对象引用】压入操作数栈**
**dup 是复制操作数栈栈顶的内容，本例即为【对象引用】，为什么需要两份引用呢：**

1. **一个是要配合 invokespecial 调用该对象的构造方法 “init”😦)V （会消耗掉栈顶一个引用）**

2. **另一个要 配合 astore_1 赋值给变量demo5**
   终方法（ﬁnal），私有方法（private），构造方法都是由 invokespecial 指令来调用，属于静态绑定
   普通成员方法是由 invokevirtual 调用，属于动态绑定，即支持多态 成员方法与静态方法调用的另一个区别是，执行方法前是否需要【对象引用】

   静态方法属于静态绑定。不需要对象来调用（否则会产生两个不必要的虚拟机指令）。
   
3.  因为invokespecial会消耗掉操作数栈顶的引用作为传给构造器的“this”参数，所以如果我们希望在invokespecial调用后在操作数栈顶还维持有一个指向新建对象的引用，就得在invokespecial之前先“复制”一份引用——这就是这个dup的来源。

### 多态

```java
/**
 * 演示多态原理，注意加上下面的 JVM 参数，禁用指针压缩
 * -XX:-UseCompressedOops -XX:-UseCompressedClassPointers
 */
public class Demo3_10 {

    public static void test(Animal animal) {
        animal.eat();
        System.out.println(animal.toString());
    }

    public static void main(String[] args) throws IOException {
        test(new Cat());
        test(new Dog());
        System.in.read();
    }
}

abstract class Animal {
    public abstract void eat();

    @Override
    public String toString() {
        return "我是" + this.getClass().getSimpleName();
    }
}

class Dog extends Animal {

    @Override
    public void eat() {
        System.out.println("啃骨头");
    }
}

class Cat extends Animal {

    @Override
    public void eat() {
        System.out.println("吃鱼");
    }
}

```

因为普通成员方法需要在运行时才能确定具体的内容，所以虚拟机需要调用**invokevirtual**指令

在执行invokevirtual指令时，经历了以下几个步骤

- 先通过栈帧中对象的引用找到对象
- 分析对象头，找到对象实际的Class （对象的类型）
- Class结构中有**vtable** 虚方法表 （可以重写方法的表）
- 查询vtable找到方法的具体地址
- 执行方法的字节码

### 异常

##### **try-catch**

```java
public class Demo1 {
	public static void main(String[] args) {
		int i = 0;
		try {
			i = 10;
		}catch (Exception e) {
			i = 20;
		}
	}
}
```

```
Code:
     stack=1, locals=3, args_size=1
        0: iconst_0
        1: istore_1
        2: bipush        10
        4: istore_1
        5: goto          12
        8: astore_2
        9: bipush        20
       11: istore_1
       12: return
     //多出来一个异常表
     Exception table:
        from    to  target type
          2     5     8   Class java/lang/Exception
```

可以看到多出来一个 Exception table 的结构，[from, to) 是前闭后开（也就是检测2~4行）的检测范围，
一旦这个范围内的字节码执行出现异常，则通过 type 匹配异常类型，如果一致，进入 target 所指示行号
8行的字节码指令 astore_2 是**将异常对象Exception e 的引用地址 存入局部变量表的2号位置（为e）**

##### 多个single-catch

```java
public class Demo1 {
	public static void main(String[] args) {
		int i = 0;
		try {
			i = 10;
		}catch (ArithmeticException e) {
			i = 20;
		}catch (Exception e) {
			i = 30;
		}
	}
}
```

```java
Code:
     stack=1, locals=3, args_size=1
        0: iconst_0
        1: istore_1
        2: bipush        10
        4: istore_1
        5: goto          19
        8: astore_2
        9: bipush        20
       11: istore_1
       12: goto          19
       15: astore_2
       16: bipush        30
       18: istore_1
       19: return
     Exception table:
        from    to  target type
            2     5     8   Class java/lang/ArithmeticException
            2     5    15   Class java/lang/Exception

```

因为异常出现时，只能进入 Exception table 中一个分支，**所以局部变量表 slot 2 位置被共用**



##### finally

```java
public class Demo2 {
	public static void main(String[] args) {
		int i = 0;
		try {
			i = 10;
		} catch (Exception e) {
			i = 20;
		} finally {
			i = 30;
		}
	}
}

```

```java
Code:
     stack=1, locals=4, args_size=1
        0: iconst_0
        1: istore_1
        //try块
        2: bipush        10
        4: istore_1
        //try块执行完后，会执行finally    
        5: bipush        30
        7: istore_1
        8: goto          27
       //catch块     
       11: astore_2 //异常信息放入局部变量表的2号槽位
       12: bipush        20
       14: istore_1
       //catch块执行完后，会执行finally        
       15: bipush        30
       17: istore_1
       18: goto          27
       //出现异常，但未被Exception捕获，会抛出其他异常，这时也需要执行finally块中的代码   
       21: astore_3 //其他异常信息放入局部变量表的2号槽位
       22: bipush        30
       24: istore_1
       25: aload_3
       26: athrow  //抛出异常
       27: return
     Exception table:
        from    to  target type
            2     5    11   Class java/lang/Exception
            2     5    21   any
           11    15    21   any

```

**可以看到 ﬁnally 中的代码被复制了 3 份，分别放入 try 流程，catch 流程以及 catch剩余的异常类型流程**，即catch块捕获不到的异常finally也要执行。

###### finally面试题

1. finally中的return

```java
public class Demo3 {
	public static void main(String[] args) {
		int i = Demo3.test();
        //结果为20
		System.out.println(i);
	}

	public static int test() {
		int i;
		try {
			i = 10;
			return i;
		} finally {
			i = 20;
			return i;
		}
	}
}

```

```
Code:
     stack=1, locals=3, args_size=0
        0: bipush        10
        2: istore_0
        3: iload_0
        4: istore_1  //暂存返回值
        5: bipush        20
        7: istore_0
        8: iload_0
        9: ireturn	//ireturn会返回操作数栈顶的整型值20
       //如果出现异常，还是会执行finally块中的内容，没有抛出异常
       10: astore_2
       11: bipush        20
       13: istore_0
       14: iload_0
       15: ireturn	//这里没有athrow了，也就是如果在finally块中如果有返回操作的话，且try块中出现异常，会吞掉异常！
     Exception table:
        from    to  target type
            0     5    10   any
```

由于 ﬁnally 中的 ireturn 被插入了所有可能的流程，因此返回结果肯定以ﬁnally的为准
至于字节码中第 2 行，似乎没啥用，且留个伏笔，看下个例子
**跟上例中的 ﬁnally 相比，发现没有 athrow 了，这告诉我们：如果在 ﬁnally 中出现了 return，会吞掉异常，这样就不知道发生了异常**
所以不要在finally中进行返回操作

2. 被吞掉的异常

```jAVA
public class Demo3 {
   public static void main(String[] args) {
      int i = Demo3.test();
      //最终结果为20
      System.out.println(i);
   }

   public static int test() {
      int i;
      try {
         i = 10;
         //这里应该会抛出异常
         i = i/0;
         return i;
      } finally {
         i = 20;
         return i;
      }
   }
}
```

3. finally不带return

```java
public class Demo4 {
	public static void main(String[] args) {
		int i = Demo4.test();
		System.out.println(i);
	}

	public static int test() {
		int i = 10;
		try {
			return i;
		} finally {
			i = 20;
		}
	}
}

```

```
Code:
     stack=1, locals=3, args_size=0
        0: bipush        10
        2: istore_0 //赋值给i 10
        3: iload_0	//加载到操作数栈顶
        4: istore_1 //加载到局部变量表的1号位置
        5: bipush        20
        7: istore_0 //赋值给i 20
        8: iload_1 //加载局部变量表1号位置的数10到操作数栈
        9: ireturn //返回操作数栈顶元素 10
       10: astore_2
       11: bipush        20
       13: istore_0
       14: aload_2 //加载异常
       15: athrow //抛出异常
     Exception table:
        from    to  target type
            3     5    10   any

```

**如果在try语句return了，finally中虽然对i做了变化，但是不会影响到返回结果的，因为它在return之前先做了一个暂存并把i 的值暂存用于return ，然后再执行了finally中的代码，然后再把暂存的值取回来，最后返回的是return时暂存的值。**



finally后有return

前面return了函数返回，后面的return不执行

```java

class python {
    public int add(int a,int b){
        try {
            return a+b;
        }
        catch (Exception e) {
            System.out.println("catch语句块");
        }
        finally{
            System.out.println("finally语句块");
        }
        System.out.println("方法最后的语句块");
        return 0;

    }
    public static void main(String argv[]){
        python test =new python();
        System.out.println("和是："+test.add(9, 34));
    }
}
```

![image-20210930141346886](笔记图库/image-20210930141346886.png)



##### 练习

最终输出：12

```java
public class ZeroTest {
    public static void main(String[] args) {
     try{
       int i = 100 / 0;
       System.out.print(i);
      }catch(Exception e){
       System.out.print(1);
       throw new RuntimeException();
      }finally{
       System.out.print(2);
      }
      System.out.print(3);
     }
 }
```

```java
 Code:
      stack=2, locals=3, args_size=1
      //try
         0: bipush        100
         2: iconst_0
         3: idiv
         4: istore_1
         5: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
         8: iload_1
         9: invokevirtual #13                 // Method java/io/PrintStream.print:(I)V
            
               
      // finally
        12: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
        15: iconst_2
        16: invokevirtual #13                 // Method java/io/PrintStream.print:(I)V
        19: goto          48
            
        //catch    
        22: astore_1   //异常对象存在 1 号槽位
            
        23: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
        26: iconst_1
        27: invokevirtual #13                 // Method java/io/PrintStream.print:(I)V
            
        30: new           #21                 // class java/lang/RuntimeException
        33: dup
        34: invokespecial #23                 // Method java/lang/RuntimeException."<init>":()V
        37: athrow
            
        38: astore_2
            
        // finally      
        39: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
        42: iconst_2
        43: invokevirtual #13                 // Method java/io/PrintStream.print:(I)V
        46: aload_2
        47: athrow
            
        //   
        48: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
            
        //try外面    
        51: iconst_3
        52: invokevirtual #13                 // Method java/io/PrintStream.print:(I)V
        55: return

```



### Synchronized

```java
public class Demo5 {
	public static void main(String[] args) {
		int i = 10;
		Lock lock = new Lock();
		synchronized (lock) {
			System.out.println(i);
		}
	}
}

class Lock{}
```

字节码

```
Code:
     stack=2, locals=5, args_size=1
        0: bipush        10
        2: istore_1
        3: new           #2                  // class com/nyima/JVM/day06/Lock
        6: dup //复制一份，放到操作数栈顶，用于构造函数消耗
        7: invokespecial #3                  // Method com/nyima/JVM/day06/Lock."<init>":()V
        ***然后因为invokespecial会消耗掉操作数栈顶的引用作为传给构造器的“this”参数，所以如果我们希望在invokespecial调用后在操作数栈顶还维持有一个指向新建对象的引用，就得在invokespecial之前先“复制”一份引用——这就是这个dup的来源。


       10: astore_2 //剩下的一份放到局部变量表的2号位置
       11: aload_2 //加载到操作数栈
       12: dup //复制一份，放到操作数栈，用于加锁时消耗
       13: astore_3 //将操作数栈顶元素弹出，暂存到局部变量表的三号槽位。这时操作数栈中有一份对象的引用
       14: monitorenter //加锁
       //锁住后代码块中的操作    
       15: getstatic     #4                  // Field java/lang/System.out:Ljava/io/PrintStream;
       18: iload_1
       19: invokevirtual #5                  // Method java/io/PrintStream.println:(I)V
       //加载局部变量表中三号槽位对象的引用，用于解锁    
       22: aload_3    
       23: monitorexit //解锁
       24: goto          34
       //异常操作    
       27: astore        4
       29: aload_3
       30: monitorexit //解锁
       31: aload         4
       33: athrow
       34: return
     //可以看出，无论何时出现异常，都会跳转到27行，将异常放入局部变量中，并进行解锁操作，然后加载异常并抛出异常。      
     Exception table:
        from    to  target type
           15    24    27   any
           27    31    27   any
```

## 编译器处理

所谓的 **语法糖** ，其实就是指 java 编译器把 *.java 源码编译为 \*.class 字节码的过程中，**自动生成**和**转换**的一些代码，主要是为了减轻程序员的负担，算是 java 编译器给我们的一个额外福利

**注意**，以下代码的分析，借助了 javap 工具，idea 的反编译功能，idea 插件 jclasslib 等工具。另外， 编译器转换的**结果直接就是 class 字节码**，只是为了便于阅读，给出了 几乎等价 的 java 源码方式，并不是编译器还会转换出中间的 java 源码，切记。

### 默认构造函数

```java
public class Candy1 {

}
```

经过编译期优化后

```java
public class Candy1 {
   //这个无参构造器是java编译器帮我们加上的
   public Candy1() {
      //即调用父类 Object 的无参构造方法，即调用 java/lang/Object." <init>":()V
      super();
   }
}
```

### 自动拆装箱

基本类型和其包装类型的相互转换过程，称为拆装箱

在JDK 5以后，它们的转换可以在编译期自动完成

```
public class Demo2 {
   public static void main(String[] args) {
      Integer x = 1;
      int y = x;
   }
}Copy
```

转换过程如下

```
public class Demo2 {
   public static void main(String[] args) {
      //基本类型赋值给包装类型，称为装箱
      Integer x = Integer.valueOf(1);
      //包装类型赋值给基本类型，称谓拆箱
      int y = x.intValue();
   }
}
```

泛型集合取值

泛型也是在 JDK 5 开始加入的特性，但 java 在**编译泛型代码后**会执行 **泛型擦除** 的动作，即泛型信息在编译为字节码之后就**丢失**了，实际的类型都当做了 **Object** 类型来处理：

```java
public class Demo3 {
   public static void main(String[] args) {
      List<Integer> list = new ArrayList<>();
      list.add(10);
      Integer x = list.get(0);
   }
}
```

对应字节码

```java
Code:
    stack=2, locals=3, args_size=1
       0: new           #2                  // class java/util/ArrayList
       3: dup
       4: invokespecial #3                  // Method java/util/ArrayList."<init>":()V
       7: astore_1
       8: aload_1
       9: bipush        10
      11: invokestatic  #4                  // Method java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
      //这里进行了泛型擦除，实际调用的是add(Objcet o)
      14: invokeinterface #5,  2            // InterfaceMethod java/util/List.add:(Ljava/lang/Object;)Z

      19: pop
      20: aload_1
      21: iconst_0
      //这里也进行了泛型擦除，实际调用的是get(Object o)   
      22: invokeinterface #6,  2            // InterfaceMethod java/util/List.get:(I)Ljava/lang/Object;
//这里进行了类型转换，将Object转换成了Integer
      27: checkcast     #7                  // class java/lang/Integer
      30: astore_2
      31: return
```

所以调用get函数取值时，内部 有一个类型转换的操作

```
Integer x = (Integer) list.get(0);Copy
```

如果要将返回结果赋值给一个int类型的变量，内部 则还有**自动拆箱**的操作

```
int x = (Integer) list.get(0).intValue();
```

**注意：**

由于泛型擦除：

使用反射只能可以得到，**参数的类型以及返回值的泛型类型**。泛型反射代码如下：


    public static void main(String[] args) throws NoSuchMethodException {
        // 1. 拿到方法
        Method method = Code_20_ReflectTest.class.getMethod("test", List.class, Map.class);
        // 2. 得到泛型参数的类型信息
        Type[] types = method.getGenericParameterTypes();
        for(Type type : types) {
            // 3. 判断参数类型是否，带泛型的类型。
            if(type instanceof ParameterizedType) {
                ParameterizedType parameterizedType = (ParameterizedType) type;
    
                // 4. 得到原始类型
                System.out.println("原始类型 - " + parameterizedType.getRawType());
                // 5. 拿到泛型类型
                Type[] arguments = parameterizedType.getActualTypeArguments();
                for(int i = 0; i < arguments.length; i++) {
                    System.out.printf("泛型参数[%d] - %s\n", i, arguments[i]);
                }
            }
        }
    }
    
    public Set<Integer> test(List<String> list, Map<Integer, Object> map) {
        return null;
    }

输出：

原始类型 - interface java.util.List
泛型参数[0] - class java.lang.String
原始类型 - interface java.util.Map
泛型参数[0] - class java.lang.Integer
泛型参数[1] - class java.lang.Object

### 可变参数

```
public class Demo4 {
   public static void foo(String... args) {
      //将args赋值给arr，可以看出String...实际就是String[] 
      String[] arr = args;
      System.out.println(arr.length);
   }

   public static void main(String[] args) {
      foo("hello", "world");
   }
}Copy
```

可变参数 **String…** args 其实是一个 **String[]** args ，从代码中的赋值语句中就可以看出来。 同 样 java 编译器会在编译期间将上述代码变换为：

```
public class Demo4 {
   public Demo4 {}

    
   public static void foo(String[] args) {
      String[] arr = args;
      System.out.println(arr.length);
   }

   public static void main(String[] args) {
      foo(new String[]{"hello", "world"});
   }
}Copy
```

注意，如果调用的是foo()，即未传递参数时，等价代码为foo(new String[]{})，**创建了一个空数组**，而不是直接传递的null

### foreach

```
public class Demo5 {
	public static void main(String[] args) {
        //数组赋初值的简化写法也是一种语法糖。
		int[] arr = {1, 2, 3, 4, 5};
		for(int x : arr) {
			System.out.println(x);
		}
	}
}Copy
```

编译器会帮我们转换为

```
public class Demo5 {
    public Demo5 {}

	public static void main(String[] args) {
		int[] arr = new int[]{1, 2, 3, 4, 5};
		for(int i=0; i<arr.length; ++i) {
			int x = arr[i];
			System.out.println(x);
		}
	}
}Copy
```

**如果是集合使用foreach**

```
public class Demo5 {
   public static void main(String[] args) {
      List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
      for (Integer x : list) {
         System.out.println(x);
      }
   }
}Copy
```

集合要使用foreach，需要该集合类实现了**Iterable接口**，因为集合的遍历需要用到**迭代器Iterator**

```
public class Demo5 {
    public Demo5 {}
    
   public static void main(String[] args) {
      List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
      //获得该集合的迭代器
      Iterator<Integer> iterator = list.iterator();
      while(iterator.hasNext()) {
         Integer x = iterator.next();
         System.out.println(x);
      }
   }
}
```

### switch字符串

```
public class Demo6 {
   public static void main(String[] args) {
      String str = "hello";
      switch (str) {
         case "hello" :
            System.out.println("h");
            break;
         case "world" :
            System.out.println("w");
            break;
         default:
            break;
      }
   }
}Copy
```

在编译器中执行的操作

```
public class Demo6 {
   public Demo6() {
      
   }
   public static void main(String[] args) {
      String str = "hello";
      int x = -1;
      //通过字符串的hashCode+value来判断是否匹配
      switch (str.hashCode()) {
         //hello的hashCode
         case 99162322 :
            //再次比较，因为字符串的hashCode有可能相等
            if(str.equals("hello")) {
               x = 0;
            }
            break;
         //world的hashCode
         case 11331880 :
            if(str.equals("world")) {
               x = 1;
            }
            break;
         default:
            break;
      }

      //用第二个switch在进行输出判断
      switch (x) {
         case 0:
            System.out.println("h");
            break;
         case 1:
            System.out.println("w");
            break;
         default:
            break;
      }
   }
}Copy
```

过程说明：

- 在编译期间，单个的switch被分为了两个
  - 第一个用来匹配字符串，并给x赋值
    - 字符串的匹配用到了字符串的hashCode，还用到了equals方法
    - 使用hashCode是为了提高比较效率，使用equals是防止有hashCode冲突（如BM和C.）
  - 第二个用来根据x的值来决定输出语句

### switch枚举

```
public class Demo7 {
   public static void main(String[] args) {
      SEX sex = SEX.MALE;
      switch (sex) {
         case MALE:
            System.out.println("man");
            break;
         case FEMALE:
            System.out.println("woman");
            break;
         default:
            break;
      }
   }
}

enum SEX {
   MALE, FEMALE;
}Copy
```

编译器中执行的代码如下

```
public class Demo7 {
   /**     
    * 定义一个合成类（仅 jvm 使用，对我们不可见）     
    * 用来映射枚举的 ordinal 与数组元素的关系     
    * 枚举的 ordinal 表示枚举对象的序号，从 0 开始     
    * 即 MALE 的 ordinal()=0，FEMALE 的 ordinal()=1     
    */ 
   static class $MAP {
      //数组大小即为枚举元素个数，里面存放了case用于比较的数字
      static int[] map = new int[2];
      static {
         //ordinal即枚举元素对应所在的位置，MALE为0，FEMALE为1
         map[SEX.MALE.ordinal()] = 1;
         map[SEX.FEMALE.ordinal()] = 2;
      }
   }

   public static void main(String[] args) {
      SEX sex = SEX.MALE;
      //将对应位置枚举元素的值 赋给x，用于case操作
      int x = $MAP.map[sex.ordinal()];
      switch (x) {
         case 1:
            System.out.println("man");
            break;
         case 2:
            System.out.println("woman");
            break;
         default:
            break;
      }
   }
}

enum SEX {
   MALE, FEMALE;
}Copy
```

### 枚举类

```
enum SEX {
   MALE, FEMALE;
}
```

转换后的代码

```
public final class Sex extends Enum<Sex> {   
   //对应枚举类中的元素
   public static final Sex MALE;    
   public static final Sex FEMALE;    
   private static final Sex[] $VALUES;
   
    static {       
    	//调用构造函数，传入枚举元素的值及ordinal
    	MALE = new Sex("MALE", 0);    
        FEMALE = new Sex("FEMALE", 1);   
        $VALUES = new Sex[]{MALE, FEMALE}; 
   }
 	
   //调用父类中的方法
    private Sex(String name, int ordinal) {     
        super(name, ordinal);    
    }
   
    public static Sex[] values() {  
        return $VALUES.clone();  
    }
    public static Sex valueOf(String name) { 
        return Enum.valueOf(Sex.class, name);  
    }    
}
```

### final

java的常量分为**编译期常量和运行期常量**

编译期常量 如 final int a = 3*2;
运行期常量 如 final int b = new Random(100);
顾名思义，编译期常量的值在编译期就确定了。



# 类的加载阶段

**类的生命周期**

**类是在运行期间第一次使用时动态加载的，而不是一次性加载所有类。因为如果一次性加载，那么会占用很多的内存。**

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/335fe19c-4a76-45ab-9320-88c90d6a0d7e.png" width="600px"> </div><br>

包括以下 7 个阶段：

-   **加载（Loading）**  
-   **验证（Verification）**  
-   **准备（Preparation）**  
-   **解析（Resolution）**  
-   **初始化（Initialization）**  
- 使用（Using）
- 卸载（Unloading）

1. **类加载器只是负责加载class文件，不管是否可以运行**

2. **类的加载过程就是 加载类的信息存放于方法区**中。当然方法区中还有运行时常量池，还可能包含字符串字面量和数字常量（这部分常量信息（常量池）是Class文件中常量信息的内存映射）

   

## 类加载过程

包含了加载、验证、准备、解析和初始化这 5 个阶段。

![第02章_类的加载过程](D:\桌面\学习资料\程序猿\JVM上篇配图\第02章_类的加载过程.jpg)

#### 1. 加载

加载是类加载的一个阶段，注意不要混淆。

加载过程完成以下三件事：

1. 通过类的完全限定名称获取定义该类的**二进制字节流**。
2. 将该字节流表示的静态存储结构**转换**为方法区的运行时**存储结构**。其中二进制字节流可以从以下方式中获取：
   - 从 ZIP 包读取，成为 JAR、EAR、WAR 格式的基础。
   - 从网络中获取，最典型的应用是 Applet。
   - 运行时计算生成，例如动态代理技术，在 java.lang.reflect.Proxy 使用 ProxyGenerator.generateProxyClass 的代理类的二进制字节流。
   - 由其他文件生成，例如由 JSP 文件生成对应的 Class 类
3. 在内存中**生成**一个代表该类的 **Class 对象**，作为方法区中该类**各种数据的访问入口**。

字节流所代表的静态存储结构转化为方法去的运行时数据接口，**根据字节码在java堆中生成一个代表这个类的java.lang.Class对象**。（将类的字节码载入方法区（1.8后为元空间，在本地内存中）中），内部采用 C++ 的 instanceKlass 描述 java 类，它的重要 ﬁeld 有：

- _java_mirror 即 java 的类镜像，例如对 String 来说，它的镜像类就是 String.class，作用是把 klass 暴露给 java 使用
- _super 即父类
- _ﬁelds 即成员变量
- _methods 即方法
- _constants 即常量池
- _class_loader 即类加载器
- _vtable 虚方法表
- _itable 接口方法



- - 如果这个类还有父类没有加载，**先加载父类**
    父类静态代码块->子类静态代码块->父类构造代码块->父类构造函数->子类构造代码块->子类构造函数
  - 加载和链接可能是**交替运行**的

![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200611205050.png)

- instanceKlass保存在**方法区**。JDK 8以后，方法区位于元空间中，而元空间又位于本地内存中
- _java_mirror则是保存在**堆内存**中
- InstanceKlass和*.class对象(JAVA镜像类，这个类对象就是在堆内存)互相保存了对方的地址
- 类的对象在对象头中保存了*.class对象的地址。让对象可以通过其找到方法区中的instanceKlass，从而获取类的各种信息。

#### 2. 验证

确保 Class 文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。

它加载完成之后，就要执行链接，执行链接的时候，第一步需要做一个验证，**看class字节码的格式是否正确**。阻止不合法的类继续运行。

#### 3. 准备

**static变量在JDK 7以前是存储与instanceKlass末尾。但在JDK 7以后就存储在_java_mirror末尾了**，**即存储在堆中**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203204034109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**若类变量是被 static 修饰的变量，准备阶段为类变量分配内存**并设置初始值，使用的是**方法区的内存。**
我们通过javap反编译它，可以看到生成的字节码里，只有a的声明，没有a的赋值动作（会默认设置初始值），**赋值动作体现在cinit类的构造方法中**（static()）。而类的构造方法是在初始化阶段中被调用的。分配空间和赋值时两个不同的动作。

实例变量不会在这阶段分配内存，它会在对象实例化时随着对象一起被分配在堆中。**应该注意到，实例化不是类加载的一个过程**，**类加载发生在所有实例化操作之前，并且类加载只进行一次，实例化可以进行多次。**

初始值一般为 0 值，例如下面的**类变量 value** 被初始化为 0 而不是 123。

```java
public static int value = 123;
```

如果类变量是常量，那么它将初始化为表达式所定义的值而不是 0。例如下面的**常量 value 被初始化为 123 而不是 0。**

```java
public static final int value = 123;
```



```java
public class Demo{
	static int a;
	static int b=10;
}


\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
{
  static int a; \\准备阶段
    descriptor: I
    flags: (0x0008) ACC_STATIC

  static int b;
    descriptor: I
    flags: (0x0008) ACC_STATIC

  Demo();
    descriptor: ()V
    flags: (0x0000)
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 21: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   LDemo;

  static {}; \\cinit构造方法
    descriptor: ()V
    flags: (0x0008) ACC_STATIC
    Code:
      stack=2, locals=0, args_size=0
         0: bipush        10
         2: putstatic     #7                  // Field b:I
         5: getstatic     #13                 // Field java/lang/System.out:Ljava/io/PrintStream;
         8: getstatic     #19                 // Field a:I
        11: invokevirtual #22                 // Method java/io/PrintStream.println:(I)V
        14: return
      LineNumberTable:
        line 23: 0
        line 25: 5
        line 26: 14
}
SourceFile: "python.java"

```



#### 4. 解析

将**常量池的符号引用替换为直接引用的过程**。（即针对 类或接口、字段、**类方法**、接口方法、方法类型等）

其中解析过程在某些情况下可以在初始化阶段之后再开始，这是为了支持 Java 的动态绑定。

##### 4.1HSDB的使用

1. 先获得要查看的进程ID

```
jps
```

2. 打开HSDB

```
java -cp F:\JAVA\JDK8.0\lib\sa-jdi.jar sun.jvm.hotspot.HSDB
运行时可能会报错，是因为缺少一个.dll的文件，我们在JDK的安装目录中找到该文件，复制到缺失的文件下即可
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203211456349.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

3. 定位需要的进程

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203211514831.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203211532817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203211541619.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

##### 4.2 解析的含义

将常量池中的符号引用解析为直接引用

```
/**

 * 解析的含义
   */
   public class Load2 {
   public static void main(String[] args) throws ClassNotFoundException, IOException {
       ClassLoader classloader = Load2.class.getClassLoader();
       Class<?> c = classloader.loadClass("cn.itcast.jvm.t3.load.C");
       //new C();
       System.in.read();
   }
   }

class C {
    D d = new D();
}

class D {

}
```


使用loadClass加载类C的时候，只会导致类C的加载，而不会导致类C的解析以及初始化。从而类D也不会被加载和初始化。而如果我们加上new C()的时候，他会让C类加载解析和初始化，从而间接的让D类加载解析和初始化。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203212035578.png)


可以看到进程id为7452
然后打开HSDB进程工具，连接到7452,。

**打开HSDB**
**可以看到此时只加载了类C![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203212818562.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)**

查看类C的常量池，可以看到类D未被解析，只是存在于常量池中的符号，

类D是未被解析的类。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203212903918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

**我们查看类D被解析的情况**

       new C();
       System.in.read();

**解析以后，会将常量池中的符号引用解析为直接引用**

**可以看到，此时已加载并解析了类C和类D**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203213113138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203213157510.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NzM2NTk3,size_16,color_FFFFFF,t_70)

解析阶段将常量池中的符号引用解析为直接引用，符号引用仅仅是个符号，它并不知道类，也有可能是属性和方法在内存的哪个位置，但是经过了解析之后，变成了直接引用，它就能确切的知道它们在内存中的位置。

#### 5. 初始化

#####  &lt;clinit\>() 方法

**初始化阶段才真正开始执行类中定义的 Java 程序代码**。

**初始化阶段是虚拟机执行类构造器 &lt;clinit\>() 方法的过程。**

在准备阶段，类变量已经赋过一次系统要求的初始值，而在**初始化阶段，根据程序员通过程序制定的主观计划去初始化类变量和其它资源。**

**&lt;clinit\>()** 是由编译器**自动**收集类中所有**类变量**的赋值动作和**静态语句块**中的语句合并产生的，编译器收集的顺序由语句在源文件中出现的顺序决定。

**注意：类变量也称为静态变量，在类中以 static 关键字声明**

特别注意的是，静态语句块只能访问到定义在它之前的类变量，定义在它之后的类变量**只能赋值，不能访问**。例如以下代码：

```java
public class Test {
    static {
        i = 0;                // 给变量赋值可以正常编译通过
        System.out.print(i);  // 这句编译器会提示“非法向前引用”
    }
    static int i = 1;
}
```

由于父类的 &lt;clinit\>() 方法先执行，也就意味着父类中定义的静态语句块的执行要优先于子类。例如以下代码：

```java
static class Parent {
    public static int A = 1;
    static {
        A = 2;
    }
}

static class Sub extends Parent {
    public static int B = A;
}

public static void main(String[] args) {
     System.out.println(Sub.B);  // 2
}
```

接口中不可以使用静态语句块，但仍然有类变量初始化的赋值操作，因此**接口与类一样都会生成 &lt;clinit\>() 方法。**但接口与类不同的是，执行接口的 &lt;clinit\>() 方法**不需要先执行父接口的 &lt;clinit\>() 方法。**只有当父接口中定义的变量使用时，父接口才会初始化。另外**，接口的实现类在初始化时也一样不会执行接口的 &lt;clinit\>() 方法。**

**虚拟机会保证一个类的 &lt;clinit\>() 方法在多线程环境下被正确的加锁和同步**，**如果多个线程同时初始化一个类，只会有一个线程执行这个类的 &lt;clinit\>() 方法**，其它线程都会阻塞等待，直到活动线程执行 &lt;clinit\>() 方法完毕。如果在一个类的 &lt;clinit\>() 方法中有耗时的操作，就可能造成多个线程阻塞，在实际过程中此种阻塞很隐蔽。



习题

```java
public class Test
{
    public static Test t1 = new Test();
    {
         System.out.println("blockA");
    }
    static
    {
        System.out.println("blockB");
    }
    public static void main(String[] args)
    {
        Test t2 = new Test();
    }
 }
```

![image-20210930143414395](笔记图库/image-20210930143414395.png)

解析：

**类加载阶段的初始化阶段 编译器会按从上至下的顺序，收集所有 static 静态代码块和静态成员赋值的代码，合并为一个特殊的方法 < cinit> ()V** 

**题目中的**

```
static``  ``{``    ``System.out.println(``"blockB"``);``  ``}
```

在 t 2 创建时中就已经被合并到 **< cinit> ()V 中了，用于后续的执行 且只执行一次**

**因此创建t 1时，没有抢先执行 blockB，blockB 的所有权早已被 t 1 夺走了 ；**



##### 类初始化时机

###### 主动引用

虚拟机规范中并没有强制约束何时进行加载，但是规范严格规定了有且只有下列五种情况必须对类进行初始化（加载、验证、准备都会随之发生）：

- 遇到 new、getstatic、putstatic、invokestatic 这四条字节码指令时，如果类没有进行过初始化，则必须先触发其初始化。最常见的生成这 4 条指令的场景是：**使用 new 关键字实例化对象的时候；读取或设置一个类的静态字段（被 final 修饰、已在编译期把结果放入常量池的静态字段除外）的时候；以及调用一个类的静态方法的时候。**

- 使用 java.lang.reflect 包的方法对类进行反射调用的时候，如果类没有进行初始化，则需要先触发其初始化。

- **当初始化一个类的时候，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化。**

- **当虚拟机启动时，用户需要指定一个要执行的主类（包含 main() 方法的那个类），虚拟机会先初始化这个主类；**

- 当使用 JDK 1.7 的动态语言支持时，如果一个 java.lang.invoke.MethodHandle 实例最后的解析结果为 REF_getStatic, REF_putStatic, REF_invokeStatic 的方法句柄，并且这个方法句柄所对应的类没有进行过初始化，则需要先触发其初始化；

  

###### **被动引用**

除了这五种之外，其他的所有引用类的方式都不会触发初始化，称为被动引用。下面是被动引用的三个例子：

1. 通过子类引用父类的的静态字段，不会导致子类初始化。

2. 通过数组定义来引用类，不会触发此类的初始化。

```javascript
public class NotInitialization { 

  public static void main(String[] args) { 

    SuperClass[] sca = new SuperClass[10]; 

  }  

}
```

3. 常量在编译阶段会存入调用类的常量池中，本质上没有直接引用到定义常量的类，因此不会触发定义常量的类的初始化。

```java
public class ConstClass { 

  static { 

    System.out.println("ConstClass init!"); 

  } 

  public static final int value = 123; 

} 

public class NotInitialization{ 

  public static void main(String[] args) { 
    int x = ConstClass.value; 
  } 

} 
```

上述代码运行之后，也没有输出“ConstClass init！”，这是因为虽然在Java源码中引用了ConstClass类中的常量HELLOWORLD，但其实**在编译阶段通过常量传播优化，已经将此常量的值“hello world”存储到了NotInitialization类的常量池中，以后NotInitialization对常量ConstClass.HELLOWORLD的引用实际都被转化为NotInitialization类对自身常量池的引用了。也就是说，实际上NotInitialization的Class文件之中并没有ConstClass类的符号引用入口，这两个类在编译成Class之后就不存在任何联系了。**

## 类与类加载器

两个类相等，需要类本身相等，并且使用同一个类加载器进行加载。这是因为每一个类加载器都拥有一个独立的类名称空间。

这里的相等，包括类的 Class 对象的 equals() 方法、isAssignableFrom() 方法、isInstance() 方法的返回结果为 true，也包括使用 instanceof 关键字做对象所属关系判定结果为 true。

## 类加载器分类

从 Java 虚拟机的角度来讲，只存在以下两种不同的类加载器：

- 启动类加载器（Bootstrap ClassLoader），使用 C++ 实现，是虚拟机自身的一部分；

- 所有其它类的加载器，使用 Java 实现，独立于虚拟机，继承自抽象类 java.lang.ClassLoader。

从 Java 开发人员的角度看，类加载器可以划分得更细致一些：

- **启动类加载器（Bootstrap ClassLoader）**此类加载器负责将**存放在 &lt;JRE_HOME\>\lib 目录中的，或者被 -Xbootclasspath 参数所指定的路径中的**，并且是虚拟机识别的（仅按照文件名识别，如 rt.jar，名字不符合的类库即使放在 lib 目录中也不会被加载）类库加载到虚拟机内存中。启动类加载器无法被 Java 程序直接引用，用户在编写自定义类加载器时，如果需要把加载请求委派给启动类加载器，**直接使用 null 代替即可**。

- **扩展类加载器（Extension ClassLoader）**这个类加载器是由 ExtClassLoader（sun.misc.Launcher$ExtClassLoader）实现的。它负责将 **&lt;JAVA_HOME\>/lib/ext** 或者被 java.ext.dir 系统变量所指定路径中的所有类库加载到内存中，开发者可以直接使用扩展类加载器。

- **应用程序类加载器（Application ClassLoader）**这个类加载器是由 AppClassLoader（sun.misc.Launcher$AppClassLoader）实现的。由于这个类加载器是 ClassLoader 中的 getSystemClassLoader() 方法的返回值，因此一般称为系统类加载器。它负责加载用户类路径（ClassPath）上所指定的类库，开发者可以直接使用这个类加载器，如果应用程序中没有自定义过自己的类加载器，一般情况下这个就是程序中默认的类加载器。

## 双亲委派模型

应用程序是由三种类加载器互相配合从而实现类加载，除此之外还可以加入自己定义的类加载器。

下图展示了类加载器之间的层次关系，称为双亲委派模型（Parents Delegation Model）。该模型要求除了顶层的启动类加载器外，其它的类加载器都要有自己的父类加载器。这里的父子关系一般通过组合关系（Composition）来实现，而不是继承关系（Inheritance）。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/0dd2d40a-5b2b-4d45-b176-e75a4cd4bdbf.png" width="500px"> </div><br>

#### 1. 工作过程

一个类加载器首先将类加载请求转发到父类加载器，只有当父类加载器无法完成时才尝试自己加载。

#### 2. 好处

使得 Java 类随着它的类加载器一起具有一种带有优先级的层次关系，从而使得基础类得到统一。

例如 java.lang.Object 存放在 rt.jar 中，如果编写另外一个 java.lang.Object 并放到 ClassPath 中，程序可以编译通过。由于双亲委派模型的存在，所以在 rt.jar 中的 Object 比在 ClassPath 中的 Object 优先级更高，这是因为 rt.jar 中的 Object 使用的是启动类加载器，而 ClassPath 中的 Object 使用的是应用程序类加载器。rt.jar 中的 Object 优先级更高，那么程序中所有的 Object 都是这个 Object。

#### 3. 实现

以下是抽象类 java.lang.ClassLoader 的代码片段，其中的 loadClass() 方法运行过程如下：先检查类是否已经加载过，如果没有则让父类加载器去加载。当父类加载器加载失败时抛出 ClassNotFoundException，此时尝试自己去加载。

```java
public abstract class ClassLoader {
    // The parent class loader for delegation
    private final ClassLoader parent;

    public Class<?> loadClass(String name) throws ClassNotFoundException {
        return loadClass(name, false);
    }

    protected Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {
        synchronized (getClassLoadingLock(name)) {
            // First, check if the class has already been loaded
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                try {
                    if (parent != null) {
                        c = parent.loadClass(name, false);
                    } else {
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }

                if (c == null) {
                    // If still not found, then invoke findClass in order
                    // to find the class.
                    c = findClass(name);
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
    }

    protected Class<?> findClass(String name) throws ClassNotFoundException {
        throw new ClassNotFoundException(name);
    }
}
```

## 自定义类加载器实现

以下代码中的 FileSystemClassLoader 是自定义类加载器，**继承自 java.lang.ClassLoader**，用于加载文件系统上的类。它首先根据类的全名在文件系统上查找类的字节代码文件（.class 文件），然后读取该文件内容，最后通过 defineClass() 方法来把这些字节代码转换成 java.lang.Class 类的实例。

**java.lang.ClassLoader 的 loadClass() 实现了双亲委派模型的逻辑，自定义类加载器一般不去重写它，但是需要重写 findClass() 方法。**

```java
public class FileSystemClassLoader extends ClassLoader {

    private String rootDir;

    public FileSystemClassLoader(String rootDir) {
        this.rootDir = rootDir;
    }

    protected Class<?> findClass(String name) throws ClassNotFoundException {
        byte[] classData = getClassData(name);
        if (classData == null) {
            throw new ClassNotFoundException();
        } else {
            return defineClass(name, classData, 0, classData.length);
        }
    }

    private byte[] getClassData(String className) {
        String path = classNameToPath(className);
        try {
            InputStream ins = new FileInputStream(path);
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            int bufferSize = 4096;
            byte[] buffer = new byte[bufferSize];
            int bytesNumRead;
            while ((bytesNumRead = ins.read(buffer)) != -1) {
                baos.write(buffer, 0, bytesNumRead);
            }
            return baos.toByteArray();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

    private String classNameToPath(String className) {
        return rootDir + File.separatorChar
                + className.replace('.', File.separatorChar) + ".class";
    }
}
```

## 参考资料

- 周志明. 深入理解 Java 虚拟机 [M]. 机械工业出版社, 2011.
- [Chapter 2. The Structure of the Java Virtual Machine](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.5.4)
- [Jvm memory](https://www.slideshare.net/benewu/jvm-memory)
[Getting Started with the G1 Garbage Collector](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/index.html)
- [JNI Part1: Java Native Interface Introduction and “Hello World” application](http://electrofriends.com/articles/jni/jni-part1-java-native-interface/)
- [Memory Architecture Of JVM(Runtime Data Areas)](https://hackthejava.wordpress.com/2015/01/09/memory-architecture-by-jvmruntime-data-areas/)
- [JVM Run-Time Data Areas](https://www.programcreek.com/2013/04/jvm-run-time-data-areas/)
- [Android on x86: Java Native Interface and the Android Native Development Kit](http://www.drdobbs.com/architecture-and-design/android-on-x86-java-native-interface-and/240166271)
- [深入理解 JVM(2)——GC 算法与内存分配策略](https://crowhawk.github.io/2017/08/10/jvm_2/)
- [深入理解 JVM(3)——7 种垃圾收集器](https://crowhawk.github.io/2017/08/15/jvm_3/)
- [JVM Internals](http://blog.jamesdbloom.com/JVMInternals.html)
- [深入探讨 Java 类加载器](https://www.ibm.com/developerworks/cn/java/j-lo-classloader/index.html#code6)
- [Guide to WeakHashMap in Java](http://www.baeldung.com/java-weakhashmap)
- [Tomcat example source code file (ConcurrentCache.java)](https://alvinalexander.com/java/jwarehouse/apache-tomcat-6.0.16/java/org/apache/el/util/ConcurrentCache.java.shtml)
