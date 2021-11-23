# Java网络编程

## 1. 概述

### 1.1软件结构

C/S结构：CLient/Server结构，是指客户端和服务端的结构。QQ

B/S结构：Browser/Server结构，是指浏览器和服务端结构。火狐

Javaweb: 网页编程

网络编程：TCP/IP

### 1.2 网络通讯的要素

1. 网络编程的两个主要问题：

   - 如何准确定位到网络上的一台或多台主机
   - 找到主机之后如何进行通信

2. 网络编程的要素
   IP+**端口** 可以定位到某计算机的 **具体应用**；

   使用协议：TCP/IP

3. 万物皆对象

**ip地址：**唯一定位一台计算机

1. 127.0.0.1：本机localhost
2. 公网（互联网）—私网（局域网）

- IPV4：四个字节，32个二进制；ABCD类地址
- 192.168.XX.XX,给组织内使用

**端口号：**IP+**端口** 可以定位到某计算机的 **具体应用**；

- 当我们使用网络软件时，那么操作系统就会为网络软件随机分配一个端口号，也可以指定
- 范围：0~65535
- 不可重复

## 2 TCP通讯程序

### 2.1概述

两端通信的步骤：

1. 服务器开，等待连接

2. 客户端主动连接服务器，连接成功进行通信

   

在Java中，提供了两个类用于实现TCP通信程序：

1. 客户端： java.net.Socket 类表示。创建Socket 对象，向服务端发出连接请求，服务端响应请求，两者建
立连接开始通信。
2. 服务端： java.net.ServerSocket 类表示。创建ServerSocket 对象，相当于开启一个服务，并等待客户端
的连接。

```java
package com.itheima.demo01.TCP;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

/*
    TCP通信的客户端:向服务器发送连接请求,给服务器发送数据,读取服务器回写的数据
    表示客户端的类:
        java.net.Socket:此类实现客户端套接字（也可以就叫“套接字”）。套接字是两台机器间通信的端点。
        套接字:包含了IP地址和端口号的网络单位

    构造方法:
        Socket(String host, int port) 创建一个流套接字并将其连接到指定主机上的指定端口号。
        参数:
            String host:服务器主机的名称/服务器的IP地址
            int port:服务器的端口号

    成员方法:
        OutputStream getOutputStream() 返回此套接字的输出流。
        InputStream getInputStream() 返回此套接字的输入流。
        void close() 关闭此套接字。

    实现步骤:
        1.创建一个客户端对象Socket,构造方法绑定服务器的IP地址和端口号
        2.使用Socket对象中的方法getOutputStream()获取网络字节输出流OutputStream对象
        3.使用网络字节输出流OutputStream对象中的方法write,给服务器发送数据
        4.使用Socket对象中的方法getInputStream()获取网络字节输入流InputStream对象
        5.使用网络字节输入流InputStream对象中的方法read,读取服务器回写的数据
        6.释放资源(Socket)
     注意:
        1.客户端和服务器端进行交互,必须使用Socket中提供的网络流,不能使用自己创建的流对象
        2.当我们创建客户端对象Socket的时候,就会去请求服务器和服务器经过3次握手建立连接通路
            这时如果服务器没有启动,那么就会抛出异常ConnectException: Connection refused: connect
            如果服务器已经启动,那么就可以进行交互了
 */
public class TCPClient {
    public static void main(String[] args) throws IOException {
        //1.创建一个客户端对象Socket,构造方法绑定服务器的IP地址和端口号
        Socket socket = new Socket("127.0.0.1",8888);
        //2.使用Socket对象中的方法getOutputStream()获取网络字节输出流OutputStream对象
        OutputStream os = socket.getOutputStream();
        //3.使用网络字节输出流OutputStream对象中的方法write,给服务器发送数据
        os.write("你好服务器".getBytes());//字符串对象.getBytes() 将字符串对象转化成字符数组

        //4.使用Socket对象中的方法getInputStream()获取网络字节输入流InputStream对象
        InputStream is = socket.getInputStream();

        //5.使用网络字节输入流InputStream对象中的方法read,读取服务器回写的数据
        byte[] bytes = new byte[1024];
        int len = is.read(bytes);
        System.out.println(new String(bytes,0,len));

        //6.释放资源(Socket)
        socket.close();

    }

}

```

### 2.2 文件传输

客户端对象:Socket s = new Socket("127.0.0.1",6666);

服务端对象:ServerSocket ss = new ServerSocket(6666);

#### 2.2.1客户端

```java
package Net.demo2;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

public class TCPCilent {
    public static void main(String[] args) throws IOException {
        //1.创建一个本地字节输入流FileInputStream对象,构造方法中绑定要读取的数据源
        FileInputStream fis = new FileInputStream("1.png");
        //2.创建一个客户端Socket对象,构造方法中绑定服务器的IP地址和端口号
        Socket socket = new Socket("127.0.0.1", 8888);
        //3.使用Socket中的方法getOutputStream,获取网络字节输出流OutputStream对象
        OutputStream os = socket.getOutputStream();
        //4.使用本地字节输入流FileInputStream对象中的方法read,读取本地文件
        int len = 0;
        byte[] bytes = new byte[1024];
        while ((len = fis.read(bytes)) != -1) {
            //5.使用网络字节输出流OutputStream对象中的方法write,把读取到的文件上传到服务器
            os.write(bytes, 0, len);
        }

        /*
            解决:上传完文件,给服务器写一个结束标记
            void shutdownOutput() 禁用此套接字的输出流。
            对于 TCP 套接字，任何以前写入的数据都将被发送，并且后跟 TCP 的正常连接终止序列。
         */
        socket.shutdownOutput();//在bytes 数据末尾添加结束标志

        //6.使用Socket中的方法getInputStream,获取网络字节输入流InputStream对象
        InputStream is = socket.getInputStream();

        System.out.println("333333333333333333333");

        //7.使用网络字节输入流InputStream对象中的方法read读取服务回写的数据
        while ((len = is.read(bytes)) != -1) {
            System.out.println(new String(bytes, 0, len));
        }

        System.out.println("444444444444444444  while死循环打印不到");

        //8.释放资源(FileInput,Socket)
        fis.close();
        socket.close();
    }
}

```
#### 2.2.2 服务器端

```java

package Net.demo2;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPSever {
    public static void main(String[] args) throws IOException {
        //1.创建一个服务器ServerSocket对象,和系统要指定的端口号
        ServerSocket server = new ServerSocket(8888);
        //2.使用ServerSocket对象中的方法accept,获取到请求的客户端Socket对象
        Socket socket = server.accept();
        //3.使用Socket对象中的方法getInputStream,获取到网络字节输入流InputStream对象
        InputStream is = socket.getInputStream();
        //4.判断d:\\upload文件夹是否存在,不存在则创建
        File file =  new File("d:\\upload");
        if(!file.exists()){
            file.mkdirs();
        }


        //5.创建一个本地字节输出流FileOutputStream对象,构造方法中绑定要输出的目的地
        FileOutputStream fos = new FileOutputStream(file+"\\1.jpg");
        //6.使用网络字节输入流InputStream对象中的方法read,读取客户端上传的文件

        System.out.println("11111111111111111111");

        int len =0;
        byte[] bytes = new byte[1024];
        while((len = is.read(bytes))!=-1){
            //7.使用本地字节输出流FileOutputStream对象中的方法write,把读取到的文件保存到服务器的硬盘上
            fos.write(bytes,0,len);
        }

        System.out.println("22222222222222222222222  while死循环打印不到");

        //8.使用Socket对象中的方法getOutputStream,获取到网络字节输出流OutputStream对象
        //9.使用网络字节输出流OutputStream对象中的方法write,给客户端回写"上传成功"
        socket.getOutputStream().write("上传成功".getBytes());
        //10.释放资源(FileOutputStream,Socket,ServerSocket)
        fos.close();
        socket.close();
        server.close();
    }
}
```

### 2.3 文件传输优化

**服务器端**

```java
package Net.demo2;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Random;

public class TCPSever {
    public static void main(String[] args) throws IOException {
        //1.创建一个服务器ServerSocket对象,和系统要指定的端口号
        ServerSocket server = new ServerSocket(8888);
        //2.使用ServerSocket对象中的方法accept,获取到请求的客户端Socket对象
        while(true){ //server.accept()死循环
            Socket socket = server.accept();

            new Thread(new Runnable(){  //匿名内部类
                @Override
                public void run() {
                    try {
                        //3.使用Socket对象中的方法getInputStream,获取到网络字节输入流InputStream对象
                        InputStream is = socket.getInputStream();
                        //4.判断d:\\upload文件夹是否存在,不存在则创建
                        File file =  new File("d:\\upload");
                        if(!file.exists()){
                            file.mkdirs();
                        }
                        String fileName = "itcast"+System.currentTimeMillis()+new Random().nextInt(999)+".jpg";

                        //5.创建一个本地字节输出流FileOutputStream对象,构造方法中绑定要输出的目的地
                        FileOutputStream fos = new FileOutputStream(file+"\\"+fileName);
                        //6.使用网络字节输入流InputStream对象中的方法read,读取客户端上传的文件

                        int len =0;
                        byte[] bytes = new byte[1024];
                        while((len = is.read(bytes))!=-1){
                            //7.使用本地字节输出流FileOutputStream对象中的方法write,把读取到的文件保存到服务器的硬盘上
                            fos.write(bytes,0,len);
                        }

                        //8.使用Socket对象中的方法getOutputStream,获取到网络字节输出流OutputStream对象
                        //9.使用网络字节输出流OutputStream对象中的方法write,给客户端回写"上传成功"
                        socket.getOutputStream().write("上传成功".getBytes());
                        //10.释放资源(FileOutputStream,Socket,ServerSocket)
                        fos.close();
                        socket.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }).start();
        }
    }
}
```

### 2.4 BS 网页TCP

```java
package Net.DemoBS;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPSever {
    public static void main(String[] args) throws IOException {
        //1 获取服务器对象，指明端口
        ServerSocket server= new ServerSocket(8081);
        while (true){
          //网页中有图片的话启动多线程加载图片
            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        // 获取客户端的socket
                        Socket socket = server.accept();
                        InputStream inputStream = socket.getInputStream();
                        //后续需要读取一行，将inputStream转化为Read-Buffer
                        BufferedReader br = new BufferedReader(new InputStreamReader(inputStream));
                        //读取一行，并提取路径（第一行是路径信息）
                        String s = br.readLine();
                        String[] ars = s.split(" ");
                        String htmlpath = ars[1].substring(1);//http://127.0.0.1:8081/src/Net/web/index.html

                        //服务器读取本地文件并传输到客户端

                        FileInputStream fileInputStream = new FileInputStream(htmlpath);

                        OutputStream outputStream = socket.getOutputStream();
                        // 写入HTTP协议响应头,固定写法
                        outputStream.write("HTTP/1.1 200 OK\r\n".getBytes());
                        outputStream.write("Content-Type:text/html\r\n".getBytes());
                        // 必须要写入空行,否则浏览器不解析
                        outputStream.write("\r\n".getBytes());

                        int len =0;
                        byte[] bytes = new byte[1024];
                        while ((len=fileInputStream.read(bytes))!=-1){
                            outputStream.write(bytes,0,len);
                        }
                        //释放资源

                        socket.close();
                        fileInputStream.close();

                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }).start();
        }

    }
}
```

