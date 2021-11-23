# Maven 介绍

## 概念

### 定义

Maven是一个项目管理工具，它包含了：

1. 一个项目对象模型 (POM：Project Object Model)
2. 一组标准集合
3. 一个项目生命周期(Project Lifecycle)
4. 一个依赖管理系统(Dependency Management System)
5. 用来运行定义在生命周期阶段(phase)中插件(plugin)目标(goal)的逻辑。

### 作用

可以用更通俗的方式来说明。我们知道，项目开发不仅仅是写写代码而已，期间会伴随着各种必不可少的事情要做，下面列举几个感受一下：

1. 我们需要引用**各种jar包**，尤其是比较大的工程，引用的jar包往往有几十个乃至上百个， 每用到一种jar包，都需要手动引入工程目录，而且经常遇到各种让人抓狂的jar包冲突，版本冲突。
2. 我们辛辛苦苦写好了Java文件，可是只懂0和1的白痴电脑却完全读不懂，需要将它**编译**成二进制字节码。好歹现在这项工作可以由各种集成开发工具帮我们完成，Eclipse、IDEA等都可以将代码即时编译。
3. 世界上没有不存在bug的代码，计算机喜欢bug就和人们总是喜欢美女帅哥一样。为了追求美为了减少bug，因此写完了代码，我们还要写一些**单元测试**，然后一个个的运行来检验代码质量。
4. 再优雅的代码也是要出来卖的。我们后面还需要把代码与各种**配置文件**、资源整合到一起，定型打包，如果是web项目，还需要将之发布到服务器，供人蹂躏。

试想，如果现在有一种工具，可以把你从上面的繁琐工作中解放出来，能帮你**构建工程，管理jar包，编译代码，还能帮你自动运行单元测试，打包，生成报表，甚至能帮你部署项目，生成Web站点，**你会心动吗？Maven就可以解决上面所提到的这些问题。

## 经典作用

### 依赖管理

Maven的一个核心特性就是依赖管理。

#### 问题提出

当我们涉及到多模块的项目（包含成百个模块或者子项目），管理依赖就变成一项困难的任务。

Maven展示出了它对处理这种情形的高度控制。

#### 问题解决

传统的WEB项目中，我们必须将工程所依赖的jar包复制到工程中，导致了工程的变得很大。那么maven工程是如何使得工程变得很少呢？

分析如下：

![image-20211109110011908](笔记图库/image-20211109110011908.png)



通过分析发现：

**maven工程中不直接将jar包导入到工程中，而是通过在pom.xml文件中添加所需jar包的坐标，**

这样就很好的避免了jar直接引入进来，在需要用到jar包的时候，只要查找pom.xml文件，再通过pom.xml文件中的坐标，到一个专门用于”存放jar包的仓库”(maven仓库)中根据坐标从而找到这些jar包，再把这些jar包拿去运行。

#### 问题再来提出

那么问题来了

第一：”存放jar包的仓库”长什么样？

第二：通过读取pom.xml 文件中的坐标，再到仓库中找到jar包，会不会很慢？从而导致这种方式不可行！

第一个问题：存放jar包的仓库长什么样，这一点我们后期会分析仓库的分类，也会带大家去看我们的本地的仓库长什么样。

第二个问题：**通过pom.xml文件配置要引入的jar包的坐标**，再读取坐标并到仓库中加载jar包，这样我们就可以直接使用jar包了，为了解决这个过程中速度慢的问题，maven中也有索引的概念，通过建立索引，可以大大提高加载jar包的速度，使得我们认为jar包基本跟放在本地的工程文件中再读取出来的速度是一样的。

### 项目一键构建

我们的项目，往往都要经历编译、测试、运行、打包、安装 ，部署等一系列过程。

#### 构建

指的是项目从编译、测试、运行、打包、安装 ，部署整个过程都交给maven进行管理，这个过程称为构建。

一键构建
指的是整个构建过程，使用maven一个命令可以轻松完成整个工作

![image-20211109110424333](笔记图库/image-20211109110424333.png)

# Maven的使用

## 安装

### 下载

为了使用Maven管理工具，我们首先要到官网去下载它的安装软件。通过百度搜索“Maven“

点击Download链接，就可以直接进入到Maven软件的下载页面

### 安装

Maven下载后，将Maven解压到一个没有中文没有空格的路径下，比如D:\software\maven下面。
解压后目录结构如下：

- bin: 存放了maven的**命令**，比如我们前面用到的mvn tomcat:run
- boot: 存放了一些maven本身的**引导程序**，如**类加载器**等
- conf: 存放了maven的一些配置文件，如**setting.xm**l文件
- lib: 存放了maven本身运行所需的**一些jar包**

至此我们的maven软件就可以使用了，前提是你的电脑上之前已经安装并配置好了JDK。

### 配置

配置 MAVEN_HOME ，变量值就是你的maven安装 的路径（bin目录之前一级目录）

![image-20211109110955803](笔记图库/image-20211109110955803.png)

上面配置了我们的Maven软件，注意这个目录就是之前你解压maven的压缩文件包在的的目录，最好不要有中文和空格。
再次检查JDK的安装目录，如下图：

![image-20211109111031759](笔记图库/image-20211109111031759.png)

### 测试

通过 **mvn -v** 命令检查 maven是否安装成功，看到maven的版本为3.5.2及java版本为1.8即为安装成功。

管理员开cmd命令，输入mvn –v命令，如下图：

![image-20211109111108769](笔记图库/image-20211109111108769.png)

## Maven 仓库

### 仓库分类

maven的工作需要从仓库下载一些jar包

- 本地的项目A、项目B等都会通过maven软件从**远程仓库**（可以理解为互联网上的仓库）下载jar包并存在**本地仓库**，本地仓库 就是本地文件夹
- 当第二次需要此jar包时则不再从远程仓库下载，因为本地仓库已经存在了，可以将本地仓库理解为缓存，有了本地仓库就不用每次从远程仓库下载了

#### 本地仓库 

用来存储从**远程仓库或中央仓库下载的插件和jar包**

项目使用一些插件或jar包，优先从本地仓库查找

默认本地仓库位置在 ${user.dir}/.m2/repository，${user.dir}表示windows用户目录。

#### 远程仓库

如果本地需要插件或者jar包，本地仓库没有，默认去远程仓库下载。

远程仓库可以在互联网内也可以在局域网内。

#### 中央仓库 

在maven软件中内置一个远程仓库地址http://repo1.maven.org/maven2 ，它是中央仓库，服务于整个互联网，它是由Maven团队自己维护，里面存储了非常全的jar包，它包含了世界上大部分流行的开源项目构件。

### 本地仓库配置

1. 本课程是在无网的状态下学习，需要配置老师提供的本地仓库，将 “repository.rar”解压至自己的电脑上

2. 我们解压在D:\repository目录下（可以放在没有中文及空格的目录下）。

3. 在MAVE_HOME/conf/settings.xml文件中配置本地仓库位置（maven的安装目录下）：

   ![image-20211109140607779](笔记图库/image-20211109140607779.png)

4. 打开settings.xml文件，配置如下

   ![image-20211109140633771](笔记图库/image-20211109140633771.png)

### 全局setting与用户setting

maven仓库地址、私服等配置信息需要在setting.xml文件中配置，分为全局配置和用户配置。

在maven安装目录下的有 conf/setting.xml文件，此setting.xml文件用于maven的所有project项目，它作为maven的全局配置。

如需要个性配置则需要在用户配置中设置，用户配置的setting.xml文件默认的位置在：${user.dir} /.m2/settings.xml目录中,${user.dir} 指windows 中的用户目录。

maven会先找用户配置，如果找到则以用户配置文件为准，否则使用全局配置文件。

![image-20211109140909367](笔记图库/image-20211109140909367.png)

## Maven工程的认识

### Maven工程的目录结构

![image-20211109141919582](笔记图库/image-20211109141919582.png)

作为一个maven工程，它的**src目录和pom.xml**是必备的。
进入src目录后，我们发现它里面的目录结构如下：

1. src/main/java —— 存放项目的.java文件
2. src/main/resources —— 存放项目资源文件，如spring, hibernate配置文件
3. src/test/java —— 存放所有单元测试.java文件，如JUnit测试类
4. src/test/resources —— 测试资源文件
5. target —— 项目输出位置，编译后的class文件会输出到此目录
6. **pom.xml**——maven项目核心配置文件

注意：如果是普通的java项目，那么就没有webapp目录。

### Maven工程的运行

进入maven工程目录（当前目录有pom.xml文件），运行tomcat:run命令

根据上边的提示信息，通过浏览器访问：http://localhost:8080/maven-helloworld/

![image-20211109142023528](笔记图库/image-20211109142023528.png)

# Maven常用命令

我们可以在cmd中通过一系列的maven命令来对我们的maven-helloworld工程进行编译、测试、运行、打包、安装、部署。

## compile

compile是maven工程的编译命令，作用是将src/main/java下的文件编译为class文件输出到target目录下。
cmd进入命令状态，执行**mvn compile**

查看 target目录，class文件已生成，编译完成。

## test

test是maven工程的测试命令 **mvn test**，会执行src/test/java下的单元测试类。

cmd执行mvn test执行src/test/java下单元测试类

## clean

clean是maven工程的清理命令，执行 clean会删除target目录及内容。

## package

package是maven工程的打包命令，对于java工程执行package打成jar包，对于web工程打成war包。

## install

install是maven工程的安装命令，**执行install将maven打成jar包或war包发布到本地仓库。**

**从运行结果中，可以看出：**
**当后面的命令执行时，前面的操作过程也都会自动执行，**

## Maven指令的生命周期

maven对项目构建过程分为三套相互独立的生命周期，请注意这里说的是“三套”，而且“相互独立”，这三套生命周期分别是：

1. Clean Lifecycle 在进行真正的构建之前进行一些清理工作。
2. Default Lifecycle 构建的核心部分，编译，测试，打包，部署等等。
3. Site Lifecycle 生成项目报告，站点，发布站点。

## maven的概念模型

Maven包含了

1. 一个项目对象模型 (Project Object Model)，一组标准集合，
2. 一个项目生命周期(Project Lifecycle)，
3. 一个依赖管理系统(Dependency Management System)，
4. 用来运行定义在生命周期阶段(phase)中插件(plugin)目标(goal)的逻辑。

### 项目对象模型 

(Project Object Model)
一个maven工程都有一个**pom.xml**文件，通过pom.xml文件定义项目的坐标、项目依赖、项目信息、插件目标等。

### 依赖管理系统

(Dependency Management System)

通过maven的依赖管理对项目所依赖的jar 包进行统一管理。
比如：项目依赖junit4.9，通过在pom.xml中定义junit4.9的依赖即使用junit4.9，如下所示是junit4.9的依赖定义：

```xml
<!-- 依赖关系 -->
<dependencies>
<!-- 此项目运行使用junit，所以此项目依赖junit -->
<dependency>
<!-- junit的项目名称 -->
<groupId>junit</groupId>
<!-- junit的模块名称 -->
<artifactId>junit</artifactId>
<!-- junit版本 -->
<version>4.9</version>
<!-- 依赖范围：单元测试时使用junit -->
<scope>test</scope>
</dependency>
```

###  一个项目生命周期

(Project Lifecycle)
使用maven完成项目的构建，项目构建包括：**清理、编译、测试、部署**等过程，maven将这些过程规范为一个生命周期，如下所示是生命周期的各各阶段：
maven通过执行一些简单命令即可实现上边生命周期的各各过程，比如执行mvn compile执行编译、执行mvn clean执行清理。

### 一组标准集合

maven将整个项目管理过程定义一组标准，比如：通过maven构建工程有标准的目录结构，有标准的生命周期阶段、依赖管理有标准的坐标定义等。

### 插件(plugin)目标(goal)

maven 管理项目生命周期过程都是基于插件完成的。

## idea开发maven项目

在实战的环境中，我们都会使用流行的工具来开发项目。

具体间Maven**基础讲义**

仅记录重点技术

### 依赖范围

A依赖B，需要在A的pom.xml文件中添加B的坐标，添加坐标时需要指定依赖范围，依赖范围包括：

- compile：编译范围，指A在编译时依赖B，此范围为默认依赖范围。
  编译范围的依赖会用在编译、测试、运行，由于运行时需要所以编译范围的依赖会被打包。
-  provided：provided依赖只有在当JDK或者一个容器已提供该依赖之后才使用
   provided依赖在编译和测试时需要，在运行时不需要，比如：servlet api被tomcat容器提供。
-  runtime：runtime依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如：jdbc的驱动包。由于运行时需要所以runtime范围的依赖会被打包。
- test：test范围依赖 在编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用，比如：junit。由于运行时不需要所以test范围依赖不会被打包。
- system：system范围依赖与provided类似，但是你必须显式的提供一个对于本地系统中JAR文件的路径，需要指定systemPath磁盘路径，system依赖不推荐使用。

![image-20211109145919365](笔记图库/image-20211109145919365.png)

在maven-web工程中测试各各scop。

测试总结：
默认引入 的jar包 ------- compile 【默认范围 可以不写】（编译、测试、运行 都有效 ）

**servlet-api 、jsp-api ------- provided （编译、测试 有效， 运行时无效 防止和tomcat下jar冲突）**

jdbc驱动jar包 ---- runtime （测试、运行 有效 ）

junit ----- test （测试有效）

依赖范围由强到弱的顺序是：compile>provided>runtime>test



