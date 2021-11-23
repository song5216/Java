# Java IO
<!-- GFM-TOC -->
* [Java IO](#java-io)
    * [一、概览](#一概览)
    * [二、磁盘操作](#二磁盘操作)
    * [三、字节操作](#三字节操作)
        * [实现文件复制](#实现文件复制)
        * [装饰者模式](#装饰者模式)
    * [四、字符操作](#四字符操作)
        * [编码与解码](#编码与解码)
        * [String 的编码方式](#string-的编码方式)
        * [Reader 与 Writer](#reader-与-writer)
        * [实现逐行输出文本文件的内容](#实现逐行输出文本文件的内容)
    * [五、对象操作](#五对象操作)
        * [序列化](#序列化)
        * [Serializable](#serializable)
        * [transient](#transient)
    * [六、网络操作](#六网络操作)
        * [InetAddress](#inetaddress)
        * [URL](#url)
        * [Sockets](#sockets)
        * [Datagram](#datagram)
    * [七、NIO](#七nio)
        * [流与块](#流与块)
        * [通道与缓冲区](#通道与缓冲区)
        * [缓冲区状态变量](#缓冲区状态变量)
        * [文件 NIO 实例](#文件-nio-实例)
        * [选择器](#选择器)
        * [套接字 NIO 实例](#套接字-nio-实例)
        * [内存映射文件](#内存映射文件)
        * [对比](#对比)
    * [八、参考资料](#八参考资料)
    <!-- GFM-TOC -->

## 一、概览

 **流的概念**

内存与存储设备之间传输数据的通道

 **流的分类**

- 输入流：将<存储设备>中的内容读到<内存>中
- 输出流：将<内存>中的内容写到<存储设备>中



Java 的 I/O 大概可以分成以下几类：

- 磁盘操作：File
- 字节操作：InputStream 和 OutputStream
- 字符操作：Reader 和 Writer
- 对象操作：Serializable
- 网络操作：Socket
- 新的输入/输出：NIO

## 二、磁盘操作

File 类可以用于。

- 创建文件/文件夹
- 删除
- 获取
- 判断存在
- 文件夹遍历

**静态成员变量**

```java
package com.JavaIO;

import java.io.File;

public class FileDemo {
    public static void main(String[] args) {
        String pathSepartor = File.pathSeparator;//与系统相关的路径分隔符
        System.out.println(pathSepartor);
        String separator = File.separator;//与系统有关的默认名称分隔符，为了方便，它被表示为一个字符串。
        System.out.println(separator);
    }
}




```

**构造方法:**

```java
File(File parent, String child)//根据 父路径名和 子路径名字符串创建一个新 File 实例。
File(String pathname)//通过将给定路径名字符串转换为抽象路径名来创建一个新 File 实例。
File(String parent, String child)//根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例。
    
package com.JavaIO;

import java.io.File;

public class FileDemo2 {
    public static void main(String[] args) {
        //File类的构造方法
        show();//打印绝对路径
    }
    private static void show(){
        File f1 = new File("D:\\javasoftware\\IDE\\test\\src\\com\\JavaIO");
        System.out.println(f1);//重写了toString方法,打印了路径
    }
}
```

**常用方法：**

获取：

```java
package com.JavaIO;

import java.io.File;

public class FileDemo3 {
    public static void main(String[] args) {
        //File类的构造方
        show();
    }
    private static void show(){
        File f1 = new File("D:\\javasoftware\\IDE\\test\\src\\com\\JavaIO");
        String path1 = f1.getAbsolutePath();//返回绝对路径
        String name = f1.getName();//返回路径结尾
        long size = f1.length();//获取文件的大小
        System.out.println(size);
        
        f1.toString();//   public String toString() {return getPath(); }
    }
}

```

**判断**：

```
public boolean exists();//判断文件夹是否存在
//路径存在的前提下
public boolean isDirectory();//判断路径结尾是否是文件夹
public boolean isFile();//判断路径结尾是否是文件
```

**创建删除：**

```
createNewFile();//文件存在返回false 它的声明会抛出异常
delete();//文件的删除
mkdir();//创建单级文件夹
mkdirs();//创建单或者多级文件夹

/~他们返回的都是boolean类型；
```

**遍历：**

```
public String[] list();\\获取路径下的文件名称
public File[] listFiles();\\获取路径下的文件
```

递归地列出一个目录下所有文件：

```java
public static void listAllFiles(File dir) {
    if (dir == null || !dir.exists()) {
        return;
    }
    if (dir.isFile()) {
        System.out.println(dir.getName());
        return;
    }
    for (File file : dir.listFiles()) {
        listAllFiles(file);
    }
}
```

从 Java7 开始，可以使用 Paths 和 Files 代替 File。

**File类的使用**

```java
/*
File类的使用
1. 分隔符
2. 文件操作
3. 文件夹操作
*/
public class Demo{
  psvm(String[] args){
    separator();
  }
  // 1. 分隔符
  public static void separator(){
    sout("路径分隔符" + File.pathSeparator);
    sout("名称分隔符" + File.separator);
  }
  // 2. 文件操作
  public static void fileOpen(){
    // 1. 创建文件
    if(!file.exists()){ // 是否存在
    	File file = new File("...");
    	boolean b = file.creatNewFile();
    }
    
    // 2. 删除文件
    // 2.1 直接删除
    file.delete(); // 成功true
    // 2.2 使用jvm退出时删除
    file.deleteOnExit();
    
    // 3. 获取文件信息
    sout("获取绝对路径" + file.getAbsolutePaht());
    sout("获取路径" + file.getPath());
    sout("获取文件名称" + file.getName());
    sout("获取夫目录" + file.getParent());
    sout("获取文件长度" + file.length());
    sout("文件创建时间" + new Date(file.lashModified()).toLocalString());
    
    // 4. 判断
    sout("是否可写" + file.canWrite());
    sout("是否是文件" + file.isFile());
    sout("是否隐藏" + file.isHidden());
  }
  
  
  // 文件夹操作
  public static void directoryOpe() throws Exception{
    // 1. 创建文件夹
    File dir = new File("...");
    sout(dir.toString());
    if(!dir.exists()){
      //dir.mkdir(); // 只能创建单级目录
      dir.mkdirs(); // 创建多级目录
    }
    
    // 2. 删除文件夹
    // 2.1 直接删除
    dir.delete(); // 只能删除最底层空目录
    // 2.2 使用jvm删除
    dir.deleteOnExit();
    
    // 3. 获取文件夹信息
 		sout("获取绝对路径" + dir.getAbsolutePaht());
    sout("获取路径" + dir.getPath());
    sout("获取文件名称" + dir.getName());
    sout("获取夫目录" + dir.getParent());
    sout("获取文件长度" + dir.length());
    sout("文件夹创建时间" + new Date(dir.lashModified()).toLocalString());
    
    // 4. 判断
    sout("是否是文件夹" + dir.isFile());
    sout("是否隐藏" + dir.isHidden());
    
    // 5. 遍历文件夹
    File dir2 = new File("...");
    String[] files = dir2.list();
    for(String string : files){
      sout(string);
    }
    
    // FileFilter接口的使用
    
    File[] files2 = dir2.listFiles(new FileFilter(){
      
      @Override
      public boolean accept(File pathname){
        if(pathname.getName().endsWith(".jpg")){
          return true;
        }
        return false;
      }
    });
    for(File file : files2){
      sout(file.getName());
    }
    
  }
}

```

**递归遍历删除文件夹下的所有文件**

```java
import java.io.File;
import java.io.FileFilter;

public class FileDemocratic {
    public static void main(String[] args) {
        File dir =new File("D:\\桌面\\临时文档\\PDF2020win");
        deletAllFile(dir);


    }

    public static void deletAllFile(File dir){
        if(dir.isFile()){
            System.out.println("删除"+dir.getAbsolutePath()+ dir.delete());

        }else{
            File[] files = dir.listFiles(new FileFilter() {
                @Override
                public boolean accept(File pathname) {
                    if(pathname.isFile()){
                        return true;
                    }else {
                        return false;
                    }
                }
            });

            for (File file : files) {
                System.out.println("删除"+file.getAbsolutePath()+ file.delete());
            }
            File[] files2 = dir.listFiles(new FileFilter() {
                @Override
                public boolean accept(File pathname) {
                    if(pathname.isDirectory()){
                        return true;
                    }else {
                        return false;
                    }
                }
            });
            for (File filedir : files2) {
                deletAllFile(filedir);
            }

        }
    }
}



//优化
package com.JavaIO.File;

import java.io.File;

public class FileDemocratic {
    public static void main(String[] args) {
        File dir =new File("D:\\桌面\\临时文档\\055 Visio");
        deletAllFile(dir);


    }

    public static void deletAllFile(File dir){
        if(dir.isFile()){
            System.out.println("删除"+dir.getAbsolutePath()+ dir.delete());

        }else{
            File[] files = dir.listFiles();
            if(files==null||files.length<=0){
                return;
            }
            for (File file : files) {
                if(file.isDirectory()){
                    deletAllFile(file);
                }else {
                    System.out.println("删除"+file.getAbsolutePath()+ "_"+file.delete());
                }
            }
        }
    }
}

```



## 三、字节流 Stream

stream结尾都是字节流，reader和writer结尾都是字符流 两者的区别就是读写的时候一个是按字节读写，一个是按字符。

**InputStream**

**OutputStream**

字节流的**父类**（抽象类）

```java
//InputStream 字节输入流
public int read(){} //从输入流读取数据的下一个字节。 值字节被返回作为int范围0至255 。 如果没有字节可用，因为已经到达流的末尾，则返回值-1 。 该方法阻塞直到输入数据可用，检测到流的结尾，或抛出异常。 
public int read(byte[] b){}//从输入流读取一些字节数，并将它们存储到缓冲区b 。 实际读取的字节数作为整数返回。 该方法阻塞直到输入数据可用，检测到文件结束或抛出异常。 如果b的长度为零，则不会读取字节并返回0 ; 否则，尝试读取至少一个字节。 如果没有字节可用，因为流在文件末尾，则返回值-1 ; 否则，读取至少一个字节并存储到b 。 第一个字节读取存储在元素b[0] ，下一个字节存入b[1]等等。 读取的字节数最多等于b的长度。 令k为实际读取的字节数; 这些字节将存储在元素b[0]至b[ k -1] ，使元素b[ k ]至b[b.length-1]不受影响。 该read(b)用于类方法InputStream具有相同的效果为： read(b, 0, b.length)  
public int read(byte[] b, int off, int len){}//从输入流读取len字节的数据到一个字节数组。 尝试读取多达len个字节，但可以读取较小的数字。 实际读取的字节数作为整数返回。 该方法阻塞直到输入数据可用，检测到文件结束或抛出异常。 如果len为零，则不会读取字节并返回0 ; 否则，尝试读取至少一个字节。 如果没有字节可用，因为流是文件的-1则返回值-1 ; 否则，读取至少一个字节并存储到b 。 第一个字节读取存储在元素b[off] ，下一个字节存入b[off+1] ，等等。 读取的字节数最多等于len 。 令k为实际读取的字节数; 这些字节将存储在元素b[off]至b[off+ k -1] ，使元素b[off+ k ]至b[off+len-1]不受影响。 在每种情况下，元件b[0]至b[off]和元件b[off+len]至b[b.length-1]不受影响。



// OutputStream 字节输出流
public void write(int n){}//将指定的字节写入此输出流。 write的一般合同是将一个字节写入输出流。 要写入的字节是参数b的八个低位。 b的24个高位被忽略。 
public void write(byte[] b){}
public void write(byte[] b, int off, int len){}
```

### 3.1文件字节流

**子类**

文件输入流

```java
psvm(String[] args) throws Exception{
  // 1 创建FileInputStream 并指定文件路径
  FileInputStream fis = new FileInputStream("d:\\abc.txt");
  // 2 读取文件
  // fis.read(); //返回读入的字节
  // 2.1单字节读取
  int data = 0;
  while((data = fis.read()) != -1){
    sout((char)data);
  }
  // 2.2 一次读取多个字节
  byte[] buf = new byte[3]; // 大小为3的缓存区
  int count = fis.read(buf); //读入的字节放在buf中，返回count 实际读取字符的个数
  sout(new String(buf));
  sout(count);
  int count2 = fis.read(buf); // 再读3个
  sout(new String(buf));
  sout(count2);
  
  // 上述优化后
  int count = 0;
  while((count = fis.read(buf)) != -1){
    sout(new String(buf, 0, count));
  }
  
  // 3 关闭
  fis.close();
}

```

文件输出流

```java
package com.JavaIO.ByteStream;

import java.io.FileOutputStream;

public class FileOutput {
    public static  void  main(String[] args) throws Exception{
        // 1 创建文件字节输出流
        FileOutputStream fos = new FileOutputStream("路径", true);// true表示不覆盖 接着写
        // 2 写入文件
        fos.write(97);
        fos.write('a');
        String string = "hello world";
        fos.write(string.getBytes());
        // 3 关闭
        fos.close();
    }
}
```



### 3.2实现文件复制

```java
public static void copyFile(String src, String dist) throws IOException {
    FileInputStream in = new FileInputStream(src);
    FileOutputStream out = new FileOutputStream(dist);

    byte[] buffer = new byte[20 * 1024];//缓冲区
    int cnt;

    // read() 最多读取 buffer.length 个字节
    // 返回的cnt是实际读取的个数
    // 返回 -1 的时候表示读到 eof，即文件尾
    while ((cnt = in.read(buffer, 0, buffer.length)) != -1) {
        out.write(buffer, 0, cnt);
    }

    in.close();
    out.close();
}

package com.JavaIO.ByteStream;

import java.io.IOException;

public class CopyTest {
    public static void main(String[] args) throws IOException {
        CopyFile.copyFile("路径","路径附件");
    }
}


//FileInputStream 的readAPI
public int read(byte[] b,
                int off,
                int len)
         throws IOException
Reads up to len bytes of data from this input stream into an array of bytes. If len is not zero, the method blocks until some input is available; otherwise, no bytes are read and 0 is returned.
Overrides: 
read in class InputStream 
Parameters: 
b - the buffer into which the data is read. 
off - the start offset in the destination array b 
len - the maximum number of bytes read. 
Returns: 
the total number of bytes read into the buffer, or -1 if there is no more data because the end of the file has been reached. 
Throws: 
NullPointerException - If b is null. 
IndexOutOfBoundsException - If off is negative, len is negative, or len is greater than b.length - off 
IOException - if an I/O error occurs.
```

### 3.3 字节缓冲流

缓冲流：BufferedInputStream/ BufferedOutputStream

```java
BufferedInputStream(InputStream in) 
创建一个 BufferedInputStream并保存其参数，输入流 in ，供以后使用。  
BufferedInputStream(InputStream in, int size) 
创建 BufferedInputStream具有指定缓冲区大小，并保存其参数，输入流 in ，供以后使用。 


BufferedOutputStream(OutputStream out) 
创建一个新的缓冲输出流，以将数据写入指定的底层输出流。  
BufferedOutputStream(OutputStream out, int size) 
创建一个新的缓冲输出流，以便以指定的缓冲区大小将数据写入指定的底层输出流。 
```



- 提高IO效率，减少访问磁盘次数
- 数据存储在缓冲区中，flush是将缓冲区的内容写入文件中，也可以直接close

```JAVA
// 使用字节缓冲流 读取 文件
psvm(String[] args) throws Exception{
  // 1 创建BufferedInputStream
  FileInputStream fis = new FileInputStream("路径");
  BufferedInputStream bis = new BufferedInputStream(fis);
  // 2 读取
  int data = 0;
  while((data = bis.read()) != -1){
    sout((char)data);
  }
  // 用自己创建的缓冲流
  byte[] buf = new byte[1024];
  int count = 0;
  while((count = bis.read(buf)) != -1){
    sout(new String(buf, 0, count));
  }
  
  // 3 关闭
  bis.close();
}
```

```JAVA
// 使用字节缓冲流 写入 文件
psvm(String[] args) throws Exception{
  // 1 创建BufferedInputStream
  FileOutputStream fos = new FileOutputStream("路径");
  BufferedOutputStream bis = new BufferedOutputStream(fos);
  // 2 写入文件
  for(int i = 0; i < 10; i ++){
    bos.write("hello".getBytes());// 写入8k缓冲区
    bos.flush(); // 刷新到硬盘
  }
  // 3 关闭
  bos.close();
}
```



```JAVA
package com.JavaIO.ByteStream;

import java.io.*;

public class Buffer {
    public static void main(String[] args) throws Exception {
        FileInputStream fileInput =new FileInputStream("1.png");
        BufferedInputStream bufferedInputStream = new BufferedInputStream(fileInput);
        FileOutputStream fileOutput =new FileOutputStream("2.png");
        BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(fileOutput);
        int count = 0 ;
        byte[] b =new byte[1024];
        while ((count= bufferedInputStream.read(b,0,b.length-1))!=-1){
            bufferedOutputStream.write(b,0,count);
        }
        bufferedOutputStream.close();
        bufferedInputStream.close();
    }

}

```



### 3.4 装饰者模式

Java I/O 使用了装饰者模式来实现。以 InputStream 为例，

- InputStream 是抽象组件；
- FileInputStream 是 InputStream 的子类，属于具体组件，提供了字节流的输入操作；
- FilterInputStream 属于抽象装饰者，装饰者用于装饰组件，为组件提供额外的功能。例如 BufferedInputStream 为 FileInputStream 提供缓存的功能。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/9709694b-db05-4cce-8d2f-1c8b09f4d921.png" width="650px"> </div><br>

实例化一个具有缓存功能的字节流对象时，只需要在 FileInputStream 对象上再套一层 BufferedInputStream 对象即可。

```java
FileInputStream fileInputStream = new FileInputStream(filePath);
BufferedInputStream bufferedInputStream = new BufferedInputStream(fileInputStream);
```

DataInputStream 装饰者提供了对更多数据类型进行输入的操作，比如 int、double 等基本类型。

## 四、字符流 WR

由于许多字符都是由多个字节组成，如汉字个字节

```
public class python {
        public static void main(String[] args){
                String s = "我";
                byte[] bs=s.getBytes();
                System.out.println(bs.length);
        }
}

输出3
```

传统的字节流无法满足字符的读写操作的完整性；编码解码程序操作复杂；因此字符流出现

抽象**父类**

**Reader**

- `public int read(){}`
- `public int read(char[] c){}`
- `public int read(char[] b, int off, int len){}`

**Writer**

- `public void write(int n){}`
- `public void write(String str){}`
- `public void write(char[] c){}`

### 4.1 编码与解码

编码就是把字符转换为字节，而解码是把字节重新组合成字符。

如果编码和解码过程使用不同的编码方式那么就出现了乱码。

- GBK 编码中，中文字符占 2 个字节，英文字符占 1 个字节；
- UTF-8 编码中，中文字符占 3 个字节，英文字符占 1 个字节；
- UTF-16be 编码中，中文字符和英文字符都占 2 个字节。

UTF-16be 中的 be 指的是 Big Endian，也就是大端。相应地也有 UTF-16le，le 指的是 Little Endian，也就是小端。

Java 的内存编码使用双字节编码 UTF-16be，这不是指 Java 只支持这一种编码方式，而是说 char 这种类型使用 UTF-16be 进行编码。char 类型占 16 位，也就是两个字节，Java 使用这种双字节编码是为了让一个中文或者一个英文都能使用一个 char 来存储。

**String 的编码方式**

String 可以看成一个字符序列，可以指定一个编码方式将它编码为字节序列，也可以指定一个编码方式将一个字节序列解码为 String。

```java
String str1 = "中文";
byte[] bytes = str1.getBytes("UTF-8");
String str2 = new String(bytes, "UTF-8");
System.out.println(str2);
```

在调用无参数 getBytes() 方法时，默认的编码方式不是 UTF-16be。双字节编码的好处是可以使用一个 char 存储中文和英文，而将 String 转为 bytes[] 字节数组就不再需要这个好处，因此也就不再需要双字节编码。getBytes() 的默认编码方式与平台有关，一般为 UTF-8。

```java
byte[] bytes = str1.getBytes();
```

### 4.2 Reader 与 Writer

不管是磁盘还是网络传输，**最小的存储单元都是字节，而不是字符**。**但是在程序中操作的通常是字符形式的数据，因此需要提供对字符进行操作的方法。**

- InputStreamReader 实现从字节流解码成字符流；
- OutputStreamWriter 实现字符流编码成为字节流。

### 4.3 字符输入流

```java
package com.JavaIO.CharStream;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class File_Reader {
    public static void main(String[] args) throws IOException {
        FileReader fileReader =new FileReader("路径");
        int date = 0;
//        while((date = fileReader.read())!=-1){
//            System.out.println((char)date);
//        }


        char[] c = new char[100];
        while((date = fileReader.read(c))!=-1){
            System.out.println(new String(c,0,date));
        }
        fileReader.close();
    }
}
```

### 4.4 字符输出流

```java

import java.io.FileWriter;
import java.io.IOException;

public class File_Writer {
    public static void main(String[] args) throws IOException {
        FileWriter fileWriter =new FileWriter("路径一");

        fileWriter.write("我爱中国");
        fileWriter.close();
    }
}

```

### 4.5 文本复制

```java
package com.JavaIO.CharStream;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FileCopy {
    public static void main(String[] args) throws IOException {
        FileWriter fileWriter =new FileWriter("路径复制2");
        FileReader fileReader =new FileReader("路径一");

//        int date = 0;
//        while ((date=fileReader.read())!=-1){
//            fileWriter.write((char)date);
//        }
//        fileReader.close();
//        fileWriter.close();

        int date = 0;
        char[] c = new char[10];
        while((date=fileReader.read(c))!=-1){
            fileWriter.write(c,0,date);
        }
        fileReader.close();
        fileWriter.close();

    }
}

```

### **4.6**Buffer

实现文本复制

```java
package com.JavaIO.CharStream;

import java.io.*;

public class FileCopy {
    public static void main(String[] args) throws IOException {
        FileWriter fileWriter =new FileWriter("路径复制2");
        FileReader fileReader =new FileReader("路径一");
        BufferedReader br = new BufferedReader(fileReader);
        BufferedWriter bw = new BufferedWriter(fileWriter);

        String s = null;
        while((s=br.readLine())!=null){
            System.out.println(s);
            bw.write(s);
            bw.newLine();
            bw.flush();

        }
        br.close();
        bw.close();

    }
}

```

“将文本写入字符输出流，缓冲各个字符，从而提供单个字符、数组和字符串的高效写入。 可以指定缓冲区的大小，或者接受默认的大小。在大多数情况下，默认值就足够大了。”这是BufferedWriter

api的前两句话，意思是：为了防止多次操作IO（操作IO很费cpu时间），提供了一个缓冲区，当缓冲区满的时候，再写入文件，从而提高效率。因此，如果缓冲区没有写满，那么就必须强制他输出到文件，即调用flush();

你问题是：**不调用flush或者close的话，内容只是写到了缓冲区里，并没有写入文件**。所以读取不到内容，程序结束后为什么文件里面又有内容呢？因为你在后面调用close()方法了，这个方法调用之前会自动调用flush()。如果你把bw.close(); 也去掉，那么文件里是没有内容的

### 4.7 打印流

**用于字符原样写入**

封装了`print() / println()` 方法 **支持写入后换行**

支持数据原样打印

```java
psvm(String[] args){
  // 1 创建打印流
  PrintWriter pw = new PrintWriter("..");
  // 2 打印
  pw.println(12);
  pw.println(true);
  pw.println(3.14);
  pw.println('a');
  // 3 关闭
  pw.close();
}
```

### 4.8 转换流

桥转换流 `InputStreamReader / OutputStreamWriter`

可将字节流转换为字符流

**可设置字符的编码方式**

```java
psvm(String[] args) throws Exception{
  // 1 创建InputStreamReader对象
  FileInputStream fis = new FisInputStream("..");
  InputStreamReader isr = new InputStreamReader(fis, "utf-8");
  // 2 读取文件
  int data = 0;
  while((data = isr.read()) != -1){
    sout((char)data);
  }
  // 3 关闭
  isr.close();
}
```



```java
psvm(String[] args) throws Exception{
  // 1 创建OutputStreamReader对象
  FileOutputStream fos = new FisOutputStream("..");
  OutputStreamWRITER osw = new OutputStreamReader(fos, "utf-8");
  // 2 写入
  for(int i = 0; i < 10; i ++){
    osw.write("写入内容");
    osw.flush();
  }
  // 3 关闭
  osw.close();
}
```



## 五、对象操作

### 5.1 序列化

序列化就是将一个对象转换成字节序列，方便存储和传输。

- 序列化：ObjectOutputStream.**writeObject()**
- 反序列化：ObjectInputStream.**readObject()**

**特点**：

1. 增强了缓冲区功能
2. 增强了读写8种基本数据类型和字符串的功能
3. **需要借助于FileInputStream和FileOutputStream 作为参数**

****

**不会对静态变量进行序列化，因为序列化只是保存对象的状态，静态变量属于类的状态**。

#### Serializable

序列化的类需要实现 Serializable 接口，它只是一个标准，没有任何方法需要实现，但是如果不去实现它的话而进行序列化，会抛出异常。

```java

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;
public class Objectinput {
    public static void main(String[] args) throws IOException {
        Student s= new Student();
        String path = "object";
        ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(path));
        objectOutputStream.writeObject(s);
        objectOutputStream.close();
    }
}




package com.JavaIO.ObjectStream;

import java.io.Serializable;

public class Student implements Serializable {
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

#### transient

transient 关键字可以使一些属性不会被序列化。

ArrayList 中存储数据的数组 elementData 是用 transient 修饰的，因为这个数组是动态扩展的，并不是所有的空间都被使用，因此就不需要所有的内容都被序列化。通过重写序列化和反序列化方法，使得可以只序列化数组中有内容的那部分数据。

```java
private transient Object[] elementData;
```

**注意事项：**

1. 某个类要想序列化必须实现Serializable接口
2. 序列化类中对象属性要求实现Serializable接口
3. 序列化版本号ID，保证序列化的类和反序列化的类是同一个类
4. 使用transient修饰属性，这个属性就不能序列化
5. 静态属性不能序列化
6. 序列化多个对象，可以借助集合来实现

### 5.2 反序列化

```java
package com.JavaIO.ObjectStream;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.ObjectInputStream;

public class Object_Input {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream("Object"));
        Student student = (Student) objectInputStream.readObject();
        System.out.println(student.name);
        objectInputStream.close();
    }
}

```



## 六、网络操作

Java 中的网络支持：

- InetAddress：用于表示网络上的硬件资源，即 IP 地址；
- URL：统一资源定位符；
- Sockets：使用 TCP 协议实现网络通信；
- Datagram：使用 UDP 协议实现网络通信。

### InetAddress

没有公有的构造函数，只能通过静态方法来创建实例。

```java
InetAddress.getByName(String host);
InetAddress.getByAddress(byte[] address);
```

### URL

可以直接从 URL 中读取字节流数据。

```java
public static void main(String[] args) throws IOException {

    URL url = new URL("http://www.baidu.com");

    /* 字节流 */
    InputStream is = url.openStream();

    /* 字符流 */
    InputStreamReader isr = new InputStreamReader(is, "utf-8");

    /* 提供缓存功能 */
    BufferedReader br = new BufferedReader(isr);

    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }

    br.close();
}
```

### Sockets

- ServerSocket：服务器端类
- Socket：客户端类
- 服务器和客户端通过 InputStream 和 OutputStream 进行输入输出。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/1e6affc4-18e5-4596-96ef-fb84c63bf88a.png" width="550px"> </div><br>

### Datagram

- DatagramSocket：通信类
- DatagramPacket：数据包类

## 七、NIO

新的输入/输出 (NIO) 库是在 JDK 1.4 中引入的，弥补了原来的 I/O 的不足，提供了高速的、面向块的 I/O。

### 流与块

I/O 与 NIO 最重要的区别是数据打包和传输的方式，I/O 以流的方式处理数据，而 NIO 以块的方式处理数据。

面向流的 I/O 一次处理一个字节数据：一个输入流产生一个字节数据，一个输出流消费一个字节数据。为流式数据创建过滤器非常容易，链接几个过滤器，以便每个过滤器只负责复杂处理机制的一部分。不利的一面是，面向流的 I/O 通常相当慢。

面向块的 I/O 一次处理一个数据块，按块处理数据比按流处理数据要快得多。但是面向块的 I/O 缺少一些面向流的 I/O 所具有的优雅性和简单性。

I/O 包和 NIO 已经很好地集成了，java.io.\* 已经以 NIO 为基础重新实现了，所以现在它可以利用 NIO 的一些特性。例如，java.io.\* 包中的一些类包含以块的形式读写数据的方法，这使得即使在面向流的系统中，处理速度也会更快。

### 通道与缓冲区

#### 1. 通道

通道 Channel 是对原 I/O 包中的流的模拟，可以通过它读取和写入数据。

通道与流的不同之处在于，流只能在一个方向上移动(一个流必须是 InputStream 或者 OutputStream 的子类)，而通道是双向的，可以用于读、写或者同时用于读写。

通道包括以下类型：

- FileChannel：从文件中读写数据；
- DatagramChannel：通过 UDP 读写网络中数据；
- SocketChannel：通过 TCP 读写网络中数据；
- ServerSocketChannel：可以监听新进来的 TCP 连接，对每一个新进来的连接都会创建一个 SocketChannel。

#### 2. 缓冲区

发送给一个通道的所有数据都必须首先放到缓冲区中，同样地，从通道中读取的任何数据都要先读到缓冲区中。也就是说，不会直接对通道进行读写数据，而是要先经过缓冲区。

缓冲区实质上是一个数组，但它不仅仅是一个数组。缓冲区提供了对数据的结构化访问，而且还可以跟踪系统的读/写进程。

缓冲区包括以下类型：

- ByteBuffer
- CharBuffer
- ShortBuffer
- IntBuffer
- LongBuffer
- FloatBuffer
- DoubleBuffer

### 缓冲区状态变量

- capacity：最大容量；
- position：当前已经读写的字节数；
- limit：还可以读写的字节数。

状态变量的改变过程举例：

① 新建一个大小为 8 个字节的缓冲区，此时 position 为 0，而 limit = capacity = 8。capacity 变量不会改变，下面的讨论会忽略它。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/1bea398f-17a7-4f67-a90b-9e2d243eaa9a.png"/> </div><br>

② 从输入通道中读取 5 个字节数据写入缓冲区中，此时 position 为 5，limit 保持不变。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/80804f52-8815-4096-b506-48eef3eed5c6.png"/> </div><br>

③ 在将缓冲区的数据写到输出通道之前，需要先调用 flip() 方法，这个方法将 limit 设置为当前 position，并将 position 设置为 0。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/952e06bd-5a65-4cab-82e4-dd1536462f38.png"/> </div><br>

④ 从缓冲区中取 4 个字节到输出缓冲中，此时 position 设为 4。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/b5bdcbe2-b958-4aef-9151-6ad963cb28b4.png"/> </div><br>

⑤ 最后需要调用 clear() 方法来清空缓冲区，此时 position 和 limit 都被设置为最初位置。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/67bf5487-c45d-49b6-b9c0-a058d8c68902.png"/> </div><br>

### 文件 NIO 实例

以下展示了使用 NIO 快速复制文件的实例：

```java
public static void fastCopy(String src, String dist) throws IOException {

    /* 获得源文件的输入字节流 */
    FileInputStream fin = new FileInputStream(src);

    /* 获取输入字节流的文件通道 */
    FileChannel fcin = fin.getChannel();

    /* 获取目标文件的输出字节流 */
    FileOutputStream fout = new FileOutputStream(dist);

    /* 获取输出字节流的文件通道 */
    FileChannel fcout = fout.getChannel();

    /* 为缓冲区分配 1024 个字节 */
    ByteBuffer buffer = ByteBuffer.allocateDirect(1024);

    while (true) {

        /* 从输入通道中读取数据到缓冲区中 */
        int r = fcin.read(buffer);

        /* read() 返回 -1 表示 EOF */
        if (r == -1) {
            break;
        }

        /* 切换读写 */
        buffer.flip();

        /* 把缓冲区的内容写入输出文件中 */
        fcout.write(buffer);

        /* 清空缓冲区 */
        buffer.clear();
    }
}
```

### 选择器

NIO 常常被叫做非阻塞 IO，主要是因为 NIO 在网络通信中的非阻塞特性被广泛使用。

NIO 实现了 IO 多路复用中的 Reactor 模型，一个线程 Thread 使用一个选择器 Selector 通过轮询的方式去监听多个通道 Channel 上的事件，从而让一个线程就可以处理多个事件。

通过配置监听的通道 Channel 为非阻塞，那么当 Channel 上的 IO 事件还未到达时，就不会进入阻塞状态一直等待，而是继续轮询其它 Channel，找到 IO 事件已经到达的 Channel 执行。

因为创建和切换线程的开销很大，因此使用一个线程来处理多个事件而不是一个线程处理一个事件，对于 IO 密集型的应用具有很好地性能。

应该注意的是，只有套接字 Channel 才能配置为非阻塞，而 FileChannel 不能，为 FileChannel 配置非阻塞也没有意义。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/093f9e57-429c-413a-83ee-c689ba596cef.png" width="350px"> </div><br>

#### 1. 创建选择器

```java
Selector selector = Selector.open();
```

#### 2. 将通道注册到选择器上

```java
ServerSocketChannel ssChannel = ServerSocketChannel.open();
ssChannel.configureBlocking(false);
ssChannel.register(selector, SelectionKey.OP_ACCEPT);
```

通道必须配置为非阻塞模式，否则使用选择器就没有任何意义了，因为如果通道在某个事件上被阻塞，那么服务器就不能响应其它事件，必须等待这个事件处理完毕才能去处理其它事件，显然这和选择器的作用背道而驰。

在将通道注册到选择器上时，还需要指定要注册的具体事件，主要有以下几类：

- SelectionKey.OP_CONNECT
- SelectionKey.OP_ACCEPT
- SelectionKey.OP_READ
- SelectionKey.OP_WRITE

它们在 SelectionKey 的定义如下：

```java
public static final int OP_READ = 1 << 0;
public static final int OP_WRITE = 1 << 2;
public static final int OP_CONNECT = 1 << 3;
public static final int OP_ACCEPT = 1 << 4;
```

可以看出每个事件可以被当成一个位域，从而组成事件集整数。例如：

```java
int interestSet = SelectionKey.OP_READ | SelectionKey.OP_WRITE;
```

#### 3. 监听事件

```java
int num = selector.select();
```

使用 select() 来监听到达的事件，它会一直阻塞直到有至少一个事件到达。

#### 4. 获取到达的事件

```java
Set<SelectionKey> keys = selector.selectedKeys();
Iterator<SelectionKey> keyIterator = keys.iterator();
while (keyIterator.hasNext()) {
    SelectionKey key = keyIterator.next();
    if (key.isAcceptable()) {
        // ...
    } else if (key.isReadable()) {
        // ...
    }
    keyIterator.remove();
}
```

#### 5. 事件循环

因为一次 select() 调用不能处理完所有的事件，并且服务器端有可能需要一直监听事件，因此服务器端处理事件的代码一般会放在一个死循环内。

```java
while (true) {
    int num = selector.select();
    Set<SelectionKey> keys = selector.selectedKeys();
    Iterator<SelectionKey> keyIterator = keys.iterator();
    while (keyIterator.hasNext()) {
        SelectionKey key = keyIterator.next();
        if (key.isAcceptable()) {
            // ...
        } else if (key.isReadable()) {
            // ...
        }
        keyIterator.remove();
    }
}
```

### 套接字 NIO 实例

```java
public class NIOServer {

    public static void main(String[] args) throws IOException {

        Selector selector = Selector.open();

        ServerSocketChannel ssChannel = ServerSocketChannel.open();
        ssChannel.configureBlocking(false);
        ssChannel.register(selector, SelectionKey.OP_ACCEPT);

        ServerSocket serverSocket = ssChannel.socket();
        InetSocketAddress address = new InetSocketAddress("127.0.0.1", 8888);
        serverSocket.bind(address);

        while (true) {

            selector.select();
            Set<SelectionKey> keys = selector.selectedKeys();
            Iterator<SelectionKey> keyIterator = keys.iterator();

            while (keyIterator.hasNext()) {

                SelectionKey key = keyIterator.next();

                if (key.isAcceptable()) {

                    ServerSocketChannel ssChannel1 = (ServerSocketChannel) key.channel();

                    // 服务器会为每个新连接创建一个 SocketChannel
                    SocketChannel sChannel = ssChannel1.accept();
                    sChannel.configureBlocking(false);

                    // 这个新连接主要用于从客户端读取数据
                    sChannel.register(selector, SelectionKey.OP_READ);

                } else if (key.isReadable()) {

                    SocketChannel sChannel = (SocketChannel) key.channel();
                    System.out.println(readDataFromSocketChannel(sChannel));
                    sChannel.close();
                }

                keyIterator.remove();
            }
        }
    }

    private static String readDataFromSocketChannel(SocketChannel sChannel) throws IOException {

        ByteBuffer buffer = ByteBuffer.allocate(1024);
        StringBuilder data = new StringBuilder();

        while (true) {

            buffer.clear();
            int n = sChannel.read(buffer);
            if (n == -1) {
                break;
            }
            buffer.flip();
            int limit = buffer.limit();
            char[] dst = new char[limit];
            for (int i = 0; i < limit; i++) {
                dst[i] = (char) buffer.get(i);
            }
            data.append(dst);
            buffer.clear();
        }
        return data.toString();
    }
}
```

```java
public class NIOClient {

    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("127.0.0.1", 8888);
        OutputStream out = socket.getOutputStream();
        String s = "hello world";
        out.write(s.getBytes());
        out.close();
    }
}
```

### 内存映射文件

内存映射文件 I/O 是一种读和写文件数据的方法，它可以比常规的基于流或者基于通道的 I/O 快得多。

向内存映射文件写入可能是危险的，只是改变数组的单个元素这样的简单操作，就可能会直接修改磁盘上的文件。修改数据与将数据保存到磁盘是没有分开的。

下面代码行将文件的前 1024 个字节映射到内存中，map() 方法返回一个 MappedByteBuffer，它是 ByteBuffer 的子类。因此，可以像使用其他任何 ByteBuffer 一样使用新映射的缓冲区，操作系统会在需要时负责执行映射。

```java
MappedByteBuffer mbb = fc.map(FileChannel.MapMode.READ_WRITE, 0, 1024);
```

### 对比

NIO 与普通 I/O 的区别主要有以下两点：

- NIO 是非阻塞的；
- NIO 面向块，I/O 面向流。

## 八、参考资料

- Eckel B, 埃克尔, 昊鹏, 等. Java 编程思想 [M]. 机械工业出版社, 2002.
- [IBM: NIO 入门](https://www.ibm.com/developerworks/cn/education/java/j-nio/j-nio.html)
- [Java NIO Tutorial](http://tutorials.jenkov.com/java-nio/index.html)
- [Java NIO 浅析](https://tech.meituan.com/nio.html)
- [IBM: 深入分析 Java I/O 的工作机制](https://www.ibm.com/developerworks/cn/java/j-lo-javaio/index.html)
- [IBM: 深入分析 Java 中的中文编码问题](https://www.ibm.com/developerworks/cn/java/j-lo-chinesecoding/index.html)
- [IBM: Java 序列化的高级认识](https://www.ibm.com/developerworks/cn/java/j-lo-serial/index.html)
- [NIO 与传统 IO 的区别](http://blog.csdn.net/shimiso/article/details/24990499)
- [Decorator Design Pattern](http://stg-tud.github.io/sedc/Lecture/ws13-14/5.3-Decorator.html#mode=document)
- [Socket Multicast](http://labojava.blogspot.com/2012/12/socket-multicast.html)
