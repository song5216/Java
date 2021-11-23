

Java 基础

# IntelliJ IDEA项目更换JDK版本

点击File–>Project Structure，按照如图所示修改
1、修改SDKs，将新的JDK的路径加载进来
![这里写图片描述](https://img-blog.csdn.net/20180711203526250?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dyZWVuNzAzMzM4MTMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
2、修改Project的sdk
![这里写图片描述](https://img-blog.csdn.net/2018071120360716?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dyZWVuNzAzMzM4MTMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
3、点击new将新的Jdk加进来
![这里写图片描述](https://img-blog.csdn.net/20180711204041999?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dyZWVuNzAzMzM4MTMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
4、保存就好了。

# JAVA工具

**javac**.exe是编译功能javaCompiler

**java**.exe是执行程序，用于执行编译好的.class文件

**javadoc**.exe用来制作java文档

**jdb**.exe是java的调试器

**javaprof**.exe是剖析工具



# 程序设计六大准则

**1、开闭原则（Open Close Principle）**

开闭原则的意思是：**对扩展开放，对修改关闭**。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。

**2、里氏代换原则（Liskov Substitution Principle）**

里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

**3、依赖倒转原则（Dependence Inversion Principle）**

这个原则是开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖于具体。

**4、接口隔离原则（Interface Segregation Principle）**

这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。

**5、迪米特法则，又称最少知道原则（Demeter Principle）**

最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

**6、合成复用原则（Composite Reuse Principle）**

合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承

# 数据类型

## 基本类型

*Java中的默认类型：整数类型是int 、浮点类型是double* 。

*1字节（byte） = 8位（bit）*

 *在16位的系统中（比如8086微机） 1字 （word）= 2字节（byte）= 16（bit）*

 *在32位的系统中（比如win32） 1字（word）= 4字节（byte）=32（bit）*

 *在64位的系统中（比如win64）1字（word）= 8字节（byte）=64（bit）*

- byte/8  一字节
- char/16 两字节
- short/16 两字节
- int/32 **四字节**
- float/32 四字节
- long/64 八字节
- double/64 八字节
- boolean/\~



boolean 只有两个值：true、false，可以使用 1 bit 来存储，但是具体大小没有明确规定。JVM 会在编译时期将 boolean 类型的数据转换为 int，使用 1 来表示 true，0 表示 false。**JVM 支持 boolean 数组，但是是通过读写 byte 数组来实现的。**

Java语言提供了八种基本类型。六种数字类型（四个整数型，两个浮点型），一种字符类型，还有一种布尔型。

#### 分类

Java中的**四类**八种基本数据类型

第一类：**整数**类型  byte short int long

第二类：**浮点型**  float double

第三类：**逻辑型**   boolean(它只有两个值可取true false)

第四类：**字符型**  char

#### **byte：**

- byte 数据类型是8位、有符号的，以二进制补码表示的整数；
- 最小值是 **-128（-2^7）**；
- 最大值是 **127（2^7-1）**；
- 默认值是 **0**；
- byte 类型用在大型数组中节约空间，主要代替整数，因为 byte 变量占用的空间只有 int 类型的四分之一；
- 例子：byte a = 100，byte b = -50。

#### **short：**

- short 数据类型是 16 位、有符号的以二进制补码表示的整数  **5位整数**
- 最小值是 **-32768（-2^15）**；
- 最大值是 **32767（2^15 - 1）**；
- Short 数据类型也可以像 byte 那样节省空间。一个short变量是int型变量所占空间的二分之一；
- 默认值是 **0**；
- 例子：short s = 1000，short r = -20000。

#### **int：**

- int 数据类型是32位、有符号的以二进制补码表示的整数；**10位整数**
- 最小值是 **-2,147,483,648（-2^31）**；
- 最大值是 **2,147,483,647（2^31 - 1）**；
- 一般地整型变量默认为 int 类型；
- 默认值是 **0** ；
- 例子：int a = 100000, int b = -200000。

#### **long：**

- long 数据类型是 64 位、有符号的以二进制补码表示的整数；
- 最小值是 **-9,223,372,036,854,775,808（-2^63）**；
- 最大值是 **9,223,372,036,854,775,807（2^63 -1）**；
- 这种类型主要使用在需要比较大整数的系统上；
- 默认值是 **0L**；
- 例子： long a = 100000**L**，Long b = -200000**L**。
  "L"理论上不分大小写，但是若写成"l"容易与数字"1"混淆，不容易分辩。所以最好大写。

#### **float：**

- float 数据类型是单精度、32位、符合IEEE 754标准的浮点数；
- float 在储存大型浮点数组的时候可节省内存空间；
- 默认值是 **0.0f**；
- 浮点数不能用来表示精确的值，如货币；
- 例子：float f1 = 234.5f。

#### **double：**

- double 数据类型是双精度、64 位、符合 IEEE 754 标准的浮点数；

- **浮点数的默认类型**为 double 类型；

- double类型同样不能表示精确的值，如货币；

- 默认值是 **0.0d**；

- **在Java中，如果你输入一个小数，系统默认的是double类型的，**

- **这个式子：**float f=double 11.1，明显错误，如果想要表达11.1为float类型的，需要在**11.1末尾加一个f标识**你输入的是float类型即可

- 例子：

  ```
  double   d1  = 7D ;
  double   d2  = 7.; 
  double   d3  =  8.0; 
  double   d4  =  8.D; 
  double   d5  =  12.9867; 
  ```

  7 是一个 int 字面量，而 7D，7. 和 8.0 是 double 字面量。

#### **boolean：**

- boolean数据类型表示一位的信息；
- 只有两个取值：true 和 false；
- 这种类型只作为一种标志来记录 true/false 情况；
- 默认值是 **false**；
- 例子：boolean one = true。

#### **char：**

- char 类型是一个单一的 16 位 Unicode 字符；
- 最小值是 **\u0000**（十进制等效值为 0）；
- 最大值是 **\uffff**（即为 65535）；
- char 数据类型可以储存任何字符；
- 例子：char letter = 'A';。



```java
public class Variable {
public static void main(String[] args){
//定义字节型变量
byte b = 100;
System.out.println(b);
//定义短整型变量
short s = 1000;
System.out.println(s);
//定义整型变量
int i = 123456;
System.out.println(i);
//定义长整型变量
long l = 12345678900L;
System.out.println(l);
//定义单精度浮点型变量
float f = 5.5F;
System.out.println(f);
//定义双精度浮点型变量
double d = 8.5;
System.out.println(d);
//定义布尔型变量
boolean bool = false;
System.out.println(bool);
//定义字符型变量
char c = 'A';
System.out.println(c);
}
}
```

1. 大小写字母ASCII码是不一样的，比如大写字母**A的ASCII码是65**，小写字母a的ASCII码是**97**。
2. 同样字母，大写字母的ASCII码值比小写字母的ASCII码值小，比如大写字母A的ASCII码值65比小写字母a的ASCII码值97小。
3. 大写字母**A~Z的ASCII码值从65~90**，其实只要记住第一个A的ASCII码值65，后面的字母的ASCII码值累加上去就行了。
4. 小写字母**a~z的ASCII码值从97~122**，其实只要记住第一个a的ASCII码值97，后面的字母的ASCII码值累加上去就行了。
5. 同样的字母，小写字母的ASCII码值比大写字母的ASCII码值大32，**小写字母ASCII = 大写字母ASCII + 32。**
6. 字母大小写转换的方法：
   1. 统一转成大写：ch & 0b11011111 简写：ch & 0xDF
   2. 统一转成小写：ch | 0b00100000 简写：ch | 0x20

#### 变量的作用域

从变量的第一行定义开始，直到变量所属的大括号结束

#### 自动转换

Java程序中要求参与的计算的数据，必须要保证数据类型的一致性，如果数据类型不一致将发生类型的转换。

```java
byte、short、char‐‐>int‐‐>long‐‐>float‐‐>double
```

注意：

**运算过程中byte,short,char型的值将被提升为int型**

```
public class Main {
        public static void main(String[] args) {
            //Scanner scanner = new Scanner(System.in);在线笔试  
            //byte测试
            byte b1=1,b2=2,b3,b6,b8;
            final byte b4=4,b5=6,b7;
            b3=(b1+b2);  /*错误行,提示错误:Incompatible types: Required byte,Found int.*/
            b6=b4+b5;    /*错误行,提示错误:Incompatible types: Required byte,Found int.*/
            b8=(b1+b4);  /*错误行,提示错误:Incompatible types: Required byte,Found int.*/
            b7=(b2+b5);  /*错误行,提示错误:Incompatible types: Required byte,Found int.*/
            System.out.println(b3+b6);

        }
    }

```

#### 强制转换

boolean类型不能和任何类型进行转换

#### float 与 double

Java 不能隐式执行向下转型，因为这会使得精度降低。

1.1 字面量属于 double 类型，不能直接将 1.1 直接赋值给 float 变量，因为这是向下转型。

```java
// float f = 1.1;
```

1.1f 字面量才是 float 类型。

```java
float f = 1.1f;
```

#### 隐式类型转换

因为字面量 1 是 int 类型，它比 short 类型精度要高，因此不能隐式地将 int 类型向下转型为 short 类型。

```java
short s1 = 1;
// s1 = s1 + 1;
```

但是使用 += 或者 ++ 运算符会执行隐式类型转换。

```java
s1 += 1;
s1++;
```

上面的语句相当于将 s1 + 1 的计算结果进行了向下转型：

```java
s1 = (short) (s1 + 1);
```

[StackOverflow : Why don't Java's +=, -=, *=, /= compound assignment operators require casting?](



## 引用数据类型

字符串 数组 类 接口 Lambda



## 数组

### **声明数组变量**

首先必须声明数组变量，才能在程序中使用数组。下面是声明数组变量的语法：

```java
dataType[] arrayRefVar;   // 首选的方法  
dataType arrayRefVar[];  // 效果相同，但不是首选方法
new int[]{left,right}
```

在java 中，**声明一个数组时，不能直接限定数组长度，只有在创建实例化对象时，才能对给定数组长度.**。

### **创建数组**

Java语言使用new操作符来创建数组，语法如下：

```java
arrayRefVar = new dataType[arraySize];
new int[]{left,right}
```

上面的语法语句做了两件事：

- 一、使用 dataType[arraySize] 创建了一个数组。
- 二、把新创建的数组的引用赋值给变量 arrayRefVar。

**数组变量的声明，和创建数组可以用一条语句完成，如下所示：**

```
dataType[] arrayRefVar = new dataType[arraySize];
```

**另外，你还可以使用如下的方式创建数组。**

```
dataType[] arrayRefVar = {value0, value1, ..., valuek};
```

数组的元素是通过索引访问的。数组索引从 0 开始，所以索引值从 0 到 arrayRefVar.length-1。

### **二维数组**

其实是一位数组的嵌套（每一行看做一个内层的一维数组）

 两种初始化形式

    格式1: 动态初始化
数据类型 数组名 [ ][ ] = new 数据类型[m][n]
数据类型 [ ][ ]  数组名 = new 数据类型[m][n]
**数据类型 [ ]   数组名 [ ] = new 数据类型[m] [n]

举例：int [ ][ ]  arr=new  int [5][3];  也可以理解为“5行3例”

格式2: 静态初始化
数据类型 [ ][ ]   数组名 = {{元素1,元素2....},{元素1,元素2....},{元素1,元素2....}.....};

举例：int [ ][ ]  arr={{22,15,32,20,18},{12,21,25,19,33},{14,58,34,24,66},};

静态初始化可用于不规则二维数组的初始化

	public static void main(String[]args){
	int [][] arr=new int[][]{{4,5,6,8},{2,3},{1,6,9}};
		System.out.println(arr.length);//输出行数
		System.out.println(arr[0].length);//输出列数
		
	}
### 处理数组

**数组的元素类型和数组的大小都是确定的**，所以当处理数组元素时候，我们通常使用基本循环或者 For-Each 循环。

示例

该实例完整地展示了如何创建、初始化和操纵数组：

TestArray.java 文件代码：

```
public class TestArray {
   public static void main(String[] args) {
      double[] myList = {1.9, 2.9, 3.4, 3.5};
 
      // 打印所有数组元素
      for (int i = 0; i < myList.length; i++) {
         System.out.println(myList[i] + " ");
      }
      // 计算所有元素的总和
      double total = 0;
      for (int i = 0; i < myList.length; i++) {
         total += myList[i];
      }
      System.out.println("Total is " + total);
      // 查找最大元素
      double max = myList[0];
      for (int i = 1; i < myList.length; i++) {
         if (myList[i] > max) max = myList[i];
      }
      System.out.println("Max is " + max);
   }
}
```

以上实例编译运行结果如下：

```
1.9
2.9
3.4
3.5
Total is 11.7
Max is 3.5
```

------

### For-Each 循环

JDK 1.5 引进了一种新的循环类型，被称为 For-Each 循环或者加强型循环，它能在不使用下标的情况下遍历数组。

语法格式如下：

```
for(type element: array)
{
    System.out.println(element);
}
```

**实例**

该实例用来显示数组 myList 中的所有元素：

TestArray.java 文件代码：

```
public class TestArray {
   public static void main(String[] args) {
      double[] myList = {1.9, 2.9, 3.4, 3.5};
 
      // 打印所有数组元素
      for (double element: myList) {
         System.out.println(element);
      }
   }
}
```

以上实例编译运行结果如下：

```
1.9
2.9
3.4
3.5
```

------

### 多维数组

多维数组可以看成是数组的数组，比如二维数组就是一个特殊的一维数组，其每一个元素都是一个一维数组，例如：

String str[][] = new String[3] [4];

##### 多维数组的动态初始化（以二维数组为例）

1. 直接为每一维分配空间，格式如下：

type[][] typeName = new type [typeLength1] [typeLength2];

type 可以为基本数据类型和复合数据类型，typeLength1 和 typeLength2 必须为正整数，typeLength1 为行数，typeLength2 为列数。

例如：

int a[][] = new int[2] [3];

解析：

二维数组 a 可以看成一个两行三列的数组。

2. 从最高维开始，分别为每一维分配空间，例如：

```
String s[][] = new String[2][];
s[0] = new String[2];
s[1] = new String[3];
s[0][0] = new String("Good");
s[0][1] = new String("Luck");
s[1][0] = new String("to");
s[1][1] = new String("you");
s[1][2] = new String("!");
```

解析：

**s[0]=new String[2]** 和 **s[1]=new String[3]** 是为最高维分配引用空间，也就是为最高维限制其能保存数据的最长的长度，然后再为其每个数组元素单独分配空间 **s0=new String("Good")** 等操作。

##### 多维数组的引用（以二维数组为例）

对二维数组中的每个元素，引用方式为 **arrayName[index1] [index2]**，例如：

```
num[1] [0];
```

## Arrays 类

java.util.Arrays 类能方便地操作数组，它提供的所有方法都是**静态的。**

具有以下功能：

- 给数组赋值：通过 fill 方法。
- 对数组排序：通过 sort 方法,按升序。 **没有返回值，直接对原数组进行操作**
- 比较数组：通过 equals 方法比较数组中元素值是否相等。
- 查找数组元素：通过 binarySearch 方法能对排序好的数组进行二分查找法操作。
- `arrays`的`toString`方法，对于数组的转化成字符
- 数组复制

具体说明请查看下表：

### 方法

| 序号 | 方法和说明                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **public static int binarySearch(Object[] a, Object key)** 用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(*插入点*) - 1)。 |
| 2    | **public static boolean equals(long[] a, long[] a2)** 如果两个指定的 long 型数组彼此*相等*，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |
| 3    | **public static void fill(int[] a, int val)** 将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |
| 4    | **public static void sort(Object[] a)** 对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |

### asList ()

**public static <T> List<T> asList(T... a)**

数组转换成List，可以使用Arrays.asList ()或者Collections.addAll ()方法。

**尽量使用引用类型的数组；**

```
List<String> stooges = Arrays.asList("Larry", "Moe", "Curly");

```

```java
Integer[] array = {1};
List<List<Integer>> list = new ArrayList();
list.add(Arrays.asList(array));

```



### copyOf()

Arrays的copyOf()方法传回的数组是**新的数组对象**，改变传回数组中的元素值，不会影响原来的数组。

copyOf()的第二个自变量指定要建立的**新数组长度**，如果新数组的长度**超过**原数组的长度，则**保留**数组默认值，例如：

```
import java.util.Arrays;

public class ArrayDemo {
	public static void main(String[] args) {
    	int[] arr1 = {1, 2, 3, 4, 5}; 
    	int[] arr2 = Arrays.copyOf(arr1, 5);
    	int[] arr3 = Arrays.copyOf(arr1, 10);
    	for(int i = 0; i < arr2.length; i++) 
        	System.out.print(arr2[i] + " "); 
    		System.out.println();
    	for(int i = 0; i < arr3.length; i++) 
        	System.out.print(arr3[i] + " ");
	}
} 
1234567891011121314
```

运行结果：

```
1 2 3 4 5 
1 2 3 4 5 0 0 0 0 0
```

与System.arrayCopy**最主要的区别**

区别是**Arrays.copyOf（）** 不只复制数组元素，也创建一个新数组。

**System.arrayCopy** 只复制已有的数组。

但是如果我们读Arrays.copyOf（）的源码也会发现，它也用到了System.arrayCopy（）。

 

```
   public static int[] copyOf(int[] original, int newLength) {
        int[] copy = new int[newLength];
        System.arraycopy(original, 0, copy, 0,
                         Math.min(original.length, newLength));
        return copy
```



#### copyOfRange

```
public static int[] copyOfRange(int[] original,
                                int from,
                                int to)
```

将指定数组的指定范围复制到新数组中。 范围（ `from`  ）的初始指数必须在零和`original.length`之间，包括在内。  `original[from]`的值被放置在副本的初始元素中（除非`from ==  original.length`或`from == to` ）。  原始数组中后续元素的值将被放置在副本中的后续元素中。 必须大于或等于`from`的范围（  `to` ）的最终指数可能大于`original.length`  ，在这种情况下`0`被放置在其索引大于或等于`original.length - from`的副本的所有元素中。  返回的数组的长度将为`to - from` 

- 参数 

  `original` - 要从中复制范围的数组 

  `from` - 要复制的范围的初始索引（包括） 

  `to` - 要复制的范围的最终索引，排他。 （该索引可能位于数组之外） 

- 结果 

  一个包含原始数组的指定范围的新数组，用零截取或填充以获得所需的长度 

- 异常 

  `ArrayIndexOutOfBoundsException`  - 如果 `from < 0`或 `from > original.length` 

  `IllegalArgumentException`  - 如果 `from > to` 

  `NullPointerException`  - 如果 `original`为空 



## 包装/引用类型 Number 类

**不可变类**

一般地，当需要使用数字的时候，我们通常使用内置数据类型，如：**byte、int、long、double** 等。

实例

int a = 5000; float b = 13.65f; byte c = 0x4a;

然而，在实际开发过程中，我们经常会遇到需要使用对象，而不是内置数据类型的情形。为了解决这个问题，Java 语言为每一个内置数据类型提供了对应的包装类。

所有的包装类**（Integer、Long、Byte、Double、Float、Short）**都是抽象类 Number 的子类。

| 包装类                                                       | 基本数据类型 |
| :----------------------------------------------------------- | :----------- |
| Boolean                                                      | boolean      |
| Byte                                                         | byte         |
| Short                                                        | short        |
| Integer                                                      | int          |
| Long                                                         | long         |
| Character                                                    | char         |
| Float                                                        | float        |
| Double <public **final** class Double extends Number implements Comparable<Double> | double       |

![Java Number类](笔记图库/OOP_WrapperClass.png)

这种由编译器特别支持的包装称为装箱，所以当内置数据类型被当作对象使用的时候，编译器会把内置类型装箱为包装类。相似的，编译器也可以把一个对象拆箱为内置类型。Number 类属于 java.lang 包。

下面是一个使用 Integer 对象的实例：

基本类型都有对应的包装类型，基本类型与其对应的包装类型之间的赋值使用自动装箱与拆箱完成。

**数组都是引用类型**

```java
Integer x = 2;     // 装箱 调用了 Integer.valueOf(2)
int y = x;         // 拆箱 调用了 X.intValue()
```

- [Autoboxing and Unboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)

## 注意点

**Integer类型在-128-->127范围之间是被缓存**，也就是每个对象的内存地址是相同的，赋值就直接从缓存中取，不会有新的对象产生，而大于这个范围，将会重新创建一个Integer对象，也就是new一个对象出来，当然地址就不同了，也就！=；

## 缓存池

new Integer(123) 与 Integer.valueOf(123) 的区别在于：

- new Integer(123) 每次都会新建一个对象；
- Integer.valueOf(123) 会使用缓存池中的对象，多次调用会取得同一个对象的引用。

```java
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   // true
```

valueOf() 方法的实现比较简单，就是先判断值是否在缓存池中，如果在的话就直接返回缓存池的内容。

```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```

在 Java 8 中，Integer 缓存池的大小默认为 -128\~127。

```java
static final int low = -128;
static final int high;
static final Integer cache[];

static {
    // high value may be configured by property
    int h = 127;
    String integerCacheHighPropValue =
        sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
    if (integerCacheHighPropValue != null) {
        try {
            int i = parseInt(integerCacheHighPropValue);
            i = Math.max(i, 127);
            // Maximum array size is Integer.MAX_VALUE
            h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
        } catch( NumberFormatException nfe) {
            // If the property cannot be parsed into an int, ignore it.
        }
    }
    high = h;

    cache = new Integer[(high - low) + 1];
    int j = low;
    for(int k = 0; k < cache.length; k++)
        cache[k] = new Integer(j++);

    // range [-128, 127] must be interned (JLS7 5.1.7)
    assert IntegerCache.high >= 127;
}
```

编译器会在自动装箱过程调用 valueOf() 方法，因此多个值相同且值在缓存池范围内的 Integer 实例使用自动装箱来创建，那么就会引用相同的对象。

```java
Integer m = 123;
Integer n = 123;
System.out.println(m == n); // true
```

基本类型对应的缓冲池如下：

- boolean values true and false
- all byte values
- short values between -128 and 127
- int values between -128 and 127
- char in the range \u0000 to \u007F

在使用这些基本类型对应的包装类型时，如果该数值范围在缓冲池范围内，就可以直接使用缓冲池中的对象。

在 jdk 1.8 所有的数值类缓冲池中，Integer 的缓冲池 IntegerCache 很特殊，这个缓冲池的下界是 - 128，上界默认是 127，但是这个上界是可调的，在启动 jvm 的时候，通过 -XX:AutoBoxCacheMax=&lt;size&gt; 来指定这个缓冲池的大小，该选项在 JVM 初始化的时候会设定一个名为 java.lang.IntegerCache.high 系统属性，然后 IntegerCache 初始化的时候就会读取该系统属性来决定上界。

##  Math类

```
public final class Math extends Object
```

Math 包含了用于执行基本数学运算的属性和方法，如初等指数、对数、平方根和三角函数。

**Math 的方法都被定义为 static 形式，通过 Math 类可以在主函数中直接调用。**

**Test.java** 文件代码：

```java
public class Test {  
    public static void main (String []args)  
    {  
        System.out.println("90 度的正弦值：" + Math.sin(Math.PI/2));  
        System.out.println("0度的余弦值：" + Math.cos(0));  
        System.out.println("60度的正切值：" + Math.tan(Math.PI/3));  
        System.out.println("1的反正切值： " + Math.atan(1));  
        System.out.println("π/2的角度值：" + Math.toDegrees(Math.PI/2));  
        System.out.println(Math.PI);  
    }  
}
```

以上实例编译运行结果如下：

```
90 度的正弦值：1.0
0度的余弦值：1.0
60度的正切值：1.7320508075688767
1的反正切值： 0.7853981633974483
π/2的角度值：90.0
3.141592653589793
```

下面的表中列出的是 Number & Math 类常用的一些方法：

| 序号 | 方法与描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [xxxValue()](https://www.runoob.com/java/number-xxxvalue.html) 将 Number 对象转换为xxx数据类型的值并返回。 |
| 2    | [compareTo()](https://www.runoob.com/java/number-compareto.html) 将number对象与参数比较。 |
| 3    | [equals()](https://www.runoob.com/java/number-equals.html) 判断number对象是否与参数相等。 |
| 4    | [valueOf()](https://www.runoob.com/java/number-valueof.html) 返回一个 Number 对象指定的内置数据类型 |
| 5    | [toString()](https://www.runoob.com/java/number-tostring.html) 以字符串形式返回值。 |
| 6    | [parseInt()](https://www.runoob.com/java/number-parseInt.html) 将字符串解析为int类型。 |
| 7    | [abs()](https://www.runoob.com/java/number-abs.html) 返回参数的绝对值。 |
| 8    | [ceil()](https://www.runoob.com/java/number-ceil.html) 返回大于等于( >= )给定参数的的最小整数，类型为双精度浮点型。 |
| 9    | [floor()](https://www.runoob.com/java/number-floor.html) 返回小于等于（<=）给定参数的最大整数 。 |
| 10   | [rint()](https://www.runoob.com/java/number-rint.html) 返回与参数最接近的整数。返回类型为double。 |
| 11   | [round()](https://www.runoob.com/java/number-round.html) 它表示**四舍五入**，算法为 **Math.floor(x+0.5)**，即将原来的数字加上 0.5 后再向下取整，所以，Math.round(11.5) 的结果为12，Math.round(-11.5) 的结果为-11。 |
| 12   | [min()](https://www.runoob.com/java/number-min.html) 返回两个参数中的最小值。 |
| 13   | [max()](https://www.runoob.com/java/number-max.html) 返回两个参数中的最大值。 |
| 14   | [exp()](https://www.runoob.com/java/number-exp.html) 返回自然数底数e的参数次方。 |
| 15   | [log()](https://www.runoob.com/java/number-log.html) 返回参数的自然数底数的对数值。 |
| 16   | [pow()](https://www.runoob.com/java/number-pow.html) 返回第一个参数的第二个参数次方。 |
| 17   | [sqrt()](https://www.runoob.com/java/number-sqrt.html) 求参数的算术平方根。 |
| 18   | [sin()](https://www.runoob.com/java/number-sin.html) 求指定double类型参数的正弦值。 |
| 19   | [cos()](https://www.runoob.com/java/number-cos.html) 求指定double类型参数的余弦值。 |
| 20   | [tan()](https://www.runoob.com/java/number-tan.html) 求指定double类型参数的正切值。 |
| 21   | [asin()](https://www.runoob.com/java/number-asin.html) 求指定double类型参数的反正弦值。 |
| 22   | [acos()](https://www.runoob.com/java/number-acos.html) 求指定double类型参数的反余弦值。 |
| 23   | [atan()](https://www.runoob.com/java/number-atan.html) 求指定double类型参数的反正切值。 |
| 24   | [atan2()](https://www.runoob.com/java/number-atan2.html) 将笛卡尔坐标转换为极坐标，并返回极坐标的角度值。 |
| 25   | [toDegrees()](https://www.runoob.com/java/number-todegrees.html) 将参数转化为角度。 |
| 26   | [toRadians()](https://www.runoob.com/java/number-toradians.html) 将角度转换为弧度。 |
| 27   | [random()](https://www.runoob.com/java/number-random.html) 返回一个随机数。不接受任何参数；随机数范围为 0.0 =< Math.random < 1.0 |

# 标识符

Java标识符命名规范是：
1）只能包含字母a-zA-Z，数字``0``-``9``，下划线_和美元符号$；
2）首字母不能为数字；
3）**关键字和保留字不能作为标识符。**
**null是关键字，NULL不是关键字，java区分大小写。**

**关键字都小写**



# String类

字符串广泛应用 在 Java 编程中，**在 Java 中字符串属于对象**，Java 提供了 String 类来创建和操作字符串。

**字符串不能进行减法；**ACSLL码是针对字符的

## 概览

**String 被声明为 final，又称为不可变类 ；常量因此它不可被继承。(Integer 等包装类也不能被继承）**

在 Java 8 中，String 内部使用 char 数组存储数据。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}
```

在 Java 9 之后，String 类的实现改用 byte 数组存储字符串，同时使用 `coder` 来标识使用了哪种编码。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final byte[] value;

    /** The identifier of the encoding used to encode the bytes in {@code value}. */
    private final byte coder;
}
```

value 数组被声明为 final，这意味着 value 数组初始化之后就不能再引用其它数组。并且 String 内部没有改变 value 数组的方法，因此可以保证 String 不可变。

------

## 创建字符串

创建字符串最简单的方式如下:

```
String str = "Runoob";
```

在代码中遇到字符串常量时，这里的值是 "**Runoob**""，编译器会使用该值创建一个 String 对象。

和其它对象一样，可以使用关键字和构造方法来创建 String 对象。

用构造函数创建字符串：

```
String str2=new String("Runoob");
```

String 创建的字符串存储在公共池中，而 new 创建的字符串对象在堆上：

```
String s1 = "Runoob";              // String 直接创建 

String s2 = "Runoob";              // String 直接创建 

String s3 = s1;                    // 相同引用 

String s4 = new String("Runoob");   // String 对象创建 

String s5 = new String("Runoob");   // String 对象创建
```

![img](https://www.runoob.com/wp-content/uploads/2013/12/java-string-1-2020-12-01.png)



**String 类有 11 种构造方法，这些方法提供不同的参数来初始化字符串，比如提供一个字符数组参数:**

StringDemo.java 文件代码：

```
public class StringDemo{   public static void main(String args[]){      

char[] helloArray = { 'r', 'u', 'n', 'o', 'o', 'b'};     

 String helloString = new String(helloArray);        S

ystem.out.println( helloString );   } }
```

以上实例编译运行结果如下：

```
runoob
```

**注意:**String 类是不可改变的，所以你一旦创建了 String 对象，那它的值就无法改变了（详看笔记部分解析）。

如果需要对字符串做很多修改，那么应该选择使用 [StringBuffer & StringBuilder 类](https://www.runoob.com/java/java-stringbuffer.html)。

------

## 字符串长度

用于获取有关对象的信息的方法称为访问器方法。

String 类的一个访问器方法是 length() 方法，它返回字符串对象包含的字符数。

下面的代码执行后，len 变量等于 14:

StringDemo.java 文件代码：

```
public class StringDemo {    public static void main(String args[]) {        

String site = "www.runoob.com";        

int len = site.length();        

System.out.println( "菜鸟教程网址长度 : " + len );   } }
```

以上实例编译运行结果如下：

```
菜鸟教程网址长度 : 14
```

------

## 连接字符串

String 类提供了连接两个字符串的方法：

```
string1.concat(string2);
```

返回 string2 连接 string1 的新字符串。也可以对字符串常量使用 **concat() 方法**，如：

```
"我的名字是 ".concat("Runoob");
```

**更常用的是使用 '+'操作符来连接字符串，如：**

```
"Hello," + " runoob" + "!"
```

结果如下:

```
"Hello, runoob!"
```

下面是一个例子:

StringDemo.java 文件代码：

public class StringDemo {    public static void main(String args[]) {            

 String string1 = "菜鸟教程网址：";             

System.out.println("1、" + string1 + "www.runoob.com");      } }

以上实例编译运行结果如下：

```
1、菜鸟教程网址：www.runoob.com
```

## String 方法

下面是 String 类支持的方法，更多详细，参看 [Java String API](https://www.runoob.com/manual/jdk1.6/java/lang/String.html) 文档:

| SN(序号) | 方法描述                                                     |
| :------- | :----------------------------------------------------------- |
| **1**    | [char charAt(int index)](https://www.runoob.com/java/java-string-charat.html) 返回指定索引处的 char 值。 |
| 2        | [int compareTo(Object o)](https://www.runoob.com/java/java-string-compareto.html) 把这个字符串和另一个对象比较。 |
| 3        | [int compareTo(String anotherString)](https://www.runoob.com/java/java-string-compareto.html) 按字典顺序比较两个字符串。 |
| 4        | [int compareToIgnoreCase(String str)](https://www.runoob.com/java/java-string-comparetoignorecase.html) 按字典顺序比较两个字符串，不考虑大小写。 |
| 5        | [String concat(String str)](https://www.runoob.com/java/java-string-concat.html) 将指定字符串连接到此字符串的结尾。 |
| 6        | [boolean contentEquals(StringBuffer sb)](https://www.runoob.com/java/java-string-contentequals.html) 当且仅当字符串与指定的StringBuffer有相同顺序的字符时候返回真。 |
| **7**    | **[static String copyValueOf(char[\] data)](https://www.runoob.com/java/java-string-copyvalueof.html) 返回指定数组中表示该字符序列的 String。** |
| **8**    | **[static String copyValueOf(char[\] data, int offset, int count)](https://www.runoob.com/java/java-string-copyvalueof.html) 返回指定数组中表示该字符序列的 String。** |
| 9        | [boolean endsWith(String suffix)](https://www.runoob.com/java/java-string-endswith.html) 测试此字符串是否以指定的后缀结束。 |
| **10**   | **[boolean equals(Object anObject)](https://www.runoob.com/java/java-string-equals.html) 将此字符串与指定的对象比较。** **注意String是对象，不能用==判断两个字符串是否相等** |
| 11       | [boolean equalsIgnoreCase(String anotherString)](https://www.runoob.com/java/java-string-equalsignorecase.html) 将此 String 与另一个 String 比较，不考虑大小写。 |
| 12       | [byte[\] getBytes()](https://www.runoob.com/java/java-string-getbytes.html)  使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。 |
| 13       | [byte[\] getBytes(String charsetName)](https://www.runoob.com/java/java-string-getbytes.html) 使用指定的字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。 |
| 14       | [void getChars(int srcBegin, int srcEnd, char[\] dst, int dstBegin)](https://www.runoob.com/java/java-string-getchars.html) 将字符从此字符串复制到目标字符数组。 |
| 15       | [int hashCode()](https://www.runoob.com/java/java-string-hashcode.html) 返回此字符串的哈希码。 |
| 16       | [int indexOf(int ch)](https://www.runoob.com/java/java-string-indexof.html) 返回指定字符在此字符串中第一次出现处的索引。 |
| 17       | [int indexOf(int ch, int fromIndex)](https://www.runoob.com/java/java-string-indexof.html) 返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索。 |
| 18       | [int indexOf(String str)](https://www.runoob.com/java/java-string-indexof.html)  返回指定子字符串在此字符串中第一次出现处的索引。 |
| 19       | [int indexOf(String str, int fromIndex)](https://www.runoob.com/java/java-string-indexof.html) 返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。 |
| 20       | [String intern()](https://www.runoob.com/java/java-string-intern.html)  返回字符串对象的规范化表示形式。 |
| 21       | [int lastIndexOf(int ch)](https://www.runoob.com/java/java-string-lastindexof.html)  返回指定字符在此字符串中最后一次出现处的索引。 |
| 22       | [int lastIndexOf(int ch, int fromIndex)](https://www.runoob.com/java/java-string-lastindexof.html) 返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索。 |
| 23       | [int lastIndexOf(String str)](https://www.runoob.com/java/java-string-lastindexof.html) 返回指定子字符串在此字符串中最右边出现处的索引。 |
| 24       | [int lastIndexOf(String str, int fromIndex)](https://www.runoob.com/java/java-string-lastindexof.html)  返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。 |
| 25       | [int length()](https://www.runoob.com/java/java-string-length.html) 返回此字符串的长度。 |
| 26       | [boolean matches(String regex)](https://www.runoob.com/java/java-string-matches.html) 告知此字符串是否匹配给定的正则表达式。 |
| 27       | [boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)](https://www.runoob.com/java/java-string-regionmatches.html) 测试两个字符串区域是否相等。 |
| 28       | [boolean regionMatches(int toffset, String other, int ooffset, int len)](https://www.runoob.com/java/java-string-regionmatches.html) 测试两个字符串区域是否相等。 |
| 29       | [String replace(char oldChar, char newChar)](https://www.runoob.com/java/java-string-replace.html) 返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。 |
| 30       | [String replaceAll(String regex, String replacement)](https://www.runoob.com/java/java-string-replaceall.html) 使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。 |
| 31       | [String replaceFirst(String regex, String replacement)](https://www.runoob.com/java/java-string-replacefirst.html)  使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。 |
| **32**   | **[String[\] split(String regex)](https://www.runoob.com/java/java-string-split.html) 根据给定正则表达式的匹配拆分此字符串。**返回字符串数组 |
| **33**   | **[String[\] split(String regex, int limit)](https://www.runoob.com/java/java-string-split.html) 根据匹配给定的正则表达式来拆分此字符串。** |
| 34       | [boolean startsWith(String prefix)](https://www.runoob.com/java/java-string-startswith.html) 测试此字符串是否以指定的前缀开始。 |
| 35       | [boolean startsWith(String prefix, int toffset)](https://www.runoob.com/java/java-string-startswith.html) 测试此字符串从指定索引开始的子字符串是否以指定前缀开始。 |
| 36       | [CharSequence subSequence(int beginIndex, int endIndex)](https://www.runoob.com/java/java-string-subsequence.html)  返回一个新的字符序列，它是此序列的一个子序列。 |
| **37**   | **[String substring(int beginIndex)](https://www.runoob.com/java/java-string-substring.html) 返回一个新的字符串，它是此字符串的一个子字符串。** |
| **38**   | **[String substring(int beginIndex, int endIndex)](https://www.runoob.com/java/java-string-substring.html) 返回一个新字符串，它是此字符串的一个子字符串。結束索引不包括** |
| 39       | [char[\] toCharArray()](https://www.runoob.com/java/java-string-tochararray.html) 将此字符串转换为一个新的字符数组。 |
| 40       | [String toLowerCase()](https://www.runoob.com/java/java-string-tolowercase.html) **使用默认语言环境的规则将此 String 中的所有字符都转换为小写。**return **new String**(result, 0, len + resultOffset); |
| 41       | [String toLowerCase(Locale locale)](https://www.runoob.com/java/java-string-tolowercase.html)  使用给定 Locale 的规则将此 String 中的所有字符都转换为小写。 |
| 42       | [String toString()](https://www.runoob.com/java/java-string-tostring.html)  返回此对象本身（它已经是一个字符串！）。 |
| 43       | [String toUpperCase()](https://www.runoob.com/java/java-string-touppercase.html) **使用默认语言环境的规则将此 String 中的所有字符都转换为大写。** |
| 44       | [String toUpperCase(Locale locale)](https://www.runoob.com/java/java-string-touppercase.html) 使用给定 Locale 的规则将此 String 中的所有字符都转换为大写。 |
| 45       | [String trim()](https://www.runoob.com/java/java-string-trim.html) 返回字符串的副本，忽略前导空白和尾部空白。 |
| 46       | [static String valueOf(primitive data type x)](https://www.runoob.com/java/java-string-valueof.html) 返回给定data type类型x参数的字符串表示形式。 |
| 47       | [contains(CharSequence chars)](https://www.runoob.com/java/java-string-contains.html) 判断是否包含指定的字符系列。 |
| 48       | [isEmpty()](https://www.runoob.com/java/java-string-isempty.html) 判断字符串是否为空。 |

## 不可变性

### 不可变的好处

**1. 可以缓存 hash 值**  

因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。

**2. String Pool 的需要**  

如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/image-20191210004132894.png"/> </div><br>

**3. 安全性**  

String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 的那一方以为现在连接的是其它主机，而实际情况却不一定是。

**4. 线程安全**  

String 不可变性天生具备线程安全，可以在多个线程中安全地使用。

[Program Creek : Why String is immutable in Java?](https://www.programcreek.com/2013/04/why-string-is-immutable-in-java/)

### 不可变性的坏处

- 每次字符串常量的拼接操作就会产生多个字符串,占用空间，效率低下

## String三剑客	

**1. 可变性**  

- String 不可变
- **StringBuffer 和 StringBuilder 可变**
  - 内存始终为一个数组效率高；

**2. 线程安全**  

- String 不可变，因此是线程安全的
- String**Builder** 不是线程安全的
- String**Buffer** 是**线程安全的**，内部使用 **synchronized 进行同步**

#### StringBuilder 

#### 概述

字符串缓冲区，可变长度，**底层数组**，效率提升

#### 使用

```java
StringBuffer s = new StringBuffer(x);  x为初始化容量长度
StringBuffer s = new StringBuffer(sb);  x为某stringBuffer实例 用来复制
```

s.append("Y"); "Y"表示长度为y的字符串

length始终返回当前长度即y；

#### 扩展

对于s.capacity()：

1.当y<x时，值为x

以下情况，容器容量需要扩展

2.当x<y<2*x+2时，值为 2*x+2

3.当y>2*x+2时，值为y

#### 方法

**append();**

**reverse();**

**toString();**



### String Pool

字符串常量池（String Pool）保存着所有字符串字面量（literal strings），这些字面量在编译时期就确定。不仅如此，还可以使用 String 的 intern() 方法在运行过程将字符串添加到 String Pool 中。

当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等（使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。

下面示例中，s1 和 s2 采用 new String() 的方式新建了两个不同字符串，而 s3 和 s4 是通过 s1.intern() 和 s2.intern() 方法取得同一个字符串引用。intern() 首先把 "aaa" 放到 String Pool 中，然后返回这个字符串引用，因此 s3 和 s4 引用的是同一个字符串。

```java
String s1 = new String("aaa");
String s2 = new String("aaa");
System.out.println(s1 == s2);           // false
String s3 = s1.intern();
String s4 = s2.intern();
System.out.println(s3 == s4);           // true
```

如果是采用 "bbb" 这种字面量的形式创建字符串，会自动地将字符串放入 String Pool 中。

```java
String s5 = "bbb";
String s6 = "bbb";
System.out.println(s5 == s6);  // true
```

在 Java 7 之前，String Pool 被放在运行时常量池中，它属于永久代。而在 Java 7，String Pool 被移到堆中。这是因为永久代的空间有限，在大量使用字符串的场景下会导致 OutOfMemoryError 错误。

- [StackOverflow : What is String interning?](https://stackoverflow.com/questions/10578984/what-is-string-interning)
- [深入解析 String#intern](https://tech.meituan.com/in_depth_understanding_string_intern.html)

### new String("abc")

使用这种方式一共会创建两个字符串对象（前提是 String Pool 中还没有 "abc" 字符串对象）。

- "abc" 属于字符串字面量，因此编译时期会在 String Pool 中创建一个字符串对象，指向这个 "abc" 字符串字面量；
- 而使用 **new 的方式会在堆中创建一个字符串对象。**

创建一个测试类，其 main 方法中使用这种方式来创建字符串对象。

```java
public class NewStringTest {
    public static void main(String[] args) {
        String s = new String("abc");
    }
}
```

使用 javap -verbose 进行反编译，得到以下内容：

```java
// ...
Constant pool:
// ...
   #2 = Class              #18            // java/lang/String
   #3 = String             #19            // abc
// ...
  #18 = Utf8               java/lang/String
  #19 = Utf8               abc
// ...

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=3, locals=2, args_size=1
         0: new           #2                  // class java/lang/String
         3: dup
         4: ldc           #3                  // String abc
         6: invokespecial #4                  // Method java/lang/String."<init>":(Ljava/lang/String;)V
         9: astore_1
// ...
```

在 Constant Pool 中，#19 存储这字符串字面量 "abc"，#3 是 String Pool 的字符串对象，它指向 #19 这个字符串字面量。在 main 方法中，0: 行使用 new #2 在堆中创建一个字符串对象，并且使用 ldc #3 将 String Pool 中的字符串对象作为 String 构造函数的参数。

以下是 String 构造函数的源码，可以看到，在将一个字符串对象作为另一个字符串对象的构造函数参数时，并不会完全复制 value 数组内容，而是都会指向同一个 value 数组。

```java
public String(String original) {
    this.value = original.value;
    this.hash = original.hash;
}
```



# 运算

## 运算符

### /

#### 整数的除法：

0做除数抛运行时异常；两整数商会做取整运算，Float或Double与一个整数做除法运算，则商位Float或者Double类型，例如：

##### Double(或Float)除法运算：

0可以做除数，得到的是一个分正负的无穷大；当两个数的绝对值均为0.0时候，商等于NaN。当0.0/x，x不等0.0时候，得到的一个带符号位0.0：

陷阱：

零在整数预算中不可以做除数，否则抛运行时异常。

零在浮点运算中可以做除数，返回值为无穷大。

NaN各不相同，可看做是Java设计上的一个缺陷。

浮点型(Float或Double)的除法运算可以接受任何数值，并且**结果总是返回一个浮点型的数值**。这个数值可能是不合法的，需要进行判断和验证。

### 与或非异或

**&  |  ~   ^** 

补充：

***&&与操作，||或操作**都是短路操作符，即与操作时一旦遇到false就停止执行后当前关系式中的后续代码*

```csharp
/**
* 测试
**/
public class Test {
    public static void main(String args[]) {

        int a = 55;
        int b = 65;

        System.out.println("a==" + Integer.toBinaryString(a));
        System.out.println("b==" + Integer.toBinaryString(b));

        //两个是 1 结果就是 1  否则是 0
        System.out.println("a&b==" + Integer.toBinaryString(a&b));
        System.out.println("a&b=" + (a&b));

        //一个是 1 结果就是 1 否则是 0
        System.out.println("a|b==" + Integer.toBinaryString(a|b));
        System.out.println("a|b=" + (a|b));

        //相互取反 1->0 0->1
        System.out.println("~a==" + Integer.toBinaryString(~a));
        System.out.println("~a=" + (~a));

        //两数相同是 0 不同是 1
        System.out.println("a^b==" + Integer.toBinaryString(a^b));
        System.out.println("a^b=" + (a^b));
    }
}
//测试打印
a==0110111
b==1000001
a&b==1
a&b=1
a|b==1110111
a|b=119
~a==11111111111111111111111111001000
~a=-56
a^b==1110110
a^b=118
```





### 移位运算符

移位运算符操作的对象就是二进制的位，可以单独用移位运算符来处理int型整数。

```
 N = N >> 1;
```

2.1  <<：左移运算符，将运算符**左边的对象**向左移动运算符 **右边指定的位数**（在低位补0）。

2.2  >>："有符号"右移运算 符，将运算符左边的对象向右移动运算符右边指定的位数。**使用符号扩展机制，也就是说，如果值为正，则在高位补0，如果值为负，则在高位补1。**

2.3  >>>： "无符号"右移运算 符，将运算符左边的对象向右移动运算符右边指定的位数。采用0扩展机制，也就是说，无论值的正负，都在高位补0。



```swift
public class Test {
    public static void main(String args[]) {

        int c = 3241;

        System.out.println("c = "+Integer.toBinaryString(c));
        System.out.println("-c = "+Integer.toBinaryString(-c));

        //如果值为正，则在高位补0，如果值为负，则在高位补1
        System.out.println("c >> 5 = "+Integer.toBinaryString(c >> 5));
        System.out.println("-c >> 5 ="+Integer.toBinaryString(-c >> 5));

        //无论值的正负，都在高位补0
        System.out.println("c >>> 5 = "+Integer.toBinaryString(c >>> 5));
        System.out.println("-c >>> 5 = "+Integer.toBinaryString(-c >>> 5));

        //低位补0
        System.out.println("c << 5 = "+Integer.toBinaryString(c << 5));
        System.out.println("-c << 5 = "+Integer.toBinaryString(-c << 5));
    }
}

//测试打印
c = 110010101001
-c = 11111111111111111111001101010111
c >> 5 = 1100101
-c >> 5 =11111111111111111111111110011010
c >>> 5 = 1100101
-c >>> 5 = 111111111111111111110011010
c << 5 = 11001010100100000
-c << 5 = 11111111111111100110101011100000
```

### 三元运算符

#### **条件运算符（?:）**

条件运算符也被称为三元运算符。该运算符有3个操作数，并且需要判断布尔表达式的值。该运算符的主要是决定哪个值应该赋值给变量。

```
variable x = (expression) ? value if true : value if false
```

```java
public class Test {
   public static void main(String[] args){
      int a , b;
      a = 10;
      // 如果 a 等于 1 成立，则设置 b 为 20，否则为 30
      b = (a == 1) ? 20 : 30;
      System.out.println( "Value of b is : " +  b );
 
      // 如果 a 等于 10 成立，则设置 b 为 20，否则为 30
      b = (a == 10) ? 20 : 30;
      System.out.println( "Value of b is : " + b );
   }
}
```

以上实例编译运行结果如下：

```
Value of b is : 30
Value of b is : 20
```

##### 转换规则

三元操作符类型的转换规则：
1.若两个操作数不可转换，则不做转换，返回值为Object类型
2.若两个操作数是明确类型的表达式（比如变量），则按照正常的二进制数字来转换，int类型转换为long类型，long类型转换为float类型等。
3.若两个操作数中有一个是数字S,另外一个是表达式，且其类型标示为T，那么，若数字S在T的范围内，则转换为T类型；若S超出了T类型的范围，则T转换为S类型。

4.若两个操作数都是直接量数字，则返回值类型为范围较大者

```java
byte b = 1;
char c = 1;
short s = 1;
int i = 1;

// 三目，一边为byte另一边为char，结果为int
// 其它情况结果为两边中范围大的。适用包装类型
i = true ? b : c; // int
b = true ? b : b; // byte
s = true ? b : s; // short

// 表达式，两边为byte,short,char，结果为int型
// 其它情况结果为两边中范围大的。适用包装类型
i = b + c; // int
i = b + b; // int
i = b + s; // int

// 当 a 为基本数据类型时，a += b，相当于 a = (a) (a + b)
// 当 a 为包装类型时， a += b 就是 a = a + b
b += s; // 没问题
c += i; // 没问题

// 常量任君搞，long以上不能越
b = (char) 1 + (short) 1 + (int) 1; // 没问题
// i = (long) 1 // 错误
```



### 运算技巧

负整数 转换成 二进制
 如：-43
 先求出 43 的二进制：101011
 求出43的反码：010100
 求出43的补码，补码就是反码+1：010101
 补码就是负数在计算机中的二进制表示方法。
 所以 -43的二进制就是：1111 1111 1111 1111 1111 1111 1101 0101

二进制负数 转换成 十进制
 如：1111 1111 1111 1111 1111 1111 1101 0101
 因为是负数所以开头肯定是 1
 和上面的计算是相反的，那么就是先做 -1 ：1111 1111 1111 1111 1111 1111 1101 0100
 然后取反：0000 0000 0000 0000 0000 0000 0010 1011
 也就是：101011
 也就是正数 43 加上负号就是 -43

https://stackoverflow.com/questions/40480/is-java-pass-by-reference-or-pass-by-value)

https://stackoverflow.com/questions/8710619/why-dont-javas-compound-assignment-operators-require-casting)

## switch

从 Java 7 开始，可以在 switch 条件判断语句中使用 String 对象。

```java
String s = "a";
switch (s) {
    case "a":
        System.out.println("aaa");
        break;
    case "b":
        System.out.println("bbb");
        break;
}
```

switch 不支持 long、float、double，是因为 switch 的设计初衷是对那些只有少数几个值的类型进行等值判断，如果值过于复杂，那么还是用 if 比较合适。

```java
// long x = 111;
// switch (x) { // Incompatible types. Found: 'long', required: 'char, byte, short, int, Character, Byte, Short, Integer, String, or an enum'
//     case 111:
//         System.out.println(111);
//         break;
//     case 222:
//         System.out.println(222);
//         break;
// }
```

[StackOverflow : Why can't your switch statement data type be long, Java?](https://stackoverflow.com/questions/2676210/why-cant-your-switch-statement-data-type-be-long-java)

#  Math 类方法

下面的表中列出的是 Number & Math 类常用的一些方法：

| 序号 | 方法与描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [xxxValue()](https://www.runoob.com/java/number-xxxvalue.html) 将 Number 对象转换为xxx数据类型的值并返回。 |
| 2    | [compareTo()](https://www.runoob.com/java/number-compareto.html) 将number对象与参数比较。 |
| 3    | [equals()](https://www.runoob.com/java/number-equals.html) 判断number对象是否与参数相等。 |
| 4    | [valueOf()](https://www.runoob.com/java/number-valueof.html) 返回一个 Number 对象指定的内置数据类型 |
| 5    | [toString()](https://www.runoob.com/java/number-tostring.html) 以字符串形式返回值。 |
| 6    | [parseInt()](https://www.runoob.com/java/number-parseInt.html) 将字符串解析为int类型。 |
| 7    | [abs()](https://www.runoob.com/java/number-abs.html) 返回参数的绝对值。 |
| 8    | [ceil()](https://www.runoob.com/java/number-ceil.html) **返回大于等于( >= )给定参数的的最小整数，类型为双精度浮点型。** |
| 9    | [floor()](https://www.runoob.com/java/number-floor.html) **返回小于等于（<=）给定参数的最大整数 。** |
| 10   | [rint()](https://www.runoob.com/java/number-rint.html) 返回与参数最接近的整数。返回类型为double。 |
| 11   | [round()](https://www.runoob.com/java/number-round.html) 它表示**四舍五入**，算法为 **Math.floor(x+0.5)**，即将原来的数字加上 0.5 后再向下取整，所以，Math.round(11.5) 的结果为12，Math.round(-11.5) 的结果为-11。 |
| 12   | [min()](https://www.runoob.com/java/number-min.html) 返回两个参数中的最小值。 |
| 13   | [max()](https://www.runoob.com/java/number-max.html) 返回两个参数中的最大值。 |
| 14   | [exp()](https://www.runoob.com/java/number-exp.html) 返回自然数底数e的参数次方。 |
| 15   | [log()](https://www.runoob.com/java/number-log.html) 返回参数的自然数底数的对数值。 |
| 16   | [pow()](https://www.runoob.com/java/number-pow.html) 返回第一个参数的第二个参数次方。 |
| 17   | [sqrt()](https://www.runoob.com/java/number-sqrt.html) 求参数的算术平方根。 |
| 18   | [sin()](https://www.runoob.com/java/number-sin.html) 求指定double类型参数的正弦值。 |
| 19   | [cos()](https://www.runoob.com/java/number-cos.html) 求指定double类型参数的余弦值。Math.cos为计算**弧度**的余弦值，Math.toRadians函数讲**角度转换为弧度** |
| 20   | [tan()](https://www.runoob.com/java/number-tan.html) 求指定double类型参数的正切值。 |
| 21   | [asin()](https://www.runoob.com/java/number-asin.html) 求指定double类型参数的反正弦值。 |
| 22   | [acos()](https://www.runoob.com/java/number-acos.html) 求指定double类型参数的反余弦值。 |
| 23   | [atan()](https://www.runoob.com/java/number-atan.html) 求指定double类型参数的反正切值。 |
| 24   | [atan2()](https://www.runoob.com/java/number-atan2.html) 将笛卡尔坐标转换为极坐标，并返回极坐标的角度值。 |
| 25   | [toDegrees()](https://www.runoob.com/java/number-todegrees.html) 将参数转化为角度。 |
| 26   | [toRadians()](https://www.runoob.com/java/number-toradians.html) 将角度转换为弧度。 |
| 27   | [random()](https://www.runoob.com/java/number-random.html) 返回一个随机数。 |

------

##  floor,round 和 ceil 方法

floor: 求小于参数的最大整数。**返回double类型**-----n. 地板，地面

​     例如：Math.floor(-4.2) = -5.0

\-----------------------------------------------------------

ceil:  求大于参数的最小整数。**返回double类型**-----vt. 装天花板；

​     例如：Math.ceil(5.6) = 6.0

\-----------------------------------------------------------

round: 对小数进行四舍五入后的结果。**返回int类型**

​     例如：Math.round(-4.6) = -5

| 参数 | Math.floor | Math.round | Math.ceil |
| :--- | :--------- | :--------- | :-------- |
| 1.4  | 1          | 1          | 2         |
| 1.5  | 1          | 2          | 2         |
| 1.6  | 1          | 2          | 2         |
| -1.4 | -2         | -1         | -1        |
| -1.5 | -2         | -1         | -1        |
| -1.6 | -2         | -2         | -1        |

ceil：大于等于 x，并且与它最接近的整数。

floor：小于等于 x，且与 x 最接近的整数。

## abs方法

对于Integer.MIN_VALUE和Long.MIN_VALUE来说，Math.abs()对他们不起作用。返回的还是原来的值。

```java
public class absTest {
 public static void main(String[] args) {
  int min = Integer.MIN_VALUE;
  System.out.println(min); // 输出-2147483648
  min = Math.abs(min);
  System.out.println(min); // 输出-2147483648
```

  // 说明Math.abs(int MIN_VALUE) 不能将其转化为正整数，同理对于Long型也是一样的
  // 在Math.abs()的文档中有特别的说明说是对于Long.MIN_VALUE、Integer.MIN_VALUE 的值（即能够表示的最小负 int、long 值），
  // 那么结果与该值相同且为负。






# 输入输出

### Java Scanner 类

java.util.Scanner 我们可以通过 Scanner 类来获取用户的输入。

下面是创建 Scanner 对象的基本语法：

```
Scanner s = new Scanner(System.in);
```

接下来我们演示一个最简单的数据输入，

1. **通过 Scanner 类的 next() 与 nextLine() 方法获取输入的字符串**
2. 在读取前我们一般需要 **使用 hasNext 与 hasNextLine 判断是否还有输入的数据**

#### 使用 next 方法：

```java
import java.util.Scanner;
 
public class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        // 从键盘接收数据
 
        // nextLine方式接收字符串
        System.out.println("nextLine方式接收：");
        // 判断是否还有输入
        if (scan.hasNextLine()) {
            String str2 = scan.nextLine();
            System.out.println("输入的数据为：" + str2);
        }
        scan.close();
    }
}
```

执行以上程序输出结果为：

```
$ javac ScannerDemo.java
$ java ScannerDemo
next方式接收：
runoob com
输入的数据为：runoob
```

可以看到 com 字符串并未输出，接下来我们看 nextLine。

### 使用 nextLine 方法：

```java
import java.util.Scanner;
 
public class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        // 从键盘接收数据
 
        // nextLine方式接收字符串
        System.out.println("nextLine方式接收：");
        // 判断是否还有输入
        if (scan.hasNextLine()) {
            String str2 = scan.nextLine();
            System.out.println("输入的数据为：" + str2);
        }
        scan.close();
    }
}
```

执行以上程序输出结果为：

```
$ javac ScannerDemo.java
$ java ScannerDemo
nextLine方式接收：
runoob com
输入的数据为：runoob com
```

可以看到 com 字符串输出。

### next() 与 nextLine() 区别

next():

- 1、一定要读取到有效字符后才可以结束输入。
- 2、对输入有效字符之前遇到的空白，next() 方法会自动将其去掉。
- 3、只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
- next() 不能得到带有空格的字符串。

nextLine()：

- 1、以Enter为结束符,也就是说 nextLine()方法返回的是输入回车之前的所有字符。
- 2、可以获得空白。

如果要输入 int 或 float 类型的数据，在 Scanner 类中也有支持，但是在输入之前最好先使用 hasNextXxx() 方法进行验证，再使用 nextXxx() 来读取：


# 关键字

### final

不可变的

#### **1. 数据**  

声明数据为常量，可以是编译时常量，也可以是在运行时被初始化后不能被改变的常量。

- 对于基本类型，final 使数值不变；在赋值之后无法改变。
- 对于引用类型，**final 使引用不变，也就不能引用其它对象，在赋值后其指向地址无法改变，但是对象内容属性 还是可以改变的**。

#### **2.赋值方式**

final修饰的成员变量在赋值时可以有三种方式。

1、在声明时直接赋值。

2、在构造器中赋值。

3、在初始代码块中进行赋值。

```java
final int x = 1;
// x = 2;  // cannot assign value to final variable 'x'
final A y = new A();
y.a = 1;
```

注意：由于成员变量有默认值（0；null），因此final修饰成员变量时必须赋值；

- 直接赋值
- 所有的构造方法都对这个成员变量进行赋值

#### **2. 方法**  

声明**方法不能被子类重写。**可以被本类**重载**

1. **private 方法隐式地被指定为 final**
2. 如果在子类中定义的方法和基类中的一个 private 方法签名相同**此时子类的方法不是重写基类方法，而是在子类中定义了一个新的方法。**

#### **3. 类**  

1. 声明类**不允许被继承**，不能有子类；

2. 对于类和方法来说，**abstract关键字和final关键字不能同时使用**

   *不能修饰抽象类*，因为抽象类一般都是需要被继承的，final修饰后就不能继承了。

#### 注意



![image-20210930113507343](笔记图库/image-20210930113507343.png)



### static

**1. 静态变量**  

- **静态变量**：又称为**类变量**，也就是说这个变量属于类的；
  **类所有的实例对象都共享静态变量**，一人修改，群员改变，**超级全局变量；**
  static的，是类属性，所以不管有多少**对象，都共用的一个变量。**
- 静态变量在内存中只存在一份；
  可以通过 类名. 的形式进行调用；
- 实例变量：每创建一个实例就会产生一个实例变量，**它与该实例同生共死**；

```java
public class A {

    private int x;         // 实例变量
    private static int y;  // 静态变量

    public static void main(String[] args) {
        // int x = A.x;  // Non-static field 'x' cannot be referenced from a static context
        A a = new A();
        int x = a.x;
        int y = A.y;
    }
}
```

**2. 静态方法**  （类方法）

- 静态方法在类加载的时候就存在了，**它不依赖于任何实例**。所以静态方法必须有实现，也就是说它不能是抽象方法。 类方法（也就是静态方法）中不能直接调用类的非静态变量
- 不能被继承

应用场景：通过 类名. 的形式进行调用；

```java
public abstract class A {
    public static void func1(){
    }
    // public abstract static void func2();  // Illegal combination of modifiers: 'abstract' and 'static'
}
```

**只能访问所属类的静态字段和静态方法，方法中不能有 this 和 super 关键字**，因为这两个关键字与具体对象关联。

**静态方法在类加载的时候创建加载，此时没有创建对象**

```java
public class A {

    private static int x;
    private int y;

    public static void func1(){
        int a = x;
        // int b = y;  // Non-static field 'y' cannot be referenced from a static context
        // int b = this.y;     // 'A.this' cannot be referenced from a static context
    }
}
```



深入练习

```java
package NowCoder;
class Test {
	public static void hello() {
	    System.out.println("hello");
	}
}
public class MyApplication {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Test test=null;
		test.hello();
	}
}
```

```
能编译通过，并正确运行
```

**invokestatic静态绑定直接就能找到方法的入口地址了。**

静态方法属于静态绑定，编译器根据引用类型所属的静态类型为它绑定其对应的方法。此语句会翻译成invokestatic，该指令的调用中不会涉及this, 所以不会依赖对象！

 还有引用类型=null，其实就是指该引用在堆中没有对应的对象，但是编译的时候还是能根据声明找到其所属的静态类型。



**3. 静态语句块**  

静态语句块在类初始化时运行一次。

```java
public class A {
    static {
        System.out.println("123");
    }

    public static void main(String[] args) {
        A a1 = new A();
        A a2 = new A();
    }
}
```

```html
123
```

**4. 静态内部类**  

非静态内部类依赖于外部类的实例，也就是说需要先创建外部类实例，才能用这个实例去创建非静态内部类。而静态内部类不需要。

```java
public class OuterClass {

    class InnerClass {
    }

    static class StaticInnerClass {
    }

    public static void main(String[] args) {
        // InnerClass innerClass = new InnerClass(); // 'OuterClass.this' cannot be referenced from a static context
        OuterClass outerClass = new OuterClass();
        InnerClass innerClass = outerClass.new InnerClass();
        StaticInnerClass staticInnerClass = new StaticInnerClass();
    }
}
```

静态内部类不能访问外部类的非静态的变量和方法。

**5. 静态导包**  

在使用静态变量和方法时不用再指明 ClassName，从而简化代码，但可读性大大降低。

```java
import static com.xxx.ClassName.*
```

**6. 初始化顺序**  

静态变量和静态语句块优先于实例变量和普通语句块，静态变量和静态语句块的初始化顺序取决于它们在代码中的顺序。

```java
public static String staticField = "静态变量";
```

```java
static {
    System.out.println("静态语句块");
}
```

```java
public String field = "实例变量";
```

```java
{
    System.out.println("普通语句块");
}
```

最后才是构造函数的初始化。

```java
public InitialOrderTest() {
    System.out.println("构造函数");
}
```

存在继承的情况下，初始化顺序为：

- 父类（静态变量、静态语句块）
- 子类（静态变量、静态语句块）
- 父类（实例变量、普通语句块）
- 父类（构造函数）
- 子类（实例变量、普通语句块）
- 子类（构造函数）

### 四种权限修饰符

![img](笔记图库/3414430_1534163213978_1901F0F5D964AA28ED2EC9CBCE07787B.png)

```
public > protect > () > private
```

#### **同一个类**：private

**被private修饰的方法不能被继承**（可以被重写）

```java
public class TestDemo{
	private int count;
	public static void main(String[] args) {
		TestDemo test=new TestDemo(88);
		System.out.println(test.count);
	}
	 TestDemo(int a) {
		 count=a;
	}
}
```

#### **同一个包**:   ()

#### **不同包子类**:  protect

#### **不同包非子类**：public

# 方法

### 可变参数

JDK 1.5 开始，Java支持传递同类型的可变参数给一个方法。

方法的可变参数的声明如下所示：

```
typeName... parameterName
```

在方法声明中，在指定参数类型后加一个省略号(...) 。

一个方法中只能指定一个可变参数，它必须是方法的最后一个参数。任何普通的参数必须在它之前声明。

**实例**

```java
public class VarargsDemo {
    public static void main(String args[]) {
        // 调用可变参数的方法
        printMax(34, 3, 3, 2, 56.5);
        printMax(new double[]{1, 2, 3});
    }
 
    public static void printMax( double... numbers) {
        if (numbers.length == 0) {
            System.out.println("No argument passed");
            return;
        }
 
        double result = numbers[0];
 
        for (int i = 1; i <  numbers.length; i++){
            if (numbers[i] >  result) {
                result = numbers[i];
            }
        }
        System.out.println("The max value is " + result);
    }
}
```



```
The max value is 56.5
The max value is 3.0
```

### finalize() 方法

Java 允许定义这样的方法，它在**对象被垃圾收集器析构(回收)之前调用**，这个方法叫做 finalize( )，它用来清除回收对象。

例如，你可以使用 finalize() 来确保一个对象打开的文件被关闭了。

在 finalize() 方法里，你必须指定在对象销毁时候要执行的操作。

finalize() 一般格式是：

protected void finalize() {   // 在这里终结代码 }

关键字 protected 是一个限定符，它确保 finalize() 方法不会被该类以外的代码调用。

当然，Java 的内存回收可以由 JVM 来自动完成。如果你手动使用，则可以使用上面的方法。

# 构造方法

构造方法是类的一种特殊方法，用来初始化类的一个新的对象，在创建对象（new 运算符）之后自动调用。[Java](http://c.biancheng.net/java/) 中的每个类都有一个默认的构造方法，并且可以有一个以上的构造方法。

当一个对象被创建时候，构造方法用来初始化该对象。构造方法和它所在类的名字相同，但构造方法没有返回值。

通常会使用构造方法给一个类的实例变量赋初值，或者执行其它必要的步骤来创建一个完整的对象。

### 构造修饰符

不管你是否自定义构造方法，所有的类都有构造方法，因为 Java 自动提供了一个默认构造方法，默认构造方法的访问修饰符和类的访问修饰符**相同**(类为 public，构造函数也为 public；类改为 protected，构造函数也改为 protected)。

一旦你定义了自己的构造方法，默认构造方法就会失效。

###  构造的特点

1. 方法名必须与类名相同
2. 可以有 0 个、1 个或多个参数
3. 没有任何返回值，包括 void
4. 默认返回类型就是对象类型本身
5. 只能与 new 运算符结合使用
6. 在一个构造方法中调用另一个构造方法，格式为this(参数)；

![img](笔记图库/3555485_1488035984543_8A5F006FFD4CE801C8E888EC3BC68361.png)

**值得注意的是，如果为构造方法定义了返回值类型或使用 void 声明构造方法没有返回值，编译时不会出错，但 Java 会把这个所谓的构造方法当成普通方法来处理。**

问题：**这时候大家可能会产生疑问，构造方法不是没有返回值吗？为什么不能用 void 声明呢？**

简单的说，这是 Java 的语法规定。**实际上，类的构造方法是有返回值的，当使用 new 关键字来调用构造方法时，构造方法返回该类的实例**，可以把**这个类的实例当成构造器的返回值**，因此构造器的返回值类型总是当前类，无须定义返回值类型。但必须注意不要在构造方法里使用 return 来返回当前类的对象，**因为构造方法的返回值是隐式的。**



**注意：**

**构造方法不能被 static、final、synchronized、abstract 和 native（类似于 abstract）修饰。构造方法用于初始化一个新对象，所以用 static 修饰没有意义。**

1. **构造方法不能被子类继承**，所以用 final 和 abstract 修饰没有意义。
2. 多个线程不会同时创建内存地址相同的同一个对象，所以用 synchronized 修饰没有必要。

### 语法格式

下面是一个构造方法示例：

```
class class_name {
    public class_name(){}    // 默认无参构造方法
    public ciass_name([paramList]){}    // 定义构造方法
    …
    // 类主体
}
```

在一个类中，与类名相同的方法就是构造方法。每个类可以具有多个构造方法，但要求它们各自包含不同的方法参数。

**有无参构造方法和有参构造方法：**

```java
public class MyClass {
    private int m;    // 定义私有变量
    MyClass() {
        // 定义无参的构造方法
        m = 0;
    }
    MyClass(int m) {
        // 定义有参的构造方法
        this.m = m;
    }
}
```

该示例定义了两个构造方法，分别是无参构造方法和有参构造方法。**在一个类中定义多个具有不同参数的同名方法，这就是方法的重载**。这两个构造方法的名称都与类名相同，均为 MyClass。在实例化该类时可以调用不同的构造方法进行初始化。

注意：

1. 类的构造方法不是要求必须定义的。如果在类中没有定义任何一个构造方法，则 Java 会自动为该类生成一个默认的构造方法。默认的构造方法不包含任何参数，并且方法体为空。
2. 类中显式地定义了一个或多个构造方法，**则 Java 不再提供默认构造方法**。
3. 创建对象时：**未指定的属性值，程序会将其值采用默认值 。**
4. 所有的 Java 对象都是在堆中构造的，构造器总是伴随着 new 操作符一起使用。
5. **构造器可以重载，而且可以使用super()、this()相互调用**
6. **每个构造器的默认第一行都是super()**，**但是一旦父类中没有无参构造，必须在子类的第一行显式的声明调用哪一个构造。**

# 类

### 单例模式

第一步，不让外部调用创建对象，所以把构造器私有化，用private修饰。

 第二步，怎么让外部获取本类的实例对象？通过本类提供一个方法，供外部调用获取实例。由于没有对象调用，所以此方法为类方法，用static修饰。

 第三步，通过方法返回实例对象，由于类方法(静态方法)只能调用静态方法，所以存放该实例的变量改为类变量，用static修饰。 

最后，类变量，类方法是在类加载时初始化的，只加载一次。所以由于外部不能创建对象，而且本来实例只在类加载时创建一次

```java
class Chinese{
    private static Chinese objref =new Chinese();
    private Chinese(){}
    public static Chinese getInstance() { return objref; }
}

public class TestChinese {
    public static void main(String [] args) {
    Chinese obj1 = Chinese.getInstance();
    Chinese obj2 = Chinese.getInstance();
    System.out.println(obj1 == obj2);
}
}
```

### 成员和方法

类中定义成员和方法，不能直接进行运算，可以写在代码块{}或者静态代码块中static{}中

### 类的修饰符

**普通类也就是外部类**：

通过 eclipse 的警告“Illegal modifier for the class Test; only **public**, **abstract** & **final** are permitted” 可知只能用 **public, abstract 和 final 修饰。**

**内部类**

则可以用 **修饰成员变量的修饰符修饰内部类，**比如 **private, static,** **protected 修饰。**

- 普通类（外部类）：只能用public、default（不写）、abstract、final修饰。
- （成员）内部类：可理解为外部类的成员，所以修饰类成员的public、protected、default、private、static等关键字都能使用。
- 局部内部类：出现在方法里的类，不能用上述关键词来修饰。
- 匿名内部类：给的是直接实现，类名都没有，没有修饰符。

# 参数传递

### **引用传递和值传递**

**值传递：方法调用时，实际参数将它的值传递给对应的形式参数，函数接收到的是原始值的副本，此时内存中存在两个相等的基本类型，若方法中对形参执行处理操作，并不会影响实际参数的值。**

引用传递：方法调用时，实际参数的引用（是指地址，而不是参数的值）被传递给方法中相应的形式参数，函数接收到的是原始值的内存地址，在方法中，对引用所指向的**内容**进行处理，返回时内容会被改变。**但注意如果只是在方法中改变引用值，实际参数引用不会改变；**

### 形参实参传递

　**（一）基本数据类型：传值，方法不会改变实参的值。**

```java
　　public class TestFun {
　　public static void testInt(int i){
　　i=5;
　　}
　　public static void main(String[] args) {
　　int a=0 ;
　　TestFun.testInt(a);
　　System.out.println("a="+a);//  a=0;
　　}
　　}
```

**（二）对象类型参数：对象作为参数传递时，将对象在内存中的地址拷贝了一份传递给了参数对形参的处理会影响实参。**

```java
//（1）String,Integer,Double等immutable类型的特殊处理，可以理解为值传递，形参操作不会影响实参对象。
　　public class TestFun2 {
　　public static void testStr(String str){
　　str="hello";//型参指向字符串 “hello”
　　}
　　public static void main(String[] args) {
　　String s="1" ;
　　TestFun2.testStr(s);
　　System.out.println("s="+s); //实参s引用没变，值也不变
　　}
　　}



public class python {
        public static void main(String[] args){
              python t= new python();
              t.test3();
        }

        public void test3(){
                String[] s1 = {"aa", "aa", "bb"};
                String s=new String("aa");
                exchange(s1);
                for (int i = 0; i < s1.length; i++) {
                        System.out.println(s1[i]);
                }
                change(s);
                System.out.println(s);

        }
        public void exchange(String[] strings){
                for(int i=0;i<strings.length;i++){
                        strings[i]="MM"; //直接修改数组元素的值
                }
        }

        public void change(String s){
                s="bb";  //赋值给String类型的变量修改
            //注意如果只是在方法中改变引用值，实际参数引用不会改变；
        }
}

```

**结论：**

**1）形参为基本类型时，对形参的处理不会影响实参。**

**2）形参为引用类型时，对形参的处理会影响实参。**

**3）String,Integer,Double等immutable类型的特殊处理，可以理解为值传递，形参操作不会影响实参对象。**

# 内部类

## 概念和分类

- 成员内部类
- 局部内部类
  - 匿名内部类

## 成员内部类

定义格式：正常的类命名方法

注意：内用外随意访问，外用内需要创建对象

类的内部可以定义成员变量和方法，而且在类的内部也可以定义另一个类，如果类Outer的内部在定义一个类Inner，此时Inner就成为内部类，而Outer则称为外部类。
**内部类可以声明为public或private**。当内部类声明为public或private，对其访问权限于成员变量和成员方法完全相同。

代码如下：

```java
package com.se.innerclass;
class Outer {
    private String info = "我爱你中国";
    class Inner{
        public void print() {
            System.out.println(info);
        }
    }
    public void fun() {
        new Inner().print();
    }
}
public class InnerClassDemo1 {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.fun();
    }
}
```

以上的程序中可以清楚的发现，Inner类作为Outer类的内部类存在，并且外部类的fun()方法中直接实例化内部类的对象调用方法print()，但是从代码中可以明显发现，内部类的存在实际上破坏了一个类的基本结构，因为类是由属性及方法组成的，所以这是内部类的一个缺点，那么内部类有哪些优点呢，如果把内部类拿到外面来就能发现内部类的优点了。
如下面的代码：

```java
package com.se.innerclass;

class Outer {
    private String info = "我爱你中国";
    public void fun() {
        new Inner(this).print();
    }
    public String getInfo(){
        return info;
    }
}
class Inner{
    private Outer outer;
    public Inner(Outer outer){
        this.outer = outer;
    }
    public void print() {
        System.out.println(outer.getInfo());
    }
}
public class InnerClassDemo2 {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.fun();
    }
}
```

在外部类访问内部类

一个内部类除了可以通过外部类访问，也可以直接在其他类当中调用，调用的基本格式为：
外部类.内部类 内部类对象 = 外部类实例.new 内部类()
以上的操作格式中，首先要找到外部类的实例化对象之后才可以通过外部类的实例化对象去实例化内部类对象。
这里我们可以观察到编译之后的内部类的.class文件。
内部类定义之后，生成的.class文件是以Outer$Inner的形式存在的，在Java中只要文件存在$,则在程序中应将其替换为”.”。
在外部访问内部类代码如下：

```java
package com.se.innerclass;

class Outer{
    private String info = "我爱你中国";
    class Inner{
        public void print() {
            System.out.println(info);
        }
    }
}

public class InnerClassDemo4 {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.print();
    }
}
```

### 静态内部类

#### 特点

1）外部类装载的时候，静态内部类不会状态

2）静态类所在的外部类使用内部类时，静态内部类会装载

3）静态内部类在装载时是线程安全的，只会装载一次

#### 使用单例-静态内部类优缺点分析

1）这种方式采用了类装载的机制来保证初始化实例时只有一个线程

2）静态内部类方式在Singleton类被装载时并不会立即实例化，而是在需要实例化时，调用getInstance方法，才会装载SingletonInstance类，从而完成Singleton的实例化

3）类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM帮助我们保证了线程的安全性，在类进行初始化时，别的线程是无法进入的

4）优点：避免了线程不安全，利用静态内部类特点实现延迟加载，效率高

5）结论：推荐使用

```javascript
public class SingletonTest07 {
    public static void main(String[] args) {
        Singleton07 instance  = Singleton07.getInstance();
        Singleton07 instance01 = Singleton07.getInstance();

        System.out.println(instance == instance01);
        System.out.println("instance,hashCode= " + instance.hashCode());
        System.out.println("instance01,hashCode = " + instance01.hashCode());
    }
}

class Singleton07{
    private static volatile Singleton07 instance;
    private Singleton07(){}
    private static class SingletonInstance{
        private static final Singleton07 INSTANCE = new Singleton07();
    }
    public static synchronized Singleton07 getInstance(){
        return SingletonInstance.INSTANCE;
    }
}

// 运行结果
true
instance,hashCode= 1846274136
instance01,hashCode = 1846274136
```

## 局部内部类

**定义在方法内部的类**

也可以在方法类定义一个内部类，但是在**方法中定义的内部类不能直接访问方法中的参数**，如果方法中的参数要想被内部类所访问，则参数前必须加上final关键字。****

局部内部类就像是方法里面的一个局部变量一样，**是不能有public、protected、private以及static修饰符的。**

**原因:**

- new出来的对象在堆内存中，生命随着垃圾回收才结束
- 局部变量跟着方法走，在栈内存中，方法结束，出栈局部变量消失
- 局部内部类和外部方法生命周期冲突，因此**需要final固定局部变量的数值**，以便内部类可以通过copy'在常量池中找到死去的局部变量值

在方法中定义内部类：

```java
package com.se.innerclass;

class Outer{
    private String info = "我爱你中国";
    public void fun(final String temp){
        class Inner{
            public void print(){
                System.out.println(info);
                System.out.println(temp);
            }
        }
        new Inner().print();
    }
}

public class InnerClassDemo5 {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.fun("123");
    }
}
```

## 匿名内部类

#### 概念

***如果父类的子类或者接口的实现类 只使用一次 那么使用匿名内部类***

否则我们要 为了实现一个接口，而单独出建立一个类 ，麻烦很多

匿名内部类是不能有访问修饰符和static修饰符的；

**注意不是匿名对象**

```
匿名对象：

没有名字的对象；

使用方法：

new 类名().[方法]

使用场景，只使用一次的情况；因为后续无法继续调用这个匿名对象
```

在java中处理内部类之外，还有一种匿名内部类。匿名内部类就是指没有一个具体名称的类，**此概念是在接口和抽象类的应用上发展起来的**，那么匿名内部类有哪些作用呢？
一个简单的操作：

 * ```java
 package com.se.innerclass;
     /**
     
     * 普通实现
    
  * ```java
    @author wzy
    *
        */
       interface A {
       public void printInfo();
       }
    
    class B implements A{
       @Override
       public void printInfo() {
           System.out.println("Hello world!!!");       
       }
       }
    
    class X{
    public void fun1(){
           this.fun2(new B());
       } 
       public void fun2(A a){
        a.printInfo();
    }     
    }
    
    public class NoInnerClassDemo6 {
    public static void main(String[] args) {
        new X().fun1();
    }
    }
    ```
    
    

通过以上的方法可以实现相应的功能，**但是现在如果接口实现类只使用一次，那么还有必要单独定义一个子类B吗？很显然是没有必要的，所以此时就可以使用匿名内部类完成，代码修改如下：**

 

```java
用 new A(){
              @Override
             public void printInfo() {
                 System.out.println("hello world");
             }}
代替B
```

#### 格式

 **接口名称** 对象名 = **new 接口名称（）{** **内容** }

 ```java
 
 
 ​```java
 package com.se.innerclass;
 interface A{
     public void printInfo();
 }
 class X{
     public void fun1() {
         this.fun2(new A(){
             @Override
             public void printInfo() {
                 System.out.println("hello world");
             }});
     }
     public void fun2(A a) {
         a.printInfo();
     }
 }
 public class NoInnerClassDemo7 {
     public static void main(String[] args) {
         new X().fun1();
     }
 }
 ```

###  注意事项

1. 它必须要实现继承的类或者实现的接口的所有抽象方法。
2. 只能使用一次，因此匿名内部类不可以定义构造器
3. 匿名内部类中不能存在任何的静态成员变量和静态方法
4. 使用匿名内部类时，我们必须是继承一个类或者实现一个接口，但是两者不可兼得，同时也只能继承一个类或者实现一个接口。

# Object 类

Java Object 类是所有类的父类，也就是说 Java 的所有类都继承了 Object，**子类可以使用 Object 的所有方法**。

![img](笔记图库/classes-object.gif)

Object 类位于 java.lang 包中，编译时会自动导入，我们创建一个类时，如果没有明确继承一个父类，那么它就会自动继承 Object，成为 Object 的子类。

Object 类可以显示继承，也可以隐式继承，以下两种方式时一样的：

显示继承:

```
public class Runoob extends Object{

}
```

隐式继承:

```
public class Runoob {

}
```

### 类的构造函数

| 序号 |       构造方法 & 描述        |
| :--- | :--------------------------: |
| 1    | **Object()**构造一个新对象。 |



### 方法概览

```java

public native int hashCode()

public boolean equals(Object obj)

protected native Object clone() throws CloneNotSupportedException

public String toString()

public final native Class<?> getClass()

protected void finalize() throws Throwable {}

public final native void notify()

public final native void notifyAll()

public final native void wait(long timeout) throws InterruptedException

public final void wait(long timeout, int nanos) throws InterruptedException

public final void wait() throws InterruptedException
```

### equals()

**1. 等价关系**  

两个对象具有等价关系，需要满足以下五个条件：

Ⅰ 自反性

```java
x.equals(x); // true
```

Ⅱ 对称性

```java
x.equals(y) == y.equals(x); // true
```

Ⅲ 传递性

```java
if (x.equals(y) && y.equals(z))
    x.equals(z); // true;
```

Ⅳ 一致性

多次调用 equals() 方法结果不变

```java
x.equals(y) == x.equals(y); // true
```

Ⅴ 与 null 的比较

对任何不是 null 的对象 x 调用 x.equals(null) 结果都为 false

```java
x.equals(null); // false;
```

**2. 等价与相等**  

- 对于基本类型，== 判断两个值是否相等，基本类型没有 equals() 方法。
- 对于引用类型，**== 判断两个变量是否引用同一个对象，而 equals() 判断引用的对象是否等价。**

```java
Integer x = new Integer(1);
Integer y = new Integer(1);
System.out.println(x.equals(y)); // true
System.out.println(x == y);      // false

public class Test {
 
    /**
     * @param args
     */
    public static void main(String[] args) {
        // TODO Auto-generated method stub
 
        int n=3;
        int m=3;
        String str1 = "hello";//"hello"存在于常量池,str1,str2都指向该常量池
        String str2 = "hello";
 
      
        System.out.println("基本数据类型的 == 　判定:"+(n==m));  //判断数据
        System.out.println("引用数据类型的 ==　判定"+(str1==str2));//判断地址
 
 
 
        String str3 = new String("hello");//"hello"存在于堆的对象内存区,非常量区
        String str4 = new String("hello");
       
        System.out.println("对象的 == 判定:"+(str3 == str4)); //判断地址
        System.out.println("对象的 equals() 判定"+str3.equals(str4));//判断数据
 
        str4 = str3;
        System.out.println("对象赋值后的 == 判定"+( str3 == str4 ) );
        System.out.println("对象的 equals() 判定"+str3.equals(str4));
    }
 
}


基本数据类型的 == 　判定:true
引用数据类型的 ==　判定true
对象的 == 判定:false
对象的 equals() 判定true
对象赋值后的 == 判定true
对象的 equals() 判定true
```

**3. 实现**  

- 检查是否为同一个对象的引用，如果是直接返回 true；
- 检查是否是同一个类型，如果不是，直接返回 false；
- 将 Object 对象进行转型；
- 判断每个关键域是否相等。

```java
public class EqualExample {

    private int x;
    private int y;
    private int z;

    public EqualExample(int x, int y, int z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        EqualExample that = (EqualExample) o;

        if (x != that.x) return false;
        if (y != that.y) return false;
        return z == that.z;
    }
}
```

### hashCode()

hashCode() 返回哈希值，而 equals() 是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价，这是因为计算哈希值具有随机性，两个值不同的对象可能计算出相同的哈希值。

在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象哈希值也相等。

HashSet  和 HashMap 等集合类使用了 hashCode()  方法来计算对象应该存储的位置，因此要将对象添加到这些集合类中，需要让对应的类实现 hashCode()  方法。

下面的代码中，新建了两个等价的对象，并将它们添加到 HashSet 中。我们希望将这两个对象当成一样的，只在集合中添加一个对象。但是 EqualExample 没有实现 hashCode() 方法，因此这两个对象的哈希值是不同的，最终导致集合添加了两个等价的对象。

```java
EqualExample e1 = new EqualExample(1, 1, 1);
EqualExample e2 = new EqualExample(1, 1, 1);
System.out.println(e1.equals(e2)); // true
HashSet<EqualExample> set = new HashSet<>();
set.add(e1);
set.add(e2);
System.out.println(set.size());   // 2
```

理想的哈希函数应当具有均匀性，即不相等的对象应当均匀分布到所有可能的哈希值上。这就要求了哈希函数要把所有域的值都考虑进来。可以将每个域都当成 R 进制的某一位，然后组成一个 R 进制的整数。

R 一般取 31，因为它是一个奇素数，如果是偶数的话，当出现乘法溢出，信息就会丢失，因为与 2 相乘相当于向左移一位，最左边的位丢失。并且一个数与 31 相乘可以转换成移位和减法：`31*x == (x<<5)-x`，编译器会自动进行这个优化。

```java
@Override
public int hashCode() {
    int result = 17;
    result = 31 * result + x;
    result = 31 * result + y;
    result = 31 * result + z;
    return result;
}
```

### toString()

默认返回 ToStringExample@4554617c 这种形式，其中 @ 后面的数值为散列码的无符号十六进制表示。

```java
public class ToStringExample {

    private int number;

    public ToStringExample(int number) {
        this.number = number;
    }
}
```

```java
ToStringExample example = new ToStringExample(123);
System.out.println(example.toString());
```

```html
ToStringExample@4554617c
```

### clone()

**1. cloneable**  

clone() 是 Object 的 protected 方法，它不是 public，一个类不显式去重写 clone()，其它类就不能直接去调用该类实例的 clone() 方法。

```java
public class CloneExample {
    private int a;
    private int b;
}
```

```java
CloneExample e1 = new CloneExample();
// CloneExample e2 = e1.clone(); // 'clone()' has protected access in 'java.lang.Object'
```

重写 clone() 得到以下实现：

```java
public class CloneExample {
    private int a;
    private int b;

    @Override
    public CloneExample clone() throws CloneNotSupportedException {
        return (CloneExample)super.clone();
    }
}
```

```java
CloneExample e1 = new CloneExample();
try {
    CloneExample e2 = e1.clone();
} catch (CloneNotSupportedException e) {
    e.printStackTrace();
}
```

```html
java.lang.CloneNotSupportedException: CloneExample
```

以上抛出了 CloneNotSupportedException，这是因为 CloneExample 没有实现 Cloneable 接口。

应该注意的是，clone() 方法并不是 Cloneable 接口的方法，而是 Object 的一个 protected 方法。Cloneable 接口只是规定，如果一个类没有实现 Cloneable 接口又调用了 clone() 方法，就会抛出 CloneNotSupportedException。

```java
public class CloneExample implements Cloneable {
    private int a;
    private int b;

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

**2. 浅拷贝**  

拷贝对象和原始对象的引用类型引用同一个对象。

```java
public class ShallowCloneExample implements Cloneable {

    private int[] arr;

    public ShallowCloneExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }

    @Override
    protected ShallowCloneExample clone() throws CloneNotSupportedException {
        return (ShallowCloneExample) super.clone();
    }
}
```

```java
ShallowCloneExample e1 = new ShallowCloneExample();
ShallowCloneExample e2 = null;
try {
    e2 = e1.clone();
} catch (CloneNotSupportedException e) {
    e.printStackTrace();
}
e1.set(2, 222);
System.out.println(e2.get(2)); // 222
```

**3. 深拷贝**  

拷贝对象和原始对象的引用类型引用不同对象。

```java
public class DeepCloneExample implements Cloneable {

    private int[] arr;

    public DeepCloneExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }

    @Override
    protected DeepCloneExample clone() throws CloneNotSupportedException {
        DeepCloneExample result = (DeepCloneExample) super.clone();
        result.arr = new int[arr.length];
        for (int i = 0; i < arr.length; i++) {
            result.arr[i] = arr[i];
        }
        return result;
    }
}
```

```java
DeepCloneExample e1 = new DeepCloneExample();
DeepCloneExample e2 = null;
try {
    e2 = e1.clone();
} catch (CloneNotSupportedException e) {
    e.printStackTrace();
}
e1.set(2, 222);
System.out.println(e2.get(2)); // 2
```

**4. clone() 的替代方案**  

使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换。Effective Java 书上讲到，最好不要去使用 clone()，可以使用拷贝构造函数或者拷贝工厂来拷贝一个对象。

```java
public class CloneConstructorExample {

    private int[] arr;

    public CloneConstructorExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public CloneConstructorExample(CloneConstructorExample original) {
        arr = new int[original.arr.length];
        for (int i = 0; i < original.arr.length; i++) {
            arr[i] = original.arr[i];
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }
}
```

```java
CloneConstructorExample e1 = new CloneConstructorExample();
CloneConstructorExample e2 = new CloneConstructorExample(e1);
e1.set(2, 222);
System.out.println(e2.get(2)); // 2
```

# 封装

### java中的封装体：

- 方法
- 类

1. 安全：调用者不知道封装体的具体实现
2. 复用性：封装体可以被重复使用
3. 简化性：

# 继承

## 访问权限

Java 中有三个访问权限修饰符：private、protected 以及 public，如果不加访问修饰符，表示包级可见。

可以对类或类中的成员（字段和方法）加上访问修饰符。

- 类可见表示其它类可以用这个类创建实例对象。
- 成员可见表示其它类可以用这个类的实例对象访问到该成员；

protected 用于修饰成员，表示在继承体系中成员对于子类可见，但是这个访问修饰符对于类没有意义。

设计良好的模块会隐藏所有的实现细节，把它的 API 与它的实现清晰地隔离开来。模块之间只通过它们的 API 进行通信，一个模块不需要知道其他模块的内部工作情况，这个概念被称为信息隐藏或封装。因此访问权限应当尽可能地使每个类或者成员不被外界访问。

如果子类的方法重写了父类的方法，那么子类中该方法的访问级别不允许低于父类的访问级别。这是为了确保可以使用父类实例的地方都可以使用子类实例去代替，也就是确保满足里氏替换原则。

字段决不能是公有的，因为这么做的话就失去了对这个字段修改行为的控制，客户端可以对其随意修改。例如下面的例子中，AccessExample 拥有 id 公有字段，如果在某个时刻，我们想要使用 int 存储 id 字段，那么就需要修改所有的客户端代码。

```java
public class AccessExample {
    public String id;
}
```

可以使用公有的 getter 和 setter 方法来替换公有字段，这样的话就可以控制对字段的修改行为。

```java
public class AccessExample {

    private int id;

    public String getId() {
        return id + "";
    }

    public void setId(String id) {
        this.id = Integer.valueOf(id);
    }
}
```

但是也有例外，如果是包级私有的类或者私有的嵌套类，那么直接暴露成员不会有特别大的影响。

```java
public class AccessWithInnerClassExample {

    private class InnerClass {
        int x;
    }

    private InnerClass innerClass;

    public AccessWithInnerClassExample() {
        innerClass = new InnerClass();
    }

    public int getValue() {
        return innerClass.x;  // 直接访问
    }
}
```

### 注意：

在一个子类被创建的时候，首先会在内存中创建一个父类对象，然后在父类对象外部放上子类独有的属性，两者合起来形成一个子类的对象。所以所谓的继承使子类拥有父类所有的属性和方法其实可以这样理解，子类对象确实拥有父类对象中所有的属性和方法，但是父类对象中的私有属性和方法，子类是无法访问到的，只是拥有，但不能使用。

就像有些东西你可能拥有，但是你并不能使用。所以子类对象是绝对大于父类对象的，所谓的子类对象只能继承父类非私有的属性及方法的说法是错误的。可以继承，只是无法访问到而已

```

```

## 继承

<img src="C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210728114249836.png" alt="image-20210728114249836" style="zoom: 80%;" />

### 继承关系的特点

- 私有成员不能继承
- 构造方法不能继承

<img src="C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210728120550634.png" alt="image-20210728120550634" style="zoom: 67%;" />

<img src="C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210728120756412.png" alt="image-20210728120756412" style="zoom: 67%;" />

### 继承关系中成员变量与方法

- **重写要求子类不能有比父类更加严格的访问修饰符。**
- 直接通过对象名访问成员变量，看等号左边是谁（创建对象时），优先用谁，没有则向上找
- 间接通过成员方法访问成员变量，看该方法属于谁，优先调用谁，没有则向上找

![image-20210728114853431](C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210728114853431.png)

![image-20210728115603980](C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210728115603980.png)

### 继承关系中构造方法

1. **子类不可以继承父类的构造方法，只可以调用父类的构造方法。**
2. 子类中所有的构造函数都会默认访问父类中的空参数构造函数，这是因为子类的构造函数内第一行都有默认的super（）语句。super（）表示子类在初始化时调用父类的空参数的构造函数来完成初始化。
3. 一个类都会有默认的空参数的构造函数，若指定了带参构造函数，那么默认的空参数的构造函数，就**不存在了**。
4. 这时如果子类的构造函数有默认的super（）语句，那么就会出现错误，因为父类中没有空参数的构造函数。因此，在子类中默认super（）语句，在父类中无对应的构造函数，必须在子类的构造函数中通过this或super（参数）指定要访问的父类中的构造函数。

#### 总结

- 子类创建对象时，会优先调用父类的构造方法；
- 子类的构造方法第一行，隐含语句super(),用于调用父类的默认无参构造
- 若父类没有无参构造，子类仍需优先手动调用父类的其他带参构造方法；





## 重写与重载

### 重写

#### 概念

**存在于继承体系**中，指子类实现了一个与父类在方法声明上**完全相同**的一个方法。

除了方法体不一样，其他都一样

#### 三个限制

为了满足里式替换原则，重写有以下三个限制：

- 子类方法的**访问权限**必须大于等于父类方法；可见度大于父类
- 子类方法的**返回类型**必须是父类方法返回类型或为其子类型。
- 子类方法**抛出的异常类型**必须是父类抛出异常类型或为其子类型。

使用 @Override 注解，可以让编译器帮忙检查是否满足上面的三个限制条件。

下面的示例中，SubClass 为 SuperClass 的子类，SubClass 重写了 SuperClass 的 func() 方法。其中：

- 子类方法访问权限为 public，大于父类的 protected。
- 子类的返回类型为 ArrayList\<Integer\>，是父类返回类型 List\<Integer\> 的子类。
- 子类抛出的异常类型为 Exception，是父类抛出异常 Throwable 的子类。
- 子类重写方法使用 @Override 注解，从而让编译器自动检查是否满足限制条件。

```java
class SuperClass {
    protected List<Integer> func() throws Throwable {
        return new ArrayList<>();
    }
}

class SubClass extends SuperClass {
    @Override
    public ArrayList<Integer> func() throws Exception {
        return new ArrayList<>();
    }
}
```

#### 方法调用

1. 在调用一个方法时，先从本类中查找看是否有对应的方法
2. 如果没有再到父类中查看，看是否从父类继承来。
3. 否则就要对参数进行转型，转成父类之后看是否有对应的方法。总的来说，方法调用的优先级为：

- this.func(this)
- super.func(this)
- this.func(super)
- super.func(super)


```java
/*
    A
    |
    B
    |
    C
    |
    D
 */


class A {

    public void show(A obj) {
        System.out.println("A.show(A)");
    }

    public void show(C obj) {
        System.out.println("A.show(C)");
    }
}

class B extends A {

    @Override
    public void show(A obj) {
        System.out.println("B.show(A)");
    }
}

class C extends B {
}

class D extends C {
}
```

```java
public static void main(String[] args) {

    A a = new A();
    B b = new B();
    C c = new C();
    D d = new D();

    // 在 A 中存在 show(A obj)，直接调用
    a.show(a); // A.show(A)
    // 在 A 中不存在 show(B obj)，将 B 转型成其父类 A
    a.show(b); // A.show(A)
    // 在 B 中存在从 A 继承来的 show(C obj)，直接调用
    b.show(c); // A.show(C)
    // 在 B 中不存在 show(D obj)，但是存在从 A 继承来的 show(C obj)，将 D 转型成其父类 C
    b.show(d); // A.show(C)

    // 引用的还是 B 对象，所以 ba 和 b 的调用结果一样
    A ba = new B();
    ba.show(c); // A.show(C)
    ba.show(d); // A.show(C)
}
```

#### 重写私有方法

**重写私有方法**

```java
//: polymorphism/PrivateOverride.java
// Trying to override a private method.
package polymorphism;
import static net.mindview.util.Print.*;

public class PrivateOverride {
  private void f() { print("private f()"); }
  public static void main(String[] args) {
    PrivateOverride po = new Derived();
    po.f();
  }
}

class Derived extends PrivateOverride {
  public void f() { print("public f()"); }
} 
```

/* Output:
**private f()**
*///:~

期望输出的是public f(),但是父类中的private方法被自动认为是final方法，对子类是屏蔽的

如果在子类中定义的方法和基类中的一个 private 方法签名相同**此时子类的方法不是重写基类方法，而是在子类中定义了一个新的方法。**

#### 静态方法

静态方法属于类，不能被重写

### 重载

**存在于同一个类中**，指一个方法与**已经存在的方法名称上相同**，但是**参数类型、个数、顺序至少有一个不同**。

**与返回值不同无关**

```java
class OverloadingExample {
    public void show(int x) {
        System.out.println(x);
    }

    public void show(int x, String y) {
        System.out.println(x + " " + y);
    }
}
```

```java
public static void main(String[] args) {
    OverloadingExample example = new OverloadingExample();
    example.show(1);
    example.show(1, "2");
}
```

### 比较

**重写** 要求两同两小一大原则，

 方法名相同，参数类型相同，

子类**返回类型**小于等于父类方法返回类型， 子类抛出异常小于等于父类方法抛出异常， 

子类访问权限大于等于父类方法访问权限。

注意：这里的返回类型必须要在有继承关系的前提下比较］

**重载** 方法名必须相同，**参数类型必须不同**，包括但不限于一项，参数数目，参数类型，参数顺序

## 抽象类

由来
父类中的方法，被它的子类们重写，子类各自的实现都不尽相同。那么父类的方法声明和方法主体，只有声明还有
意义，而方法主体则没有存在的意义了。我们把没有方法主体的方法称为抽象方法。Java语法规定，包含抽象方法
的类就是抽象类。

#### 定义

抽象方法 ： 没有方法体的方法。
抽象类：包含抽象方法的类。

抽象类和普通类最大的区别是，**抽象类不能被实例化，只能被继承。**

#### abstract使用格式

**抽象方法：**
使用**abstract 关键字修饰方法**，该方法就成了抽象方法，**抽象方法只包含一个方法名，而没有方法体**。

#### 抽象类

如果一个类包含抽象方法，那么该类必须是抽象类。

**抽象的使用：**

继承抽象类的子类必须重写父类所有的抽象方法。否则，该子类也必须声明为抽象类。最终，必须有子类实现该父
类的抽象方法，否则，从最初的父类到最终的子类都不能创建对象，失去意义。

**定义格式：**

```
修饰符 abstract 返回值类型 方法名 (参数列表)；
public abstract void run()；
```

```java
public abstract class AbstractClassExample {

    protected int x;
    private int y;

    public abstract void func1();//抽象方法

    public void func2() {
        System.out.println("func2");
    }
}
```

```java
public class AbstractExtendClassExample extends AbstractClassExample {
    @Override//重写父类方法
    public void func1() {
        System.out.println("func1");
    }
}
```

```java
// AbstractClassExample ac1 = new AbstractClassExample(); // 'AbstractClassExample' is abstract; cannot be instantiated
AbstractClassExample ac2 = new AbstractExtendClassExample();
ac2.func1();
```



#### 注意事项

关于抽象类的使用，以下为语法上要注意的细节，虽然条目较多，但若理解了抽象的本质，无需死记硬背。

1. 抽象类不能创建对象，如果创建，编译无法通过而报错。只能创建其非抽象子类的对象。 理解：假设创建了抽象类的对象，调用抽象的方法，而抽象方法没有具体的方法体，没有意义。
2. 抽象类中，可以有构造方法，是供子类创建对象时，**初始化父类成员使用的**。 理解：**子类的构造方法中，有默认的super()，需要访问父类构造方法。**
3.  抽象类中，不一定包含抽象方法，但是有抽象方法的类必定是抽象类。理解：未包含抽象方法的抽象类，目的就是不想让调用者创建该类对象，通常用于某些特殊的类结构设 计。 
4.  抽象类的子类，必须重写抽象父类中所有的抽象方法，否则，编译无法通过而报错。除非该子类也是抽象 类。 理解：假设不重写所有抽象方法，则类中可能包含抽象方法。那么创建对象后，调用抽象的方法，没有 意义。



1. JDK 1.8以前，抽象类的方法默认访问权限为protected
2. JDK 1.8时，抽象类的方法默认访问权限变为default
3. 关于接口
4. JDK 1.8以前，接口中的方法必须是public的
5. JDK 1.8时，接口中的方法可以是public的，也可以是default的
6. JDK 1.9时，接口中的方法可以是private的

## **接口**  

```java
public interface InterFaceName {
public abstract void method();
}
public interface InterFaceName {
public default void method() {
// 执行语句
}
public static void method2() {
// 执行语句
}
}
public interface InterFaceName {
private void method() {
// 执行语句
}}
```



#### 概述

接口，是Java语言中一种引用类型，是方法的集合，如果说类的内部封装了成员变量、构造方法和成员方法，那么
接口的内部主要就是封装了方法，**包含抽象方法（JDK 7及以前），默认方法和静态方法（JDK 8），私有方法** **（JDK 9）。**

接口的定义，它与定义类方式相似，但是使用 interface 关键字。它也会被编译成.class文件，但一定要明确它并
不是类，而是另外一种引用数据类型。

*引用数据类型：数组，类，接口。*

接口的使用，它不能创建对象，但是可以被实现（ implements ，类似于被继承）。一个实现接口的类（可以看做是接口的子类），需要实现接口中所有的抽象方法，创建该类对象，就可以调用方法了，否则它必须是一个抽象
类。

##### **定义格式**

```
public interface 接口名称 {
// 抽象方法
// 默认方法
// 静态方法
// 私有方法
}
```

##### 含有抽象方法

抽象方法：使用abstract 关键字修饰，可以省略，没有方法体。该方法供子类实现使用。
代码如下：

```
public interface InterFaceName {
public abstract void method();
}
```

##### 含有默认方法和静态方法

默认方法：使用 default 修饰，不可省略，供子类调用或者子类重写。
静态方法：使用 static 修饰，供接口直接调用。
代码如下：

```java
public interface InterFaceName {
public default void method() {
// 执行语句
}
public static void method2() {
// 执行语句
}
}
```

##### 含有私有方法和私有静态方法

私有方法：使用 private 修饰，**供接口中的默认方法或者静态方法调用。**
代码如下：

```java
public interface InterFaceName {
private void method() {
// 执行语句
}
}
```

#### 基本的实现

##### 实现的概述

类与接口的关系为实现关系，即类实现接口，该类可以称为接口的实现类，也可以称为接口的子类。实现的动作类
似继承，格式相仿，只是关键字不同，实现使用 implements 关键字。

非抽象子类实现接口：

1. 抽象方法：**必须重写接口中所有抽象方法。**
2. 默认方法：**继承了接口的默认方法，即可以直接调用，也可以重写**。
3. 静态方法：静态与.class 文件相关，**只能使用接口名调用**，不可以通过实现类的类名或者实现类的对象调用，
4. 私有方法：Java 9 开始可以有private 方法
    私有方法：**只有默认方法可以调用。**
    私有静态方法：**默认方法和静态方法可以调用。**
    如果一个接口中有多个默认方法，并且方法中有重复的内容，那么可以抽取出来，封装到私有方法中，供默认方法去调用。从设计的角度讲，私有的方法是对默认方法和静态方法的辅助。
5. 实现格式：

```java
class 类名 implements 接口名 {
// 重写接口中抽象方法【必须】
// 重写接口中默认方法【可选】
}
```

```java
私有方法：

public interface LiveAble {
default void func(){
func1();
func2();
}
private void func1(){
System.out.println("跑起来~~~");
}
private void func2(){
System.out.println("跑起来~~~");
}
}
```

#### 成员变量

接口中，无法定义成员变量，但是可以定义常量，其值不可以改变，默认使用public static final修饰。
接口中，没有构造方法，不能创建对象。
接口中，没有静态代码块。

小贴士：

- *接口是抽象类的延伸，在 **Java 8 之前**，它可以看成是一个**完全抽象的类**，也就是说它**不能有任何的方法实现**。*

- ***从 Java 8 开始，接口也可以拥有默认的方法实现**，这是因为不支持默认方法的接口的维护成本太高了。在 Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类，让它们都实现新增的方法。*

1. *接口的成员（字段 + 方法）默认都是 **public abstract** 的，**并且不允许定义为 private 或者 protected**。从 Java 9 开始，允许将方法定义为 private，这样就能定义某些复用的代码又不会把方法暴露出去。*
2. *接口**方法**默认是**public abstract**的，且**实现该接口的类中对应的方法的可见性不能小于接口方法的可见性**，因此也只能是public的、*
3. ***接口的字段默认都是 static 和 final 的。**不能用private修饰(由于私有不能被继承)；在接口中，属性默认public static final，这三个关键字可以省略；final修饰的属性必须赋值；*

```java
public interface InterfaceExample {

    void func1();

    default void func2(){
        System.out.println("func2");
    }

    int x = 123;
    // int y;               // Variable 'y' might not have been initialized
    public int z = 0;       // Modifier 'public' is redundant for interface fields
    // private int k = 0;   // Modifier 'private' not allowed here
    // protected int l = 0; // Modifier 'protected' not allowed here
    // private void fun3(); // Modifier 'private' not allowed here
}
```

```java
public class InterfaceImplementExample implements InterfaceExample {
    @Override
    public void func1() {
        System.out.println("func1");
    }
}
```

```java
// InterfaceExample ie1 = new InterfaceExample(); // 'InterfaceExample' is abstract; cannot be instantiated
InterfaceExample ie2 = new InterfaceImplementExample();
ie2.func1();
System.out.println(InterfaceExample.x);
```

## 抽象类和接口比较  

从设计层面上看，抽象类提供了一种 IS-A 关系，需要满足里式替换原则，即子类对象必须能够替换掉所有父类对象。而接口更像是一种 LIKE-A 关系，它只是提供一种方法实现契约，并不要求接口和实现接口的类具有 IS-A 关系。

**继承方面**

- 一个类可以**实现多个接口，但是不能继承多个抽象类。**

**方法方面**

- **抽象类可以有构造方法**，接口中不能有构造方法；
  抽象类有构造方法，但是不是用来实例化的，**是用来初始化的**。
-  一个抽象类中的方法不一定是抽象方法，即其中的方法可以有实现（有方法体），java8 前 **接口中的方法都是抽象方法，不能有方法体，只有声明；**8时 有默认方法 可以有方法体 
- j8 **接口中的方法只能是public和default（J9 有private），而抽象类的成员可以有多种访问权限。**

**成员变量方面**

- **抽象类可以有普通成员变量**而接口不可以.
- 接口的字段只能是 static 和 final 类型的，而抽象类的字段没有这种限制。

```
1、一个子类只能继承一个抽象类（虚类），但能实现多个接口；
2、一个抽象类可以有构造方法，接口没有构造方法；
3、一个抽象类中的方法不一定是抽象方法，即其中的方法可以有实现（有方法体），接口中的方法都是抽象方法，不能有方法体，只有声明；
4、一个抽象类可以是public、private、protected、default,
   接口只有public;
5、一个抽象类中的方法可以是public、private、protected、default，
   接口中的方法只能是public和default
```

**4. 使用选择**  

使用接口：

- 需要让不相关的类都实现一个方法，例如不相关的类都可以实现 Comparable 接口中的 compareTo() 方法；
- 需要使用多重继承。

使用抽象类：

- 需要在几个相关的类中共享代码。
- 需要能控制继承来的成员的访问权限，而不是都为 public。
- 需要继承非静态和非常量字段。

在很多情况下，接口优先于抽象类。因为接口没有抽象类严格的类层次结构要求，可以灵活地为一个类添加行为。并且从 Java 8 开始，接口也可以有默认的方法实现，使得修改接口的成本也变的很低。

- [Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
- [深入理解 abstract class 和 interface](https://www.ibm.com/developerworks/cn/java/l-javainterface-abstract/)
- [When to Use Abstract Class and Interface](https://dzone.com/articles/when-to-use-abstract-class-and-intreface)
- [Java 9 Private Methods in Interfaces](https://www.journaldev.com/12850/java-9-private-methods-interfaces)



## super

<img src="C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210728115015096.png" alt="image-20210728115015096" style="zoom:67%;" />

- 访问父类的构造函数：可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。应该注意到，子类一定会调用父类的构造函数来完成初始化工作，一般是调用父类的默认构造函数，如果子类需要调用父类其它构造函数，那么就可以使用 super() 函数。
- 访问父类的成员：如果子类重写了父类的某个方法，可以通过使用 super 关键字来引用父类的方法实现。

```java
public class SuperExample {

    protected int x;
    protected int y;

    public SuperExample(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public void func() {
        System.out.println("SuperExample.func()");
    }
}
```

```java
public class SuperExtendExample extends SuperExample {

    private int z;

    public SuperExtendExample(int x, int y, int z) {
        super(x, y);
        this.z = z;
    }

    @Override
    public void func() {
        super.func();
        System.out.println("SuperExtendExample.func()");
    }
}
```

```java
SuperExample e = new SuperExtendExample(1, 2, 3);
e.func();
```

```html
SuperExample.func()
SuperExtendExample.func()
```

[Using the Keyword super](https://docs.oracle.com/javase/tutorial/java/IandI/super.html)

<img src="C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210728114007019.png" alt="image-20210728114007019" style="zoom:67%;" />

![image-20210728114116038](C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210728114116038.png)

# 多态

## 概念

多态存在的三个必要条件：**继承**，**重写**，**父类引用指向子类对象**

## 特点

多态性是**对象多种表现形式**的体现。

- **多态就是同一个接口，使用不同的实例而执行不同操作**
- 当使用多态方式调用方法时，**首先检查父类中是否有该方法**，如果没有，则编译错误；如果有，再去调用子类的同名方法。

**extends继承或者implements实现 是多态的前题**

## 优点

1. 消除类型之间的耦合关系
2. 可替换性
3. 可扩充性
4. 接口性
5. 灵活性
6. 简化性

## 成员变量访问

### 直接

通过对象名访问成员变量，看等号左边是谁（创建对象时），优先用谁，没有则向上找

### 间接

通过成员方法访问成员变量，看该方法属于谁，优先调用谁，没有则向上找

**子类没有覆盖重写就是父；覆盖重写就是子**



## 成员方法访问

编译看左（左边的类或者接口是否有这个方法）无则不通过

运行看右（右边的类或者接口是否有这个方法）优则调用，无则想上找

​	先看子，子没有向上找

**没有super修饰时，不管什么情况（进入父类也一样）都调用被重写的子类方法；**

### 例外

**重写私有方法**

```java
//: polymorphism/PrivateOverride.java
// Trying to override a private method.
package polymorphism;
import static net.mindview.util.Print.*;

public class PrivateOverride {
  private void f() { print("private f()"); }
  public static void main(String[] args) {
    PrivateOverride po = new Derived();
    po.f();
  }
}

class Derived extends PrivateOverride {
  public void f() { print("public f()"); }
} 
```

/* Output:
private f()
*///:~

期望输出的是public f(),但是父类中的private方法被自动认为是final方法，对子类是屏蔽的，所以不能被重载；所以在这种情况下，子类的f()被当作一个全新的方法。



## 存在的三个必要条件

- **继承**/实现
- **重写**
- **父类引用指向子类对象：Parent p = new Child();**

<img src="https://www.runoob.com/wp-content/uploads/2013/12/2DAC601E-70D8-4B3C-86CC-7E4972FC2466.jpg" alt="img" style="zoom: 25%;" />

```java
class Shape {
    void draw() {}
}
 
class Circle extends Shape {
    void draw() {
        System.out.println("Circle.draw()");
    }
}
 
class Square extends Shape {
    void draw() {
        System.out.println("Square.draw()");
    }
}
 
class Triangle extends Shape {
    void draw() {
        System.out.println("Triangle.draw()");
    }
}
```

**当使用多态方式调用方法时，首先检查父类中是否有该方法，如果没有，则编译错误；如果有，再去调用子类的同名方法。**

## 转型

### 向上转型

父类引用指向子类对象



### 向下转型

将父类对象**本来**子类对象



# 反射

### 反射的概念与作用

**框架设计的灵魂**

**框架**：半成品软件。可以在框架的基础上进行软件开发，简化编码

**反射：**

- 通过类的字节码文件获取类中的成员并使用
- **将类的各个组成部分（成员变量、构造方法、方法）封装为其他对象**，这就是反射机制

反射可以提供运行时的类信息，并且这个类可以在运行时才加载进来，甚至在编译时期该类的 .class 不存在也可以加载进来。

- 分析类
  - 加载并初始化一个类
  - 查看类的所有属性和方法
- 查看并使用对象
  - 查看一个对象的所有属性和方法
  - 使用对象的任意属性和方法



每个类都有一个   **Class**   对象，包含了与类有关的信息。当编译一个新类时，会产生一个同名的 .class 文件，该文件内容保存着 Class 对象。

类加载相当于 Class 对象的加载，类在第一次使用时才动态加载到 JVM 中。也可以使用 `Class.forName("com.mysql.jdbc.Driver")` 这种方式来控制类的加载，该方法会返回一个 Class 对象。

反射可以提供运行时的类信息，并且这个类可以在运行时才加载进来，甚至在编译时期该类的 .class 不存在也可以加载进来。

Class 和 java.lang.reflect 一起对反射提供了支持，java.lang.reflect 类库主要包含了以下三个类：

-  **Field**  ：可以使用 get() 和 set() 方法读取和修改 Field 对象关联的字段；
-  **Method**  ：可以使用 invoke() 方法调用与 Method 对象关联的方法；
-  **Constructor**  ：可以用 Constructor 的 newInstance() 创建新的对象。

**反射的优点：**  

-  **可扩展性**   ：应用程序可以利用全限定名创建可扩展对象的实例，来使用来自外部的用户自定义类。
-  **类浏览器和可视化开发环境**   ：一个类浏览器需要可以枚举类的成员。可视化开发环境（如 IDE）可以从利用反射中可用的类型信息中受益，以帮助程序员编写正确的代码。
-  **调试器和测试工具**   ： 调试器需要能够检查一个类里的私有成员。测试工具可以利用反射来自动地调用类里定义的可被发现的 API 定义，以确保一组测试中有较高的代码覆盖率。

**反射的缺点：**  

尽管反射非常强大，但也不能滥用。如果一个功能可以不用反射完成，那么最好就不用。在我们使用反射技术时，下面几条内容应该牢记于心。

- **性能开销**   ：反射涉及了动态类型的解析，所以 JVM 无法对这些代码进行优化。因此，反射操作的效率要比那些非反射操作低得多。我们应该避免在经常被执行的代码或对性能要求很高的程序中使用反射。

- **安全限制**   ：使用反射技术要求程序必须在一个没有安全限制的环境中运行。如果一个程序必须在有安全限制的环境中运行，如 Applet，那么这就是个问题了。

- **内部暴露**   ：由于反射允许代码执行一些在正常情况下不被允许的操作（比如访问私有的属性和方法），所以使用反射可能会导致意料之外的副作用，这可能导致代码功能失调并破坏可移植性。反射代码破坏了抽象性，因此当平台发生改变的时候，代码的行为就有可能也随着变化。

  


**反射相关的类**

- Class类（重点）
- Classloader类
- Constructor类（重点）
- Method类（重点）
- Field类（重点）

### 反射的用法

先获取class对象，才能**通过Class对象的方法获取此对象的 构造的方法、成员方法、成员变量**

具体地：

Class 和 java.lang.reflect 一起对反射提供了支持，java.lang.reflect 类库主要包含了以下三个类：

-  **Field**  ：可以使用 get() 和 set() 方法读取和修改 Field 对象关联的字段；
-  **Method**  ：可以使用 invoke() 方法调用与 Method 对象关联的方法；
-  **Constructor**  ：可以用 Constructor 的 newInstance() 创建新的对象。

![Java代码的三个阶段](D:\桌面\学习资料\程序猿\笔记图库\截图\Java代码的三个阶段.bmp)

####  类加载器与Class对象

- 类加载器负责将类的字节码文件加载到内存中，并生成对象的Class对象
- 每个类都有一个   **Class**   对象，包含了与类有关的信息。当编译一个新类时，会产生一个同名的 .class 文件，该文件内容保存着 Class 对象。
- 类加载相当于 Class 对象的加载，类在第一次使用时才动态加载到 JVM 中。也可以使用 `Class.forName("com.mysql.jdbc.Driver")` 这种方式来控制类的加载，该方法会返回一个 Class 对象。

#### 类加载的时机

**第一次使用类中的成员时，类加载器就会将类的字节码文件加载到内存中**

- 创建类的实例

```
Student s = new student();
```

注意：一个类的字节码文件只会被加载一次

- 访问类的静态成员

```
Calendar.getInstance()；
```

- 初始化类的子类

```
class User extends Person{}
User user = new User();//先加载父类后加载子类
```

- 反射方式创建类的Class对象

```
Class class = Class.forName("类的正名")
Class.forName("com.mysql.jdbc.Driver")`这种方式来控制类的加载，该方法会返回一个 Class 对象。
```

**有了类的字节码文件，如何获取Class对象？**

#### **获取class对象**

1. 对象
2. 类名
3. 类名字符串-类加载器

具体地：

- **Object类的getClass()方法**
  - **Class class = 对象名.getClass();**
- **类的静态方法属性**
  - **Class calss = 类名.class;**
- **Class类的静态方法**
  - **Class class = Class.forName("类的正名")**

```
public class python {
        public static void main(String[] args) throws ClassNotFoundException {
                Student student = new Student();
                Class clazz1 = student.getClass();
                Class clazz2 = Student.class;
         
                Class clazz3 = Class.forName("Student");
                System.out.println(clazz1.equals(clazz2));
                System.out.println(clazz3.equals(clazz2));

        }
}

//true true
```

**注意**：****

​		**同一个字节码文件(*.class)在一次程序运行过程中，只会被加载一次，不论通过哪一种方式获取的Class对象都是同一个。**

##### Class对象的作用

```

	* 获取功能：
		1. 获取成员变量们
			* Field[] getFields() ：获取所有public修饰的成员变量
			* Field getField(String name)   获取指定名称的 public修饰的成员变量

			* Field[] getDeclaredFields()  获取所有的成员变量，不考虑修饰符
			* Field getDeclaredField(String name)  
		2. 获取构造方法们
			* Constructor<?>[] getConstructors()  
			* Constructor<T> getConstructor(类<?>... parameterTypes)  

			* Constructor<T> getDeclaredConstructor(类<?>... parameterTypes)  
			* Constructor<?>[] getDeclaredConstructors()  
		3. 获取成员方法们：
			* Method[] getMethods()  
			* Method getMethod(String name, 类<?>... parameterTypes)  

			* Method[] getDeclaredMethods()  
			* Method getDeclaredMethod(String name, 类<?>... parameterTypes)  

		4. 获取全类名	
			* String getName()  
```



#### 获取构造方法并使用

**Constructor<T> 对象：构造器对象**



**通过**Class**对象获取构造器对象**

```java
public class Student {
    String name;
    int age;

    public Student() {
    }

    public Student(String name) {
        this.name = name;
        System.out.println("您录入的值为"+name);
    }

    private Student(int age) {
        this.age = age;
    }
}


import org.python.util.PythonInterpreter;

import java.lang.reflect.Constructor;

public class python {
        public static void main(String[] args) throws  NoSuchMethodException {
                //获取类的字节码文件对象
                Student student = new Student();
                Class clazz1 = student.getClass();
                //通过class对象 获取构造器对象
                Constructor stu_c = clazz1.getConstructor();//获取public的无参构造
                System.out.println(stu_c);

                Constructor stu_c1 = clazz1.getConstructor(String.class);//获取public的带参构造
                System.out.println(stu_c1);

                Constructor stu_c2 =clazz1.getDeclaredConstructor(int.class);//获取私有的构造
                System.out.println(stu_c2);

                Constructor[] stu_c3 = clazz1.getConstructors();//获取public的构造 数组
                for (Constructor constructor : stu_c3) {
                        System.out.println(constructor);
                }
                
        }
        
}
```

**通过**Class**对象获取构造器对象并使用**

```
* Constructor:构造方法
	* 创建对象：
		* T newInstance(Object... initargs)  
		* 如果使用空参数构造方法创建对象，操作可以简化：Class对象的newInstance方法
```



```java
public class python {
        public static void main(String[] args) throws  Exception {
                //获取类的字节码文件对象

                Class clazz1 =Class.forName("Student");
                //通过class对象 获取构造器对象

                Constructor stu_c1 = clazz1.getConstructor(String.class);//获取public的带参构造
                System.out.println(stu_c1);

                System.out.println(stu_c1.getName());

                //通过构造器对象创建对象
                Student stu = (Student) stu_c1.newInstance("张三");

        }

}

public Student(java.lang.String)
Student
您录入的值为张三
```

####  获取成员方法并使用

**Method 方法对象**

```java
public class Student {
    String name;
    int age;

    public Student() {
    }

    public Student(String name) {
        this.name = name;
        System.out.println("您录入的值为"+name);
    }

    private Student(int age) {
        this.age = age;
    }

    public void show1(){
        System.out.println("公共的空参方法");
    }

    public int show2(int i, int j){
        System.out.println("公共的带参方法");
        return i;
    }

    private void show3(){
        System.out.println("私有的空参方法");
    }
}


import java.lang.reflect.Constructor;
import java.lang.reflect.Method;


public class python {
        public static void main(String[] args) throws  Exception {
                //获取类的字节码文件对象

                Class clazz1 =Class.forName("Student");
                //通过class对象 获取构造器对象

                Constructor stu_c1 = clazz1.getConstructor(String.class);//获取public的无参构造

                //通过构造器对象创建对象
                Student stu = (Student) stu_c1.newInstance("张三");

                //创建方法对象并调用成员方法
                Method method1 = clazz1.getMethod("show1");//公共的空参方法
                method1.invoke(stu);

                Method method2 = clazz1.getMethod("show2",int.class,int.class);//公共的带参方法
                int i = (int)method2.invoke(stu,1,2);

                Method method3 = clazz1.getDeclaredMethod("show3");//私有的带参方法
                method3.setAccessible(true);//暴力反射
                method3.invoke(stu);
            
            	 Method[] methods = clazz1.getMethods();//返回公共的方法数组
                for (Method method : methods) {
                        System.out.println(method);
                }

        }
}

您录入的值为张三
公共的空参方法
公共的带参方法
私有的空参方法

```

#### 获取成员变量并使用

Field**对象**
域（属性、成员变量）对象

```
* Method：方法对象
	* 执行方法：
		* Object invoke(Object obj, Object... args)  

	* 获取方法名称：
		* String getName:获取方法名
```

```java
import org.python.util.PythonInterpreter;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;


public class python {
        public static void main(String[] args) throws  Exception {
                //获取类的字节码文件对象
                Class clazz1 =Class.forName("Student");

                //通过class对象 获取构造器对象
                Constructor stu_c1 = clazz1.getConstructor();//获取public的无参构造

                //通过构造器对象创建对象
                Student stu = (Student) stu_c1.newInstance();

                //调用成员变量 并设置
                Field field = clazz1.getField("name");//公共的成员变量
                field.set(stu,"张无忌");
                Field field1= clazz1.getDeclaredField("age");//私有的成员变量
                field1.setAccessible(true);
                field1.set(stu,100);

                System.out.println(field);
                System.out.println(stu);

        }
}



public class Student {
    public String name;//默认是private
    private int age;

    public Student() {
    }

    public Student(String name) {
        this.name = name;
        System.out.println("您录入的值为"+name);
    }

    private Student(int age) {
        this.age = age;
    }

    public void show1(){
        System.out.println("公共的空参方法");
    }

    public int show2(int i, int j){
        System.out.println("公共的带参方法");
        return i;
    }

    private void show3(){
        System.out.println("私有的空参方法");
    }

    @Override
    public String toString() {
        return name+this.age;
    }
}

```

### 反射举例

需求：写一个"框架"，不能改变该类的任何代码的前提下，可以帮我们创建任意类的对象，并且执行其中任意方法
* 实现：
	1. 配置文件
	2. 反射
* 步骤：
	1. 将需要创建的对象的全类名和需要执行的方法定义在**配置文件**properties中
	2. 在程序中加载读取配置文件
	3. 使用反射技术来加载类文件进内存
	4. 创建对象
	5. 执行方法

# 异常

什么是异常？Java代码在运行时期发生的问题就是异常。

在Java中，把异常信息封装成了一个类。当出现了问题时，就会创建异常类对象并抛出异常相关的信息（如异常出现的位置、原因等）。

要理解Java异常处理是如何工作的，你需要掌握以下三种类型的异常：

- **检查性异常：**最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。
- **运行时异常：** 运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
- **错误：** 错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。

## 异常的继承体系

Throwable 可以用来表示任何可以作为异常抛出的类，分为两种：  **Error**   和 **Exception**。其中 Error 用来表示 JVM 无法处理的错误，Exception 分为两种：

  Throwable: 它是所有错误与异常的超类（祖宗类）

​    |- Error 错误

​    |- Exception 编译期异常,进行编译JAVA程序时出现的问题

​      |- RuntimeException 运行期异常, JAVA程序运行过程中出现的问题



-   **受检异常**  ：需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复；
-   **非受检异常**  ：是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序崩溃并且无法恢复。

![img](笔记图库/690102-20160728164909622-1770558953.png)

### Exception 类

Exception 类及其子类是 Throwable 的一种形式，它用来表示java程序中可能会产生的异常，并要求对产生的异常进行合理的异常处理。

它的父类是Throwable。Throwable是Java 语言中所有错误或异常的父类，即祖宗类。



### Error类

与异常Exception平级的有一个Error，它是Throwable的子类，它用来表示java程序中可能会产生的严重错误。解决办法只有一个，修改代码避免Error错误的产生。

`Error`和`Exception`的区别：`Error`通常是灾难性的致命的错误，是程序无法控制和处理的，当出现这些异常时，Java虚拟机（JVM）一般会选择终止线程；`Exception`通常情况下是可以被程序处理的，并且在程序中应该尽可能的去处理这些异常

### 编译期异常

检查异常

在正确的程序运行过程中，很容易出现的、情理可容的异常状况，在一定程度上这种异常的发生是可以预测的，并且一旦发生该种异常，就必须采取某种方式进行处理。

> ♠提示
>
> **除了`RuntimeException`及其子类以外，其他的`Exception`类及其子类都属于检查异常**，当程序中可能出现这类异常，**要么使用`try-catch`语句进行捕获，要么用`throws`子句抛出**，否则编译无法通过。

### 运行时异常

不受检查异常

**包括`RuntimeException`及其子类和`Error`**。

> ♠提示
>
> `不受检查异常`为编译器不要求强制处理的异常，`检查异常`则是编译器要求必须处置的异常。

**RuntimeException**

异常Exception类中，RuntimeException子类，RuntimeException及其它的子类**只能在Java程序运行过程中出现。**

- 在`Exception`分支中有一个重要的子类`RuntimeException`（运行时异常），该类型的异常自动为你所编写的程序定义`ArrayIndexOutOfBoundsException`（数组下标越界）、`NullPointerException`（空指针异常）、`ArithmeticException`（算术异常）、`MissingResourceException`（丢失资源）、`ClassNotFoundException`（找不到类）等异常，**这些异常是不检查异常**，RuntimeException和他的所有子类异常,都属于运行时期异常。NullPointerException,ArrayIndexOutOfBoundsException等都属于运行时期异常.

- 方法中抛出运行时期异常,方法定义中无需throws声明,调用者也无需处理此异常 

  运行时期异常一旦发生,需要程序人员修改源代码.

- 而`RuntimeException`之外的异常我们统称为非运行时异常，类型上属于`Exception`类及其子类，从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过。如`IOException`、`SQLException`等以及用户自定义的`Exception`异常，一般情况下**不自定义检查异常。**

- **编译时异常 必须显示处理**
- **运行时异常可以不处理。当出现这样的异常时，总是由虚拟机接管。**
  比如我们从来没有人去处理过Null Pointer Exception异常，它就是运行时异常，并且这种异常还是最常见的异常之一。
- 运行时异常也是可以被Catch块处理的。只不过往往不对它处理罢了。也就是说，如果不对运行时异常进行处理，那么出现运行时异常之后，要么是线程中止，要么是主程序终止
- **出现运行时异常后，系统会把异常一直往上层抛，一直遇到处理代码。如果没有处理块，到最上层，如果是多线程就由Thread.run()抛出，如果是单线程就被main()抛出。抛出之后，如果是线程，这个线程也就退出了。如果是主程序抛出的异常，整个程序也就退出了。**



### 异常处理throws

#### throw作用

*在编写程序时，我们必须要考虑程序出现问题的情况。或者若程序出了异常，JVM它会打包异常对象并抛出。*

**但是它所提供的信息不够给力。想要更清晰，需要自己抛出异常信息。*当方法内部抛出异常，那么我们必须处理这个异常对象；**

- 可以使用**throws**关键字处理异常对象，**会把异常对象声明**并抛出给方法的调用者处理，最终交给jvm处理（中断处理）
- throw用在方法内，用来抛出一个异常对象

比如，在定义方法时，方法需要接受参数。那么，**当调用方法使用接受到的参数时，首先需要先对参数数据进行合法的判断，数据若不合法，就应该告诉调用者，传递合法的数据进来。这时需要使用抛出异常的方式来告诉调用者。**

在java中，提供了一个throw关键字，它用来抛出一个指定的异常对象。那么，抛出一个异常具体如何操作呢？

1. 创建一个异常对象。封装一些提示信息(信息可以自己编写)。
2. 需要将这个异常对象告知给调用者。怎么告知呢？怎么将这个异常对象传递到调用者处呢？通过关键字throw就可以完成。throw 异常对象；

**throw用在方法内，用来抛出一个异常对象**，将这个异常对象传递到调用者处，并结束当前方法的执行。

```
使用格式：

throw new 异常类名(参数)
```

```

throw new NullPointerException("要访问的arr数组不存在");

throw new ArrayIndexOutOfBoundsException("该索引在数组中不存在，已超出范围");
```

#### throw的使用

- throw关键字必须写在方法的内部
- throw关键字后边new的对象必须是Exception或者Exception的子类对象
- throw关键字后创建的是RuntimeException或者RuntimeException的子类时，默认件给jvm处理
- throw关键字后创建的编译时异常，我们必须处理这个异常使用try catch或者往上面甩否则（写代码时报错）

编写工具类，提供获取数组指定索引处的元素值

```java
class ArrayTools{

  //通过给定的数组，返回给定的索引对应的元素值。

  public static int getElement(int[] arr,int index) {

   /*

    若程序出了异常，JVM它会打包异常对象并抛出。但是它所提供的信息不够给力。想要更清晰，需要自己抛出异常信息。

下面判断条件如果满足，当执行完throw抛出异常对象后，方法已经无法继续运算。这时就会结束当前方法的执行，并将异常告知给调用者。这时就需要通过异常来解决。

   */

    if(arr==null){

      throw new NullPointerException("arr指向的数组不存在");

    }

    if(index<0 || index>=arr.length){

      throw new ArrayIndexOutOfBoundsException("错误的角标，"+index+"索引在数组中不存在");

   }

    int element = arr[index];

    return element;

  }
}
```



```java
测试类

class ExceptionDemo3 {

  public static void main(String[] args) {

​    int[] arr = {34,12,67}; //创建数组

​    int num = ArrayTools.getElement(null,2);// 调用方法，获取数组中指定索引处元素

//int num = ArrayTools.getElement(arr,5);// 调用方法，获取数组中指定索引处元素

​    System.out.println("num="+num);//打印获取到的元素值

  }

}
```

#### 声明异常throws

声明：将问题标识出来，报告给调用者。如果方法内通过throw抛出了编译时异常，而没有捕获处理（稍后讲解该方式），**那么必须通过throws进行声明，让调用者去处理。**

​    声明异常格式：

```
修饰符 返回值类型 方法名(参数) throws 异常类名1,异常类名2… {  }
```

声明异常的代码演示：

```java
class Demo{

  /*

  如果定义功能时有问题发生需要报告给调用者。可以通过在方法上使用throws关键字进行声明。

  */

  public void show(int x)**throws Exception**  {

​    if(x>0){

​      throw new Exception();

​    } else {

​      System.out.println("show run");

​     }

  }

}
```



throws用于进行异常类的声明，若该方法可能有多种异常情况产生，那么在throws后面可以写多个异常类，用逗号隔开。也可只用这些异常的父类

多个异常的情况，例如:

```java
public static int getElement(int[] arr,int index) throws NullPointerException, ArrayIndexOutOfBoundsException {

  if(arr==null){

    throw new NullPointerException("arr指向的数组不存在");

  }

  if(index<0 || index>=arr.length){

    throw new ArrayIndexOutOfBoundsException("错误的角标，"+index+"索引在数组中不存在");

  }

  int element = arr[index];

  return element;

}
```



### 异常的处理try catch

运行时异常虚拟机可以自动处理，可以捕获也可以不捕获，编译不会报错；其他异常 不抛就捕，否则编译报错

####  try catch finally

捕获：Java中对异常有针对性的语句进行捕获，可以对出现的异常进行指定方式的处理

捕获异常格式：

```
try {

  //需要被检测的语句。

}

catch(异常类 变量) { //参数。

  //异常的处理语句。

}

finally {

  //一定会被执行的语句。

}
```

**try：**该代码块中编写可能产生异常的代码。

**catch：**用来进行某种异常的捕获，实现对捕获到的异常进行处理。

**finally**：有一些特定的代码无论异常是否发生，都需要执行。另外，**因为异常会引发程序跳转**，**导致有些语句执行不到**。而finally就是解决这个问题的，在finally代码块中存放的代码都是一定会被执行的。

演示如下：

```java
class ExceptionDemo{

  public static void main(String[] args){ //throws ArrayIndexOutOfBoundsException

    try {

       int[] arr = new int[3];

      System.out.println( arr[5] );// 会抛出ArrayIndexOutOfBoundsException

    }

    catch (ArrayIndexOutOfBoundsException e) { //括号中需要定义什么呢？try中抛出的是什么异常，在括号中就定义什么异常类型。

      System.out.println("异常发生了");

    } finally {

        arr = null; //把数组指向null，通过垃圾回收器，进行内存垃圾的清除**

}

    System.out.println("程序运行结果");

  }

}
```



#### 组合方式

**try catch finally组合：**检测异常，并传递给catch处理，并在finally中进行资源释放。

 **try catch组合** : 对代码进行异常检测，并对检测的异常传递给catch处理。对异常进行捕获处理。

```java
void show(){ //不用throws 

  try{

    throw new Exception();//产生异常，直接捕获处理

  }catch(Exception e){

//处理方式  

  }

}
```

**一个try 多个catch组合** : 对代码进行异常检测，并对检测的异常传递给catch处理。对每种异常信息进行不同的捕获处理。

```java
void show(){ //不用throws 

  try{

    throw new Exception();//产生异常，直接捕获处理

  }catch(XxxException e){

//处理方式  

  }catch(YyyException e){

//处理方式  

  }catch(ZzzException e){

//处理方式  

  }

}
```

注意:这种异常处理方式，要求**多个catch中的异常不能相同**，

并且若catch中的多个异常之间有子父类异常的关系，那么**子类异常要求在上面的catch处理，父类异常在下面的catch处理**。

**否则所有的异常，都被父类捕获了，下面的子类异常无效**



 **try finally 组合**: 对代码进行异常检测，检测到异常后因为没有catch，所以一样会被默认jvm抛出。异常是没有捕获处理的。但是功能所开启资源需要进行关闭，所有finally。只为关闭资源。

void show(){//需要throws 

  try{

​    throw new Exception();

  }finally {

​    //释放资源

  }

}

 



###  异常在方法细节

#### 重写

1. 子类覆盖父类方法时，如果父类的方法声明异常，**子类只能声明父类异常或者该异常的子类，或者不声明。**

例如：

```java
class Fu {

  public void method () throws RuntimeException {

}

}

class Zi extends Fu {

  public void method() throws RuntimeException { } //抛出父类一样的异常

  //public void method() throws NullPointerException{ } //抛出父类子异常

}
```

2. 当父类方法声明多个异常时，子类覆盖时只能声明多个异常的子集。

例如：

```java
class Fu {

  public void method () throws NullPointerException, ClassCastException{

}

}

class Zi extends Fu {

  public void method()throws NullPointerException, ClassCastException { }     public void method() throws NullPointerException{ } //抛出父类异常中的一部分

  public void method() throws ClassCastException { } //抛出父类异常中的一部分

}
```

3. 当被覆盖的方法没有异常声明时，**子类覆盖时无法声明异常的。**

例如：

```java
class Fu {

  public void method (){

}

}

class Zi extends Fu {

  public void method() throws Exception { }//错误的方式

}
```

举例：父类中会存在下列这种情况，接口也有这种情况

问题：接口中没有**声明**异常，而实现的子类覆盖方法时发生了异常，怎么办？

答：无法进行throws声明，**只能catch的捕获**。万一问题处理不了呢？catch中继续throw抛出，但是只能将异常转换成RuntimeException子类抛出。

```java
interface Inter {

  public abstract void method();

}

class Zi implements Inter {

  public void method(){ //无法声明 throws Exception

    int[] arr = null;

    if (arr == null) {

      //只能捕获处理

      try{

throw new Exception(“哥们，你定义的数组arr是空的!”);

} catch(Exception e){

  System.out.println(“父方法中没有异常抛出，子类中不能抛出Exception异常”);

   //我们把异常对象e，采用RuntimeException异常方式抛出

  throw new RuntimeException(e);

}

}

}

}
```

#### return

finally 中有return 永远返回finally中的结果；不看try中的return

### 异常中常用方法

在Throwable类中为我们提供了很多操作异常对象的方法，常用的如下：

![img](笔记图库/clip_image001.png)

**getMessage方法**：返回该异常的详细信息字符串，即异常提示信息

**toString方法**：返回该异常的名称与详细信息字符串

**printStackTrace**：在控制台输出该异常的名称与详细信息字符串、异常出现的代码位置

异常的常用方法代码演示：

```java
try {

  Person p= null;

  if (p==null) {

   throw new NullPointerException(“出现空指针异常了，请检查对象是否为null”);

}

} catch (NullPointerException e) {

  String message = e.getMesage();

  System.out.println(message );

  String result = e.toString();

  System.out.println(result); 

  e.printStackTrace();

}


```

### 自定义异常

**自定义异常的情况**，请仔细读：**我们可以用违例（Exception）来抛出一些 并非 错误的消息**，可以，并非错误的消息。比如我自定义一个异常，若一个变量大于10就抛出一个异常，这样就对应了B选项说的情况，我用抛出异常说明这个变量大于10，而不是用一个函数体（函数体内判断是否大于10，然后返回true或false）判断，**因为函数调用是入栈出栈，栈是在寄存器之下的速度最快，且占的空间少，而自定义异常是存在堆中，肯定异常的内存开销大！**



在上述代码中，发现这些异常都是JDK内部定义好的，并且这些异常不好找。书写时也很不方便，那么能不能自己定义异常呢？

之前的几个异常都是java通过类进行的描述。并将问题封装成对象，异常就是将问题封装成了对象。这些异常不好认，书写也很不方便，能不能定义一个符合我的程序要求的异常名称。既然JDK中是使用类在描述异常信息，那么我们也可以模拟Java的这种机制，我们自己定义异常的信息，异常的名字，让异常更符合自己程序的阅读。准确对自己所需要的异常进行类的描述。

#### 定义

通过阅读异常源代码：发现java中所有的异常类，都是继承Throwable，或者继承Throwable的子类。这样该异常才可以被throw抛出。

说明这个异常体系具备一个特有的特性：**可抛性：即可以被throw关键字操作。**

并且查阅异常子类源码，发现每个异常中都调用了父类的构造方法，把异常描述信息传递给了父类，让父类帮我们进行异常信息的封装。

例如NullPointerException异常类源代码：

```java
public class NullPointerException extends RuntimeException {

  public NullPointerException() {

​    super();//调用父类构造方法

  }

  public NullPointerException(String s) {

​    super(s);//调用父类具有异常信息的构造方法

  }

}
```

现在，我们来定义个自己的异常，即自定义异常。

**格式：**

```java
Class 异常名 extends Exception{ **//****或继承RuntimeException**

  public 异常名(){

}

  public 异常名(String s){ 

super(s); 

}

}
```

#### 继承Exception演示

```
class MyException extends Exception{

  /*

  为什么要定义构造函数，因为看到Java中的异常描述类中有提供对异常对象的初始化方法。

  */

  public MyException(){

​    **super();**

  }

  public MyException(String message) {

​    **super(message);//** **如果自定义异常需要异常信息，可以通过调用父类的带有字符串参数的构造函数即可。**

  }

}
```



#### 继承RuntimeException

```
class MyException extends RuntimeException{

  /*

  为什么要定义构造函数，因为看到Java中的异常描述类中有提供对异常对象的初始化方法。

  */

  MyException(){

​    super();

  }

  MyException(String message) {

​    super(message);// **如果自定义异常需要异常信息，可以通过调用父类的带有字符串参数的构造函数即可。**

  }

}
```



#### 总结

构造函数到底抛出这个NoAgeException是  继承Exception呢？还是继承RuntimeException呢？

**继承Exception，必须要throws声明，**一声明就告知调用者进行捕获，一旦问题处理了调用者的程序会继续执行。

**继承RuntimeExcpetion,不需要throws声明的**，这时调用是不需要编写捕获代码的，因为调用根本就不知道有问题。一旦发生NoAgeException，调用者程序会停掉，并有jvm将信息显示到屏幕，让调用者看到问题，修正代码。

###  总结

异常：就是程序中出现的不正常的现象(错误与异常)

异常的继承体系:

  Throwable: 它是所有错误与异常的超类（祖宗类）

​    |- Error 错误，修改java源代码

​    |- Exception 编译期异常, javac.exe进行编译的时候报错

​      |- RuntimeException 运行期异常, java出现运行过程中出现的问题

 异常处理的两种方式：

 1，出现问题，自己解决 try…catch…finally

​    try{

​      可能出现异常的代码

​    } catch(异常类名 对象名){

​      异常处理代码 

​    } finally {

​      异常操作中一定要执行的代码

​    }

2，出现问题，别人解决 throws

​    格式：

​    修饰符 返回值类型 方法名(参数) throws 异常类名1,异常类名2,...{}

​    public void method() throws Exception{}

 异常分类

异常的根类是Throwable，其下有两个子类：Error与Exception，平常所说的异常指Exception。

l 严重错误Error，无法通过处理的错误

l 编译时异常Exception，编译时无法编译通过。如日期格式化异常

l 运行时异常RuntimeException，是Exception的子类，运行时可能会报错，可以不处理。如空指针异常



l 异常基本操作

l 创建异常对象

l 抛出异常

l 处理异常：

l 捕获处理，将异常获取，使用try/catch做分支处理

​          try{

​              需要检测的异常；

} catch(异常对象) {

​      通常我们只使用一个方法：**printStackTrace****打印异常信息**

}

声明抛出处理，出现异常后不处理，声明抛出给调用者处理。

​          方法声明上加throws 异常类名

 注意：异常的处理，指处理异常的一种可能性，即有了异常处理的代码，不一定会产生异常。如果没有产生异常，则代码正常执行，如果产生了异常，则中断当前执行代码，执行异常处理代码。

- [Java提高篇——Java 异常处理](https://www.cnblogs.com/Qian123/p/5715402.html)

# 泛型

### 泛型的定义

一种未知的数据类型，当我们不知道用什么数据类型时，可以使用泛型

**创建集合时不使用泛型**

好处：元素默认类型为Object。可以存储任意类型

坏处：不安全，容易异常

**创建集合时使用泛型**

好处：安全；避免了类型转换的麻烦，存储的是什么类型就是什么类型

坏处：不可以存储任意类型

### 泛型的使用

- [Java 泛型详解](https://www.cnblogs.com/Blue-Keroro/p/8875898.html)
- [10 道 Java 泛型面试题](https://cloud.tencent.com/developer/article/1033693)

#### 泛型类

泛型类的声明和非泛型类的声明类似，除了在类名后面添加了类型参数声明部分。

和泛型方法一样**，泛型类的类型参数声明部分也包含一个或多个类型参数，参数间用逗号隔开**。一个泛型参数，也被称为一个类型变量，是用于指定一个泛型类型名称的标识符。因为他们接受一个或多个参数，这些类被称为参数化的类或参数化的类型。

```java
public class Box<T> {
   
  private T t;
 
  public void add(T t) {
    this.t = t;
  }
 
  public T get() {
    return t;
  }
 
  public static void main(String[] args) {
    Box<Integer> integerBox = new Box<Integer>();
    Box<String> stringBox = new Box<String>();
 
    integerBox.add(new Integer(10));
    stringBox.add(new String("菜鸟教程"));
 
    System.out.printf("整型值为 :%d\n\n", integerBox.get());
    System.out.printf("字符串为 :%s\n", stringBox.get());
  }
}
```

```
整型值为 :10

字符串为 :菜鸟教程
```

#### 泛型方法

你可以写一个泛型方法，该方法在调用时可以接收不同类型的参数。根据传递给泛型方法的参数类型，编译器适当地处理每一个方法调用。

下面是定义泛型方法的规则：

- 所有泛型方法声明都有一个类型参数声明部分（由尖括号分隔），该类型参数声明部分在方法返回类型之前（在下面例子中的<E>）。
- 每一个类型参数声明部分包含一个或多个类型参数，参数间用逗号隔开。一个泛型参数，也被称为一个类型变量，是用于指定一个泛型类型名称的标识符。
- **类型参数能被用来声明返回值类型，并且能作为泛型方法得到的实际参数类型的占位符。**
- 泛型方法体的声明和其他方法一样。注意类型参数只能代表引用型类型，不能是原始类型（像int,double,char的等）。

实例

下面的例子演示了如何使用泛型方法打印不同类型的数组元素：

```java
public class GenericMethodTest
{
   // 泛型方法 printArray                         
   public static < E > void printArray( E[] inputArray )
   {
      // 输出数组元素            
         for ( E element : inputArray ){        
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }
 
    public static void main( String args[] )
    {
        // 创建不同类型数组： Integer, Double 和 Character
        Integer[] intArray = { 1, 2, 3, 4, 5 };
        Double[] doubleArray = { 1.1, 2.2, 3.3, 4.4 };
        Character[] charArray = { 'H', 'E', 'L', 'L', 'O' };
 
        System.out.println( "整型数组元素为:" );
        printArray( intArray  ); // 传递一个整型数组
 
        System.out.println( "\n双精度型数组元素为:" );
        printArray( doubleArray ); // 传递一个双精度型数组
 
        System.out.println( "\n字符型数组元素为:" );
        printArray( charArray ); // 传递一个字符型数组
    } 
}
```

```
整型数组元素为:
1 2 3 4 5 

双精度型数组元素为:
1.1 2.2 3.3 4.4 

字符型数组元素为:
H E L L O 
```

#### **有界的类型参数:**

**可能有时候，你会想限制那些被允许传递到一个类型参数的类型种类范围**。例如，一个操作数字的方法可能只希望接受Number或者Number子类的实例。这就是有界类型参数的目的。

**要声明一个有界的类型参数，首先列出类型参数的名称，后跟extends关键字，最后紧跟它的上界。**

实例

下面的例子演示了"extends"如何使用在一般意义上的意思"extends"（类）或者"implements"（接口）。该例子中的泛型方法返回三个可比较对象的最大值。

```java
public class MaximumTest
{
   // 比较三个值并返回最大值
   public static <T extends Comparable<T>> T maximum(T x, T y, T z)
   {                     
      T max = x; // 假设x是初始最大值
      if ( y.compareTo( max ) > 0 ){
         max = y; //y 更大
      }
      if ( z.compareTo( max ) > 0 ){
         max = z; // 现在 z 更大           
      }
      return max; // 返回最大对象
   }
   public static void main( String args[] )
   {
      System.out.printf( "%d, %d 和 %d 中最大的数为 %d\n\n",
                   3, 4, 5, maximum( 3, 4, 5 ) );
 
      System.out.printf( "%.1f, %.1f 和 %.1f 中最大的数为 %.1f\n\n",
                   6.6, 8.8, 7.7, maximum( 6.6, 8.8, 7.7 ) );
 
      System.out.printf( "%s, %s 和 %s 中最大的数为 %s\n","pear",
         "apple", "orange", maximum( "pear", "apple", "orange" ) );
   }
}
```

```
3, 4 和 5 中最大的数为 5

6.6, 8.8 和 7.7 中最大的数为 8.8

pear, apple 和 orange 中最大的数为 pear
```

#### T extends Comparable<? super T>

不禁纳闷为什么要这么写呢？有什么好处吗，extends和super在这里的作用着实让人有点不清楚。

　　接下来，我将结合代码跟大家分享一下我关于这里泛型应用的看法。

##### 　　**1.**  **<T extends Comparable<? super T>>代表什么意思**

- 大家可以明白的是这里应用到了Java的泛型，那么首先向大家说明一下这里extends的作用

　　**extends后面跟的类型，如<任意字符 extends 类/接口>表示泛型的上限**。示例代码如下：

```
import java.util.*;
class Demo<T extends List>{}
public class Test
{
  public static void main(String[] args) {
  Demo<ArrayList> p = null; // 编译正确
//这里因为ArrayList是List的子类所以通过
//如果改为Demo<Collection> p = null;就会报错这样就限制了上限
  }
}
```

- 在理解了extends所表示的泛型的上限后，接下来介绍一下super的作用，它与extends相反，表示的是泛型的下限。
- 所以结合上述两点，我们来分析一下这句话整体代表什么意思。首先，extends对泛型上限进行了限制即T必须是Comparable<? super T>的子类，然后<? super T>表示Comparable<>中的类型下限为T！



##### 　　2.  <T extends Comparable<T>> 和 <T extends Comparable<? super T>>有什么不同

　　接下来我们通过对比，使得大家对为何要这样编写代码有更加深刻的印象。

****

　　**它代表的意思是：类型T必须实现`Comparable`接口，并且这个接口的类型是T。这样，T的实例之间才能相互比较大小。**这边我们以Java中GregorianCalendar这个类为例。

　　代码如下所示：

```
import java.util.GregorianCalendar;
class Demo<T extends Comparable<T>>{}
//注意这里是没有? super的
public class Test
{
  public static void main(String[] args) {
   Demo<GregorianCalendar> p = null;
    }
}
```

　　这里编译报错，因为这里的<T extends Comparable<T>>相当于<GregorianCalendar extends Comparable<GregorianCalendar>>,**但是GregorianCalendar中并没有实现Comparable<GregorianCalendar>**，而是仅仅持有从Calendar继承过来的Comparable<Calendar>，这样就会因为不在限制范围内而报错。

**<T extends Comparable<? super T>>**　

　　它代表的意思是：类型T必须实现`Comparable`接口，**并且这个接口的类型是T或者是T的任一父类**。这样声明后，T的实例之间和T的父类的实例之间可以相互比较大小。同样还是以GregorianCalendar为例。代码如下所示：

import java.util.GregorianCalendar;

class Demo<T extends Comparable<? super T>>{}

public class Test1
{
  public static void main(String[] args) {
   Demo<GregorianCalendar> p = null; // 编译正确
  }
}　

　　此时编译通过，这里可以理解为<GregorianCalendar extends Comparable<Calendar>>！因为Calendar为GregorianCalendar 的父类并且GregorianCalendar 实现了Comparable<Calendar>,具体可以在API中进行查看！



##### 　**3.　实例代码演示**

　　代码如下所示：

```
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Test
{
  //第一种声明：简单，灵活性低
  public static <T extends Comparable<T>> void mySort1(List<T> list)
  {
    Collections.sort(list);
  }

  //第二种声明：复杂，灵活性高
  public static <T extends Comparable<? super T>> void mySort2(List<T> l)
  {
    Collections.sort(list);
  }

  public static void main(String[] args)
  {
    //主函数中将分别创建Animal和Dog两个序列，然后调用排序方法对其进行测试

　　　　　//main函数中具体的两个版本代码将在下面具体展示


  }
}
```

```
class Animal implements Comparable<Animal>
{
  protected int age;

  public Animal(int age)

  {
    this.age = age;
  }

  //使用年龄与另一实例比较大小
  @Override
  public int compareTo(Animal other)
  {
    return this.age - other.age;
  }
}

class Dog extends Animal
{
  public Dog(int age)
  {
    super(age);
  }
}
```

上面的代码包括三个类：

1. `Animal`实现了`Comparable<Animal>`接口，通过年龄来比较实例的大小
2. Dog从Animal继承，为其子类。
3. `Test`类中提供了两个排序方法和测试用的`main()`方法：
   - `mySort1()`使用`<T extends Comparable<T>>`类型参数
   - `mySort2()`使用`<T extends Comparable<? super T>>`类型参数
   - `main()`测试方法。在这里将分别创建Animal和Dog两个序列，然后调用排序方法对其进行测试。

　　**3.1　对mySort1()进行测试，main方法代码如下所示：**

// 创建一个 Animal List
List<Animal> animals = new ArrayList<Animal>();
animals.add(new Animal(25));
animals.add(new Dog(35));

// 创建一个 Dog List
List<Dog> dogs = new ArrayList<Dog>();
dogs.add(new Dog(5));
dogs.add(new Dog(18));

// 测试 mySort1() 方法
mySort1(animals);
mySort1(dogs);

　　结果编译出错，报错信息为：

```
The method mySort1(List<T>) in the type TypeParameterTest is not applicable for the arguments (List<Dog>)
```

　　mySort1() 方法的类型参数是<T extends Comparable<T>>，它要求的类型参数是类型为T的Comparable。

　　如果传入的是List<Animal>程序将正常执行，因为Animal实现了接口Comparable<Animal>。

　　但是，如果传入的参数是List<Dog>程序将报错，因为Dog类中没有实现接口Comparable<Dog>，它只从Animal继承了一个Comparable<Animal>接口。

　　注意：animals list中实际上是包含一个Dog实例的。如果碰上类似的情况（子类list不能传入到一个方法中），可以考虑把子类实例放到一个父类 list 中，避免编译错误。

　　**3.2　对mySort12()进行测试，main方法代码如下所示：**

public static void main(String[] args)
{
  // 创建一个 Animal List List<Animal> animals = new ArrayList<Animal>();
  animals.add(new Animal(25));
  animals.add(new Dog(35));

  // 创建一个 Dog List
  List<Dog> dogs = new ArrayList<Dog>();
  dogs.add(new Dog(5));
  dogs.add(new Dog(18));

  // 测试 mySort2() 方法
  mySort2(animals);
  mySort2(dogs);
}

　　这时候我们发现该程序可以正常运行。它不但能够接受Animal implements Comparable<Animal>这样的参数，也可以接收：Dog implements Comparable<Animal>这样的参数。

　　**3.3　是否可以通过将Dog实现Comparable<Dog>来解决问题？**

　　由分析可得程序出现问题是因为Dog类没有实现接口Comparable<Dog>，那么我们能否将该类实现接口Comparable<Dog>来解决问题呢？

　　代码如下所示：

 

```
class Dog extends Animal implements Comparable<Dog>
{
    public Dog(int age)
    {
        super(age);
    }
}
```

　　结果程序编译报错，错误信息如下所示：

```
The interface Comparable cannot be implemented more than once with different arguments: Comparable<Animal> and Comparable<Dog>
```

　　意义是Dog类已经从Animal中继承了Comparable该接口，无法再实现一个Comparable。

　　若子类需要使用自己的比较方法，则需要重写父类的public int CompareTo(Animal other)方法。

　　**4. 总结**　

　　对Animal/Dog这两个有父子关系的类来说：`<T extends Comparable<? super T>>`可以接受List<Animal>，也可以接收 List<Dog> 。而`<T extends Comparable<T>>`只可以接收 List<Animal>所以，<T extends Comparable<? super T>>这样的类型参数对所传入的参数限制更少，提高了 API 的灵活性。总的来说，在保证类型安全的前提下，要使用限制最少的类型参数。

# 数据结构

java工具包提供了强大的数据结构。在Java中的数据结构主要包括以下几种接口和类：

- 枚举（Enumeration）
- 位集合（BitSet）
- 向量（Vector）
- 栈（Stack）
- 字典（Dictionary）
- 哈希表（Hashtable）
- 属性（Properties）

**以上这些类是传统遗留的，在Java2中引入了一种新的框架-集合框架(Collection)，我们后面再讨论**。

------

### 枚举（Enumeration）

枚举（Enumeration）接口虽然它本身不属于数据结构,但它在其他数据结构的范畴里应用很广。 枚举（The Enumeration）接口定义了一种从数据结构中取回连续元素的方式。

例如，枚举定义了一个叫nextElement 的方法，该方法用来得到一个包含多元素的数据结构的下一个元素。

关于枚举接口的更多信息，[请参见枚举（Enumeration）](https://www.runoob.com/java/java-enumeration-interface.html)。

------

### 位集合（BitSet）

位集合类实现了一组可以单独设置和清除的位或标志。

该类在处理一组布尔值的时候非常有用，你只需要给每个值赋值一"位"，然后对位进行适当的设置或清除，就可以对布尔值进行操作了。

关于该类的更多信息，[请参见位集合（BitSet）](https://www.runoob.com/java/java-bitset-class.html)。

------

### 向量（Vector）

Vector 类实现了一个动态数组。**和 ArrayList 很相似**，但是两者是不同的：

- Vector 是**同步**访问的。
- Vector 包含了许多传统的方法，**这些方法不属于集合框架。**

Vector **主要用在事先不知道数组的大小，或者只是需要一个可以改变大小的数组的情况。**

#### 构造方法。

第一种构造方法创建一个默认的向量，默认大小为 10：

```
Vector()
```

第二种构造方法创建指定大小的向量。

```
Vector(int size)
```

第三种构造方法创建指定大小的向量，并且增量用 incr 指定。增量表示向量每次增加的元素数目。

```
Vector(int size,int incr)
```

第四种构造方法创建一个包含集合 c 元素的向量：

```
Vector(Collection c)
```

除了从父类继承的方法外 

#### 方法

| 序号 | 方法描述                                                     |
| :--- | :----------------------------------------------------------- |
| 1    | void add(int index, Object element)   在此向量的**指定位置**插入指定的元素。 |
| 2    | boolean add(Object o)   将指定元素添加到此向量的**末尾。**   |
| 3    | boolean addAll(Collection c)  将指定 Collection 中的所有元素添加到此向量的末尾，按照指定 collection 的迭代器所返回的顺序添加这些元素。 |
| 4    | boolean addAll(int index, Collection c)  在指定位置将指定 Collection 中的所有元素插入到此向量中。 |
| 5    | void addElement(Object obj)   将指定的组件添加到此向量的末尾，将其大小增加 1。 |
| 6    | int capacity()  返回此向量的当前容量。                       |
| 7    | void clear()  从此向量中移除所有元素。                       |
| 8    | Object clone()  返回向量的一个副本。                         |
| 9    | boolean contains(Object elem)  如果此向量包含指定的元素，则返回 true。 |
| 10   | boolean containsAll(Collection c)  如果此向量包含指定 Collection 中的所有元素，则返回 true。 |
| 11   | void copyInto(Object[] anArray)   将此向量的组件复制到指定的数组中。 |
| 12   | Object elementAt(int index)  返回指定索引处的组件。          |
| 13   | Enumeration elements()  返回此向量的组件的枚举。             |
| 14   | void ensureCapacity(int minCapacity)  增加此向量的容量（如有必要），以确保其至少能够保存最小容量参数指定的组件数。 |
| 15   | boolean equals(Object o)  比较指定对象与此向量的相等性。     |
| 16   | Object firstElement()  返回此向量的第一个组件（位于索引 0) 处的项）。 |
| 17   | Object get(int index)  返回向量中指定位置的元素。            |
| 18   | int hashCode()  返回此向量的哈希码值。                       |
| 19   | int indexOf(Object elem)   返回此向量中第一次出现的指定元素的索引，如果此向量不包含该元素，则返回 -1。 |
| 20   | int indexOf(Object elem, int index)   返回此向量中第一次出现的指定元素的索引，从 index 处正向搜索，如果未找到该元素，则返回 -1。 |
| 21   | void insertElementAt(Object obj, int index)  将指定对象作为此向量中的组件插入到指定的 index 处。 |
| 22   | boolean isEmpty()  测试此向量是否不包含组件。                |
| 23   | Object lastElement()  返回此向量的最后一个组件。             |
| 24   | int lastIndexOf(Object elem)   返回此向量中最后一次出现的指定元素的索引；如果此向量不包含该元素，则返回 -1。 |
| 25   | int lastIndexOf(Object elem, int index)  返回此向量中最后一次出现的指定元素的索引，从 index 处逆向搜索，如果未找到该元素，则返回 -1。 |
| 26   | Object remove(int index)   移除此向量中指定位置的元素。      |
| 27   | boolean remove(Object o)  移除此向量中指定元素的第一个匹配项，如果向量不包含该元素，则元素保持不变。 |
| 28   | boolean removeAll(Collection c)  从此向量中移除包含在指定 Collection 中的所有元素。 |
| 29   | void removeAllElements()  从此向量中移除全部组件，并将其大小设置为零。 |
| 30   | boolean removeElement(Object obj)  从此向量中移除变量的第一个（索引最小的）匹配项。 |
| 31   | void removeElementAt(int index)  删除指定索引处的组件。      |
| 32   | protected void removeRange(int fromIndex, int toIndex) 从此 List 中移除其索引位于 fromIndex（包括）与 toIndex（不包括）之间的所有元素。 |
| 33   | boolean retainAll(Collection c)  在此向量中仅保留包含在指定 Collection 中的元素。 |
| 34   | Object set(int index, Object element)  用指定的元素替换此向量中指定位置处的元素。 |
| 35   | void setElementAt(Object obj, int index)  将此向量指定 index 处的组件设置为指定的对象。 |
| 36   | void setSize(int newSize)   设置此向量的大小。               |
| 37   | int size()   返回此向量中的组件数。                          |
| 38   | List subList(int fromIndex, int toIndex)  返回此 List 的部分视图，元素范围为从 fromIndex（包括）到 toIndex（不包括）。 |
| 39   | Object[] toArray()  返回一个数组，包含此向量中以恰当顺序存放的所有元素。 |
| 40   | Object[] toArray(Object[] a)  返回一个数组，包含此向量中以恰当顺序存放的所有元素；返回数组的运行时类型为指定数组的类型。 |
| 41   | String toString()  返回此向量的字符串表示形式，其中包含每个元素的 String 表示形式。 |
| 42   | void trimToSize()   对此向量的容量进行微调，使其等于向量的当前大小。 |

------

### 栈（Stack）

栈（Stack）实现了一个后进先出（LIFO）的数据结构。

你可以把栈理解为对象的垂直分布的栈，当你添加一个新元素时，就将新元素放在其他元素的顶部。

当你从栈中取元素的时候，就从栈顶取一个元素。换句话说，最后进栈的元素最先被取出。

栈是Vector的一个子类，它实现了一个标准的后进先出的栈。

堆栈只定义了默认构造函数，用来创建一个空栈。 堆栈除了包括由Vector定义的所有方法，也定义了自己的一些方法。

```
Stack()
```

除了由Vector定义的所有方法，

#### 方法

| 序号 | 方法描述                                                     |
| :--- | :----------------------------------------------------------- |
| 1    | boolean **empty()**  测试堆栈是否为空。                      |
| 2    | Object **peek( )** 查看堆栈顶部的对象，但不从堆栈中移除它。  |
| 3    | Object pop( ) **移除堆栈顶部的对象**，并作为此函数的值返回该对象。 |
| 4    | Object push(Object element) 把项压入**堆栈顶部**。           |
| 5    | int search(Object element) 返回对象在堆栈中的位置，以 1 为基数。 |

#### add与push

**相同：**同样可以添加元素至stack

**不同点：**

**push和pop对应**；用于**数组首**的增删 （栈顶的增删）

add 用于数组尾部元素增加；尾部删除用**removeLast（）**

#### poll和pop

poll是队列数据结构实现类的方法，从队首获取元素，同时获取的这个元素将从原队列删除；
pop是栈结构的实现类的方法，表示返回栈顶的元素，同时该元素从栈中删除，当栈中没有元素时，调用该方法会发生异常
**两个函数的代码实现是基本一致的**，如果一定要说区别那么就是当头结点为空的时候，两个函数的处理方式不同：poll()选择返回null，pop()选择抛出异常

------

### 字典（Dictionary）

字典（Dictionary） 类是一个抽象类，它定义了键映射到值的数据结构。

当你想要通过特定的键而不是整数索引来访问数据的时候，这时候应该使用Dictionary。

由于Dictionary类是抽象类，所以它只提供了键映射到值的数据结构，而没有提供特定的实现。

关于该类的更多信息，[请参见字典（ Dictionary）](https://www.runoob.com/java/java-dictionary-class.html)。

------

### 哈希表（Hashtable）

Hashtable类提供了一种在用户定义键结构的基础上来组织数据的手段。

例如，在地址列表的哈希表中，你可以根据邮政编码作为键来存储和排序数据，而不是通过人名。

哈希表键的具体含义完全取决于哈希表的使用情景和它包含的数据。

关于该类的更多信息，[请参见哈希表（HashTable）](https://www.runoob.com/java/java-hashTable-class.html)。

------

### 属性（Properties）

Properties 继承于 Hashtable.Properties 类表示了一个持久的属性集.属性列表中每个键及其对应值都是一个字符串。

Properties 类被许多Java类使用。例如，在获取环境变量时它就作为System.getProperties()方法的返回值。

关于该类的更多信息，[请参见属性（Properties）](https://www.runoob.com/java/java-properties-class.html)。

# 集合

![这里写图片描述](https://img-blog.csdn.net/20180803193134355?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZlaXlhbmFmZmVjdGlvbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

早在 Java 2 中之前，Java 就提供了特设类。比如：Dictionary, Vector, Stack, 和 Properties 这些类用来存储和操作对象组。

虽然这些类都非常有用，但是它们缺少一个核心的，统一的主题。由于这个原因，使用 Vector 类的方式和使用 Properties 类的方式有着很大不同。

集合框架被设计成要满足以下几个目标。

- 该框架必须是高性能的。基本集合（动态数组，链表，树，哈希表）的实现也必须是高效的。
- 该框架允许不同类型的集合，以类似的方式工作，具有高度的互操作性。
- 对一个集合的扩展和适应必须是简单的。

## JAVA集合框架图

为此，整个集合框架就围绕一组标准接口而设计。你可以直接使用这些接口的标准实现，诸如： **LinkedList**, **HashSet**, 和 **TreeSet** 等,除此之外你也可以通过这些接口实现自己的集合。

![img](https://www.runoob.com/wp-content/uploads/2014/01/2243690-9cd9c896e0d512ed.gif)

从上面的集合框架图可以看到，Java 集合框架主要包括两种类型的容器，

- **一种是集合（Collection），存储一个元素集合**
- **另一种是图（Map），存储键/值对映射。**

**Collection 接口又有 3 种子类型，List、Set 和 Queue**，再下面是一些抽象类，最后是具体实现类，常用的有 [ArrayList](https://www.runoob.com/java/java-arraylist.html)、[LinkedList](https://www.runoob.com/java/java-linkedlist.html)、[HashSet](https://www.runoob.com/java/java-hashset.html)、LinkedHashSet、[HashMap](https://www.runoob.com/java/java-hashmap.html)、LinkedHashMap 等等。



### 集合框架具体内容

集合框架是一个用来代表和操纵集合的统一架构。所有的集合框架都包含如下内容：

- **接口：**是代表集合的抽象数据类型。例如 Collection、List、Set、Map 等。之所以定义多个接口，是为了以不同的方式操作集合对象
- **实现（类）：**是集合接口的具体实现。从本质上讲，它们是可重复使用的数据结构，例如：ArrayList、LinkedList、HashSet、HashMap。
- **算法：**是实现集合接口的对象里的方法执行的一些有用的计算，例如：搜索和排序。这些算法被称为多态，那是因为相同的方法可以在相似的接口上有着不同的实现。

除了集合，该框架也定义了几个 Map 接口和类。Map 里存储的是键/值对。尽管 Map 不是集合，但是它们完全整合在集合中。

### 集合框架体系

![img](https://www.runoob.com/wp-content/uploads/2014/01/java-coll-2020-11-16.png)

Java 集合框架提供了一套性能优良，使用方便的接口和类，java集合框架位于java.util包中， 所以当使用集合框架的时候需要进行导包。

------

### 小结

Collection 接口的接口 对象的集合（单列集合）
├——-List 接口：元素按进入先后有序保存，可重复
│—————-├ LinkedList 接口实现类， 链表， 插入删除， 没有同步， 线程不安全
│—————-├ ArrayList 接口实现类， 数组， 随机访问， 没有同步， 线程不安全
│—————-└ Vector 接口实现类 数组， 同步， 线程安全
│ ———————-└ Stack 是Vector类的实现类
└——-Set 接口： 仅接收一次，不可重复，并做内部排序
├—————-└HashSet 使用hash表（数组）存储元素
│————————└ LinkedHashSet 链表维护元素的插入次序
└ —————-TreeSet 底层实现为二叉树，元素排好序

Map 接口 键值对的集合 （双列集合）
├———Hashtable 接口实现类， 同步， 线程安全
├———HashMap 接口实现类 ，没有同步， 线程不安全-
│—————–├ LinkedHashMap 双向链表和哈希表实现
│—————–└ WeakHashMap
├ ——–TreeMap 红黑树对所有的key进行排序
└———IdentifyHashMap

## 集合接口

集合框架定义了一些接口。本节提供了每个接口的概述：

| 序号 | 接口描述                                                     |
| :--- | :----------------------------------------------------------- |
| 1    | **Collection 接口 Collection 是最基本的集合接口**，一个 Collection 代表一组 Object，即 Collection 的元素, Java不提供直接继承自Collection的类，**只提供继承于的子接口(如List和set)**。Collection 接口存储一组不唯一，无序的对象。 |
| 2    | List 接口 List接口是一个有序的 Collection，使用此接口能够精确的控制每个元素插入的位置，能够通过索引(元素在List中位置，类似于数组的下标)来访问List中的元素，第一个元素的索引为 0，而且允许有相同的元素。List 接口存储一组不唯一，有序（插入顺序）的对象。 |
| 3    | Set Set 具有与 Collection 完全一样的接口，只是行为上不同，Set 不保存重复的元素。Set 接口存储一组唯一，无序的对象。 |
| 4    | SortedSet 继承于Set保存有序的集合。                          |
| 5    | Map Map 接口存储一组键值对象，提供key（键）到value（值）的映射。 |
| 6    | Map.Entry 描述在一个Map中的一个元素（键/值对）。是一个 Map 的内部接口。 |
| 7    | SortedMap 继承于 Map，使 Key 保持在升序排列。                |
| 8    | Enumeration 这是一个传统的接口和定义的方法，通过它可以枚举（一次获得一个）对象集合中的元素。这个传统接口已被迭代器取代。 |

#### Collection接口的方法

<img src="https://img-blog.csdn.net/20180803193423722?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZlaXlhbmFmZmVjdGlvbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="这里写图片描述" style="zoom: 67%;" />

#### Set和List的区别

- \1. Set 接口实例存储的是无序的，不重复的数据。List 接口实例存储的是有序的，可以重复的元素。
- \2. Set检索效率低下，删除和插入效率高，插入和删除不会引起元素位置改变 **<实现类有HashSet,TreeSet>**。
- \3. List和数组类似，可以动态增长，根据实际存储的数据的长度自动增长List的长度。查找元素效率高，插入删除效率低，因为会引起其他元素位置改变 **<实现类有ArrayList,LinkedList,Vector>** 。

![这里写图片描述](https://img-blog.csdn.net/20180803201610610?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZlaXlhbmFmZmVjdGlvbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

------

## 集合实现类（集合类）

Java提供了一套实现了**Collection**接口的标准集合类。其中一些是具体类，这些类可以直接拿来使用，而另外一些是抽象类，提供了接口的部分实现。

标准集合类汇总于下表：

| 序号 | 类描述                                                       |
| :--- | :----------------------------------------------------------- |
| 1    | **AbstractCollection**  实现了大部分的集合接口。             |
| 2    | **AbstractList**  继承于AbstractCollection 并且实现了大部分List接口。 |
| 3    | **AbstractSequentialList**  继承于 AbstractList ，提供了对数据元素的链式访问而不是随机访问。 |
| 4    | [LinkedList](https://www.runoob.com/java/java-linkedlist.html) 该类实现了List接口，允许有null（空）元素。主要用于创建链表数据结构，该类没有同步方法，如果多个线程同时访问一个List，则必须自己实现访问同步，解决方法就是在创建List时候构造一个同步的List。例如：`List list=Collections.synchronizedList(newLinkedList(...));`LinkedList 查找效率低。 |
| 5    | [ArrayList](https://www.runoob.com/java/java-arraylist.html) 该类也是实现了List的接口，实现了可变大小的数组，随机访问和遍历元素时，提供更好的性能。该类也是非同步的,在多线程的情况下不要使用。ArrayList 增长当前长度的50%，插入删除效率低。 |
| 6    | **AbstractSet**  继承于AbstractCollection 并且实现了大部分Set接口。 |
| 7    | [HashSet](https://www.runoob.com/java/java-hashset.html) 该类实现了Set接口，不允许出现重复元素，不保证集合中元素的顺序，允许包含值为null的元素，但最多只能一个。 |
| 8    | LinkedHashSet 具有可预知迭代顺序的 `Set` 接口的哈希表和链接列表实现。 |
| 9    | TreeSet 该类实现了Set接口，可以实现排序等功能。              |
| 10   | **AbstractMap**  实现了大部分的Map接口。                     |
| 11   | [HashMap](https://www.runoob.com/java/java-hashmap.html) HashMap 是一个散列表，它存储的内容是键值对(key-value)映射。 该类实现了Map接口，根据键的HashCode值存储数据，具有很快的访问速度，最多允许一条记录的键为null，不支持线程同步。 |
| 12   | TreeMap 继承了AbstractMap，并且使用一颗树。                  |
| 13   | WeakHashMap 继承AbstractMap类，使用弱密钥的哈希表。          |
| 14   | LinkedHashMap 继承于HashMap，使用元素的自然顺序对元素进行排序. |
| 15   | IdentityHashMap 继承AbstractMap类，比较文档时使用引用相等。  |

在前面的教程中已经讨论通过java.util包中定义的类，如下所示：

| 序号 | 类描述                                                       |
| :--- | :----------------------------------------------------------- |
| 1    | Vector 该类和ArrayList非常相似，但是该类是同步的，可以用在多线程的情况，该类允许设置默认的增长长度，默认扩容方式为原来的2倍。 |
| 2    | Stack 栈是Vector的一个子类，它实现了一个标准的后进先出的栈。 |
| 3    | Dictionary Dictionary 类是一个抽象类，用来存储键/值对，作用和Map类相似。 |
| 4    | Hashtable Hashtable 是 Dictionary(字典) 类的子类，位于 java.util 包中。 |
| 5    | Properties Properties 继承于 Hashtable，表示一个持久的属性集，属性列表中每个键及其对应值都是一个字符串。 |
| 6    | BitSet 一个Bitset类创建一种特殊类型的数组来保存位值。BitSet中数组大小会随需要增加。 |



### 迭代器（方法）

通常情况下，你会希望遍历一个集合中的元素。例如，显示集合中的每个元素。

一般遍历数组都是采用for循环或者增强for，这两个方法也可以用在集合框架，

**但是还有一种方法是采用迭代器遍历集合框架，它是一个对象，实现了[Iterator](https://www.runoob.com/java/java-iterator.html) 接口或 ListIterator接口。**

迭代器，使你能够通过循环来得到或删除集合的元素。ListIterator 继承了 Iterator，**以允许双向遍历列表和修改元素。**



Java **Iterator（迭代器）不是一个集合，它是一种用于访问集合的方法**，可用于迭代 [ArrayList](https://www.runoob.com/java/java-arraylist.html) 和 [HashSet](https://www.runoob.com/java/java-hashset.html) 等集合。

Iterator 是 Java 迭代器最简单的实现，ListIterator 是 Collection API 中的接口， 它扩展了 Iterator 接口。

![img](https://www.runoob.com/wp-content/uploads/2020/07/ListIterator-Class-Diagram.jpg)

#### 方法

迭代器 it 的两个基本操作是 next 、hasNext 和 remove。

**调用 it.next() 会返回迭代器的下一个元素，并且更新迭代器的状态。**

**调用 it.hasNext() 用于检测集合中是否还有元素。**

调用 it.remove() 将迭代器返回的元素删除。

Iterator 类位于 java.util 包中，使用前需要引入它，语法格式如下：

```
import java.util.Iterator; // 引入 Iterator 类
```

#### 获取一个迭代器

集合想获取一个迭代器可以使用 **iterator()** 方法:

```java
public void test6(){

        String sql = "SELECT * from users ";
        List<Map<String,Object>> list= template.queryForList(sql);
    
   //创建迭代器对象    
        Iterator iterator = list.iterator();
    //利用hashNext和next遍历
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
    
        System.out.println("***********************************");
        for(Map<String,Object> map:list){
            System.out.println(map);
        }
    }
```



**Iterator**:迭代器，**它是Java集合的顶层接口（不包括 map 系列的集合，Map接口 是 map 系列集合的顶层接口）**

　　Object next()：返回迭代器刚越过的元素的引用，返回值是 Object，需要强制转换成自己需要的类型

　　boolean hasNext()：判断容器内是否还有可供访问的元素

　　void remove()：删除迭代器刚越过的元素

所以除了 map 系列的集合，我们都能通过迭代器来对集合中的元素进行遍历。

注意：我们可以在源码中追溯到集合的顶层接口，比如 Collection 接口，可以看到它继承的是类 Iterable

![img](https://images2015.cnblogs.com/blog/1120165/201703/1120165-20170315161812151-117827845.png)

####  Iterator 和 Iterable 的区别

 **Iterable** ：存在于 java.lang 包中。

　　　　![img](https://images2015.cnblogs.com/blog/1120165/201703/1120165-20170315162024541-482452136.png)

   我们可以看到，里面封装了 Iterator 接口。所以只要实现了只要实现了Iterable接口的类，就可以使用Iterator迭代器了。

 **Iterator** ：存在于 java.util 包中。核心的方法next(),hasnext(),remove()。

 这里我们引用一个Iterator 的实现类 ArrayList 来看一下迭代器的使用：暂时先不管 List 集合是什么，只需要看看迭代器的用法就行了

```java
 1         //产生一个 List 集合，典型实现为 ArrayList。
 2         List list = new ArrayList();
 3         //添加三个元素
 4         list.add("Tom");
 5         list.add("Bob");
 6         list.add("Marry");
 7         //构造 List 的迭代器
 8         Iterator it = list.iterator();
 9         //通过迭代器遍历元素
10         while(it.hasNext()){
11             Object obj = it.next();
12             System.out.println(obj);
13         }
```



#### 遍历 ArrayList

实例

```java
import java.util.*;
 
public class Test{
 public static void main(String[] args) {
     List<String> list=new ArrayList<String>();
     list.add("Hello");
     list.add("World");
     list.add("HAHAHAHA");
     //第一种遍历方法使用 For-Each 遍历 List
     for (String str : list) {            //也可以改写 for(int i=0;i<list.size();i++) 这种形式
        System.out.println(str);
     }
 
     //第二种遍历，把链表变为数组相关的内容进行遍历
     String[] strArray=new String[list.size()];
     list.toArray(strArray);
     for(int i=0;i<strArray.length;i++) //这里也可以改写为  for(String str:strArray) 这种形式
     {
        System.out.println(strArray[i]);
     }
     
    //第三种遍历 使用迭代器进行相关遍历
     
     Iterator<String> ite=list.iterator();
     while(ite.hasNext())//判断下一个元素之后有值
     {
         System.out.println(ite.next());
     }
 }
}

```

**解析：**

三种方法都是用来遍历ArrayList集合，第三种方法是采用迭代器的方法，该方法可以不用担心在遍历的过程中会超出集合的长度。

#### 遍历 Map

实例

```java
import java.util.*;
 
public class Test{
     public static void main(String[] args) {
      Map<String, String> map = new HashMap<String, String>();
      map.put("1", "value1");
      map.put("2", "value2");
      map.put("3", "value3");
      
      //第一种：普遍使用，二次取值
      System.out.println("通过Map.keySet遍历key和value：");
      for (String key : map.keySet()) {
       System.out.println("key= "+ key + " and value= " + map.get(key));
      }
      
      //第二种
      System.out.println("通过Map.entrySet使用iterator遍历key和value：");
      Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
      while (it.hasNext()) {
       Map.Entry<String, String> entry = it.next();
       System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
      }
      
      //第三种：推荐，尤其是容量大时
      System.out.println("通过Map.entrySet遍历key和value");
      for (Map.Entry<String, String> entry : map.entrySet()) {
       System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
      }
    
      //第四种
      System.out.println("通过Map.values()遍历所有的value，但不能遍历key");
      for (String v : map.values()) {
       System.out.println("value= " + v);
      }
     }
}

```



------

### 比较器

TreeSet和TreeMap的按照排序顺序来存储元素. 然而，这是通过比较器来精确定义按照什么样的排序顺序。

这个接口可以让我们以不同的方式来排序一个集合。



##  List

1、List 接口的三个典型实现：

- **List** list1 = new ArrayList();
  底层数据结构是数组，查询快，增删慢;线程不安全，效率高
- **List** list2 = new Vector();
  底层数据结构是数组，查询快，增删慢;线程安全，效率低,几乎已经淘汰了这个集合
-  **List** list3 = new LinkedList();
  底层数据结构是链表，查询慢，增删快;线程不安全，效率高

![这里写图片描述](https://img-blog.csdn.net/20180803201736883?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZlaXlhbmFmZmVjdGlvbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```java
//产生一个 List 集合，典型实现为 ArrayList
 2         List list = new ArrayList();
 3         //添加三个元素
 4         list.add("Tom");
 5         list.add("Bob");
 6         list.add("Marry");
 7         //构造 List 的迭代器
 8         Iterator it = list.iterator();
 9         //通过迭代器遍历元素
10         while(it.hasNext()){
11             Object obj = it.next();
12             //System.out.println(obj);
13         }
14         
15         //在指定地方添加元素
16         list.add(2, 0);
17          
18         //在指定地方替换元素
19         list.set(2, 1);
20          
21         //获得指定对象的索引
22         int i=list.indexOf(1);
23         System.out.println("索引为："+i);
24         
25         //遍历：普通for循环
26         for(int j=0;j<list.size();j++){
27              System.out.println(list.get(j));
28         }
```

### 方法

| ` boolean`     | add(E e)        向列表的尾部添加指定的元素（可选操作）。     |
| -------------- | ------------------------------------------------------------ |
| ` void`        | add(int index, E element)        在列表的指定位置插入指定元素（可选操作）。 |
| boolean        | addAll(Collection<? extends E> c)       添加指定 collection 中的所有元素到此列表的结尾，顺序是指定 collection  的迭代器返回这些元素的顺序（可选操作）。 |
| boolean        | addAll(int index,  Collection<? extends E> c)        将指定 collection 中的所有元素都插入到列表中的指定位置（可选操作）。 |
| ` void`        | clear()      从列表中移除所有元素（可选操作）。              |
| ` boolean`     | contains(Object o)    如果列表包含指定的元素，则返回 true。  |
| ` boolean`     | containsAll(Collection<?> c) 如果列表包含指定 collection 的所有元素，则返回 `true`。 |
| boolean        | equals(Object o)        比较指定的对象与列表是否相等。       |
| E              | get(int index)        返回列表中指定位置的元素。             |
| ` int`         | hashCode()        返回列表的哈希码值。                       |
| ` int`         | indexOf(Object o)       返回此列表中第一次出现的指定元素的索引；如果此列表不包含该元素，则返回 -1。 |
| ` boolean`     | isEmpty()       如果列表不包含元素，则返回 `true`。          |
| ` Iterator<E>` | iterator()        返回按适当顺序在列表的元素上进行迭代的迭代器。 |
| ` int`         | lastIndexOf(Object o)       返回此列表中最后出现的指定元素的索引；如果列表不包含此元素，则返回 -1。 |
| ` E`           | remove(int index)      移除列表中指定位置的元素（可选操作）。 |
| boolean        | remove(Object o)        从此列表中移除第一次出现的指定元素（如果存在）（可选操作）。 |
| ` boolean`     | removeAll(Collection<?> c)      从列表中移除指定 collection 中包含的其所有元素（可选操作）。 |
| ` E`           | set((int index, E element)      用指定元素替换列表中指定位置的元素（可选操作）。 |
| int            | size()     返回列表中的元素数。                              |
| void           | sort(Comparator<? super E> c)  Sorts this list using the supplied Comparator to  compare elements. |
| ` List<E>`     | subList(int fromIndex,  int toIndex)`       返回列表中指定的 `fromIndex`（包括 ）和  `toIndex`（不包括）之间的部分视图。 |
| ` Object[]`    | toArray()        返回按适当顺序包含列表中的所有元素的数组（从第一个元素到最后一个元素）。 |
| `  <T>  T[]`   | toArray(T[] a)       返回按适当顺序（从第一个元素到最后一个元素）包含列表中所有元素的数组；返回数组的运行时类型是指定数组的运行时类型。 |

#### sort()

```java
list.sort(new Comparator<Integer>() {
                @Override
                public int compare(Integer o1, Integer o2) {
                    return o1 -o2;
                }
            });
```

**Collections.sort(list,Comparator<T>）;**
**list.sort(Comparator<T>);**



#### toArray

toArray() 方法将 Arraylist 对象转换为数组。

toArray() 方法的语法为：

```
arraylist.toArray(T[] arr)
```

**注：**arraylist 是 ArrayList 类的一个对象。

**参数说明：**

- **T [] arr**（可选参数）- 用于存储数组元素的数组
- 数组长度和 ArrayList 长度一样

**注意：**这里 T 指的是数组的类型。

**返回值**

如果参数 **T[] arr** 作为参数传入到方法，则返回 T 类型的数组。

如果未传入参数，则返回 Object 类型的数组。

**实例**

```java
import java.util.ArrayList;
import java.util.Comparator;
class Main {
    public static void main(String[] args){

        // 创建一个动态数组
        ArrayList<String> sites = new ArrayList<>();
       
        sites.add("Runoob");
        sites.add("Google");
        sites.add("Wiki");
        sites.add("Taobao");
        System.out.println("网站列表: " + sites);

        // 创建一个新的 String 类型的数组
        // 数组长度和 ArrayList 长度一样
        String[] arr = new String[sites.size()];

        // 将ArrayList对象转换成数组
        sites.toArray(arr);

        // 输出所有数组的元素
        System.out.print("Array: ");
        for(String item:arr) {
            System.out.print(item+", ");
        }
    }
}
```

#### remove()      

boolean remove(int index)移除列表中指定位置的元素（可选操作）

E remove(Object o) 移除列表中出现的第一个元素（可选操作）

参数为Integer时注意：自动拆箱带来问题；建议使用（Integer）进行转换



### ArrayList()

ArrayList是一个实现了List接口的动态数组，其容量能够动态增长，其允许包括null在内的所有元素。它继承了AbstractList，实现了List、RandomAccess, Cloneable, java.io.Serializable。

 因为其基于数组实现的特性，所以随机访问 get 和 set 效率较高，但是在于添加和删除操作，由于要移动数据，因此效率会低些。

 需要注意的是，ArrayList中的操作**不是线程安全**的！如果多线程同时访问 ArrayList ，而其中有个线程对表结构做了修改，则它可能会抛出错误。所以，建议在单线程中才使用ArrayList，

而在多线程中可以选择Vector或者CopyOnWriteArrayList。

下面我们就了解一下ArrayList一些常用的方法属性，以及一些需要注意的地方。

ArrayList 继承了 AbstractList ，并实现了 List 接口。

![img](笔记图库/ArrayList-1-768x406-1.png)

ArrayList 类位于 java.util 包中，使用前需要引入它，语法格式如下：

```
import java.util.ArrayList; // 引入 ArrayList 类

ArrayList<E> objectName =new ArrayList<>();　 // 初始化
```

- **E**: 泛型数据类型，用于设置 objectName 的数据类型，**只能为引用数据类型**。
- **objectName**: 对象名。

ArrayList 是一个数组队列，提供了相关的添加、删除、修改、遍历等功能。

#### 引用类型

ArrayList 中的元素实际上是对象，在以上实例中，数组列表元素都是字符串 String 类型。

如果我们要存储其他类型，而 <E> 只能为引用数据类型，这时我们就需要使用到基本类型的包装类。

基本类型对应的包装类表如下：

| 基本类型 | 引用类型  |
| :------- | :-------- |
| boolean  | Boolean   |
| byte     | Byte      |
| short    | Short     |
| int      | Integer   |
| long     | Long      |
| float    | Float     |
| double   | Double    |
| char     | Character |

此外，**BigInteger**、**BigDecimal** 用于高精度的运算，BigInteger 支持任意精度的整数，也是引用类型，但它们没有相对应的基本类型。

```
ArrayList<Integer> li=new Arraylist<>();     // 存放整数元素
ArrayList<Character> li=new Arraylist<>();   // 存放字符元素
```



> 本文转自[ArrayList源码分析（基于JDK8）](https://blog.csdn.net/fighterandknight/article/details/61240861)，本人仅在自己的理解上做了些许的修改。

#### 属性

属性中比较重要的两个就是用于记录当前数组长度的 size，以及**元素的缓存数组 elementData**，除此之外还有一个经常用到的属性就是从AbstractList继承过来的modCount属性，代表ArrayList集合的修改次数。

```java
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, Serializable {  
    // 序列化id  
    private static final long serialVersionUID = 8683452581122892189L;  
    // 默认初始的容量  
    private static final int DEFAULT_CAPACITY = 10;  
    // 用于空实例的共享空数组实例
    private static final Object[] EMPTY_ELEMENTDATA = {};
    // 为默认大小的空实例构建的共享空数组实例。
    // 和 EMPTY_ELEMENTDATA 区别在于添加第一个元素需要膨胀多少
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    // 当前数据对象存放地方，当前对象不参与序列化  
    transient Object[] elementData;  
    // 当前数组长度  
    private int size;  
    // 数组最大长度  
    private static final int MAX_ARRAY_SIZE = 2147483639;  

    // 省略方法。。  
}
```

#### 构造方法

##### **无参构造方法**

如果不传入参数，则使用默认无参构建方法创建ArrayList对象，如下：

```java
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

*注意：此时我们创建的ArrayList对象中的elementData中的长度是1，size是0,**当进行第一次add的时候，elementData将会变成默认的长度：10.***



##### **带int类型**

传入的参数用于指定初始数组长度，若参数大于等于0，则给elementData 赋予对应长度的Object数组，

**这个构造方法只是提供了缓冲区容量的大小，和size()没关系，创建时size仍然是0**

 若参数小于0，则抛出IllegalArgumentException异常，构造方法如下：



```java
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: "+
                                           initialCapacity);
    }
}
```



##### **带Collection对象**

构造一个包含指定 Collection 的元素的列表，**这些元素是按照该 collection 的迭代器返回它们的顺序排列的。**
 其内部实现大致为：

1. 将 Collection 对象转为数组，然后将数组的地址的赋给elementData。
2. 更新size的值，同时判断size的大小，如果是size等于0，直接将空对象EMPTY_ELEMENTDATA的地址赋给elementData
3. 如果size的值大于0，且 **elementData 的 class 不等于 Object 数组的 class**，则执行Arrays.copyOf方法，把 Collection对象的内容（可以理解为深拷贝）copy到 elementData 中。

*注意：this.elementData = arg0.toArray(); 这里执行的简单赋值时浅拷贝，**所以要执行Arrays,copy 做深拷贝***



```java
public ArrayList(Collection<? extends E> c) {
    elementData = c.toArray();
    if ((size = elementData.length) != 0) {
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // replace with empty array.
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```

#### 扩容

1. ArrayList底层是数组elementData，用于存放插入的数据。
2. **初始大小是0，当有数据插入时，默认大小DEFAULT_CAPACITY = 10。**
3. 如果在创建ArrayList时指定了initialCapacity，则初始大小是ArrayList
4. 当添加元素时，如果元素个数+1> 当前数组长度 【size + 1 > elementData.length】时，进行扩容，扩容后的数组大小是： oldCapacity + (oldCapacity >> 1)
5. 将旧数组内容通过Array.copyOf全部复制到新数组，此时新旧列表的size大小相同，但elementData的长度即容量不同


**注意：扩容并不是严格的1.5倍，是扩容前的数组长度右移一位 + 扩容前的数组长度**

##### **相关的源代码**

##### add(E e)

```
public boolean add(E e) {
        //判断是否可以容纳e，若能，则直接添加在末尾；若不能，则进行扩容，然后再把e添加在末尾
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        //将e添加到数组末尾
        elementData[size++] = e;
        return true;
    }
```

每次在add()一个元素时，arraylist都需要对这个list的容量进行一个判断。通过ensureCapacityInternal()方法确保当前ArrayList维护的数组具有存储新元素的能力，经过处理之后将元素存储在数组elementData的尾部;

##### ensureCapacityInternal(int minCapacity)

```
private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        ensureExplicitCapacity(minCapacity);
    }
```

ensureCapacityInternal判断ArrayList默认的元素存储数据是否为空，

为空则**设置最小要求的存储能力**(size + 1)为必要存储的元素和默认存储元素个数的两个数据之间的最大值，然后调用ensureExplicitCapacity方法实现这种最低要求的存储能力

##### ensureExplicitCapacity方法

```
private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
```

若ArrayList已有的存储能力**满足最低存储要求**，则返回add直接添加元素；如果最低要求的存储能力>ArrayList已有的存储能力，这就表示ArrayList的存储能力不足，因此需要调用 grow();方法进行扩容;

##### grow()

```
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
       //新容量扩大到原容量的大约1.5倍，右移一位相当于原数值除以2，扩容并不是严格的1.5位
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

当ArrayList扩容的时候，

首先会设置新的存储能力为原来的1.5倍，如果扩容之后仍小于必要存储要求minCapacity，则取值为minCapacity。

若新的存储能力大于MAX_ARRAY_SIZE，则取值为Integer.MAX_VALUE

**在grow方法中，确定ArrayList扩容后的新存储能力后，调用的Arrays.copyof()方法进行对原数组的复制，再通过调用System.arraycopy()方法（native修饰）进行复制，达到扩容的目的**

##### hugeCapacity

```
private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

从hugeCapacity方法看出，ArrayList最大的存储能力：存储元素的个数为整型的范围。
例：当ArrayList中的当前容量已经为Integer.MAX_VALUE，仍向ArrayList中添加元素时，抛出异常



**为什么按大约1.5倍扩容**:

扩容因子的大小选择，需要考虑如下情况：

扩容容量不能太小，防止频繁扩容，频繁申请内存空间 + 数组频繁复制

扩容容量不能太大，需要充分利用空间，避免浪费过多空间；

为了能充分使用之前分配的内存空间，最好把增长因子设为 1<k<2.

k=1.5时，就能充分利用前面已经释放的空间。如果k >= 2，新容量刚刚好永远大于过去所有废弃的数组容量

并且充分利用移位操作（右移一位），减少浮点数或者运算时间和运算次数
————————————————
版权声明：本文为CSDN博主「架构师忠哥」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/penriver/article/details/117447113

#### 方法

Java ArrayList 常用方法列表如下：

| 方法                                                         | 描述                                          |
| :----------------------------------------------------------- | :-------------------------------------------- |
| [add()](https://www.runoob.com/java/java-arraylist-add.html) | 将元素插入到指定位置的 arraylist 中           |
| [addAll()](https://www.runoob.com/java/java-arraylist-addall.html) | 添加集合中的所有元素到 arraylist 中           |
| [clear()](https://www.runoob.com/java/java-arraylist-clear.html) | 删除 arraylist 中的所有元素                   |
| [clone()](https://www.runoob.com/java/java-arraylist-clone.html) | 复制一份 arraylist                            |
| [contains()](https://www.runoob.com/java/java-arraylist-contains.html) | 判断元素是否在 arraylist                      |
| [get()](https://www.runoob.com/java/java-arraylist-get.html) | 通过索引值获取 arraylist 中的元素             |
| [indexOf()](https://www.runoob.com/java/java-arraylist-indexof.html) | 返回 arraylist 中元素的索引值                 |
| [removeAll()](https://www.runoob.com/java/java-arraylist-removeall.html) | 删除存在于指定集合中的 arraylist 里的所有元素 |
| [remove()](https://www.runoob.com/java/java-arraylist-remove.html) | 删除 arraylist 里的单个元素                   |
| [size()](https://www.runoob.com/java/java-arraylist-size.html) | 返回 arraylist 里元素数量                     |
| [isEmpty()](https://www.runoob.com/java/java-arraylist-isempty.html) | 判断 arraylist 是否为空                       |
| [subList()](https://www.runoob.com/java/java-arraylist-sublist.html) | 截取部分 arraylist 的元素                     |
| set()                                                        | **替换 arraylist 中指定索引的元素**           |
| [sort()](https://www.runoob.com/java/java-arraylist-sort.html) | 对 arraylist 元素进行排序                     |
| [toArray()](https://www.runoob.com/java/java-arraylist-toarray.html) | 将 arraylist 转换为数组                       |
| [toString()](https://www.runoob.com/java/java-arraylist-tostring.html) | 将 arraylist 转换为字符串                     |
| [ensureCapacity](https://www.runoob.com/java/java-arraylist-surecapacity.html)() | 设置指定容量大小的 arraylist                  |
| [lastIndexOf()](https://www.runoob.com/java/java-arraylist-lastindexof.html) | 返回指定元素在 arraylist 中最后一次出现的位置 |
| [retainAll()](https://www.runoob.com/java/java-arraylist-retainall.html) | 保留 arraylist 中在指定集合中也存在的那些元素 |
| [containsAll()](https://www.runoob.com/java/java-arraylist-containsall.html) | 查看 arraylist 是否包含指定集合中的所有元素   |
| [trimToSize()](https://www.runoob.com/java/java-arraylist-trimtosize.html) | 将 arraylist 中的容量调整为数组中的元素个数   |
| [removeRange()](https://www.runoob.com/java/java-arraylist-removerange.html) | 删除 arraylist 中指定索引之间存在的元素       |
| [replaceAll()](https://www.runoob.com/java/java-arraylist-replaceall.html) | 将给定的操作内容替换掉数组中每一个元素        |
| [removeIf()](https://www.runoob.com/java/java-arraylist-removeif.html) | 删除所有满足特定条件的 arraylist 元素         |
| [forEach()](https://www.runoob.com/java/java-arraylist-foreach.html) | 遍历 arraylist 中每一个元素并执行特定操作     |



##  Set

### 概念

Set：典型实现 HashSet()是一个**无序（并非随机），不可重复的集合**

**不可重复性：**保证元素按照equals判断时，不能返回true：

Set中没有额外的方法，**只继承Collection的方法**

### 使用要求

1、向set添加数据时，**其所在的类一定要重写hashCode（）和equals（）**

2、**重写的hashCode（）和equals尽可能保持一致性（重写技巧两个函数都有共同的属性）**；对于 HashSet 集合，我们要保证如果两个对象通过 equals() 方法返回 true，这两个对象的 hashCode 值也应该相同。

###  hashSet

#### 使用

HashSet 类位于 java.util 包中，使用前需要引入它，语法格式如下：

```
import java.util.HashSet; // 引入 HashSet 类
```

以下实例我们创建一个 HashSet 对象 sites，用于保存字符串元素：

```
HashSet<String> sites = new HashSet<String>();
```

#### 特点

- HashSet 基于 HashMap 来实现的，是一个不允许有重复元素的集合。
- HashSet 允许有 null 值。
- HashSet 是无序的，即不会记录插入的顺序。
- HashSet 不是线程安全的， 如果多个线程尝试同时修改 HashSet，则最终结果是不确定的。 您必须在多线程访问时显式同步对 HashSet 的并发访问。
- HashSet 实现了 Set 接口。

#### 底层

其底层其实是一个数组+链表，存在的意义是加快查询速度**。我们知道在一般的数组中，元素在数组中的索引位置是随机的，元素的取值和元素的位置之间不存在确定的关系，因此，在数组中查找特定的值时，需要把查找值和一系列的元素进行比较，此时的查询效率依赖于查找过程中比较的次数。而 **HashSet 集合底层数组的索引和对象哈希值 有一个确定的关系：index=hash(value),那么只需要调用这个公式，就能快速的找到元素或者索引。

#### equals()和hashCode 

- 对于 HashSet: **如果两个对象通过 equals() 方法返回 true，这两个对象的 hashCode 值也应该相同。**

-  当向HashSet集合中存入一个元素时，HashSet会先调用该对象的hashCode（）方法来得到该对象的hashCode值，然后根据hashCode值决定该对象在HashSet中的存储位置

　　　如果 hashCode 值不同，直接把该元素存储到 hashCode() 指定的位置

　　　如果 hashCode 值相同，那么会继续判断该元素和集合对象的 equals() 作比较

　　　　　hashCode 相同，equals 为 true，则视为同一个对象，不保存在 hashSet()中

　　　　　hashCode 相同，equals 为 false，则**存储在之前对象同槽位的链表上**；

**注意：每一个存储到 哈希 表中的对象，都得提供 hashCode() 和 equals() 方法的实现，用来判断是否是同一个对象**

　　　**对于 HashSet 集合，我们要保证如果两个对象通过 equals() 方法返回 true，这两个对象的 hashCode 值也应该相同。**

#### 常见的 hashCode()算法

![img](https://images2015.cnblogs.com/blog/1120165/201705/1120165-20170509001300847-1382384473.png)

 

![image-20210819103146504](C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210819103146504.png)

#### 方法

- **添加元素**

HashSet 类提供类很多有用的方法，添加元素可以使用 add() 方法:

被添加了两次，它在集合中也只会出现一次，因为集合中的每个元素都必须是唯一的。

- **判断元素是否存在**

我们可以使用 contains() 方法来判断元素是否存在于集合当中:

- **删除元素**

我们可以使用 remove() 方法来删除集合中的元素:

- **计算大小**

如果要计算 HashSet 中的元素数量可以使用 size() 方法：

- 迭代 **HashSet**

可以使用 for-each 来迭代 HashSet 中的元素。

**实例**

*// 引入 HashSet 类*    

```java
import java.util.HashSet;

public class RunoobTest {
  public static void main(String[] args) {
  HashSet<String> sites = new HashSet<String>();
    sites.add("Google");
    sites.add("Runoob");
    sites.add("Taobao");
    sites.add("Zhihu");
    sites.add("Runoob");   *// 重复的元素不会被添加*
    for(String i : sites) {
      System.out.println(i);
    }
  }
}
```

执行以上代码，输出结果如下：

```
Google
Runoob
Zhihu
Taobao
```

### linkedHashSet

**Set linkedHashSet = new LinkedHashSet()**

1. 　　不可以重复，有序
2. ​        在原有hashSet的基础上，在每个数据添加一对双向指针
3. 　　**因为底层采用 链表 和 哈希表的算法。**链表保证元素的添加顺序，哈希表保证元素的唯一性

4. LinkedHashSet: 作为HasSet的子类；遍历其数据时，可以按照添加的顺序遍历
5. 

###  treeSet

**Set treeSet = new TreeSet();**

　　TreeSet特点:

1. 有序；
2. 不可重复，**底层使用 红黑树算法**，擅长于范围查询，
3. **可以按照添加对象的指定属性进行排序；**
4. 如果使用 TreeSet() 无参数的构造器创建一个 TreeSet 对象, 则要求**放入其中的元素的类必须实现 Comparable 接口**所以, 在其中不能放入 null 元素

5.    **必须放入同样类的对象**.(默认会进行排序) 否则可能会发生类型转换异常.我们可以使用泛型来进行限制


```java
Set treeSet = new TreeSet();   
treeSet.add(``1``); //添加一个 Integer 类型的数据``    
treeSet.add("a");  ``//添加一个 String 类型的数据``    
System.out.println(treeSet); ``//会报类型转换异常的错误
```

　　![img](https://images2015.cnblogs.com/blog/1120165/201705/1120165-20170509004119019-1767190549.png)

  *　自动排序：添加自定义对象的时候，必须要实现 Comparable 接口，并要覆盖 compareTo(Object obj) 方法来自定义比较规则

　　　　如果 this > obj,返回正数 1

　　　　如果 this < obj,返回负数 -1

　　　　如果 this = obj,返回 0 ，则认为这两个对象相等

   　　　　　*  两个对象通过 Comparable 接口 compareTo(Object obj) 方法的返回值来比较大小, 并进行升序排列

![img](https://images2015.cnblogs.com/blog/1120165/201705/1120165-20170509081622488-1398934456.png)

 

 

  \*  定制排序: 创建 TreeSet 对象时, 传入 Comparator 接口的实现类. 要求: Comparator 接口的 compare 方法的返回值和 两个元素的 equals() 方法具有一致的返回值  

   \*  当需要把一个对象放入 TreeSet 中，**重写该对象对应的 equals() 方法时，应保证该方法与 compareTo(Object obj) 方法有一致的结果**

###  Set 接口的实现类比较

　　共同点：1、都不允许元素重复

　　　　　　2、都不是线程安全的类，解决办法：Set set = Collections.synchronizedSet(set 对象)

　　不同点：

　　　　HashSet:不保证元素的添加顺序，底层采用 哈希表算法，查询效率高。判断两个元素是否相等，equals() 方法返回 true,hashCode() 值相等。即要求存入 HashSet 中的元素要覆盖 equals() 方法和 hashCode()方法

　　　　LinkedHashSet:HashSet 的子类，底层采用了 哈希表算法以及 链表算法，既保证了元素的添加顺序，也保证了查询效率。但是整体性能要低于 HashSet　　　　

　　　　TreeSet:不保证元素的添加顺序，但是会对集合中的元素进行排序。底层采用 红-黑 树算法（树结构比较适合范围查询）

## Queue

队列是一种特殊的线性表，它只允许在表的前端进行删除操作，而在表的后端进行插入操作。

**LinkedList类实现了Queue接口，因此我们可以把LinkedList当成Queue来用。**

| 变量和类型 | 方法         | 描述                                                         |
| :--------- | :----------- | :----------------------------------------------------------- |
| `boolean`  | **add(E e)** | 如果可以在不违反容量限制的情况下立即执行此操作，则将指定的元素插入此队列，成功时返回 `true` ，如果当前没有空间，则抛出 `IllegalStateException` 。 |
| `E`        | `element()`  | 检索但不删除此队列的头部。                                   |
| `boolean`  | `offer(E e)` | 如果可以在不违反容量限制的情况下立即执行此操作，则将指定的元素插入此队列。 |
| `E`        | `peek()`     | **检索但不删除此队列的头部，如果此队列为空，则返回 `null` 。** |
| E          | **poll()**   | **检索并删除此队列的头部**，如果此队列为空，则**返回 `null` 。** |
| `E`        | `remove()`   | 检索并删除此队列的头部。                                     |

##  Collections

工具类collection**s**用于**操作集合类**，如List,Set：



### Comparable接口

- Collections有**两种比较规则方式**，第一种是使用自身的比较规则：
- **该类 ** 必须实现Comparable接口并重写comparTo方法。我们可以看到Java中很多类都是实现类这个接口的 如：`Integer`，`Long` 等等...
- this可以想象为1，传入对象o想象为2，返回**1-2**即按升序排序。返回**2-1**即按降序排序。

需求：
1.定义一个学生类Student，具有年龄age和姓名username两个属性，并通过Comparable接口提供比较规则；

2.定义测试类Test，在测试类Test中定义测试方法**Comparable getMax**

(Comparable c1,Comparable c2)完成测试

```java
public class Student implements Comparable<Student>{
private String username;
private int age;
public String getUsername() {
return username;
}
public void setUsername(String username) {
this.username = username;
}
public int getAge() {
return age;
}
public void setAge(int age) {
this.age = age;
}
@Override
public String toString() {
return "Student{" +
"username='" + username + '\'' +
", age=" + age +
'}';
}
    
//定义比较规则
@Override
public int compareTo(Student o) {
return this.getAge()-o.getAge();
}
}
//测试类
public class Test {
public static void main(String[] args) {
Student stu1 = new Student();
stu1.setUsername("zhangsan");
stu1.setAge(17);
Student stu2 = new Student();
stu2.setUsername("lisi");
stu2.setAge(19);
Comparable max = getMax(stu1, stu2);
System.out.println(max);
}
//测试方法，获取两个元素中的较大值
public static Comparable getMax(Comparable c1,Comparable c2){
int cmp = c1.compareTo(c2);
if (cmp>=0){
return c1;
}else{
return c2;
}
}
}
```

### new Comparator

**第二个参数为比较器，可以使用它来定义针对集合排序时的比较元素大小的规则。**

**使用这种方式时，sort方法不要求集合元素必须实现Comparable接口了，因为不会使用元素自身的比较规则**。

```java
package com.abcd;
 
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
 
public class PersonTest {
    public static void main(String[] args){
        List<Person> people = new ArrayList<>();
        people.add(new Person("AAA",20,100));
        people.add(new Person("BBB",18,109));
        people.add(new Person("CCC",30,58));
 
        System.out.println(people);　　　　　//排序规则 salary降序
        Collections.sort(people, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return o2.getSalary()- o1.getSalary();
            }
        });
        System.out.println(people);
    }
}
```



### 排序

#### sort 方法一

​	开发中，很多时候都需要对一些数据进行排序的操作。然而那些数据一般都是放在一个集合中如：`Map` ，`Set` ，`List` 等集合中。他们都提共了一个排序方法 `sort()`，要对数据排序直接使用这个方法就行，但是

1. 要保证集合中的对象是 **可比较的**。
2. **列表中的所有元素都必须实现 Comparable 接口**。
3. Collections是一个工具类，sort是静态


假设我们有一个学生类，默认需要按学生的年龄字段 age 进行排序 代码如下：

public class Student implements Comparable<Student> {
    private int id;
    private int age;
    private String name;

```java
public Student(int id, int age, String name) {
    this.id = id;
    this.age = age;
    this.name = name;
}
@Override
public int compareTo(Student o) {
    //降序
    //return o.age - this.age;
    //升序
    return this.age - o.age;        
}
@Override
public String toString() {
    return "Student{" +
            "id=" + id +
            ", age=" + age +
            ", name='" + name + '\'' +
            '}';
}
```

这里说一下重写的 public int compareTo(Student o){} 这个方法，它返回三种 int 类型的值： 负整数，零 ，正整数。

返回值	含义
负整数	当前对象的值 < 比较对象的值 ， 位置排在前
零	当前对象的值 = 比较对象的值 ， 位置不变
正整数	当前对象的值 > 比较对象的值 ， 位置排在后
测试代码：

```java
public static void main(String args[]){
        List<Student> list = new ArrayList<>();
        list.add(new Student(1,25,"关羽"));
        list.add(new Student(2,21,"张飞"));
        list.add(new Student(3,18,"刘备"));
        list.add(new Student(4,32,"袁绍"));
        list.add(new Student(5,36,"赵云"));
        list.add(new Student(6,16,"曹操"));
        System.out.println("排序前:");
        for (Student student : list) {
            System.out.println(student.toString());
        }
        //使用默认排序
        Collections.sort(list);
        System.out.println("默认排序后:");
        for (Student student : list) {
            System.out.println(student.toString());
        }
}
```

#### sort方法二

##### 比较器的使用

这个时候需求又来了，默认是用 age 排序，但是有的时候需要用 id 来排序怎么办？ 这个时候比较器 ：Comparator 就排上用场了。

**Comparator 的使用有两种方式：**

**Collections.sort(list,Comparator<T>）;**
**list.sort(Comparator<T>);**

其实主要是看 Comparator 接口的实现，重写里面的 compare 方法。代码如下：

//自定义排序1

```java
Collections.sort(list, new Comparator<Student>() {
    @Override
    public int compare(Student o1, Student o2) {
        return o1.getId() - o2.getId();
    }
});
```

compare(Student o1, Student o2) 方法的返回值跟 Comparable<> 接口中的 compareTo(Student o) 方法 返回值意思相同。

另一种写法如下：

```java
//自定义排序2
list.sort(new Comparator<Student>() {
    @Override
    public int compare(Student o1, Student o2) {
        return o1.getId() - o2.getId();
    }
});

```

**总结**

1.对于**String或Integer这些已经实现Comparable接口的类**来说，可以直接使用Collections.sort方法传入list参数来实现默认方式（正序）排序；

2.如果不想使用默认方式（正序）排序，可以**通过Collections.sort传入第二个参数类型为Comparator来自定义排序规则；**

3.对于自定义类型(如本例子中的Emp)，如果想使用Collections.sort的方式一进行排序，可以通过实现Comparable接口的compareTo方法来进行，如果不实现，则参考第2点；

4.jdk1.8的Comparator接口有很多新增方法，其中有个[reversed()](http://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#reversed--)方法比较实用，是用来切换正序和逆序的，代码如下：

###  混排

​     混排算法所做的正好与 sort 相反： 它打乱在一个 List 中可能有的任何排列的踪迹。也就是说，基于随机源的输入重排该 List，这样的排列具有相同的可能性（假设随机源是公正的）。这个算法在实现一个碰运气的游戏中是非常有用的。例如，它可被用来混排代表一副牌的 Card 对象的一个 List .另外，在生成测试案例时，它也是十分有用的。

```java
Collections.Shuffling(list)
List<Integer> list = new ArrayList<Integer>();
		int array[] = { 112, 111, 23, 456, 231 };
		for (int i = 0; i < array.length; i++) {
			list.add(new Integer(array[i]));
		}
		Collections.shuffle(list);
		for (int i = 0; i < array.length; i++) {
			System.out.print(list.get(i) + ",");
		}
```

//结果：112,111,23,456,231   112,231,111,456,23,      随机的。

### 反转

使用Reverse方法可以根据元素的自然顺序 对指定列表按降序进行排序。
Collections.reverse(list)

```
List<Double> list = new ArrayList<Double>();
		double array[] = {112, 111, 23, 456, 231 };
		for (int i = 0; i < array.length; i++) {
		list.add(new Double(array[i]));
		}
		Collections. reverse (list);
		for (int i = 0; i < array.length; i++) {
		System.out.print(list.get(i)+",");
		}
```

//结果：231.0,456.0,23.0,111.0,112.0,  结果反转反转。

### 拷贝

用两个参数，一个目标 List 和一个源 List， 将源的元素拷贝到目标，并覆盖它的内容。目标 List 至少与源一样长。如果它更长，则在目标 List 中的剩余元素不受影响。
Collections.copy（destlist，srclist）： 后面一个参数是源列表 ，前一个是目标列表

```java
	double array[] = { 112, 111, 23, 456, 231 };
		List<Double> list = new ArrayList<Double>();
		List<Double> li = new ArrayList<Double>();
		for (int i = 0; i < array.length; i++) {
			list.add(new Double(array[i]));
		}
		double arr[] = { 1131, 333 };
		for (int j = 0; j < arr.length; j++) {
			li.add(new Double(arr[j]));
		}
		Collections.copy(list, li);
		list.forEach(x->System.out.print(x)+",   ")
```

//结果：1131.0,  333.0,  23.0,  456.0,  231.0,  

### min/max

根据指定比较器产生的顺序，返回给定 collection 的最小(大)元素。collection 中的所有元素都必须是通过指定比较器可相互比较的。

```java
Collections.min(list)  Collections.max(list)
	Integer[] array = { 112, 222, 23, 456, 231 };
		List<Integer> list =new ArrayList<Integer>();
	    for (Integer integer : array) {
	    	list.add(integer);
		}
		System.out.println(Collections.min(list));
		System.out.println(Collections.max(list));
```

//结果：23  456



## Map

Map 接口中键和值一一映射. 可以通过键来获取值。

- 给定一个键和一个值，你可以将该值存储在一个 Map 对象。之后，你可以通过键来访问对应的值。
- 当访问的值不存在的时候，方法就会抛出一个 NoSuchElementException 异常。
- 当对象的类型和 Map 里元素类型不兼容的时候，就会抛出一个 ClassCastException 异常。
- 当在不允许使用 Null 对象的 Map 中使用 Null 对象，会抛出一个 NullPointerException 异常。
- 当尝试修改一个只读的 Map 时，会抛出一个 UnsupportedOperationException 异常。

### 方法

| 序号 | 方法描述                                                     |
| :--- | :----------------------------------------------------------- |
| 1    | void clear( )  从此映射中移除所有映射关系（可选操作）。      |
| 2    | boolean containsKey(Object k) 如果此映射包含指定键的映射关系，则返回 true。 |
| 3    | boolean containsValue(Object v) 如果此映射将一个或多个键映射到指定值，则返回 true。 |
| 4    | Set **entrySet( )** **返回此映射中包含的映射关系的 Set 视图。** |
| 5    | boolean equals(Object obj) 比较指定的对象与此映射是否相等。  |
| 6    | Object get(Object k) 返回指定键所映射的值；如果此映射不包含该键的映射关系，则返回 null。 |
| 7    | int hashCode( ) 返回此映射的哈希码值。                       |
| 8    | boolean isEmpty( ) 如果此映射未包含键-值映射关系，则返回 true。 |
| 9    | Set **keySet( )** 返回此映射中包含的键的 Set 视图。          |
| 10   | Object put(Object k, Object v) 将指定的值与此映射中的指定键关联（可选操作）。 |
| 11   | void putAll(Map m) 从指定映射中将所有映射关系复制到此映射中（可选操作）。 |
| 12   | Object remove(Object k) 如果存在一个键的映射关系，则将其从此映射中移除（可选操作）。 |
| 13   | int size( ) 返回此映射中的键-值映射关系数。                  |
| 14   | Collection **values( )** **返回此映射中包含的值的 Collection 视图。** |

#### values方法

```
public Collection<V> values() {
      Collection<V> vs = values;
      return (vs != null ? vs : (values = new Values()));
  }
```

values()方法只是返回了一个Collection集合，在向下转型的时候经常出现了类型转换错误。

在通常与ArrayList中，有一个构造函数连用

```java
public ArrayList(Collection<? extends E> c) {
    elementData = c.toArray();
    size = elementData.length;
    // c.toArray might (incorrectly) not return Object[] (see 6260652)
    if (elementData.getClass() != Object[].class)
        elementData = Arrays.copyOf(elementData, size, Object[].class);
}
```



**Map 接口 键值对的集合 （双列集合）
├———Hashtable 接口古老实现类， 同步， 线程安全，不可以存储null的key和value
├———HashMap 接口主要实现类 ，没有同步， 线程不安全，效率高，可以存储null的key和value
│—————–├ LinkedHashMap 双向链表和哈希表实现
│—————–└ WeakHashMap
├ ——–TreeMap 红黑树对所有的key进行排序,可以排序遍历
└———IdentifyHashMap**，

- Map 双列数据，存储key-Value



**Map的结构**

<img src="C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210819115115791.png" alt="image-20210819115115791" style="zoom:50%;" />

###  HashMap

**底层：**

- 数组+**链表**（jdk7前）
- 数组+**链表**+红黑树（jdk8）
- ThreadLocalMap中使用开放地址法来处理散列冲突，
- HashMap中使用的是**分离链表法**处理散列冲突。
- 之所以采用不同的方式主要是因为：在ThreadLocalMap中的散列值分散得十分均匀，很少会出现冲突。并且ThreadLocalMap经常需要清除无用的对象，使用纯数组更加方便。

*开放定址法：基本思想是：当关键字key的哈希地址p=H（key）出现冲突时，以p为基础，产生另一个哈希地址p1，如果p1仍然冲突，再以p为基础，产生另一个哈希地址p2，…，直到找出一个不冲突的哈希地址pi ，将相应元素存入其中。*

*再哈希法：这种方法是同时构造多个不同的哈希函数：Hi=RH1（key） i=1，2，…，k当哈希地址Hi=RH1（key）发生冲突时，再计算Hi=RH2（key）……，直到冲突不再产生。这种方法不易产生聚集，但增加了计算时间。*

*链地址法：这种方法的基本思想是将所有哈希地址为i的元素构成一个称为同义词链的单链表，并将单链表的头指针存在哈希表的第i个单元中，因而查找、插入和删除主要在同义词链中进行。链地址法适用于经常进行插入和删除的情况。*

面试题：

1. HashMap底层原理

以jdk7为例：

HashMap map = new HashMap():

实例化后，底层创建了长度为**16的一维Entry[] table**

- HashMap 是一个**散列表**，它存储的内容是键值对(key-value)映射。
- HashMap 实现了 Map 接口，根据键的 HashCode 值存储数据，具有很快的访问速度，最多允许一条记录的键为 null，**不支持线程同步。**
- HashMap 是无序的，即不会记录插入的顺序。
- HashMap 继承于AbstractMap，实现了 Map、Cloneable、java.io.Serializable 接口。

![img](https://www.runoob.com/wp-content/uploads/2020/07/WV9wXLl.png)

HashMap 的 key 与 value 类型可以相同也可以不同，可以是字符串（String）类型的 key 和 value，也可以是整型（Integer）的 key 和字符串（String）类型的 value。

![img](https://static.runoob.com/images/mix/java-map.svg)

**HashMap 中的元素实际上是对象，一些常见的基本类型可以使用它的包装类。**

基本类型对应的包装类表如下：

| 基本类型 | 引用类型  |
| :------- | :-------- |
| boolean  | Boolean   |
| byte     | Byte      |
| short    | Short     |
| int      | Integer   |
| long     | Long      |
| float    | Float     |
| double   | Double    |
| char     | Character |

HashMap 类位于 java.util 包中，使用前需要引入它，语法格式如下：

```
import java.util.HashMap; // 引入 HashMap 类
```

以下实例我们创建一个 HashMap 对象 Sites， 整型（Integer）的 key 和字符串（String）类型的 value：

```
HashMap<Integer, String> Sites = new HashMap<Integer, String>();
```

#### 添加元素

HashMap 类提供了很多有用的方法，添加键值对(key-value)可以使用 **put()** 方法:

实例

```java
// 引入 HashMap 类*    
 impor  java.util.HashMap;

public class  RunoobTest {
  public static void main(String[] args) {
    *// 创建 HashMap 对象 Sites*
    HashMap<Integer, String> Sites =  new HashMap<Integer, String>();
    *// 添加键值对*
    Sites.put(1, "Google");
    Sites.put(2, "Runoob");
    Sites.put(3, "Taobao");
    Sites.put(4, "Zhihu");
    System.out.println(Sites);
  }
}
```

执行以上代码，输出结果如下：

```
{1=Google, 2=Runoob, 3=Taobao, 4=Zhihu}
```

以下实例创建一个字符串（String）类型的 key 和字符串（String）类型的 value：

实例

```java
// 引入 HashMap 类*    
**import** java.util.HashMap;

**public** **class** RunoobTest {
  **public** **static** **void** main(String[] args) {
    *// 创建 HashMap 对象 Sites*
    HashMap<String, String> Sites = **new** HashMap<String, String>();
    *// 添加键值对*
    Sites.put("one", "Google");
    Sites.put("two", "Runoob");
    Sites.put("three", "Taobao");
    Sites.put("four", "Zhihu");
    System.out.println(Sites);
  }
}
```

执行以上代码，输出结果如下：

```
{four=Zhihu, one=Google, two=Runoob, three=Taobao}
```

#### 访问元素

我们可以使用 **get(key)** 方法来获取 key 对应的 value:

实例

```java
// 引入 HashMap 类*    
**import** java.util.HashMap;

**public** **class** RunoobTest {
  **public** **static** **void** main(String[] args) {
    *// 创建 HashMap 对象 Sites*
    HashMap<Integer, String> Sites = **new** HashMap<Integer, String>();
    *// 添加键值对*
    Sites.put(1, "Google");
    Sites.put(2, "Runoob");
    Sites.put(3, "Taobao");
    Sites.put(4, "Zhihu");
    System.out.println(Sites.get(3));
  }
}
```

执行以上代码，输出结果如下：

```
Taobao
```

#### 删除元素

我们可以使用 **remove(key)** 方法来删除 key 对应的键值对(key-value):

**实例**

*// 引入 HashMap 类*    
**import** java.util.HashMap;

**public** **class** RunoobTest {
  **public** **static** **void** main(String[] args) {
    *// 创建 HashMap 对象 Sites*
    HashMap<Integer, String> Sites = **new** HashMap<Integer, String>();
    *// 添加键值对*
    Sites.put(1, "Google");
    Sites.put(2, "Runoob");
    Sites.put(3, "Taobao");
    Sites.put(4, "Zhihu");
    Sites.remove(4);
    System.out.println(Sites);
  }
}

执行以上代码，输出结果如下：

```
{1=Google, 2=Runoob, 3=Taobao}
```

删除所有键值对(key-value)可以使用 clear 方法：

**实例**

```java
// 引入 HashMap 类*    
**import** java.util.HashMap;

**public** **class** RunoobTest {
  **public** **static** **void** main(String[] args) {
    *// 创建 HashMap 对象 Sites*
    HashMap<Integer, String> Sites = **new** HashMap<Integer, String>();
    *// 添加键值对*
    Sites.put(1, "Google");
    Sites.put(2, "Runoob");
    Sites.put(3, "Taobao");
    Sites.put(4, "Zhihu");
    Sites.clear();
    System.out.println(Sites);
  }
}
```

执行以上代码，输出结果如下：

```
{}
```

#### 计算大小

如果要计算 HashMap 中的元素数量可以使用 size() 方法：

**实例**

```java
// 引入 HashMap 类*    
**import** java.util.HashMap;

**public** **class** RunoobTest {
  **public** **static** **void** main(String[] args) {
    *// 创建 HashMap 对象 Sites*
    HashMap<Integer, String> Sites = **new** HashMap<Integer, String>();
    *// 添加键值对*
    Sites.put(1, "Google");
    Sites.put(2, "Runoob");
    Sites.put(3, "Taobao");
    Sites.put(4, "Zhihu");
    System.out.println(Sites.size());
  }
}
```

执行以上代码，输出结果如下：

```
4
```

#### 迭代 HashMap

可以使用 for-each 来迭代 HashMap 中的元素。

如果你只想获取 key，可以使用 **keySet()** 方法，然后可以通过 get(key) 获取对应的 value

如果你只想获取 value，可以使用 **values()** 方法。

**实例**

```java
// 引入 HashMap 类*    
import java.util.HashMap;

**public** **class** RunoobTest {
  **public** **static** **void** main(String[] args) {
    *// 创建 HashMap 对象 Sites*
    HashMap<Integer, String> Sites = **new** HashMap<Integer, String>();
    *// 添加键值对*
    Sites.put(1, "Google");
    Sites.put(2, "Runoob");
    Sites.put(3, "Taobao");
    Sites.put(4, "Zhihu");
    *// 输出 key 和 value*
    for (Integer i : Sites.keySet()) {
      System.out.println("key: " + i + " value: " + Sites.get(i));
    }
    *// 返回所有 value 值*
    for(String value: Sites.values()) {
     // 输出每一个value
     System.out.print(value + ", ");
    }
  }
}
```

执行以上代码，输出结果如下：

```
key: 1 value: Google
key: 2 value: Runoob
key: 3 value: Taobao
key: 4 value: Zhihu
Google, Runoob, Taobao, Zhihu,
```

------

#### HashMap 方法

hashmap

Java HashMap 常用方法列表如下：

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [clear()](https://www.runoob.com/java/java-hashmap-clear.html) | 删除 hashMap 中的所有键/值对                                 |
| [clone()](https://www.runoob.com/java/java-hashmap-clone.html) | 复制一份 hashMap                                             |
| [isEmpty()](https://www.runoob.com/java/java-hashmap-isempty.html) | 判断 hashMap 是否为空                                        |
| [size()](https://www.runoob.com/java/java-hashmap-size.html) | 计算 hashMap 中键/值对的数量                                 |
| [put()](https://www.runoob.com/java/java-hashmap-put.html)   | 将键/值对添加到 hashMap 中                                   |
| [putAll()](https://www.runoob.com/java/java-hashmap-putall.html) | 将所有键/值对添加到 hashMap 中                               |
| [putIfAbsent()](https://www.runoob.com/java/java-hashmap-putifabsent.html) | 如果 hashMap 中不存在指定的键，则将指定的键/值对插入到 hashMap 中。 |
| [remove()](https://www.runoob.com/java/java-hashmap-remove.html) | 删除 hashMap 中指定键 key 的映射关系                         |
| [containsKey()](https://www.runoob.com/java/java-hashmap-containskey.html) | 检查 hashMap 中是否存在指定的 key 对应的映射关系。           |
| [containsValue()](https://www.runoob.com/java/java-hashmap-containsvalue.html) | 检查 hashMap 中是否存在指定的 value 对应的映射关系。         |
| [replace()](https://www.runoob.com/java/java-hashmap-replace.html) | 替换 hashMap 中是指定的 key 对应的 value。                   |
| [replaceAll()](https://www.runoob.com/java/java-hashmap-replaceall.html) | 将 hashMap 中的所有映射关系替换成给定的函数所执行的结果。    |
| [get()](https://www.runoob.com/java/java-hashmap-get.html)   | 获取指定 key 对应对 value                                    |
| [getOrDefault()](https://www.runoob.com/java/java-hashmap-getordefault.html) | 获取指定 key 对应对 value，如果找不到 key ，则返回设置的默认值 |
| [forEach()](https://www.runoob.com/java/java-hashmap-foreach.html) | 对 hashMap 中的每个映射执行指定的操作。                      |
| [entrySet()](https://www.runoob.com/java/java-hashmap-entryset.html) | 返回 hashMap 中所有映射项的集合集合视图。                    |
| [keySet](https://www.runoob.com/java/java-hashmap-keyset.html)() | 返回 hashMap 中所有 key 组成的集合视图。                     |
| [values()](https://www.runoob.com/java/java-hashmap-values.html) | 返回 hashMap 中存在的所有 value 值。                         |
| [merge()](https://www.runoob.com/java/java-hashmap-merge.html) | 添加键值对到 hashMap 中                                      |
| [compute()](https://www.runoob.com/java/java-hashmap-compute.html) | 对 hashMap 中指定 key 的值进行重新计算                       |
| [computeIfAbsent()](https://www.runoob.com/java/java-hashmap-computeifabsent.html) | 对 hashMap 中指定 key 的值进行重新计算，如果不存在这个 key，则添加到 hasMap 中 |
| [computeIfPresent()](https://www.runoob.com/java/java-hashmap-computeifpresent.html) | 对 hashMap 中指定 key 的值进行重新计算，前提是该 key 存在于 hashMap 中。 |

### Hash源码解析

HashMap**实现原理和源码详细分析**

基于Jdk1.8

>  学习要点： 
>
> 1、知道HashMap的数据结构
>
> 2、了解HashMap中的散列算法
>
> 3、知道HashMap中put、remove、get的代码实现
>
> 4、HashMap的哈希冲突是什么？怎么处理的？ 
>
> 5、知道HashMap的扩容机制 

#### 概念

HashMap 基于哈希表的 Map 接口实现，是以 key-value 存储形式存在 ，HashMap 的实现不是同步的，这意味着它不是线程安全的。它的 key、value 都可以为 null，此外，HashMap 中的映射不是有序的。

#### 特性

- Hash存储无序的
- key和value都可以存储null值，但是key只能存唯一的一个null值
- jdk8之前的数据结构是数组+链表，Jdk8之后变成数组+链表+红黑树
- 阀值大于8并且数组长度大于64才会转为红黑树

#### 数据结构

JDK7的情况，是**数组加链接，hash冲突时候，就转换为链表：** 

jdk8的情况，jdk8加上**了红黑树，链表的数量大于8而且数组长度大于64之后，就转换为红黑树**，**红黑树节点小于6之后，就又转换为链表：** 

翻下HashMap源码，对应的元素节点信息：

```javascript
static class Node<K,V> implements Map.Entry<K,V> {
        // hashCode
        final int hash;
        final K key;
        V value;
        // 链表的next指针就不为null
        Node<K,V> next;
        // 构造函数。
       // 输入参数包括"哈希值(h)", "键(k)", "值(v)", "下一节点(n)"
        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
        // ...
}        
```

#### 初始化操作

##### 4.1、成员变量

```javascript
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {
	/**
     * 序列号版本号
     */
    private static final long serialVersionUID = 362498820763181265L;

   /**
     * 初始化容量，为16=2的4次幂
     */
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16

   /**
     * 最大容量，为2的30次幂
     */
    static final int MAXIMUM_CAPACITY = 1 << 30;

    /**
     * 默认的负载因子，默认值是0.75
     */
    static final float DEFAULT_LOAD_FACTOR = 0.75f;

    /**
     * 链表节点树超过8就转为为红黑树
     */
    static final int TREEIFY_THRESHOLD = 8;
    
    /**
     * 红黑树节点少于6就再转换回链表
     */
    static final int UNTREEIFY_THRESHOLD = 6;
    
    /**
     * 桶中结构转化为红黑树对应的数组长度最小的值
     */
    static final int MIN_TREEIFY_CAPACITY = 64;
    
    // ...
	
	 /**
     * HashMap存储元素的数组
     */
	transient Node<K,V>[] table;

    /**
     * 用来存放缓存
     */
    transient Set<Map.Entry<K,V>> entrySet;

    /**
     * HashMap存放元素的个数
     */
    transient int size;

    /**
     * 用来记录HashMap的修改次数
     */
    transient int modCount;

    /**
     * 用来调整大小下一个容量的值（容量*负载因子）
     */
    int threshold;

    /**
     * Hash表的负载因子
     */
    final float loadFactor;

}    
```

##### 4.2、 构造方法

```javascript
public HashMap(int initialCapacity, float loadFactor) {
    // 初始容量不能小于0，小于0直接抛出IllegalArgumentException
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
     // 初始容量大于最大容量的时候，取最大容量作为初始容量
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
     // 负载因子不能小于0，而且要是数值类型，isNan:true,表示就是非数值类型
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
    // 将指定的负载因子赋值给全局变量
    this.loadFactor = loadFactor;
    // threshold  = (容量) * (负载因子)
    this.threshold = tableSizeFor(initialCapacity);
}


public HashMap(int initialCapacity) {
     // 初始化容量和默认负载因子
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}
public HashMap() {
    // 默认的负载因子为0.75
	this.loadFactor = DEFAULT_LOAD_FACTOR;
}
```

然后，我们知道HashMap的默认容量是16，然后是在哪里赋值的？从上面这个代码就可以知道`this.threshold = tableSizeFor(initialCapacity);`

```javascript
static final int tableSizeFor(int cap) {
    int n = cap - 1;
    n |= n >>> 1;
    n |= n >>> 2;
    n |= n >>> 4;
    n |= n >>> 8;
    n |= n >>> 16;
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

这里涉及到计算机基本知识的，右移运算和或运算，下面给出图例：通过比较麻烦的计算得出n为16 

 往代码里翻，还找到下面这个构造方法`public HashMap(Map<? extends K, ? extends V> m)`：这个构造方法是用于构造一个映射关系与指定 Map 相同的新 HashMap：

```javascript
public HashMap(Map<? extends K, ? extends V> m) {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
     putMapEntries(m, false);
 }
```

看一下`putMapEntries`这个方法：

```javascript
final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
    // 传入的集合长度
    int s = m.size();
    if (s > 0) {
     	// 判断table是否已经初始化处理
        if (table == null) { // pre-size 未初始化的情况
            // 加上1.0F的目的是对小数向上取整，保证最大容量，减少resize的调用次数
            float ft = ((float)s / loadFactor) + 1.0F;
            int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                     (int)ft : MAXIMUM_CAPACITY);
            // 计算出来的t大于HashMap的阀值，进行tableSizeFor
            if (t > threshold)
                threshold = tableSizeFor(t);
        }
        else if (s > threshold) // 已经初始化的情况，进行扩容resize
            resize();
         // 遍历，将map中的所有元素都添加到hashMap中
        for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
            K key = e.getKey();
            V value = e.getValue();
            putVal(hash(key), key, value, false, evict);
        }
    }
}
```

#### Jdk8中HashMap的算法

##### HashMap中散列算法

**hash方法**

在HashMap的`java.util.HashMap#hash`，这个方法中有特定的用于计算哈希值的方法：

1. **先计算出hashCode，**
2. **然后将hashCode右移16位，**
3. **然后这两个数再做异或运算。**

这个方法的作用？

这个方法就是用于hashMap当put对应的key之后，计算特定的hashCode，然后再**(length-1)&hash**计算对应的数组table的下标，这个后面跟一下HashMap源码才比较清楚：

```javascript
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

看起来代码只有两行，然后其实蕴含了一种**散列算法**的思想，下面简单分析一下：这里先将代码进行拆分，看起来清晰点：

```javascript
static final int hash(Object key) {
    // 传入的key为null，返回默认值0
    if (key == null) return 0;
    // 计算哈希code
    int h = key.hashCode();
    // 将计算出来的hashCode右移16位，相当于乘于(1/2)的16次方 无符号"右移运算 符
    int t = h >>> 16;
    // 将两个值做异或运算然后返回
    return h ^ t;
}
```

然后作者的意图是什么？首先既然是散列算法，**散列算法的目的就是为了让数据均匀分布**

与运算出现 0 的概率大；使用或运算出现 1 的概率大

**使用异或运算，出现0和1的概率是相等的**，所以这就是为什么要使用**异或运算的原因，散列算法的本质目的就是为了让数据均匀分布，使用异或运算得出的哈希值因为比较均匀散列分布，所以出现哈希冲突的概率就小很多**

>  补充： 

- 与运算：两个数相应的位数字都是1，与运算后是1，其余情况是0；
- 或运算：两个数相应的位数字只要有1个是1，或运算后是1，否则是0；
- 异或运算：两个数相应的位数字相同，结果是0，否则是1；

然后为什么再进行右移16位？我们知道，int类型最大的数值是`2的32次方`，然后可以分为高16位加上低16位，右移16位就是使数值变小了，`“左大右小”`，这个是位移运算的准则

##### 哈希冲突？

哈希冲突也可以称之为哈希碰撞，理论上的哈希冲突是指计算出来的哈希值一样，导致冲突了，不过在HashMap中的哈希冲突具体是指**(n-1)&hash**，这个值是hashMap里数组的下标。

Jdk8之前的处理方法是通过链表处理，只要hash冲突了，就会将节点添加到链表尾部；

jdk8之后的做法是通过链表+红黑树的方法，最开始哈希冲突了，也是用链表，然后链表节点达到8个，数组长度超过64的情况，转成红黑树，这个可以在源码里找到答案

翻下源码，`HashMap#putVal`，里面的逻辑，先校验计算出来的，数组tab的下标，`i=(n-1)&hash`是否冲突了，**不冲突就新增节点，冲突的情况，转链表或者红黑树**

```javascript
if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
```

##### 下标计算方法

以上hash函数计算出的值，通过indexFor进一步处理来获取实际的存储位置

```javascript
/**
     * 返回数组下标
     */
    static int indexFor(int h, int length) {
        return h & (length-1);
    }
```

h&（length-1）**保证获取的index一定在数组范围内**，这样计算的结果 h 比（length-1）的高位均为0；不会大于length-1；举个例子，默认容量16，length-1=15，h=18

最终计算出的index=2。（HashMap中有大量位运算）

所以最终存储位置的确定流程是这样的：

1. **key 计算hashCode**
2. **hash函数散列均匀化**
3. **h&（length-1）计算坐标**

```javascript
void addEntry(int hash, K key, V value, int bucketIndex) {
        if ((size >= threshold) && (null != table[bucketIndex])) {
            resize(2 * table.length);//当size超过临界阈值threshold，并且即将发生哈希冲突时进行扩容
            hash = (null != key) ? hash(key) : 0;
            bucketIndex = indexFor(hash, table.length);
        }

        createEntry(hash, key, value, bucketIndex);
    }
```

　　通过以上代码能够得知，当发生哈希冲突并且size大于阈值的时候，需要进行数组扩容，扩容时，需要新建一个长度为之前数组2倍的新的数组，然后将当前的Entry数组中的元素全部传输过去，扩容后的新数组长度为之前的2倍，所以扩容相对来说是个耗资源的操作。

##### **扩容机制**

这个知识点是HashMap中的一个重点之一，也是一个比较难的问题

- **什么时候需要扩容**？

当hashMap中元素个数超过`threshold`，`threshold`为数组长度乘以负载因子loadFactor，loadFactor默认是`0.75f`

- **数组长度为何保持2的次幂**/为何以2倍扩容：

**背景：**

我们来继续看resize方法（非java8）

```javascript
void resize(int newCapacity) {
        Entry[] oldTable = table;
        int oldCapacity = oldTable.length;
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }

        Entry[] newTable = new Entry[newCapacity];
        transfer(newTable, initHashSeedAsNeeded(newCapacity));
        table = newTable;
        threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
    }
```

**起因：**

如果数组进行扩容，**数组长度发生变化**，而**存储位置 index = h&(length-1), index也可能会发生变化，需要重新计算index，**需要进行大量的copy操作；我们先来看看transfer这个方法

```javascript
void transfer(Entry[] newTable, boolean rehash) {
        int newCapacity = newTable.length;
　　　　　//for循环中的代码，逐个遍历链表，重新计算索引位置，将老数组数据复制到新数组中去（数组不存储实际数据，所以仅仅是拷贝引用而已）
        for (Entry<K,V> e : table) {
            while(null != e) {
                Entry<K,V> next = e.next;
                if (rehash) {
                    e.hash = null == e.key ? 0 : hash(e.key);
                }
                int i = indexFor(e.hash, newCapacity);
　　　　　　　　　 //将当前entry的next链指向新的索引位置,newTable[i]有可能为空，有可能也是个entry链，如果是entry链，直接在链表头部插入。
                e.next = newTable[i];
                newTable[i] = e;
                e = next;
            }
        }
    }
```

**方法理解：**

这个方法将老数组中的数据逐个链表地遍历，扔到新的扩容后的数组中，我们的数组索引位置的计算是通过 对key值的hashcode进行hash扰乱运算后，再通过和 length-1进行位运算得到最终数组索引位置。

**二倍扩容的原因**：

1. hashMap的数组长度一定保持2的次幂，比如
   16的二进制表示为 10000，那么length-1就是15，二进制为**0**1111，
   同理扩容后的数组长度为32，二进制表示为100000，length-1为31，二进制表示为0**1**1111。
   我们也能看到这样会保证低位全为1，而**扩容后只有一位差异，也就是多出了最左位的1**，

2. **这样在通过 h&(length-1)的时候，只要h对应的最左边的那一个差异位为0，就能保证得到的新的数组索引和老数组索引一致(大大减少了之前已经散列良好的老数组的数据位置重新调换)**

3. 还有，数组长度保持2的次幂，length-1的低位都为1，会使得获得的数组索引index更加均匀，

   　 **如果不是2的次幂，也就是低位不是全为1此时**，要使得index=21，**h的低位部分不再具有唯一性了，哈希冲突的几率会变的更大，同时，index对应的这个bit位无论如何不会等于1了，而对应的那些数组位置也就被白白浪费了。**



附 resize**的源码实现**

经过上面比较详细的分析，这个实现逻辑是可以在代码里找到对应的，ok，跟一下对应的源码：

```javascript
final Node<K,V>[] resize() {
   // 得到当前的节点数组
    Node<K,V>[] oldTab = table;
    // 数组的长度
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    int oldThr = threshold;
    int newCap, newThr = 0;
    // 计算扩容后的大小
    if (oldCap > 0) {
        if (oldCap >= MAXIMUM_CAPACITY) { // 超过最大容量 即 1 <<< 30
             // 超过最大容量就不扩充了，修改阀值为最大容量
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        // 没超过的情况，扩大为原来的两倍
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold
    }
    else if (oldThr > 0) // initial capacity was placed in threshold
         // 老阀值赋值给新的数组长度
        newCap = oldThr;
    else {               // zero initial threshold signifies using defaults
        // 使用默认值16
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    // 重新计算阀值，然后要赋值给threshold
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    // 新的阀值，原来默认是12，现在变为24
    threshold = newThr;
    // 创建新的节点, newCap是新的数组长度，为32
    @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    table = newTab;
    if (oldTab != null) {
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)
                     // 是红黑树节点，调用split方法
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else { // preserve order 是链表的情况
                	// 定义相关的指针     
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        next = e.next;
                        // 不需要移动位置
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else { // 需要移动位置 ，调整到“原位置+旧容量”这个位置 
                            if (hiTail == null)
                                hiHead = e;
                            else
                                // hiTail指向要移动的节点e
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        // 位置不变
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        // hiTail指向null
                        hiTail.next = null;
                        // oldCap是旧容量 ，移动到“原位置+旧容量”这个位置
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```



#### Jdk8中HashMap的put操作

- put方法的核心流程

1. **根据hashcode计算数组的下标**
2. **对应下标数组为空的情况，新增节点**
3. **否则就是哈希冲突了，如果桶使用链表节点，就新增到链表节点尾部，使用了红黑树就新增到红黑树里**

上面是核心的流程，忽略了存在重复的键，则为该键替换新值 value， size 大于阈值 threshold，则进行扩容等等这些情况

ok，还是跟一下put源码：

```javascript
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // 第1次新增，初始数据resize
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    // 判断是否出现hash冲突
    if ((p = tab[i = (n - 1) & hash]) == null)
        // hash不冲突，新增节点
        tab[i] = newNode(hash, key, value, null);
    else { // 哈希冲突的情况，使用链表或者红黑树处理
        Node<K,V> e; K k;
        // 存在重复的键的情况，key和hash都相等
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            // 将旧的节点对象赋值给新的e
            e = p;
        else if (p instanceof TreeNode) // 使用了红黑树节点
              // 将节点放到红黑树中
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else { // 链表的情况
             // 无限循环
            for (int binCount = 0; ; ++binCount) {
                // 一直遍历，找到尾节点
                if ((e = p.next) == null) {
                    // 将新节点添加到尾部
                    p.next = newNode(hash, key, value, null);
                    // 节点数量大于8，转为红黑树
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    // 跳出循环
                    break;
                }
                // 也是为了避免hashCode和key一样的情况
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                // 重新赋值，用于链表的遍历
                p = e;
            }
        }
        // 桶中找到的key、hash相等的情况，也就是找到了重复的键，要使用新值替换旧值
        if (e != null) { // existing mapping for key
            // 记录e的值
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                // 用新值替换旧值
                e.value = value;
            // 访问后回调
            afterNodeAccess(e);
            // 返回旧值
            return oldValue;
        }
    }
    // 记录修改次数
    ++modCount;
    // size大于threshole，进行扩容
    if (++size > threshold)
        resize();
     // 回调方法
    afterNodeInsertion(evict);
    return null;
}
```

然后是怎么转换为红黑树的？红黑树的知识相对比较复杂，所以比较详细的可以参考我之前的博客[Java手写实现红黑树](https://smilenicky.blog.csdn.net/article/details/119598161)：

```javascript
final void treeifyBin(Node<K,V>[] tab, int hash) {
    int n, index; Node<K,V> e;
    // MIN_TREEIFY_CAPACITY值为64，也就是说数组长度小于64是不会真正转红黑树的
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
    	// 扩容方法
        resize();
     // 转红黑树操作
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        // 红黑树的头节点hd和尾节点t1
        TreeNode<K,V> hd = null, tl = null;
        do {
            // 构建树节点
            TreeNode<K,V> p = replacementTreeNode(e, null);
            if (tl == null)
                // 新节点p赋值给红黑树的头节点
                hd = p;
            else {
                // 新节点的前节点就是原来的尾节点t1
                p.prev = tl;
                // 尾部节点t1的next节点就是新节点
                tl.next = p;
            }
            tl = p;
        } while ((e = e.next) != null);
        // 让数组的节点执行新建的树节点，之后这个节点就变成TreeNode
        if ((tab[index] = hd) != null)
            hd.treeify(tab);
    }
}
```

#### Jdk8中HashMap的remove操作

remove方法，这里思路是先要找到元素的位置，如果是链表，遍历链表remove元素就可以，红黑树的情况就遍历红黑树找到节点，然后remove树节点，如果这时候树节点数小于6，这种情况就要转链表

```javascript
@Override
public boolean remove(Object key, Object value) {
    return removeNode(hash(key), key, value, true, true) != null;
}

final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
    Node<K,V>[] tab; Node<K,V> p; int n, index;
    // 数组下标是(n-1)&hash，能找得到元素的情况
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (p = tab[index = (n - 1) & hash]) != null) {
        Node<K,V> node = null, e; K k; V v;
        // 桶上的节点就是要找的key
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            // 将Node指向该节点
            node = p;
        else if ((e = p.next) != null) { // 链表或者是红黑树节点的情况
            if (p instanceof TreeNode)
                // 找到红黑树节点
                node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
            else { // 链表的情况
                // 遍历链表，找到需要找的节点
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key ||
                         (key != null && key.equals(k)))) {
                        node = e;
                        break;
                    }
                    p = e;
                } while ((e = e.next) != null);
            }
        }
        // 找到节点之后
        if (node != null && (!matchValue || (v = node.value) == value ||
                             (value != null && value.equals(v)))) {
            if (node instanceof TreeNode)
            	// 红黑树remove节点
                ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
            else if (node == p)
                // 链表remove，通过改变指针
                tab[index] = node.next;
            else
                p.next = node.next;
            // 记录修改次数
            ++modCount;
            // 变动的数量
            --size;
            afterNodeRemoval(node);
            return node;
        }
    }
    return null;
}
```

#### Jdk8中HashMap的get操作

get方法：通过key找到value，这个方法比较容易理解

```javascript
 public V get(Object key) {
   Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}

final Node<K,V> getNode(int hash, Object key) {
   Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
   // 如果哈希表不为空并且key对应的桶上不为空
   if ((tab = table) != null && (n = tab.length) > 0 &&
       (first = tab[(n - 1) & hash]) != null) {
       // 根据索引的位置检查第一个节点
       if (first.hash == hash && // always check first node
           ((k = first.key) == key || (key != null && key.equals(k))))
           return first;
       if ((e = first.next) != null) { // 不是第1个节点的情况，那就有可能是链表或者红黑树节点
           if (first instanceof TreeNode)
               // 根据getTreeNode获取红黑树节点
               return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            // 链表的情况，只能遍历链表
           do {
               if (e.hash == hash &&
                   ((k = e.key) == key || (key != null && key.equals(k))))
                   return e;
           } while ((e = e.next) != null);
       }
   }
   return null;
}
```

#### HashMap相关面试题

- HashMap的数据结构是什么？ 在jdk8之前是数组+链表，jdk8之后是数组+链表+红黑树
- HashMap 中 hash 函数是怎么实现的？ 先通过jdk的`hashCode()`方法获取hashCode，右移16位，然后这两个数再做异或运算
- 什么是HashMap中的哈希冲突？ 哈希冲突，也可以称之为哈希碰撞，一般是值计算出的哈希值一样的，在HashMap中是根据计算出的hash，再去计算数组table下标(n-1)&hash一样了，也就是冲突了
- HashMap是如何处理哈希冲突问题的？ 在jdk8之前是通过链表的方法，jdk8之后是通过链表+红黑树的方法
- HashMap是线程安全的？ HashMap不是线程安全的，因为源码里没加同步锁也没其它保证线程安全的操作
- HashMap不是线程安全的，然后有什么方法？ 可以使用ConcurrentHashMap
- ConcurrentHashMap是怎么保证线程安全的？ ConcurrentHashMap在Jdk8中 使用了CAS加上synchronized同步锁来保证线程安全



# 枚举类

## 枚举类的定义

**枚举类：**类的对象只有有限个，确定的（日期，季节，支付方式等）

需要定义一组常量时，强雷建议使用枚举类

**自定义：**

```java
package com.practice;

public class SeasonTest {
    public static void main(String[] args) {
        Season spring = Season.season2;
        System.out.println(spring);

    }

}
//自定义枚举类
class Season{
    //private final
    private final String name;
    private final int ord;

    private Season(String name, int ord) {
        this.name = name;
        this.ord = ord;
    }
//提供当前枚举类的多个对象
    public static final Season season1= new Season("冬天",12);
    public static final Season season2= new Season("春天",3);
    public static final Season season3= new Season("夏天",6);
    public static final Season season4= new Season("秋",9);

    public String getName() {
        return name;
    }

    public int getOrd(){
        return ord;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", ord=" + ord +
                '}';
    }
}

```

## 使用enum定义枚举类

默认继承于Enum

```java
package com.practice;

public class SeasonTest {
    public static void main(String[] args) {
        Season spring = Season.season2;
        System.out.println(spring);
        System.out.println(Season.class.getSuperclass());

    }

}
//自定义枚举类
enum  Season{
    //提供当前枚举类的多个对象,逗号隔开
    season1("冬天",12),
    season2("春天",3),
    season3("夏天",6),
    season4("秋",9);
    //private final
    private final String name;
    private final int ord;

    private Season(String name, int ord) {
        this.name = name;
        this.ord = ord;
    }


    public String getName() {
        return name;
    }

    public int getOrd(){
        return ord;
    }
}

season2//由于默认继承于Enum不是Object 所以没有打印地址值
class java.lang.Enum
```

## 枚举类的主要方法

```
package com.practice;

public class SeasonTest {
    public static void main(String[] args) {
        Season spring = Season.season2;
        System.out.println(spring.toString());//打印名称
        
        //返回对象名称数组
        Season[] values = Season.values();
        for (Season value : values) {
            System.out.println(value);
        }
        System.out.println(spring.ordinal());
        
        //找指定名称的对象
        Season winter = Season.valueOf("season1");
        System.out.println(winter);

    }

}
//自定义枚举类
enum  Season{
    //提供当前枚举类的多个对象,逗号隔开
    season1("冬天",12),
    season2("春天",3),
    season3("夏天",6),
    season4("秋",9);
    //private final
    private final String name;
    private final int ord;

    private Season(String name, int ord) {
        this.name = name;
        this.ord = ord;
    }


    public String getName() {
        return name;
    }

    public int getOrd(){
        return ord;
    }

}

season2
season1
season2
season3
season4
1
season1

Process finished with exit code 0
```

## 枚举类实现接口

1. 与类一样，实现接口并重写接口中的抽象方法
2. 每个类对象都实现接口中的抽象方法



# Properties

属性集合

### 特点：

1. 存储属性名和属性值
2. **属性名和属性值都是字符串类型**
3. 没有泛型
4. 和流有关

### 方法

**主要方法：**

- list 打印
- store 保存
- **load 加载**

除了从 Hashtable 中所定义的方法，Properties 还定义了以下方法：

| **序号** | **方法描述**                                                 |
| :------- | :----------------------------------------------------------- |
| 1        | **String getProperty(String key)**  **用指定的键在此属性列表中搜索属性。** |
| 2        | **String getProperty(String key, String defaultProperty)** 用指定的键在属性列表中搜索属性。 |
| 3        | **void list(PrintStream streamOut)**  将属性列表输出 到指定的输出流。 |
| 4        | **void list(PrintWriter streamOut)** 将属性列表输出 到指定的输出流。 |
| **5**    | **void load(InputStream streamIn) throws IOException**  **从输入流中读取属性列表（键和元素对）。** |
| 6        | **Enumeration propertyNames( )** 按简单的面向行的格式从输入字符流中读取属性列表（键和元素对）。 |
| 7        | **Object setProperty(String key, String value)**  调用 Hashtable 的方法 put。 |
| 8        | **void store(OutputStream streamOut, String description)**  以适合使用 load(InputStream)方法加载到 Properties 表中的格式，将此 Properties 表中的属性列表（键和元素对）写入输出流。 |

### 使用

```java
import java.util.*;
 
public class PropDemo {
 
   public static void main(String args[]) {
      Properties capitals = new Properties();
      Set states;
      String str;
      
      capitals.put("Illinois", "Springfield");
      capitals.put("Missouri", "Jefferson City");
      capitals.put("Washington", "Olympia");
      capitals.put("California", "Sacramento");
      capitals.put("Indiana", "Indianapolis");
 
      // Show all states and capitals in hashtable.
      states = capitals.keySet(); // get set-view of keys
      Iterator itr = states.iterator();
      while(itr.hasNext()) {
         str = (String) itr.next();
         System.out.println("The capital of " +
            str + " is " + capitals.getProperty(str) + ".");
      }
      System.out.println();
 
      // look for state not in list -- specify default
      str = capitals.getProperty("Florida", "Not Found");
      System.out.println("The capital of Florida is "
          + str + ".");
   }
}
```

读取配置文件

```java
class myProperties {
    public static void main(String[] args) throws Exception {
        Properties pps = new Properties();
         //获取src路径下的文件的方式--->ClassLoader 类加载器//一定要在src下
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            URL res  = classLoader.getResource("jdbc.properties");
            String path = res.getPath()                                                               System.out.println(path);

        pps.load(new FileInputStream(path));
        
        String s = ppo.getProperty("url");
        
        Enumeration fileName = pps.propertyNames();
        while (fileName.hasMoreElements()) {
            String strKey = (String) fileName.nextElement();
            String strValue = pps.getProperty(strKey);
            System.out.println(strKey + "," + strValue);
        }
    }
}
```

### 配置文件 

**属性值没有引号**

文件名.properties

```java
age = 25

address = beijing
```

# 线程安全

### 线程不安全的集合类

在集合中学到的ArrayList、LinkedList、HashSet、TreeSet、HashMap、TreeMap等都是线程不安全的，也就是说，当多个并发线程向这些集合中存、取元素时，就可能会破坏这些集合的数据完整性。

#### Collections保证线程安全

如果程序中有多个线程可能访问以上这些集合，就可以使用Collections提供的类方法把这些集合包装成线程安全的集合。

Collections提供了如下静态方法。

比如，如果想在多线程中使用线程安全的HashMap，则可以使用以下代码：

 使用Collections的synchronizedMap方法将一个普通的

```
 HashMap map = (HashMap) Collections.synchronizedMap(new HashMap<>());
```

注意:
如果需要把某个集合包装成线程安全的集合,则应该在创建之后立即包装,如上程序所示—当HashMap对象创建后立即包装成线程安全的HashMap对象.

### 线程安全的集合类

#### java.util.concurren

从Java5开始，在java.util.concurrent包下提供了大量支持高效并发访问的集合接口和实现类，

这些线程安全的集合类分为两大类：

(1) 以Concurrent开头的集合类: 如 ConcurrentHashMap、ConcurrentLinkedQueue、ConcurrentLinkedDeque、ConcurrentSkipListMap和ConcurrentSkipListSet .

(2) 以CopyOnWrite开头的集合类:如 CopyOnWriteArrayList、CopyOnWriteArraySet .

其中以Concurrent开头的集合类代表了支持并发访问的集合，它们可以支持多个线程并发写入访问，这些写入线程的所有操作都是线程安全的，但读取操作不必锁定。以Concurrent开头的集合类采用了更复杂的算法来保证永远不会锁住整个集合，因此在并发写入时有较好的性能。

当多个线程共享访问一个公共集合时，ConcurrentLinkedQueue是一个恰当的选择。ConcurrentLinkedQueue不允许使用null元素。ConcurrentLinkedQueue实现了多线程的高效访问，多个线程访问ConcurrentLinkedQueue集合时无须等待。

在默认情况下，ConcurrentHashMap支持16个线程并发写入，当有超过16个线程并发向该Map中写入数据时，可能有一些线程需要等待。实际上，程序通过设置concurrencyLevel构造参数(默认值为16)来支持更多的并发写入线程。

与前面介绍的HashMap和普通集合不同的是，因为ConcurrentLinkedQueue和ConcurrentHashMap支持多线程并发访问，所以当使用迭代器来遍历元素时，该迭代器可能不能反映出创建迭代器之后所做的修改，但程序不会抛出任何异常。

Java8拓展了ConcurrentHashMap的功能，为该类新增了30多个新方法，这些方法可借助于Stream和Lambda表达式支持执行聚焦操作。ConcurrentHashMap新增的方法大致可分为如下三类：

除此之外，ConcurrentHashMap还新增了mappingCount()、newKeySet()等方法，增强后的ConcurrentHashMap更适合作为缓存实现类使用。

注意:
使用java.util包下的Collection作为集合对象时,如果该集合对象创建迭代器集合元素发生改变,则会引发ConcurrentModificationException异常.

由于CopyOnWriteArraySet的底层封装了CopyOnWriteArrayList，因此它的实现机制完全类似于CopyOnWriteList集合。

对应CopyOnWriteArrayList集合，正如它的名字所暗示的，它采用复制底层数组的方式来实现写操作。

当线程对CopyOnWriteArrayList集合执行读取操作时，线程将会直接读取集合本身，无须加锁与阻塞。当线程对CopyOnWriteArrayList集合执行写入操作时(包括调用add()、remove()、set()等方法)，该集合会在底层复制一份新的数组，接下来对新的数组执行写入操作。由于对CopyOnWriteArrayList集合的写入操作都是对数组的副本执行操作，因此它是线程安全的。

需要指出的是，由于CopyOnWriteArrayList执行写入操作时需要频繁地复制数组，性能比较差，但由于读操作与写操作不是操作同一个数组，并且读操作也不需要加锁，因此读操作就很快、很安全。由此可见，CopyOnWriteArrayList适合用在读取操作远远大于写入操作的场景中，比如缓存等。

# 正表达式

1. 任意一个字符表示匹配任意对应的字符，如a匹配a，7匹配7，-匹配-。

2. []代表匹配中括号中其中任一个字符，如[abc]匹配a或b或c。

3. -在中括号里面和外面代表含义不同，如在外时，就匹配-，如果在中括号内[a-b]表示匹配26个小写字母中的任一个；[a-zA-Z]匹配大小写共52个字母中任一个；[0-9]匹配十个数字中任一个。

4. ^在中括号里面和外面含义不同，如在外时，就表示开头，如^7[0-9]表示匹配开头是7的，且第二位是任一数字的字符串；如果在中括号里面，表示除了这个字符之外的任意字符(包括数字，特殊字符)，如[^abc]表示匹配出去abc之外的其他任一字符。

5. .表示匹配任意的字符。

6. \d表示数字。

7. \D表示非数字。

8. \s表示由空字符组成，[ \t\n\r\x\f]。

9. \S表示由非空字符组成，[^\s]。

10. \w表示字母、数字、下划线，[a-zA-Z0-9_]。

11. \W表示不是由字母、数字、下划线组成。

12. ?: 表示出现0次或1次。

13. +表示出现1次或多次。

14. *表示出现0次、1次或多次。

15. {n}表示出现n次。

16. {n,m}表示出现n~m次。

17. {n,}表示出现n次或n次以上。

18. XY表示X后面跟着Y，这里X和Y分别是正则表达式的一部分。

19. X|Y表示X或Y，比如"food|f"匹配的是foo（d或f），而"(food)|f"匹配的是food或f。

20. (X)子表达式，将X看做是一个整体

# 注解

Java 注解是附加在代码中的一些元信息，用于一些工具在编译、运行时进行解析和使用，起到说明、配置的功能。注解不会也不能影响代码的实际逻辑，仅仅起到辅助性的作用。

* 概念：说明程序的。**给计算机看的**
* 注释：用文字描述程序的。给程序员看的

* 定义：注解（Annotation），也叫元数据。一种代码级别的说明。它是JDK1.5及以后版本引入的一个特性，与类、接口、枚举是在同一个层次。**它可以声明在包、类、字段、方法、局部变量、方法参数等的前面，用来对这些元素进行说明，注释。**
* 概念描述：
	* JDK1.5之后的新特性
	* 说明程序的
	* 使用注解：@注解名称

### 作用分类：

​	①编写文档：通过代码里标识的注解生成文档【生成文档doc文档】
​	②代码分析：通过代码里标识的注解对代码进行分析【使用反射】
​	③编译检查：通过代码里标识的注解让编译器能够实现基本的编译检查【Override】

### JDK中预定义的一些注解

* @Override	：检测被该注解标注的方法是否是继承自父类(接口)的
* @Deprecated：该注解标注的内容，表示已过时
* @SuppressWarnings：压制警告
	* 一般传递参数all  @SuppressWarnings("all")

### 自定义注解

* 格式：
	元注解
	public @interface 注解名称{
		属性列表;
	}

* 本质：注解本质上就是一个接口，该接口默认继承Annotation接口
	* public interface MyAnno extends java.lang.annotation.Annotation {}

* 属性：接口中的抽象方法
	* 要求：
		1. 属性的返回值类型有下列取值
			* 基本数据类型
			* String
			* 枚举
			* 注解
			* 以上类型的数组

		2. 定义了属性，在使用时需要给属性赋值
			1. 如果定义属性时，使用default关键字给属性默认初始化值，则使用注解时，可以不进行属性的赋值。
			2. 如果只有一个属性需要赋值，并且属性的名称是value，则value可以省略，直接定义值即可。
			3. 数组赋值时，值使用{}包裹。如果数组中只有一个值，则{}可以省略
	



```java
package cn.itcast.annotation;


import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * 描述需要执行的类名，和方法名
 */

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Pro {

    String className();//代表了一套规范
    String methodName();
}


```



### 元注解

用于描述注解的注解

* @Target：描述注解能够作用的位置
	* ElementType取值：
		* TYPE：可以作用于类上
		* METHOD：可以作用于方法上
		* FIELD：可以作用于成员变量上
* @Retention：描述注解被保留的阶段
	* @Retention(RetentionPolicy.RUNTIME)：当前被描述的注解，会保留到class字节码文件中，并被JVM读取到
* @Documented：描述注解是否被抽取到api文档中
* @Inherited：描述注解是否被子类继承

### 注解的解析

在程序使用(解析)注解：获取注解中定义的属性值
 	1. 获取注解定义的位置的对象  （Class，Method,Field）
	2. 获取指定的注解
  * getAnnotation(Class)
    //其实就是在内存中生成了一个该注解接口的子类实现对象

```java
public class ProImpl implements Pro{
                public String className(){
                    return "cn.itcast.annotation.Demo1";
                }
                public String methodName(){
                    return "show";
                }
            }
```

3. 调用注解中的抽象方法获取配置的属性值

案例：简单的测试框架

```java
package cn.itcast.annotation;

import java.io.InputStream;
import java.lang.reflect.Method;
import java.util.Properties;

/**
 * 框架类
 */


@Pro(className = "cn.itcast.annotation.Demo1",methodName = "show")
public class ReflectTest {
    public static void main(String[] args) throws Exception {

        /*
            前提：不能改变该类的任何代码。可以创建任意类的对象，可以执行任意方法
         */


        //1.解析注解
        //1.1获取该类的字节码文件对象
        Class<ReflectTest> reflectTestClass = ReflectTest.class;
        //1.2.获取上边的注解对象
        //其实就是在内存中生成了一个该注解接口的子类实现对象
        /*

            public class ProImpl implements Pro{
                public String className(){
                    return "cn.itcast.annotation.Demo1";
                }
                public String methodName(){
                    return "show";
                }

            }
 */

        Pro an = reflectTestClass.getAnnotation(Pro.class);
        //3.调用注解对象中定义的抽象方法，获取返回值
        String className = an.className();
        String methodName = an.methodName();
        System.out.println(className);
        System.out.println(methodName);



        //3.加载该类进内存
        Class cls = Class.forName(className);
        //4.创建对象
        Object obj = cls.newInstance();
        //5.获取方法对象
        Method method = cls.getMethod(methodName);
        //6.执行方法
        method.invoke(obj);
    }
}
```



### 小结

1. 以后大多数时候，我们会使用注解，而不是自定义注解
2. 注解给谁用？
	1. 编译器
	2. 给解析程序用
3. 注解不是程序的一部分，可以理解为**注解就是一个标签**



# Java实例

### instanceof 关键字用法

instanceof  可以用来**判断某个实例变量是否属于某种类的类型**，但它的功能不局限于此，比如**还可以判断某个类是否属于某个类的子类的类型。**

instanceof 是 Java 的一个二元操作符，类似于 ==，>，< 等操作符。

instanceof 是 Java 的保留关键字。它的作用是**测试它左边的对象是否是它右边的类的实例**，返回 boolean 的数据类型。

以下实例创建了 displayObjectClass() 方法来演示 Java instanceof 关键字用法：

```java
class A{}

class B extends A{}

class C extends A{}

class D extends B{}

A obj = new D();

System.out.println(obj instanceof B); //ture

System.out.println(obj instanceof C); //false

System.out.println(obj instanceof D); //ture

System.out.println(obj instanceof A); //ture
```

   A

|    |

B   C

|

D



# 特性

## Java 各版本的新特性

### Java SE 8 

#### Lambda

Lambda 表达式，也可称为闭包，它是推动 Java 8 发布的最重要新特性。

Lambda 允许把函数作为一个方法的参数（**函数作为参数传递进方法中**）。

使用 Lambda 表达式可以使代码变的更加简洁紧凑。

##### 语法

lambda 表达式的语法格式如下：

(parameters) -> expression 或 (parameters) ->{ statements; }

以下是lambda表达式的重要特征:

- **可选类型声明：****不需要声明参数类型，编译器可以统一识别参数值。**
- **可选的参数圆括号：**一个参数无需定义圆括号，但多个参数需要定义圆括号。
- **可选的大括号：**如果主体包含了一个语句，就不需要使用大括号。
- **可选的返回关键字：**如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定表达式返回了一个数值。

------

##### 实例

Lambda 表达式的简单例子:

```
// 1. 不需要参数,返回值为 5  
() -> 5  
  
// 2. 接收一个参数(数字类型),返回其2倍的值  
x -> 2 * x  
  
// 3. 接受2个参数(数字),并返回他们的差值  
(x, y) -> x – y  
  
// 4. 接收2个int型整数,返回他们的和  
(int x, int y) -> x + y  
  
// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
(String s) -> System.out.print(s)
```

在 Java8Tester.java 文件输入以下代码：

Java8Tester.java 文件

```java
public class Java8Tester {
   public static void main(String args[]){
      Java8Tester tester = new Java8Tester();
       
       interface MathOperation {
          int operation(int a, int b);
       }

       interface GreetingService {
          void sayMessage(String message);
       }

       private int operate(int a, int b, MathOperation mathOperation){
          return mathOperation.operation(a, b);
       }
        
      // 类型声明
      MathOperation addition = (int a, int b) -> a + b;
        
      // 不用类型声明
      MathOperation subtraction = (a, b) -> a - b;
        
      // 大括号中的返回语句
      MathOperation multiplication = (int a, int b) -> { return a * b; };
        
      // 没有大括号及返回语句
      MathOperation division = (int a, int b) -> a / b;
        
      System.out.println("10 + 5 = " + tester.operate(10, 5, addition));
      System.out.println("10 - 5 = " + tester.operate(10, 5, subtraction));
      System.out.println("10 x 5 = " + tester.operate(10, 5, multiplication));
      System.out.println("10 / 5 = " + tester.operate(10, 5, division));
        
      // 不用括号
      GreetingService greetService1 = message ->
      System.out.println("Hello " + message);
        
      // 用括号
      GreetingService greetService2 = (message) ->
      System.out.println("Hello " + message);
        
      greetService1.sayMessage("Runoob");
      greetService2.sayMessage("Google");
   }
    
  
}
```

执行以上脚本，输出结果为：

```
$ javac Java8Tester.java 
$ java Java8Tester
10 + 5 = 15
10 - 5 = 5
10 x 5 = 50
10 / 5 = 2
Hello Runoob
Hello Google
```

使用 Lambda 表达式需要注意以下两点：

- Lambda 表达式**主要用来定义行内执行的方法类型接口**，例如，一个简单方法接口。在上面例子中，我们使用各种类型的Lambda表达式来定义MathOperation接口的方法。然后我们定义了sayMessage的执行。
- Lambda 表达式免去了使用匿名方法的麻烦，并且给予Java简单但是强大的函数化的编程能力。

------

##### 变量作用域

lambda 表达式只能引用标记了 final 的外层局部变量，

**这就是说不能在 lambda 内部修改定义在域外的局部变量**，否则会编译错误。

在 Java8Tester.java 文件输入以下代码：

```java
public class Java8Tester {
 
   final static String salutation = "Hello! ";
   
   public static void main(String args[]){
      GreetingService greetService1 = message -> 
      System.out.println(salutation + message);
      greetService1.sayMessage("Runoob");
   }
    
   interface GreetingService {
      void sayMessage(String message);
   }
}
```

执行以上脚本，输出结果为：

```
$ javac Java8Tester.java 
$ java Java8Tester
Hello! Runoob
```

我们也可以直接在 lambda 表达式中**访问**外层的局部变量：

Java8Tester.java 文件

```java
public class Java8Tester {
    public static void main(String args[]) {
        final int num = 1;
        Converter<Integer, String> s = (param) -> System.out.println(String.valueOf(param + num));
        s.convert(2);  // 输出结果为 3
    }
 
    public interface Converter<T1, T2> {
        void convert(int i);
    }
}
```

lambda 表达式的局部变量可以不用声明为 final，但是必须不可被后面的代码修改（即隐性的具有 final 的语义）

```java
int num = 1;  
Converter<Integer, String> s = (param) -> System.out.println(String.valueOf(param + num));
s.convert(2);
num = 5;  
//报错信息：Local variable num defined in an enclosing scope must be final or effectively 
 final
```

在 Lambda 表达式当中不允许声明一个与局部变量同名的参数或者局部变量。

```
String first = "";  
Comparator<String> comparator = (first, second) -> Integer.compare(first.length(), second.length());  //编译会出错 
```

#### Functional Interface

##### 概念

函数式接口(Functional Interface)就是一个有且仅有一个抽象方法，但是可以有多个非抽象方法的接口。

函数式接口可以被隐式转换为 lambda 表达式。

Lambda 表达式和方法引用（实际上也可认为是Lambda表达式）上。

##### 定义

如定义了一个函数式接口如下：

```java
@FunctionalInterface
interface GreetingService 
{
    void sayMessage(String message);
}
```

那么就可以使用Lambda表达式来表示该接口的一个实现(注：JAVA 8 之前一般是用匿名类实现的)：

```
GreetingService greetService1 = message -> System.out.println("Hello " + message);
```

##### 已存在的

函数式接口可以对现有的函数友好地支持 lambda。

JDK 1.8 之前已有的函数式接口:

- **java.lang.Runnable**
- java.util.concurrent.Callable
- java.security.PrivilegedAction
- java.util.Comparator
- java.io.FileFilter
- java.nio.file.PathMatcher
- java.lang.reflect.InvocationHandler
- java.beans.PropertyChangeListener
- java.awt.event.ActionListener
- javax.swing.event.ChangeListener

JDK 1.8 新增加的函数接口：

- java.util.function

java.util.function 它包含了很多类，用来支持 Java的 函数式编程，该包中的函数式接口有：

| 序号 | 接口 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **BiConsumer<T,U>**代表了一个接受两个输入参数的操作，并且不返回任何结果 |
| 2    | **BiFunction<T,U,R>**代表了一个接受两个输入参数的方法，并且返回一个结果 |
| 3    | **BinaryOperator<T>**代表了一个作用于于两个同类型操作符的操作，并且返回了操作符同类型的结果 |
| 4    | **BiPredicate<T,U>**代表了一个两个参数的boolean值方法        |
| 5    | **BooleanSupplier**代表了boolean值结果的提供方               |
| 6    | **Consumer<T>**代表了接受一个输入参数并且无返回的操作        |
| 7    | **DoubleBinaryOperator**代表了作用于两个double值操作符的操作，并且返回了一个double值的结果。 |
| 8    | **DoubleConsumer**代表一个接受double值参数的操作，并且不返回结果。 |
| 9    | **DoubleFunction<R>**代表接受一个double值参数的方法，并且返回结果 |
| 10   | **DoublePredicate**代表一个拥有double值参数的boolean值方法   |
| 11   | **DoubleSupplier**代表一个double值结构的提供方               |
| 12   | **DoubleToIntFunction**接受一个double类型输入，返回一个int类型结果。 |
| 13   | **DoubleToLongFunction**接受一个double类型输入，返回一个long类型结果 |
| 14   | **DoubleUnaryOperator**接受一个参数同为类型double,返回值类型也为double 。 |
| 15   | **Function<T,R>**接受一个输入参数，返回一个结果。            |
| 16   | **IntBinaryOperator**接受两个参数同为类型int,返回值类型也为int 。 |
| 17   | **IntConsumer**接受一个int类型的输入参数，无返回值 。        |
| 18   | **IntFunction<R>**接受一个int类型输入参数，返回一个结果 。   |
| 19   | **IntPredicate**：接受一个int输入参数，返回一个布尔值的结果。 |
| 20   | **IntSupplier**无参数，返回一个int类型结果。                 |
| 21   | **IntToDoubleFunction**接受一个int类型输入，返回一个double类型结果 。 |
| 22   | **IntToLongFunction**接受一个int类型输入，返回一个long类型结果。 |
| 23   | **IntUnaryOperator**接受一个参数同为类型int,返回值类型也为int 。 |
| 24   | **LongBinaryOperator**接受两个参数同为类型long,返回值类型也为long。 |
| 25   | **LongConsumer**接受一个long类型的输入参数，无返回值。       |
| 26   | **LongFunction<R>**接受一个long类型输入参数，返回一个结果。  |
| 27   | **LongPredicate**R接受一个long输入参数，返回一个布尔值类型结果。 |
| 28   | **LongSupplier**无参数，返回一个结果long类型的值。           |
| 29   | **LongToDoubleFunction**接受一个long类型输入，返回一个double类型结果。 |
| 30   | **LongToIntFunction**接受一个long类型输入，返回一个int类型结果。 |
| 31   | **LongUnaryOperator**接受一个参数同为类型long,返回值类型也为long。 |
| 32   | **ObjDoubleConsumer<T>**接受一个object类型和一个double类型的输入参数，无返回值。 |
| 33   | **ObjIntConsumer<T>**接受一个object类型和一个int类型的输入参数，无返回值。 |
| 34   | **ObjLongConsumer<T>**接受一个object类型和一个long类型的输入参数，无返回值。 |
| 35   | **Predicate<T>**接受一个输入参数，返回一个布尔值结果。       |
| 36   | **Supplier<T>**无参数，返回一个结果。                        |
| 37   | **ToDoubleBiFunction<T,U>**接受两个输入参数，返回一个double类型结果 |
| 38   | **ToDoubleFunction<T>**接受一个输入参数，返回一个double类型结果 |
| 39   | **ToIntBiFunction<T,U>**接受两个输入参数，返回一个int类型结果。 |
| 40   | **ToIntFunction<T>**接受一个输入参数，返回一个int类型结果。  |
| 41   | **ToLongBiFunction<T,U>**接受两个输入参数，返回一个long类型结果。 |
| 42   | **ToLongFunction<T>**接受一个输入参数，返回一个long类型结果。 |
| 43   | **UnaryOperator<T>**接受一个参数为类型T,返回值类型也为T。    |

##### 实例

Predicate <T> 接口是一个函数式接口，它接受一个输入参数 T，返回一个布尔值结果。

该接口包含多种默认方法来将Predicate组合成其他复杂的逻辑（比如：与，或，非）。

该接口用于测试对象是 true 或 false。

我们可以通过以下实例（Java8Tester.java）来了解函数式接口 Predicate <T> 的使用：

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
 
public class Java8Tester {
   public static void main(String args[]){
      List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        
      // Predicate<Integer> predicate = n -> true
      // n 是一个参数传递到 Predicate 接口的 test 方法
      // n 如果存在则 test 方法返回 true
        
      System.out.println("输出所有数据:");
        
      // 传递参数 n
      eval(list, n->true);
        
      // Predicate<Integer> predicate1 = n -> n%2 == 0
      // n 是一个参数传递到 Predicate 接口的 test 方法
      // 如果 n%2 为 0 test 方法返回 true
        
      System.out.println("输出所有偶数:");
      eval(list, n-> n%2 == 0 );
        
      // Predicate<Integer> predicate2 = n -> n > 3
      // n 是一个参数传递到 Predicate 接口的 test 方法
      // 如果 n 大于 3 test 方法返回 true
        
      System.out.println("输出大于 3 的所有数字:");
      eval(list, n-> n > 3 );
   }
    
   public static void eval(List<Integer> list, Predicate<Integer> predicate) {
      for(Integer n: list) {
        
         if(predicate.test(n)) {
            System.out.println(n + " ");
         }
      }
   }
}
```







1. 
2. Pipelines and Streams
3. Date and Time API
4. Default Methods
5. Type Annotations
6. Nashhorn JavaScript Engine
7. Concurrent Accumulators
8. Parallel operations
9. PermGen Error Removed

### **Java SE 7**  

1. **Strings** in Switch Statement

   ```
   在Java7之前，switch只能支持 
   byte、short、char、int或者其对应的封装类以及Enum类型。
   在Java7中，也支持了String类型
   String byte short int char Enum 类型
   ```

2. Type Inference for Generic Instance Creation

3. Multiple Exception Handling

4. Support for Dynamic Languages

5. Try with Resources

6. Java nio Package

7. Binary Literals, Underscore in literals

8. Diamond Syntax

- [Difference between Java 1.8 and Java 1.7?](http://www.selfgrowth.com/articles/difference-between-java-18-and-java-17)
- [Java 8 特性](http://www.importnew.com/19345.html)

### Java 与 C++ 的区别

- Java 是纯粹的面向对象语言，所有的对象都继承自 java.lang.Object，C++ 为了兼容 C 即支持面向对象也支持面向过程。
- Java 通过虚拟机从而实现跨平台特性，但是 C++ 依赖于特定的平台。
- Java 没有指针，它的引用可以理解为安全指针，而 C++ 具有和 C 一样的指针。
- Java 支持自动垃圾回收，而 C++ 需要手动回收。
- Java 不支持多重继承，只能通过实现多个接口来达到相同目的，而 C++ 支持多重继承。
- Java 不支持操作符重载，虽然可以对两个 String 对象执行加法运算，但是这是语言内置支持的操作，不属于操作符重载，而 C++ 可以。
- Java 的 goto 是保留字，但是不可用，C++ 可以使用 goto。

[What are the main differences between Java and C++?](http://cs-fundamentals.com/tech-interview/java/differences-between-java-and-cpp.php)

### JRE or JDK

- JRE：Java Runtime Environment，Java 运行环境的简称，为 Java 的运行提供了所需的环境。它是一个 JVM 程序，主要包括了 JVM 的标准实现和一些 Java 基本类库。
- JDK：Java Development Kit，Java 开发工具包，提供了 Java 的开发及运行环境。JDK 是 Java 开发的核心，集成了 JRE 以及一些其它的工具，比如编译 Java 源码的编译器 javac 等。

- 



# Junit 测试

### 测试分类：

​	1. 黑盒测试：不需要写代码，给输入值，看程序是否能够输出期望的值。
​	2. 白盒测试：需要写代码的。关注程序具体的执行流程。

### Junit使用：白盒测试

* 步骤：
	1. 定义一个测试类(测试用例)
		* 建议：
			* 测试类名：被测试的类名Test		CalculatorTest
			* 包名：xxx.xxx.xx.test		cn.itcast.test

	2. 定义测试方法：可以独立运行
		* 建议：
			* 方法名：test测试的方法名		testAdd()  
			* 返回值：void
			* 参数列表：空参

	3. **给方法加@Test**
	4. **导入junit依赖环境**
* 判定结果：
	* 红色：失败
	* 绿色：成功
	* **一般我们会使用断言操作来处理结果**
		* **Assert.assertEquals(期望的结果,运算的结果);**

### 补充：

* @Before:
	* 修饰的方法会在测试方法之前被自动执行
* @After:
	* 修饰的方法会在测试方法执行之后自动被执行

