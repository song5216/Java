# SQL 语法
## 绪论

**概念**

- DB：数据库，是长期储存在计算机内、有组织的、可共享的大量数据的集合。

- DBMS：数据库管理系统，是位于用户与操作系统之间的一层数据管理软件，用于科学地组织、存储和管理数据、高效地获取和维护数据。

- DBS：数据库系统，指在计算机系统中引入数据库后的系统，一般由数据库、数据库管理系统、应用系统、数据库管理员（DBA）构成。MYSQL

- SQL：结构化查询语言，用于与DBMS通信

**如何存储数据**

- 将数据放入表中，表放入库中
- 数据库有多个表，每个表有唯一标识
- 表具有一些特性，定义了表是如何存储的
- 表的列 定义为，类似于java属性
- 行类似于java中的对象

### MySQL

**DBMS共两类**

- 基于共享文件系统
- 基于客户机-服务器MySQL

#### **MySQL服务的启动和停止**

```
net stop mysql/net start mysql
```

#### **MySQL服务的登录和退出**

```
mysql -h localhost -P3306 -u root -p //-h 主机IP -u用户名 -P端口 -p密码
```

#### **MySQL常用命令**

```
show datasets;查看所有数据库
use test;打开指定库
show tables;查看其他库所有表
show tables from test;
select database(); 查看当前所在库
create table stuinfo(id int,name varchar(20));在库中创建一个表 （列名 列类型）
desc; 查看表结构
select version(); mysql内看版本
mysql --version 查看版本基础环境下
```

#### **MySQL规范**

- 不区分大小写
- 关键字大写，表名列名小写
- 每行命令后加分号；
- 注释

### SQL语言

Structured Query Language：结构化查询语言
其实就是定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。

#### SQL通用语法

	1. SQL 语句可以单行或多行书写，以分号结尾。
	2. 可使用空格和缩进来增强语句的可读性。
	3. MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。
	4. 3 种注释
		* 单行注释: -- 注释内容 或 # 注释内容(mysql 特有) 
		* 多行注释: /* 注释 */

****

#### SQL分类

	1) DDL(Data Definition Language)数据**定义**语言
		用来定义数据库对象：数据库，表，列等。关键字：create, drop,alter 等
	2) DQL(Data Query Language)数据**查询**语言
	 	用来查询数据库中表的记录(数据)。关键字：select, where 
	3) DCL(Data Control Language)数据控制语言(了解)
	 	用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等
	4) DML(Data Manipulation Language)数据**操作**语言
	    用来对数据库中表的数据进行增删改。关键字：insert, delete, update 等

#### DDL:操作数据库、表

	1. 操作数据库：CRUD
		1. C(Create):创建
			* 创建数据库：
				* create database 数据库名称;
			* 创建数据库，判断不存在，再创建：
				* create database if not exists 数据库名称;
			* 创建数据库，并指定字符集
				* create database 数据库名称 character set 字符集名;
	
			* 练习： 创建db4数据库，判断是否存在，并制定字符集为gbk
				* create database if not exists db4 character set gbk;
		2. R(Retrieve)：查询
			* 查询所有数据库的名称:
				* show databases;
			* 查询某个数据库的字符集:查询某个数据库的创建语句
				* show create database 数据库名称;
		3. U(Update):修改
			* 修改数据库的字符集
				* alter database 数据库名称 character set 字符集名称;
		4. D(Delete):删除
			* 删除数据库
				* drop database 数据库名称;
			* 判断数据库存在，存在再删除
				* drop database if exists 数据库名称;
		5. 使用数据库
			* 查询当前正在使用的数据库名称
				* select database();
			* 使用数据库
				* use 数据库名称;


	2. 操作表
		1. C(Create):创建
			1. 语法：
				create table 表名(
					列名1 数据类型1,
					列名2 数据类型2,
					....
					列名n 数据类型n
				);
				* 注意：最后一列，不需要加逗号（,）
				* 数据库类型：
					1. int：整数类型
						* age int,
					2. double:小数类型
						* score double(5,2)
					3. date:日期，只包含年月日，yyyy-MM-dd
					4. datetime:日期，包含年月日时分秒	 yyyy-MM-dd HH:mm:ss
					5. timestamp:时间错类型	包含年月日时分秒	 yyyy-MM-dd HH:mm:ss	
						* 如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值
	
					6. varchar：字符串
						* name varchar(20):姓名最大20个字符
						* zhangsan 8个字符  张三 2个字符


			* 创建表
				create table student(
					id int,
					name varchar(32),
					age int ,
					score double(4,1),
					birthday date,
					insert_time timestamp
				);
			* 复制表：
				* create table 表名 like 被复制的表名;	  	
		2. R(Retrieve)：查询
			* 查询某个数据库中所有的表名称
				* show tables;
			* 查询表结构
				* desc 表名;
		3. U(Update):修改
			1. 修改表名
				alter table 表名 rename to 新的表名;
			2. 修改表的字符集
				alter table 表名 character set 字符集名称;
			3. 添加一列
				alter table 表名 add 列名 数据类型;
			4. 修改列名称 类型
				alter table 表名 change 列名 新列别 新数据类型;
				alter table 表名 modify 列名 新数据类型;
			5. 删除列
				alter table 表名 drop 列名;
		4. D(Delete):删除
			* drop table 表名;
			* drop table  if exists 表名 ;

* 客户端图形化工具：SQLYog

#### DML：增删改表中数据

	1. 添加数据：
		* 语法：
			* insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);
		* 注意：
			1. 列名和值要一一对应。
			2. 如果表名后，不定义列名，则默认给所有列添加值
				insert into 表名 values(值1,值2,...值n);
			3. 除了数字类型，其他类型需要使用引号(单双都可以)引起来
	2. 删除数据：
		* 语法：
			* delete from 表名 [where 条件]
		* 注意：
			1. 如果不加条件，则删除表中所有记录。
			2. 如果要删除所有记录
				1. delete from 表名; -- 不推荐使用。有多少条记录就会执行多少次删除操作
				2. TRUNCATE TABLE 表名; -- 推荐使用，效率更高 先删除表，然后再创建一张一样的表。
	3. 修改数据：
		* 语法：
			* update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];
	
		* 注意：
			1. 如果不加任何条件，则会将表中所有记录全部修改。

#### DQL：查询表中的记录

	* select * from 表名;
	
	1. 语法：
		select
			字段列表
		from
			表名列表
		where
			条件列表
		group by
			分组字段
		having
			分组之后的条件
		order by
			排序
		limit
			分页限定


	2. 基础查询
		1. 多个字段的查询
			select 字段名1，字段名2... from 表名；
			* 注意：
				* 如果查询所有字段，则可以使用*来替代字段列表。
		2. 去除重复：
			* distinct
		3. 计算列
			* 一般可以使用四则运算计算一些列的值。（一般只会进行数值型的计算）
			* ifnull(表达式1,表达式2)：null参与的运算，计算结果都为null
				* 表达式1：哪个字段需要判断是否为null
				* 如果该字段为null后的替换值。
		4. 起别名：
			* as：as也可以省略


	3. 条件查询
		1. where子句后跟条件
		2. 运算符
			* > 、< 、<= 、>= 、= 、<>
			* BETWEEN...AND  
			* IN( 集合) 
			* LIKE：模糊查询
				* 占位符：
					* _:单个任意字符
					* %：多个任意字符
			* IS NULL  
			* and  或 &&
			* or  或 || 
			* not  或 !
			
				-- 查询年龄大于20岁
	
				SELECT * FROM student WHERE age > 20;
				
				SELECT * FROM student WHERE age >= 20;
				
				-- 查询年龄等于20岁
				SELECT * FROM student WHERE age = 20;
				
				-- 查询年龄不等于20岁
				SELECT * FROM student WHERE age != 20;
				SELECT * FROM student WHERE age <> 20;
				
				-- 查询年龄大于等于20 小于等于30
				
				SELECT * FROM student WHERE age >= 20 &&  age <=30;
				SELECT * FROM student WHERE age >= 20 AND  age <=30;
				SELECT * FROM student WHERE age BETWEEN 20 AND 30;
				
				-- 查询年龄22岁，18岁，25岁的信息
				SELECT * FROM student WHERE age = 22 OR age = 18 OR age = 25
				SELECT * FROM student WHERE age IN (22,18,25);
				
				-- 查询英语成绩为null
				SELECT * FROM student WHERE english = NULL; -- 不对的。null值不能使用 = （!=） 判断
				
				SELECT * FROM student WHERE english IS NULL;
				
				-- 查询英语成绩不为null
				SELECT * FROM student WHERE english  IS NOT NULL;
	
				-- 查询姓马的有哪些？ like
				SELECT * FROM student WHERE NAME LIKE '马%';
				-- 查询姓名第二个字是化的人
				
				SELECT * FROM student WHERE NAME LIKE "_化%";
				
				-- 查询姓名是3个字的人
				SELECT * FROM student WHERE NAME LIKE '___';
	
				-- 查询姓名中包含德的人
				SELECT * FROM student WHERE NAME LIKE '%德%';


## 一、基础

模式定义了数据如何存储、存储什么样的数据以及数据如何分解等信息，数据库和表都有模式。

主键的值不允许修改，也不允许复用（不能将已经删除的主键值赋给新数据行的主键）。

SQL（Structured Query Language)，标准 SQL 由 ANSI 标准委员会管理，从而称为 ANSI SQL。各个 DBMS 都有自己的实现，如 PL/SQL、Transact-SQL 等。

SQL 语句不区分大小写，但是数据库表名、列名和值是否区分依赖于具体的 DBMS 以及配置。

SQL 支持以下三种注释：

```sql
## 注释
SELECT *
FROM mytable; -- 注释
/* 注释1
   注释2 */
```

数据库创建与使用：

### 数据库的基本操作

**创建与删除**

```java
CREATE DATABASE hsp
DROP DATABASE hsp;
```

**查询与备份**

```java
SHOW DATABASES //查询所有数据库

mysqldump -u root -B mysql > d:\\mysql.sql //需要在命令窗口（DOS）下执行
source 文件路径 //需要在mysql命令行上执行
```

### 数据类型

/* 数据类型（列类型） */
#### 数值类型

-- a. **整型** ----------
 类型            字节        范围（有符号位）
 tinyint        1字节    -128 ~ 127        无符号位：0 ~ 255
 smallint    2字节    -32768 ~ 32767
 mediumint    3字节    -8388608 ~ 8388607
 int            4字节
 bigint        8字节

 int(M)    M表示总位数

 - 默认存在符号位，unsigned 属性修改
 - 显示宽度，如果某个数不够定义字段时设置的位数，则前面以0补填，zerofill 属性修改
     例：int(5)    插入一个数'123'，补填后为'00123'
 - 在满足要求的情况下，越小越好。
 - 1表示bool值真，0表示bool值假。MySQL没有布尔类型，通过整型0和1表示。常用tinyint(1)表示布尔型。

-- b. **浮点型 -**---------
    类型                字节        范围
    float(单精度)        4字节
    double(双精度)    8字节
    浮点型既支持符号位 unsigned 属性，也支持显示宽度 zerofill 属性。
        不同于整型，前后均会补填0.
    定义浮点型时，需指定总位数和小数位数。
        float(M, D)        double(M, D)
        M表示总位数，D表示小数位数。
        M和D的大小会决定浮点数的范围。不同于整型的固定范围。
        M既表示总位数（不包括小数点和正负号），也表示显示宽度（所有显示符号均包括）。
        支持科学计数法表示。
        浮点数表示近似值。

-- c. **定点数** ----------
    decimal    -- 可变长度
    decimal(M, D)    M也表示总位数，D表示小数位数。
    保存一个精确的数值，不会发生数据的改变，不同于浮点数的四舍五入。
    将浮点数转换为字符串来保存，每9位数字保存为4个字节。

#### 字符串类型

-- a. char, varchar ----------
 char    定长字符串，速度快，但浪费空间
 varchar    变长字符串，速度慢，但节省空间

**注意：**定长：分配多少空间就占用多少空间 ；不定长：使用多少空间就用多少空间

**特点**：char的查询速度快；varchar慢（头部有三个字节记录长度）

 M表示能存储的最大长度，此长度是字符数，非字节数。
 不同的编码，所占用的空间不同。
 **char,最多255个字节，与编码无关。**
 **varchar,最多65532字节，与编码有关**。
 一条有效记录最大不能超过65532个字节。
 **utf8 最大为21844个字符，gbk 最大为32766个字符，latin1 最大为65532个字符**
 varchar 是变长的，需要利用存储空间保存 varchar 的长度，如果数据小于255个字节，则采用一个字节来保存长度，反之需要两个字节来保存。
 varchar 的最大有效长度由最大行大小和使用的字符集确定。
 最大有效长度是65532字节，因为在varchar存字符串时，第一个字节是空的，不存在任何数据，然后还需两个字节来存放字符串的长度，所以有效长度是64432-1-2=65532字节。

```
CREATE TABLE T11(
	`name` VARCHAR(65535) charset=utf8);
```

 例：若一个表定义为 CREATE TABLE tb(c1 int, c2 char(30), c3 varchar(N)) **charset=utf8**; 问N的最大值是多少？ 答：(65535-1-2-4-30*3)/3

**char(4)和varchar(4)表示的是4个字符，具体字节数和编码有关**

-- b. blob, text ----------
    blob 二进制字符串（字节字符串）
        tinyblob, blob, mediumblob, longblob
    **text 非二进制字符串（字符字符串）**
        **tinytext, text, mediumtext, longtext**
    **text 在定义时，不需要定义长度，也不会计算总长度。**
    **text 类型在定义时，不可给default值**

-- c. binary, varbinary ----------
    类似于char和varchar，用于保存二进制字符串，也就是保存字节字符串而非字符字符串。
    char, varchar, text 对应 binary, varbinary, blob.

#### 日期时间类型

一般用整型保存时间戳，因为PHP可以很方便的将时间戳进行格式化。
datetime    8字节    日期及时间        1000-01-01 00:00:00 到 9999-12-31 23:59:59
date        3字节    日期            1000-01-01 到 9999-12-31
timestamp    4字节    时间戳        19700101000000 到 2038-01-19 03:14:07
time        3字节    时间            -838:59:59 到 838:59:59
year        1字节    年份            1901 - 2155

datetime    “YYYY-MM-DD hh:mm:ss”
timestamp    “YY-MM-DD hh:mm:ss”
            “YYYYMMDDhhmmss”
            “YYMMDDhhmmss”
            YYYYMMDDhhmmss
            YYMMDDhhmmss
date        “YYYY-MM-DD”
            “YY-MM-DD”
            “YYYYMMDD”
            “YYMMDD”
            YYYYMMDD
            YYMMDD
time        “hh:mm:ss”
            “hhmmss”
            hhmmss
year        “YYYY”
            “YY”
            YYYY
            YY

#### 枚举和集合

-- 枚举(enum) ----------
enum(val1, val2, val3...)
 在已知的值中进行单选。最大数量为65535.
 枚举值在保存时，以2个字节的整型(smallint)保存。每个枚举值，按保存的位置顺序，从1开始逐一递增。
 表现为字符串类型，存储却是整型。
 NULL值的索引是NULL。
 空字符串错误值的索引值是0。

-- 集合（set） ----------
set(val1, val2, val3...)
    create table tab ( gender set('男', '女', '无') );
    insert into tab values ('男, 女');
    最多可以有64个不同的成员。以bigint存储，共8个字节。采取位运算的形式。
    当创建表时，SET成员值的尾部空格将自动被删除。

#### 选择类型

-- PHP角度

1. 功能满足
2. 存储空间尽量小，处理效率更高
3. 考虑兼容问题

-- IP存储 ----------
1. 只需存储，可用字符串
2. 如果需计算，查找等，可存储为4个字节的无符号int，即unsigned
    1) PHP函数转换
        ip2long可转换为整型，但会出现携带符号问题。需格式化为无符号的整型。
        利用sprintf函数格式化字符串
        sprintf("%u", ip2long('192.168.3.134'));
        然后用long2ip将整型转回IP字符串
    2) MySQL函数转换(无符号整型，UNSIGNED)
        INET_ATON('127.0.0.1') 将IP转为整型
        INET_NTOA(2130706433) 将整型转为IP
        



### 列属性（列约束）

1. 主键
    - 能唯一标识记录的字段，可以作为主键。
    - 一个表只能有一个主键。
    - 主键具有唯一性。
    - 声明字段时，用 primary key 标识。
        也可以在字段列表之后声明
            例：create table tab ( id int, stu varchar(10), primary key (id));
    - 主键字段的值不能为null。
    - 主键可以由多个字段共同组成。此时需要在字段列表后声明的方法。
        例：create table tab ( id int, stu varchar(10), age int, primary key (stu, age));

2. unique 唯一索引（唯一约束）
    使得某字段的值也不能重复。
    
3. null 约束
    null不是数据类型，是列的一个属性。
    表示当前列是否可以为null，表示什么都没有。
    null, 允许为空。默认。
    not null, 不允许为空。
    insert into tab values (null, 'val');
        -- 此时表示将第一个字段的值设为null, 取决于该字段是否允许为null
    
4. default 默认值属性
    当前字段的默认值。
    insert into tab values (default, 'val');    -- 此时表示强制使用默认值。
    create table tab ( add_time timestamp default current_timestamp );
        -- 表示将当前时间的时间戳设为默认值。
        current_date, current_time

5. auto_increment 自动增长约束
    自动增长必须为索引（主键或unique）
    只能存在一个字段为自动增长。
    默认为1开始自动增长。可以通过表属性 auto_increment = x进行设置，或 alter table tbl auto_increment = x;

6. comment 注释
    例：create table tab ( id int ) comment '注释内容';

7. foreign key 外键约束
    用于限制主表与从表数据完整性。
    alter table t1 add constraint `t1_t2_fk` foreign key (t1_id) references t2(id);
        -- 将表t1的t1_id外键关联到表t2的id字段。
        -- 每个外键都有一个名字，可以通过 constraint 指定

    存在外键的表，称之为从表（子表），外键指向的表，称之为主表（父表）。

    作用：保持数据一致性，完整性，主要目的是控制存储在外键表（从表）中的数据。

    MySQL中，可以对InnoDB引擎使用外键约束：
    语法：
    foreign key (外键字段） references 主表名 (关联字段) [主表记录删除时的动作] [主表记录更新时的动作]
    此时需要检测一个从表的外键需要约束为主表的已存在的值。外键在没有关联的情况下，可以设置为null.前提是该外键列，没有not null。

    可以不指定主表记录更改或更新时的动作，那么此时主表的操作被拒绝。
    如果指定了 on update 或 on delete：在删除或更新时，有如下几个操作可以选择：
    1. cascade，级联操作。主表数据被更新（主键值更新），从表也被更新（外键值更新）。主表记录被删除，从表相关记录也被删除。
    2. set null，设置为null。主表数据被更新（主键值更新），从表的外键被设置为null。主表记录被删除，从表相关记录外键被设置成null。但注意，要求该外键列，没有not null属性约束。
    3. restrict，拒绝父表删除和更新。

    注意，外键只被InnoDB存储引擎所支持。其他引擎是不支持的。

###  建表规范 

​    -- Normal Format, NF

        - 每个表保存一个实体信息
                - 每个具有一个ID字段作为主键
                - ID主键 + 原子表
            -- 1NF, 第一范式
                        字段不能再分，就满足第一范式。
            -- 2NF, 第二范式
                        满足第一范式的前提下，不能出现部分依赖。
                        消除符合主键就可以避免部分依赖。增加单列关键字。
            -- 3NF, 第三范式
                        满足第二范式的前提下，不能出现传递依赖。
                        某个字段依赖于主键，而有其他字段依赖于该字段。这就是传递依赖。
                        将一个实体信息的数据放在一个表内实现。

## 二、创建表

```sql
CREATE TABLE `user`(
		id INT,
		`name` VARCHAR(255),
		`birthday` DATE)
CHARACTER SET utf8 COLLATE utf8_bin ENGINE INNODB
// CHARACTER SET 字符集 COLLATE 校验方法 ENGINE 引擎

DESC Table；//显示表结构
```

### 复制表

```
1、复制表的结构
create table 表名 like 旧表
2、复制表的结构加数据
create table 表名
select 查询列表 from 旧表{where 筛选条件};
```



## 三、修改表

**插入列**

```sql
ALTER TABLE emp
ADD id INT//类型 INT
AFTER `name` #定义插入列的位置 first 定义在第一列
```



**删除列**

```sql
ALTER TABLE emp
DROP id;
```

**修改列的属性**

```sql
ALTER TABLE emp
MODIFY id DOUBLE
```

**修改表名**

```sql
RENAME TABLE emp TO employee
```

**修改列名**

```sql
ALTER TABLE employee CHANGE `name` user_name VARCHAR(10)
```

## 四、插入

**普通插入数据**

```sql
INSERT INTO mytable(col1, col2)
VALUES(val1, val2);
```

```mysql
CREATE TABLE `emp` (
id INT,`name` VARCHAR(32),`sex` CHAR(1),
birthday DATE, entry_date DATETIME, salary DOUBLE,
`resume` TEXT)
SELECT * FROM emp

//如果给所有的列添加数据，前面的字段名可以省略
INSERT INTO emp
VALUE(100,'按他买','男','2020-07-05'
,'2020-08-09 11:11:1','3000','我是按特卖')

//直接插入多条记录
INSERT INTO emp
VALUE(100,'按他买','男','2020-07-05'
,'2020-08-09 11:11:1','3000','我是按特卖'),(100,'按他买','男','2020-07-05'
,'2020-08-09 11:11:1','3000','我是按特卖'),(100,'按他买','男','2020-07-05'
,'2020-08-09 11:11:1','3000','我是按特卖') //直接插入多条记录



//INSERT INTO emp重复执行相同的数据不会覆盖；会重复添加
```

**插入检索出来的数据**

```sql
INSERT INTO mytable1(col1, col2)
SELECT col1, col2
FROM mytable2;
```

**将一个表的内容插入到一个新表**

```sql
CREATE TABLE newtable AS
SELECT * FROM mytable;
```

## 五、更新

```sql
UPDATE mytable
SET col1 = val1,col2 = val2
WHERE id = 1;

UPDATE `good` SET good_name='华为'
WHERE id = 1;
```

## 六、删除

### **delete**

```sql
DELETE FROM mytable
WHERE id = 1;
```

**删除后再插入，标识从断点开始**

**可以回滚**

**多表删除**：

```SQL
DELETE T1，T2
FROM Table1 T1, Table2 T2
where 连接条件
and 筛选条件

DELETE T1
FROM Table1 T1
inner|left|right join Table2 T2 on 连接条件
where 筛选条件
```

### **TRUNCATE TABLE**   

可以清空表，也就是删除所有行。

**无法添加筛选条件**

**不可以回滚**

**删除后再插入，标识从1开始**

```sql
TRUNCATE TABLE mytable;
```

使用更新和删除操作时一定要用 WHERE 子句，不然会把整张表的数据都破坏。可以先用 SELECT 语句进行测试，防止错误删除。

## 七、查询

### 总结

```
select 查询列表 
from 表1 别名
连接类型 join 表2
on 连接条件
where 筛选
group by 分组列表
having 筛选
order by 排序列表
limit 起始索引，条目数；
```



### DISTINCT

DISTINCT 去重**相同值只会出现一次。它作用于所有列，也就是说所有列的值都相同才算相同。**

```sql
SELECT DISTINCT col1, col2
FROM mytable;
```

### LIMIT

限制返回的行数。可以有两个参数，第一个参数为起始行，从 0 开始；第二个参数为返回的总行数。

返回前 5 行：

```sql
SELECT *
FROM mytable
LIMIT 5;
```

```sql
SELECT *
FROM mytable
LIMIT 0, 5;
```

返回第 3 \~ **5** 行：

```sql
SELECT *
FROM mytable
LIMIT 2, 3;
```

### AS

别名

```mysql
Select `name`,(chinese+english+math) as `all` from mytable;
```



## 八、排序

**ORDER BY**

-   **ASC**  ：升序（默认）
-   **DESC**  ：降序

可以按多个列进行排序，并且为每个列指定不同的排序方式：

```sql
SELECT *
FROM mytable
ORDER BY col1 DESC, col2 ASC;

SELECT job_id AS jd FROM employees 
ORDER BY salary ASC


SELECT salary,job_id AS jd,phone_number FROM employees 
ORDER BY salary ASC,phone_number DESC
```

除了limit子句，ORDER BY一般都在后面

## 九、过滤

**WHERE**

不进行过滤的数据非常大，导致通过网络传输了多余的数据，从而浪费了网络带宽。因此尽量使用 SQL 语句来过滤不必要的数据，而不是传输所有的数据到客户端中然后由客户端进行过滤。

```sql
SELECT *
FROM mytable
WHERE col IS NULL;

SELECT * FROM employees
WHERE salary>3000
```

下表显示了 WHERE 子句可用的操作符

|  操作符 | 说明  |
| :---: | :---: |
| = | 等于 |
| &lt; | 小于 |
| &gt; | 大于 |
| &lt;&gt; != | 不等于 |
| &lt;= !&gt; | 小于等于 |
| &gt;= !&lt; | 大于等于 |
| BETWEEN | 在两个值之间 |
| IS NULL | 为 NULL 值 |
| in | 在（a,b,c）区间内 |
| like | 和通配符使用 |
|  |  |

```
SELECT * FROM employees
WHERE salary BETWEEN 3000 AND 5000 //闭区间

SELECT * FROM employees
WHERE salary IN (3000,4000)
```

应该注意到，NULL 与 0、空字符串都不同。

**AND 和 OR**   用于连接多个过滤条件。优先处理 AND，当一个过滤表达式涉及到多个 AND 和 OR 时，可以使用 () 来决定优先级，使得优先级关系更清晰。

**IN**   操作符用于匹配一组值，其后也可以接一个 SELECT 子句，从而匹配子查询得到的一组值。

**NOT**   操作符用于否定一个条件。

## 十、通配符

通配符也是用在过滤语句中，但它只能用于文本字段。

-   **%**   匹配 \>=0 个任意字符；

-   **\_**   匹配 ==1 个任意字符；

-   **[ ]**   可以匹配集合内的字符，例如 [ab] 将匹配字符 a 或者 b。用脱字符 ^ 可以对其进行否定，也就是不匹配集合内的字符。

使用 Like 来进行通配符匹配。

```sql
SELECT *
FROM mytable
WHERE col LIKE '[^AB]%'; -- 不以 A 和 B 开头的任意文本

SELECT last_name 
FROM employees
WHERE last_name LIKE '_a%'
```

不要滥用通配符，通配符位于开头处匹配会非常慢。

## 十一、计算字段

在数据库服务器上完成数据的转换和格式化的工作往往比客户端上快得多，并且转换和格式化后的数据量更少的话可以减少网络通信量。

计算字段通常需要使用   **AS**   来取别名，否则输出的时候字段名为计算表达式。

```sql
SELECT col1 * col2 AS alias
FROM mytable;
```

**CONCAT()**   用于连接两个字段。许多数据库会使用空格把一个值填充为列宽，因此连接的结果会出现一些不必要的空格，使用 **TRIM()** 可以去除首尾空格。

```sql
SELECT CONCAT(TRIM(col1), '(', TRIM(col2), ')') AS concat_col
FROM mytable;
```

## 十二、函数

各个 DBMS 的函数都是不相同的，因此不可移植，以下主要是 MySQL 的函数。

### 汇总

|函 数 |说 明|
| :---: | :---: |
| AVG(列) | 返回某列的平均值 |
| COUNT(*/列) | 返回某列的行数（不等于null） |
| MAX(列) | 返回某列的最大值 |
| MIN(列) | 返回某列的最小值 |
| SUM(列) |返回某列值之和 |

AVG() 会忽略 NULL 行。

使用 DISTINCT 可以汇总不同的值。

```sql
SELECT AVG(DISTINCT col1) AS avg_col
FROM mytable;
```

**"+"只能做加法运算**

### 文本处理

| 函数  | 说明  |
| :---: | :---: |
|  LEFT() |  左边的字符 |
| RIGHT() | 右边的字符 |
| LOWER() | 转换为小写字符 |
| UPPER() | 转换为大写字符 |
| LTRIM() | 去除左边的空格 |
| RTRIM() | 去除右边的空格 |
| LENGTH() | 获取~字节~长度 |
| SOUNDEX() | 转换为语音值 |
| CONCAT('a','b','c') | 字符拼接 |
| SUBSTR('字符'，1) | 截取从第一个1个字符开始 |
| INSTR（‘父串’，‘子串’） | 返回子串出现的第一个索引 |

```
SELECT LENGTH ('张')  /  =3
```

其中，  **SOUNDEX()**   可以将一个字符串转换为描述其语音表示的字母数字模式。

```sql
SELECT *
FROM mytable
WHERE SOUNDEX(col1) = SOUNDEX('apple')
```

### 日期和时间处理


- 日期格式：YYYY-MM-DD
- 时间格式：HH:\<zero-width space\>MM:SS

|函 数 | 说 明|
| :---: | :---: |
| ADDDATE() | 增加一个日期（天、周等）|
| ADDTIME() | 增加一个时间（时、分等）|
| CURDATE() | 返回当前日期 |
| CURTIME() | 返回当前时间 |
| DATE() |返回日期时间的日期部分|
| DATEDIFF() |计算两个日期之差|
| DATE_ADD() |高度灵活的日期运算函数|
| DATE_FORMAT() |返回一个格式化的日期或时间串|
| DAY()| 返回一个日期的天数部分|
| DAYOFWEEK() |对于一个日期，返回对应的星期几|
| HOUR() |返回一个时间的小时部分|
| MINUTE() |返回一个时间的分钟部分|
| MONTH() |返回一个日期的月份部分|
| NOW() |返回当前日期和时间|
| SECOND() |返回一个时间的秒部分|
| TIME() |返回一个日期时间的时间部分|
| YEAR() |返回一个日期的年份部分|

```sql
mysql> SELECT NOW();
```

```
2018-4-14 20:25:11
```

### 数值处理

| 函数 | 说明 |
| :---: | :---: |
| SIN() | 正弦 |
| COS() | 余弦 |
| TAN() | 正切 |
| ABS() | 绝对值 |
| SQRT() | 平方根 |
| MOD() | 余数 |
| EXP() | 指数 |
| PI() | 圆周率 |
| RAND() | 随机数 |
| ROUND() | 四舍五入 |
| CEIL | 向上取整 |
| floor | 向下取整 |
| MOD | %取余 |

### 流程控制函数

**if**

```
SELECT last_name, commission_pct , IF(commission_pct IS NULL ,'操','✌') as 备注
FROM employees
```

**CASE**

用法一：

CASE 列 WHen (列值/条件) Then 要显示的值或者语句

else 要显示的值或者语句

end as 别名

```

SELECT salary AS 原始工资,department_id,
CASE department_id
WHEN 30 THEN salary*1.1
WHEN 40 THEN salary*1.2
WHEN 50 THEN salary*1.3
ELSE salary
END AS 新工资
FROM employees;
```

用法二:

CASE 

WHen (列值/条件) Then 要显示的值或者语句

else 要显示的值或者语句

end as 别名

```
SELECT salary AS 原始工资,department_id,
CASE
WHEN  department_id<30 THEN salary*0.5

ELSE salary 
END AS 新工资
FROM employees;
```



### 

## 十三、分组

**把具有相同的数据值的行放在同一组中。**

**GROUP BY  将数据分成若干组**，**每组中在进行汇中函数运算；**

可以对同一分组数据使用汇总函数进行处理，例如求分组数据的平均值等。

指定的分组字段除了能按该字段进行分组，也会自动按该字段进行排序。

```sql
SELECT col, COUNT(*) AS num
FROM mytable
GROUP BY col;
```

1. **GROUP BY 自动按分组字段进行排序**，**ORDER BY 也可以按汇总字段来进行排序。**

```sql
SELECT col, COUNT(*) AS num
FROM mytable
GROUP BY col
ORDER BY num;
```

2. **分组后筛选HAVING和WHERE分组前筛选：**

**WHERE 过滤行，HAVING 过滤分组，行过滤应当先于分组过滤。**

```sql
SELECT col, COUNT(*) AS num
FROM mytable
WHERE col > 2
GROUP BY col
HAVING num >= 2;
```

分组规定：

- **GROUP BY 子句出现在 WHERE 子句之后，ORDER BY 子句之前；**
- 除了汇总字段外，SELECT 语句中的每一字段都必须在 GROUP BY 子句中给出；
- NULL 的行会单独分为一组；
- 大多数 SQL 实现不支持 GROUP BY 列具有可变长度的数据类型。



## 

## 十四、子查询

子查询中只能返回一个字段的数据。

可以将**子查询的结果**作为 WHRER 语句的过滤条件：

```sql
SELECT *
FROM mytable1
WHERE col1 IN (SELECT col2
               FROM mytable2);
```

下面的语句可以检索出客户的订单数量，子查询语句会对第一个查询检索出的每个客户执行一次：

```sql
SELECT cust_name, (SELECT COUNT(*)
                   FROM Orders
                   WHERE Orders.cust_id = Customers.cust_id)
                   AS orders_num
FROM Customers
ORDER BY cust_name;
```

## 十五、连接

**连接用于连接多个表，使用 JOIN 关键字，并且条件语句使用 ON 而不是 WHERE。**

**筛选条件用where；连接条件用on**

连接可以替换子查询，并且比子查询的效率一般会更快。

可以用 AS 给列名、计算字段和表名取别名，给表名取别名是为了简化 SQL 语句以及连接相同表。

### 等值连接

内连接又称等值连接，使用 **INNER JOIN** 关键字。

```sql
SELECT A.value, B.value
FROM tablea AS A INNER JOIN tableb AS B
ON A.key = B.key;
```

可以不明确使用 INNER JOIN，而使用普通查询并在 WHERE 中将两个表中要连接的列用等值方法连接起来。

```sql
SELECT A.value, B.value
FROM tablea AS A, tableb AS B
WHERE A.key = B.key;
```

### 非等值连接

where 后面非等值条件，可用于为数据分级

### 自连接

自连接可以看成内连接的一种，只是连接的表是自身而已。

一张员工表，包含员工姓名和员工所属部门，要找出与 Jim 处在同一部门的所有员工姓名。

子查询版本

```sql
SELECT name
FROM employee
WHERE department = (
      SELECT department
      FROM employee
      WHERE name = "Jim");
```

自连接版本

```sql
SELECT e1.name
FROM employee AS e1 INNER JOIN employee AS e2
ON e1.department = e2.department
      AND e2.name = "Jim";
```

员工和老板对接

```mysql
SELECT e.employee_id, e.last_name,m.employee_id, m.last_name
FROM employees e,employees m
WHERE e.manager_id=m.employee_id;
```

```mysql
SELECT e.employee_id, e.last_name,m.employee_id, m.last_name
FROM employees e
INNER JOIN employees m 
ON e.manager_id=m.employee_id;
```



### 自然连接

自然连接是把同名列通过等值测试连接起来的，同名列可以有多个。

内连接和自然连接的区别：内连接提供连接的列，而自然连接自动连接所有同名列。

```sql
SELECT A.value, B.value
FROM tablea AS A NATURAL JOIN tableb AS B;
```

### 外连接



外连接保留了没有关联的那些行。分为左外连接，右外连接以及全外连接，**左外连接以LEFT OUTER JOIN左边的为主表。**

检索所有顾客的订单信息，包括还没有订单信息的顾客。

```sql
SELECT Customers.cust_id, Customer.cust_name, Orders.order_id
FROM Customers LEFT OUTER JOIN Orders
ON Customers.cust_id = Orders.cust_id;
```

customers 表：

| cust_id | cust_name |
| :---: | :---: |
| 1 | a |
| 2 | b |
| 3 | c |

orders 表：

| order_id | cust_id |
| :---: | :---: |
|1    | 1 |
|2    | 1 |
|3    | 3 |
|4    | 3 |

结果：

| cust_id | cust_name | order_id |
| :---: | :---: | :---: |
| 1 | a | 1 |
| 1 | a | 2 |
| 3 | c | 3 |
| 3 | c | 4 |
| 2 | b | Null |

## 十六、组合查询

使用   **UNION**   来组合两个查询，如果第一个查询返回 M 行，第二个查询返回 N 行，那么组合查询的结果一般为 M+N 行。

每个查询必须包含相同的列、表达式和聚集函数。

**默认会去除相同行，如果需要保留相同行，使用 UNION ALL。**

只能包含一个 ORDER BY 子句，并且必须位于语句的最后。

```sql
SELECT col
FROM mytable
WHERE col = 1
UNION
SELECT col
FROM mytable
WHERE col =2;
```

## 十七 查询约束

#### NOT NULL

非空，保证字段的值不能为空

#### DEFAULT

默认，用于保证该字段的值具有唯一性，并且非空

比如学号、员工编号等

#### UNIQUE

唯一，用于保证该字段的值具有唯一性，可以为空

#### PRIMARY KEY

主键，用于保证该字段的值具有唯一性，并且非空

#### CHECK

数据范围约束

#### FOREIGN KEY

外键，用于限制两个表的关系，保证该字段的值必须来自于主表的关联列值

#### 创建表时添加约束 方法

```sql
Create Table stauinfo(
	id INT PRIMARY KEY,#主键
	name Varchar(20) not null,#非空
	gender CHAR(1) ChECK(gender = `man` or gender = `woman`)，#范围约束
	seat INT UNIQUE，#唯一
	age INT DEFAULT 18，#默认约束
	majorID INT Foreign Key References major(id)#外键 没效果
)
Create Tabbe major(
	id INT primary key,
	majorName Varchar(20)
)
```

#### 添加表级约束 方法

语法：在各字段的最下面

（constraint 约束名） 约束类型（字段名）

```sql
Create Table stauinfo(
	id INT ,#主键
	name Varchar(20) not null,#非空
	gender CHAR(1) ChECK(gender = `man` or gender = `woman`)，#范围约束
	seat INT UNIQUE，#唯一
	age INT DEFAULT 18，#默认约束
	majorID INT  References major(id)#外键
	
	CONstraint pk PRIMARY KEY(id),
	CONstraint fk_stu_major Foreign Key(majorID) References major(id)
)
```

#### 修改表时添加约束

ALTER TABLE 表名 MODIFY COLUMN 列名 属性约束

```sql
Create Table stauinfo(
	id INT ,#主键
	name Varchar(20) not null,#非空
	gender CHAR(1) ChECK(gender = `man` or gender = `woman`)，#范围约束
	seat INT UNIQUE，#唯一
	age INT ，#默认约束
	majorID INT  References major(id)#外键
	
	CONstraint pk PRIMARY KEY(id),
	CONstraint fk_stu_major Foreign Key(majorID) References major(id)
)

ALTER TABLE stauinfo MODIFY COLUMN age INT DEFAULT 18;
ALTER TABLE stauinfo ADD PRIMARY kEY(id)
```

#### 修改表时删除约束

```sql
ALTER TABLE stauinfo MODIFY COLUMN age INT;
```

#### 列级约束与表级约束的区别

列级约束：

- 位置列的后面
- 语法都支持但外键没有效果
- 不可以起约束名

表级约束：

- 所有列的下面
- 默认和非空不支持
- 可以起名字，但主键没有效果

#### 主键和唯一的区别

共同点：

1. 保证唯一性
2. 允许组合成 该类型

不同点：

1. 主键非空，唯一可空
2. 主键类型的字段只能有一个， 唯一类型 可以多个

#### 外键的注意事项

- 要求从表设置外键约束
- 从表的外键列的类型和主表的关联列的类型要求一致或兼容，名称无要求
- 主表的关联列必须是（主键或者唯一）
- 插入有数据时，先插入主表后插入从表
- 删除数据先删除从表数据后删除主表数据



### 标识列约束

AUTO_INCREMENT 又称为自增长列，系统默认提供自增长值

SET auto_increment_increament 用来设置自增长步长

```
Create Table stauinfo(
	id INT AUTO_INCREMENT,#主键
	name Varchar(20) not null,#非空
)
INSERT INTO stauinfo(name) VALUES('join'),
SET auto_increment_increament = 3;
```

注意事项：

1、标识列必须与键搭配（并非为主键）

2、只能有一个自增长列

3、标识列的类型只能是 数值型



## 十七、视图

视图是虚拟的表，本身不包含数据，也就不能对其进行索引操作。

对视图的操作和对普通表的操作一样。

视图具有如下好处：

- 简化复杂的 SQL 操作，比如复杂的连接；
- 只使用实际表的一部分数据；
- 通过只给用户访问视图的权限，保证数据的安全性；
- 更改数据格式和表示。

```sql
CREATE VIEW myview AS
SELECT Concat(col1, col2) AS concat_col, col3*col4 AS compute_col
FROM mytable
WHERE col5 = val;
```

## 十八、存储过程

存储过程可以看成是对一系列 SQL 操作的批处理。

使用存储过程的好处：

- 代码封装，保证了一定的安全性；
- 代码复用；
- 由于是预先编译，因此具有很高的性能。

命令行中创建存储过程需要自定义分隔符，因为命令行是以 ; 为结束符，而存储过程中也包含了分号，因此会错误把这部分分号当成是结束符，造成语法错误。

包含 in、out 和 inout 三种参数。

给变量赋值都需要用 select into 语句。

每次只能给一个变量赋值，不支持集合的操作。

```sql
delimiter //

create procedure myprocedure( out ret int )
    begin
        declare y int;
        select sum(col1)
        from mytable
        into y;
        select y*y into ret;
    end //

delimiter ;
```

```sql
call myprocedure(@ret);
select @ret;
```

## 十九、游标

在存储过程中使用游标可以对一个结果集进行移动遍历。

游标主要用于交互式应用，其中用户需要对数据集中的任意行进行浏览和修改。

使用游标的四个步骤：

1. 声明游标，这个过程没有实际检索出数据；
2. 打开游标；
3. 取出数据；
4. 关闭游标；

```sql
delimiter //
create procedure myprocedure(out ret int)
    begin
        declare done boolean default 0;

        declare mycursor cursor for
        select col1 from mytable;
        # 定义了一个 continue handler，当 sqlstate '02000' 这个条件出现时，会执行 set done = 1
        declare continue handler for sqlstate '02000' set done = 1;

        open mycursor;

        repeat
            fetch mycursor into ret;
            select ret;
        until done end repeat;

        close mycursor;
    end //
 delimiter ;
```

## 二十、触发器

触发器会在某个表执行以下语句时而自动执行：DELETE、INSERT、UPDATE。

触发器必须指定在语句执行之前还是之后自动执行，之前执行使用 BEFORE 关键字，之后执行使用 AFTER 关键字。BEFORE 用于数据验证和净化，AFTER 用于审计跟踪，将修改记录到另外一张表中。

INSERT 触发器包含一个名为 NEW 的虚拟表。

```sql
CREATE TRIGGER mytrigger AFTER INSERT ON mytable
FOR EACH ROW SELECT NEW.col into @result;

SELECT @result; -- 获取结果
```

DELETE 触发器包含一个名为 OLD 的虚拟表，并且是只读的。

UPDATE 触发器包含一个名为 NEW 和一个名为 OLD 的虚拟表，其中 NEW 是可以被修改的，而 OLD 是只读的。

MySQL 不允许在触发器中使用 CALL 语句，也就是不能调用存储过程。

## 二十一、事务管理

基本术语：

- 事务（transaction）指一组 SQL 语句组成的执行单元，要么全部执行，要么全部不执行；
- 回退（rollback）指撤销指定 SQL 语句的过程；
- 提交（commit）指将未存储的 SQL 语句结果写入数据库表；
- 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），你可以对它发布回退（与回退整个事务处理不同）。

### 什么是事务

**一个事务是一个完整的业务逻辑单元，不可再分。**
比如：银行账户转账，从A账户向B账户转账10000.需要执行两条update语句：

```
update t_act set balance = balance - 10000 where actno = 'act-001';
update t_act set balance = balance + 10000 where actno = 'act-002';
```

- 以上两条DML语句必须同时成功，或者同时失败，不允许出现一条成功，一条失败。
  **要想保证以上的两条DML语句同时成功或者同时失败，那么就需要使用数据库的“事务机制”**

- **和事务相关的语句只有：DML语句。（insert delete update）**
  为什么？因为它们这三个语句都是和数据库表当中的“数据”相关的。**事务的存在是为了保证数据的完整性，安全性**。

不能回退 SELECT 语句，回退 SELECT 语句也没意义；也不能回退 CREATE 和 DROP 语句。

**MySQL 的事务提交默认是隐式提交，每执行一条语句就把这条语句当成一个事务然后进行提交。**当出现 START TRANSACTION 语句时，会关闭隐式提交；当 COMMIT 或 ROLLBACK 语句执行后，事务会自动关闭，重新恢复隐式提交。

设置 autocommit 为 0 可以取消自动提交；autocommit 标记是针对每个连接而不是针对服务器的。

如果没有设置保留点，ROLLBACK 会回退到 START TRANSACTION 语句处；如果设置了保留点，并且在 ROLLBACK 中指定该保留点，则会回退到该保留点。

### 事物的原理

假设一个事儿，需要先执行一条insert,再执行一条update,最后执行一条delete，这个事儿才算完成。
开启事务机制：
  执行insert语句----->insert…(这个执行成功之后，把这个执行记录到数据库的操作历史当中，并不会向文件中保存一条数据，不会真正的修改硬盘上的数据)
  执行update语句----->update…(这条与上一条相似)
  执行delete语句----->delete…(这条与上一条相似)[记录到缓存中]
  **提交事务或者回滚事务（结束）（这个时候才会把数据写到硬盘上，以上所有操作的历史数据将会被清空）**
**提交语句用commit,回滚事务用rollback.**
TCL(事务控制语言)
可以设置一个保存点，让其回滚到某个地方。

### 事务的ACID特性：

1. 原子性
2. 一致性：事务必须保证多条DML语句同时成功或者同时失败。
3. 隔离性：一个事务不受其他事务干扰
4. 持久性：事务一旦提价，结束标记，不可撤回；持久性说的是最终数据必须持久化到硬盘文件中，事务才算成功的结束。

```
原子性：事务是一组不可分割的操作单元，这组单元要么同时成功要么同时失败（由DBMS的事务管理子系统来实现）；
一致性：事务前后的数据完整性要保持一致（由DBMS的完整性子系统执行测试任务）；
隔离性:多个用户的事务之间不要相互影响，要相互隔离（由DBMS的并发控制子系统实现）；
持久性:一个事务一旦提交，那么它对数据库产生的影响就是永久的不可逆的，如果后面再回滚或者出异常，都不会影响已提交的事务（由DBMS的恢复管理子系统实现的）
```

```sql
START TRANSACTION
// ...
SAVEPOINT delete1
// ...
ROLLBACK TO delete1
// ...
COMMIT
```

### 事物的创建

- 隐式的事务

没有确定的开启和结束符，自动提交功能：自动开启

- 显示事务
  - **必须先设置，自动提交功能为禁用**
    
    ```
    set  autocommit = 0;
    或者
    start transaction;
    ```
    
  - 编写sql语句（select insert update delete）
  
  - 结束事务
    commit；提交
    rollback；回滚事务

```sql
set  autocommit = 0；
-----------------
commit；
```

### 事物的隔离性

<img src="C:\Users\songjiaqiang\AppData\Roaming\Typora\typora-user-images\image-20210825112529044.png" alt="image-20210825112529044" style="zoom:50%;" />

事务隔离性存在隔离级别，理论上隔离级别包括4个：
**第一级别：读未提交（read uncommitted）**
  对方事务还没有提交，我们当前事务可以读取到对方未提交的数据。
  读未提交存在**脏读**（Dirty Read）现象：表示读到了脏的数据。
  数据是不稳定的，还有持久到磁盘中，断电就没有了，也有可能会回滚**第二级别：读已提交（read committed）（默认）**
  对方事务提交之后的数据我方可以读取到。
  这种隔离级别解决了: 脏读现象没有了。
  读已提交存在的问题是：**不可重复读**。
  不可重复读：比如第一次我在emp表中读取到了14条数据，后来不经意间别人在emp表中删除了一条数据，那么第二次我读的时候，变成了13条数据，假设两次读在同一个事务中。；两次读取时事务没有结束。也就是同一个事务内多次读取的时候读到的数据不一致。
**第三级别：可重复读（repeatable read）**
  这种隔离级别解决了：不可重复读问题。
  这种隔离级别存在的问题是：读取到的数据是**幻象**。
  提交之后的数据读不到，永远读取的都是开启事务之后的数据。只要事务没关，我读到的永远都是14条数据。
**第四级别：序列化读/串行化读（serializable）**
  解决了所有问题。
  效率低。需要事务排队。
Oracle数据库的默认隔离级别是：读已提交
Mysql数据库默认隔离界别是：可重复读

### 隔離級別的演示

使用两个事务演示以上的隔离级别

#### **读未提交**

1. 设置事务的全局隔离级别

```
set global transaction isolation level read uncommitted;
```



2. 查看事务的全局隔离级别

   ```
   select @@global.tx_isolation;
   +-----------------------+
   | @@global.tx_isolation |
   +-----------------------+
   | READ-UNCOMMITTED      |
   +-----------------------+
   1 row in set, 1 warning (0.00 sec)
   ```

3. 事务1：

```
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)
mysql> select * from t_user;
+----+----------+
| id | username |
+----+----------+
|  1 | zs       |
+----+----------+
1 row in set (0.00 sec)
```

//在这之间事务2插入了一条数据smith

```
mysql> select * from t_user;
+----+----------+
| id | username |
+----+----------+
|  1 | zs       |
|  4 | SMITH    |
+----+----------+
2 rows in set (0.00 sec)
```

4. 事务2

```
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)
mysql> insert into t_user(username) values("SMITH");
Query OK, 1 row affected (0.04 sec)
```

#### 演示读已提交

设置事务隔离级别为读已提交

```
mysql> set global transaction isolation level read committed;
Query OK, 0 rows affected (0.00 sec)
```

```
mysql> select @@global.tx_isolation;
+-----------------------+
| @@global.tx_isolation |
+-----------------------+
| READ-COMMITTED        |
+-----------------------+
1 row in set, 1 warning (0.00 sec)
```


事务1

```
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from t_user;
+----+----------+
| id | username |
+----+----------+
|  1 | zs       |
+----+----------+
1 row in set (0.00 sec)
```

//事务2插入了一条数据

```
mysql> select * from t_user;
+----+----------+
| id | username |
+----+----------+
|  1 | zs       |
+----+----------+
1 row in set (0.02 sec)
```

//事务2提交

````


```
mysql> select * from t_user;
+----+----------+
| id | username |
+----+----------+
|  1 | zs       |
|  5 | test     |
+----+----------+
```

2 rows in set (0.00 sec)
````


事务2

```
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t_user(username) values("test");
Query OK, 1 row affected (0.04 sec)

mysql> commit
    -> ;
Query OK, 0 rows affected (0.05 sec)
```

#### 演示可重复读

设置事务隔离级别

```
mysql> set global transaction isolation level repeatable read;
Query OK, 0 rows affected (0.00 sec)
```

```
mysql> select @@global.tx_isolation;
+-----------------------+
| @@global.tx_isolation |
+-----------------------+
| REPEATABLE-READ       |
+-----------------------+
```


事务1

```
mysql> select * from t_user;
+----+----------+
| id | username |
+----+----------+
|  1 | zs       |
|  5 | test     |
+----+----------+
2 rows in set (0.00 sec)
```

//事务2在这之间对数据进行无数次修改并进行了提交。

```
mysql> select * from t_user;
+----+----------+
| id | username |
+----+----------+
|  1 | zs       |
|  5 | test     |
+----+----------+
```

2 rows in set (0.00 sec)

这里读到的数据是没修改过的，但事实上数据库中的内容早就被修改了，因此读到的是幻象。

事务2

```
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> delete from t_user;
Query OK, 2 rows affected (0.06 sec)

mysql> commit
    -> ;
Query OK, 0 rows affected (0.05 sec)
```

```
mysql> select * from t_user;
Empty set (0.00 sec)
```

#### 演示串行化读

效率低下一般不用

```
设置事务隔离级别

mysql> set global transaction isolation level serializable;
Query OK, 0 rows affected (0.00 sec)

mysql> select @@global.tx_isolation;
+-----------------------+
| @@global.tx_isolation |
+-----------------------+
| SERIALIZABLE          |
+-----------------------+
```

```
第一种情况，事务1未提交时

mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from t_user;
+----+----------+
| id | username |
+----+----------+
|  6 | lisi     |
|  7 | wangwu   |
+----+----------+
2 rows in set (0.00 sec)

mysql> insert into t_user(username) values('hehe');
Query OK, 1 row affected (0.00 sec)
```

```
此时，事务1还未提交
事务2

mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)
mysql> select * from t_user;
```


**卡住了**
当事务1提交之后

```
mysql> commit;
Query OK, 0 rows affected (0.06 sec)
```

//事务2迅速显示出结果

```
mysql> select * from t_user;
+----+----------+
| id | username |
+----+----------+
|  6 | lisi     |
|  7 | wangwu   |
|  8 | hehe     |
+----+----------+
3 rows in set (5.82 sec)
```



## 二十二、字符集

基本术语：

- 字符集为字母和符号的集合；
- 编码为某个字符集成员的内部表示；
- 校对字符指定如何比较，主要用于排序和分组。

除了给表指定字符集和校对外，也可以给列指定：

```sql
CREATE TABLE mytable
(col VARCHAR(10) CHARACTER SET latin COLLATE latin1_general_ci )
DEFAULT CHARACTER SET hebrew COLLATE hebrew_general_ci;
```

可以在排序、分组时指定校对：

```sql
SELECT *
FROM mytable
ORDER BY col COLLATE latin1_general_ci;
```

## 二十三、权限管理

MySQL 的账户信息保存在 mysql 这个数据库中。

```sql
USE mysql;
SELECT user FROM user;
```

**创建账户**  

新创建的账户没有任何权限。

```sql
CREATE USER myuser IDENTIFIED BY 'mypassword';
```

**修改账户名**  

```sql
RENAME USER myuser TO newuser;
```

**删除账户**  

```sql
DROP USER myuser;
```

**查看权限**  

```sql
SHOW GRANTS FOR myuser;
```

**授予权限**  

账户用 username@host 的形式定义，username@% 使用的是默认主机名。

```sql
GRANT SELECT, INSERT ON mydatabase.* TO myuser;
```

**删除权限**  

GRANT 和 REVOKE 可在几个层次上控制访问权限：

- 整个服务器，使用 GRANT ALL 和 REVOKE ALL；
- 整个数据库，使用 ON database.\*；
- 特定的表，使用 ON database.table；
- 特定的列；
- 特定的存储过程。

```sql
REVOKE SELECT, INSERT ON mydatabase.* FROM myuser;
```

**更改密码**  

必须使用 Password() 函数进行加密。

```sql
SET PASSWROD FOR myuser = Password('new_password');
```

## 二十四 数据库的范式

### 数据库设计的范式

	* 概念：设计数据库时，需要遵循的一些规范。要遵循后边的范式要求，必须先遵循前边的所有范式要求

​	设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。
​	目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。

### 分类：

1. 第一范式（1NF）：每一列都是不可分割的原子数据项

   存在的问题

   - 信息冗余严重
   - 数据添加存在问题（添加独立的单元格数据问题）
   - 删除数据存在问题（相关联的行数据全被删除）

2. 第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于码（**在1NF基础上消除非主属性对主码的部分函数依赖**）

  几个概念：
  1. 函数依赖：A-->B,如果通过A属性(属性组)的值，可以确定唯一B属性的值。则称B依赖于A

  	例如：学号-->姓名。  （学号，课程名称） --> 分数
  2. 完全函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有的属性值。

  	例如：（学号，课程名称） --> 分数
  3. **部分函数依赖**：A-->B， 如果A是一个属性组，则B属性值得确定只需要依赖于A属性组中某一些值即可。

  	例如：（学号，课程名称） -- > 姓名
  4. **传递函数依赖**：A-->B, B -- >C . 如果通过A属性(属性组)的值，可以确定唯一B属性的值，在通过B属性（属性组）的值可以确定唯一C属性的值，则称 C 传递函数依赖于A

  	例如：学号-->系名，系名-->系主任
  5. **码：**如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性(属性组)为该表的码

  	例如：该表中码为：（学号，课程名称）
  	* 主属性：码属性组中的所有属性
  	* 非主属性：除过码属性组的属性

  存在的问题

  - 数据添加存在问题（添加独立的单元格数据问题）
  - 删除数据存在问题（相关联的行数据全被删除）

3. 第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（**在2NF基础上消除传递依赖**）

# JDBC

数据库存储技术：

- JDBC 直接访问
- JDO技术
- 第三方O/R工具,Mybatis

## 什么是JDBC

JDBC：Java Database Connectivity

**它是java应用程序与数据库之间的桥梁，是一组通用的接口**

1. 概念：Java DataBase Connectivity  Java 数据库连接， Java语言操作数据库
	* JDBC本质：其实是官方（sun公司）定义的**一套操作所有关系型数据库的规则**，即接口**。各个数据库厂商去实现这套接口**，提供数据库驱动jar包。**我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类。**


## 快速入门

### 步骤

1. 导入驱动jar包 mysql-connector-java-5.1.37-bin.jar
	1.复制mysql-connector-java-5.1.37-bin.jar到项目的libs目录下
	2.右键.jar包-->Add As Library
	
2. 注册驱动加载驱动方法

   ```java
   Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
   
   DriverManager.registerDriver(new com.mysql.jdbc.Driver());
   
   System.setProperty("jdbc.drivers", "com.mysql.jdbc.Driver");
   ```

3. 获取数据库连接对象 Connection

4. 定义sql

5. 获取执行sql语句的对象 Statement

6. 执行sql，接受返回结果

7. 处理结果

8. 释放资源

### 代码实现

​	

```java
package com.it.jdbc;

import com.mysql.jdbc.Driver;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCdemo1 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1、导入mysql.jar
        //2、注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //3、获取数据库连接对象
        Connection cnn = DriverManager.getConnection("jdbc:mysql://localhost:3306/girls", "root", "password");

        //4、定义sql语句
        String sql = "UPDATE `beauty` SET name ='华为' WHERE id = 1";

        //5、获取数据库对象
        Statement s = cnn.createStatement();

        //6、执行
        s.execute(sql);

        //7、释放
        s.close();
        cnn.close();
    }

}
```



```java
package cn.itcast.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

/**
 * JDBC快速入门
 */
public class JdbcDemo1 {
    public static void main(String[] args) throws Exception {

        //1. 导入驱动jar包
        //2.注册驱动
        // Class.forName("com.mysql.jdbc.Driver");
        //3.获取数据库连接对象
//        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root", "root");
        Connection conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
        //4.定义sql语句
//        String sql = "update account set balance = 2000 where id = 1";
        String sql = "update account set balance = 2000";
        //5.获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        //6.执行sql
        int count = stmt.executeUpdate(sql);
        //7.处理结果
        System.out.println(count);
        //8.释放资源
        stmt.close();
        conn.close();

    }
}

```

## 对象详解

详解各个对象：

### DriverManager：驱动管理对象
* 功能：
  		1. 注册驱动：告诉程序该使用哪一个数据库驱动jar
    			static void registerDriver(Driver driver) :注册与给定的驱动程序 DriverManager 。 
    			写代码使用：  Class.forName("com.mysql.jdbc.Driver");
    			**通过查看源码发现：在com.mysql.jdbc.Driver类中存在静态代码块**
    			 static {
    			        try {
    			            java.sql.DriverManager.registerDriver(new Driver());
    			        } catch (SQLException E) {
    			            throw new RuntimeException("Can't register driver!");
    			        }
    				}

   ​    注意：mysql5之后的驱动jar包可以省略注册驱动的步骤。
   ​	2. 获取数据库连接：
   ​		* 方法：static Connection getConnection(String url, String user, String password) 
   ​		* 参数：
   ​			* url：指定连接的路径
   ​				* 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称
   ​				* 例子：jdbc:mysql://localhost:3306/db3
   ​				* 细节：如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称
   ​			* user：用户名
   ​			* password：密码 
   
   
### Connection：数据库连接对象

#### 功能：

   1. **获取执行sql 的对象**
      Statement createStatement()
      PreparedStatement prepareStatement(String sql)  
      
   2. 管理事务：
      开启事务：setAutoCommit(boolean autoCommit) ：**调用该方法设置参数autoCommit为false，即开启事务**       

      提交事务：commit()

      回滚事务：rollback()

### Statement：执行sql的对象

#### 执行sql方法

- boolean execute(String sql) ：可以执行任意的sql 了解 
- int executeUpdate(String sql) ：执行DML（insert、update、delete）语句、**DDL**(create，alter、drop)语句
  **返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 **
  **返回值>0的则执行成功，反之，则失败。**
-  **ResultSet executeQuery(String sql)**  ：执行**DQL**（**select**)语句

 #### 练习：

        1. account表 添加一条记录
        2. account表 修改记录
        3. account表 删除一条记录

```java
		Statement stmt = null;
        Connection conn = null;
        try {
            //1. 注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2. 定义sql
            String sql = "insert into account values(null,'王五',3000)";
            //3.获取Connection对象
            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
            //4.获取执行sql的对象 Statement
            stmt = conn.createStatement();
            //5.执行sql
            int count = stmt.executeUpdate(sql);//影响的行数
            //6.处理结果
            System.out.println(count);
            if(count > 0){
                System.out.println("添加成功！");
            }else{
                System.out.println("添加失败！");
            }

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //stmt.close();
            //7. 释放资源
            //避免空指针异常
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
       
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
```

### ResultSet

执行 ：**ResultSet executeQuery(String sql)**  ：执行**DQL**（**select**)语句

返回 ：结果集对象,封装查询结果

**ResultSet 也是资源同样需要关闭**

#### 方法

* **boolean** next(): 游标向下移动一行，判断当前行是否是最后一行末尾(是否有数据)，如果是，则返回false，如果不是则返回true
* getXxx(参数):**获取数据**
	* Xxx：代表数据类型   如： int getInt() ,	String getString()
	* 参数：
		1. int：代表列的编号,从1开始   如： getString(1)
		2. String：代表列名称。 如： getDouble("balance")
		3. 

#### 步骤

```java
1. 游标向下移动一行
2. 判断是否有数据
3. 获取数据
 //循环判断游标是否是最后一行末尾。
 while(rs.next()){
 //获取数据
 //6.2 获取数据
 int id = rs.getInt(1);
 String name = rs.getString("name");
 double balance = rs.getDouble(3);
 System.out.println(id + "---" + name + "---" + balance);
  }
```

#### 练习

* 定义一个方法，查询emp表的数据将其封装为对象，然后装载集合，返回。
	1. 定义Emp类
	2. 定义方法 public List<Emp> findAll(){}
	3. 实现方法 select * from emp;

```java
//定义Emp类
package com.it.jdbc;

public class Emp {
    private int id;
    private  String name;

    public void setId(int id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```



```java
//1. 定义方法 public List<Emp> findAll(){}
//2. 实现方法 select * from emp;
package com.it.jdbc;
import java.sql.*;
import java.util.LinkedList;
import java.util.List;

public class JDBCdemo1 {
    public static void main(String[] args) {
        System.out.println(JDBCdemo1.FillAll());
    }
    public static List<Emp> FillAll() {
        //1、导入mysql.jar
        //2、注册驱动
        Connection cnn = null;
        Statement s = null;
        ResultSet resultSet = null;
        List<Emp> list = new LinkedList<>();
        try {
            Class.forName("com.mysql.jdbc.Driver");
            //3、获取数据库连接对象
            cnn = DriverManager.getConnection("jdbc:mysql://localhost:3306/girls", "root", "2016cp5979");

            //4、定义sql语句
            String sql = "select * from beauty";

            //5、获取数据库对象
            s = cnn.createStatement();

            //6、执行
            resultSet = s.executeQuery(sql);


            Emp emp = new Emp();

            while (resultSet.next()){
                int id = resultSet.getInt("id");
                String name = resultSet.getString(2);
                emp.setId(id);
                emp.setName(name);
                list.add(emp);
            }
          

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //7、释放
            if(s!=null){
                try {
                    s.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(cnn!=null){
                try {
                    cnn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(resultSet!=null){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return list;
    }
}
```



### PreparedStatement：执行sql的对象

PreparedStatement：执行sql的对象
	1. SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全性问题
		1. 输入用户随便，输入密码：a' or 'a' = 'a
		2. sql：select * from user where username = 'fhdsjkf' and password = 'a' or 'a' = 'a' 
	
2. 解决sql注入问题：使用PreparedStatement对象来解决

3. 预编译的SQL：**参数使用?作为占位符**
4. 步骤：
	1. 导入驱动jar包 mysql-connector-java-5.1.37-bin.jar
	
	2. 注册驱动
	
	3. 获取数据库连接对象 Connection
	
	4. 定义sql
		* 注意：sql的参数使用？作为占位符。 如：select * from user where username = ? and password = ?;
		
	5. 获取执行sql语句的对象 
	
	   ```
	   PreparedStatement  Connection.prepareStatement(String sql) 
	   ```
	
	   然后 给   **？**赋值：
	
	   * 方法： **setXxx(参数1,参数2)**
	   	* 参数1：？的位置编号 从1 开始
	   	* 参数2：？的值
	
	6. 执行sql，接受返回结果，**不需要传递sql语句**
	
	7. 处理结果
	
	9. 释放资源
	
5. 注意：后期都会使用PreparedStatement来完成增删改查的所有操作
	1. 可以防止SQL注入
	2. 效率更高
	3. 

```java
package com.it.jdbc;
        import java.sql.*;
        import java.util.Scanner;

public class JDBCdemo1 {
    public static void main(String[] args) {
        //键盘录入，接受用户名和密码w
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        JDBCdemo1.Register(username,password);

    }

    public static void Register(String username,String password) {
        //1、导入mysql.jar

        PreparedStatement s = null;
        Connection c = null;
        ResultSet resultSet = null;

        try {
            c = JDBCUtil.CreateConnection();


            //4、定义sql语句
            String sql = "select * from users where username = ? and password = ?";
            System.out.println(sql);

            s = c.prepareStatement(sql);
            s.setString(1,username);
            s.setString(2,password);

            //6、执行
            resultSet= s.executeQuery();//不需要传参
            if(resultSet.next()){
                System.out.println("密码正确");
            }else {
                System.out.println("密码错误");
            }
        }catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //7、释放
            JDBCUtil.close(s,c,resultSet);
        }

    }
}

```



## 设计JDBC工具类

 **JDBCUtils**

* 目的：简化书写
* 分析：
  1. 注册驱动也抽取
  2. 抽取一个方法获取连接对象
    * 需求：不想传递参数（麻烦），还得保证工具类的通用性。
    * 解决：配置文件
      jdbc.properties
      	url=
      	user=
      	password=

  3. 抽取一个方法释放资源  

### 配置文件

属性值没有引号

```
url =jdbc:mysql://localhost:3306/girls
username = root
password = 2016cd5679
driver = com.mysql.jdbc.Driver
```

###  JDBCUtil

```java
  package com.it.jdbc;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.net.URL;
import java.sql.*;

import java.util.Properties;

public class JDBCUtil {
    private static String url;
    private static String username;
    private static String password;
    private static String driver;
    static {

        try {
            //3.1 读取配置文件
            Properties pro = new Properties();
            //3.2 读取配置文件路径
            //获取src路径下的文件的方式--->ClassLoader 类加载器
            ClassLoader classLoader = JDBCUtil.class.getClassLoader();
            URL res  = classLoader.getResource("jdbc.properties");
            String path = res.getPath();
            pro.load(new FileInputStream(path));
            
            String url = pro.getProperty("url");
            System.out.println(url);
            String username = pro.getProperty("username");
            System.out.println(username);
            String password = pro.getProperty("password");
            System.out.println(password);
            String driver = pro.getProperty("driver");
            //加载驱动
            Class.forName(driver);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }


    public static Connection CreateConnection() throws SQLException {

        return DriverManager.getConnection(url,username,password);

    }

    public static void close(Statement s,Connection c){
        if(s != null){
            try {
                s.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(c != null){
            try {
                c.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    public static void close(Statement s, Connection c, ResultSet r){
        if(s != null){
            try {
                s.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(c != null){
            try {
                c.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(r!=null){
            try {
                r.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

}

```

### 练习

* 需求：

  通过键盘录入用户名和密码

  判断用户是否登录成功
  * select * from user where username = "" and password = "";
  * 如果这个sql有查询结果，则成功，反之，则失败

- 创建表：

```mysql
		   创建数据库表 user
			CREATE TABLE USER(
				id INT PRIMARY KEY AUTO_INCREMENT,
				username VARCHAR(32),
				PASSWORD VARCHAR(32)
			);

			INSERT INTO USERs VALUES(NULL,'zhangsan','123');
			INSERT INTO USERs VALUES(NULL,'lisi','234');	
```

- 
  代码实现


```java
package com.it.jdbc;
import java.sql.*;
import java.util.Scanner;

public class JDBCdemo1 {
    public static void main(String[] args) {
        //键盘录入，接受用户名和密码w
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        JDBCdemo1.Register(username,password);

    }

    public static void Register(String username,String password) {
        //1、导入mysql.jar

        Statement s = null;
        Connection c = null;

        try {
            c = JDBCUtil.CreateConnection();

            //4、定义sql语句
            String sql = "select * from users where username = '"+username+"' and password = '"+password+"' ";
            System.out.println(sql);

            s = c.createStatement();
            //6、执行
            
            boolean judgment =s.execute(sql);//不可以用这个，会一直true
            if(judgment){
                System.out.println("密码正确");
            }else {
                System.out.println("密码错误");
            }

        }catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //7、释放
            JDBCUtil.close(s,c);
        }

    }
}

```


​				

## JDBC控制事务

1. 事务：一个包含多个步骤的业务操作。如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败。
2. 操作：
	1. 开启事务
	2. 提交事务
	3. 回滚事务
3. 使用Connection对象来管理事务
   	* 开启事务：**setAutoCommit(boolean autoCommit)** ：调用该方法设置参数为false，即开启事务
   		* 在执行sql之前开启事务
   	* 提交事务：commit() 
   		* 当所有sql都执行完提交事务
   	* 回滚事务：rollback() 
   		* 在catch中回滚事务
4. 代码

```java
package com.it.jdbc;
        import java.sql.*;
        import java.util.Scanner;

public class JDBCdemo1 {
    public static void main(String[] args) {
        //键盘录入，接受用户名和密码w
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        JDBCdemo1.Register(username,password);

    }

    public static void Register(String username,String password) {
        //1、导入mysql.jar

        PreparedStatement s = null;
        Connection c = null;


        try {
            c = JDBCUtil.CreateConnection();
            c.setAutoCommit(false);
            //4、定义sql语句
            String sql = "insert into users values(?,?)";
            System.out.println(sql);

            s = c.prepareStatement(sql);
            s.setString(1,username);
            s.setString(2,password);

            //6、执行
            s.executeUpdate();//不需要传参
            
            //提交事务
            //int i = 1/0;
            c.commit();

        }catch (Exception e) {
            try {
                //事务回滚
                c.rollback();
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            e.printStackTrace();
        }finally {
            //7、释放
            JDBCUtil.close(s,c);
        }

    }
}

```

## 数据库连接池

### 概念

其实就是一个容器(集合)，存放数据库连接的容器。
    当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。

2. 好处：
	- 节约资源
	- 用户访问高效


### 实现：

1. 标准接口：DataSource   javax.sql包下的
	1. 方法：
		* 获取连接：**getConnection()**
		* 归还连接：**Connection.close()**。如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，则不会再关闭连接了。而**是归还连接**

2. 一般我们不去实现它，有数据库厂商来实现
	1. C3P0：数据库连接池技术
	2. Druid：数据库连接池实现技术，由阿里巴巴提供的

#### C3P0

##### 步骤

1. 导入jar包 (两个) c3p0-0.9.5.2.jar mchange-commons-java-0.2.12.jar ，
	* 不要忘记导入数据库驱动jar包
2. 定义配置文件：
	* 名称： c3p0.properties 或者 c3p0-config.xml
	* 路径：直接将文件放在src目录下即可。

3. 创建核心对象 数据库连接池对象 ComboPooledDataSource
4. 获取连接： getConnection

##### 代码：

```java
//1.创建数据库连接池对象
  DataSource ds  = new ComboPooledDataSource();
 //2. 获取连接对象
  Connection conn = ds.getConnection();
```

#### Druid

Druid：数据库连接池实现技术，由阿里巴巴提供的

##### 步骤

1. 导入jar包 druid-1.0.9.jar

2. 定义配置文件：
	* 是properties形式的
	* 可以叫任意名称，可以放在任意目录下
	
3. 加载配置文件。Properties

4. **获取数据库连接池对象：通过工厂来来获取  DruidDataSourceFactory**

   ```java
    DataSource ds = DruidDataSourceFactory.createDataSource(pro);
   ```

5. 获取连接：getConnection

##### 代码

```java
  //3.加载配置文件
  Properties pro = new Properties();
  InputStream is =         DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
  pro.load(is);
  //4.获取连接池对象
  DataSource ds = DruidDataSourceFactory.createDataSource(pro);
  //5.获取连接
  Connection conn = ds.getConnection();
```

### 定义工具类

1. 定义一个类 JDBCUtils

2. 提供静态代码块加载配置文件，初始化连接池对象

3. 提供方法
	1. 获取连接方法：通过数据库连接池获取连接
	2. 释放资源
	3. 获取连接池的方法
	
	```java
	
	
	import com.alibaba.druid.pool.DruidDataSourceFactory;
	import javax.sql.DataSource;
	import java.io.IOException;
	import java.io.InputStream;
	import java.sql.*;
	
	import java.util.Properties;
	
	public class JDBCUtil {
	    public static DataSource ds = null;
	
	    static {
	
	        try {
	            //3.1 读取配置文件
	            Properties pro = new Properties();
	            //3.2 读取配置文件路径
	            //获取src路径下的文件的方式--->ClassLoader 类加载器
	
	            InputStream fs = JDBCUtil.class.getClassLoader().getResourceAsStream("druid.properties");
	
	            pro.load(fs);
	            //4、 获取连接池
	            ds = DruidDataSourceFactory.createDataSource(pro);
	
	        } catch (IOException e) {
	            e.printStackTrace();
	        } catch (ClassNotFoundException e) {
	            e.printStackTrace();
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
	    }
	
	    public static Connection CreateConnection() throws SQLException {
	
	        return ds.getConnection();
	
	    }
	
	    public static void close(Statement s,Connection c){
	        close(s,c,null);
	    }
	
	    public static void close(Statement s, Connection c, ResultSet r){
	        if(s != null){
	            try {
	                s.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	
	        if(c != null){
	            try {
	                c.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	
	        if(r!=null){
	            try {
	                r.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	    }
	
	    // 获取连接池
	
	    public static DataSource getDs() {
	        return ds;
	    }
	}
	
	```
	

### SpringJDBC

Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发

#### 步骤

1. 导入jar包
2. 创建JdbcTemplate对象。依赖于数据源DataSource
	* JdbcTemplate template = new JdbcTemplate(ds);

3. 调用JdbcTemplate的方法来完成CRUD的操作
	* **update():**执行DML语句。增、删、改语句
	* **queryForMap()**:查询结果将结果集封装为map集合，将列名作为key，将值作为value 将这条记录封装为一个map集合
		* 注意：这个方法查询的结果集长度只能是1
	* **queryForList()**:查询结果将结果集封装为list集合
		* 注意：将每一条记录封装为一个Map集合，再将Map集合装载到List集合中
	* **query()**:查询结果，**将结果封装为JavaBean对象**
		* query的参数：RowMapper
			* 一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
			* new BeanPropertyRowMapper<类型>(类型.class)//**类型中的成员变量必须和变种的列名一致**
	* **queryForObject**：查询结果，将结果封装为对象
		* 一般用于聚合函数的查询

#### 举例

* 需求：
	1. 修改1号数据的 salary 为 10000
	2. 添加一条记录
	3. 删除刚才添加的记录
	4. 查询id为1的记录，将其封装为Map集合
	5. 查询所有记录，将其封装为List
	6. 查询所有记录，将其封装为Emp对象的List集合
	7. 查询总记录数

* 代码：
	
	```java
	package com.it.jdbc;
	
	
	import org.junit.Test;
	import org.springframework.jdbc.core.BeanPropertyRowMapper;
	import org.springframework.jdbc.core.JdbcTemplate;
	
	import java.sql.*;
	import java.util.Iterator;
	
	import java.util.List;
	import java.util.Map;
	
	
	public class TestJDBC {
	    public static JdbcTemplate template;
	    static {
	        template = new JdbcTemplate(JDBCUtil.getDs());
	    }
	
	    @Test
	    public void test1(){
	
	        String sql = "Update `users` Set username = '郭德纲' where username = '王五' ";
	        int count = template.update(sql);
	        System.out.println(count);
	
	    }
	    @Test
	    public void test2(){
	
	        String sql = "INSERT into users VALUE ('张建华',1000)";
	        int count = template.update(sql);
	        System.out.println(count);
	
	    }
	@Test
	    public void test3(){
	
	        String sql = "Delete from users where username ='张建华'";
	        int count = template.update(sql);
	        System.out.println(count);
	
	    }
	    @Test
	    public void test4(){
	
	        String sql = "SELECT * from users WHERE username = '张三'";
	        Map<String,Object> map = template.queryForMap(sql);
	        System.out.println(map);
	
	    }
	
	    @Test
	    public void test5(){
	
	        String sql = "SELECT * from users WHERE username = '张三'";
	        Map<String,Object> map = template.queryForMap(sql);
	        System.out.println(map);
	
	    }
	
	
	    @Test
	    public void test6(){
	
	        String sql = "SELECT * from users ";
	        List<Map<String,Object>> list= template.queryForList(sql);
	        Iterator iterator = list.iterator();
	        while(iterator.hasNext()){
	            System.out.println(iterator.next());
	        }
	        System.out.println("***********************************");
	        for(Map<String,Object> map:list){
	            System.out.println(map);
	        }
	    }
	    @Test
	    public void test7(){
	
	        String sql = "SELECT * from users ";
	
	        List<Emp> list= template.query(sql,new BeanPropertyRowMapper<Emp>(Emp.class));
	        //Emp中的成员变量必须和变种的列名一致
	        Iterator iterator = list.iterator();
	        while(iterator.hasNext()){
	            System.out.println(iterator.next());
	        }
	        System.out.println("***********************************");
	        for(Emp map:list){
	            System.out.println(map);
	        }
	    }
	
	    @Test
	    public void mysql(){
	        Statement stmt = null;
	        Connection conn = null;
	        try {
	            //1. 注册驱动
	            Class.forName("com.mysql.jdbc.Driver");
	            //2. 定义sql
	           // String sql = "CREATE TABLE `users`(`username` VARCHAR(255) , `password` int )";
	//            CREATE TABLE `user`(
	//                    id INT,
	//                    `name` VARCHAR(255),
	//                    `birthday` DATE)
	            String sql = "insert into users values('王五',3000)";
	            //3.获取Connection对象
	            conn = JDBCUtil.CreateConnection();
	            //4.获取执行sql的对象 Statement
	            stmt = conn.createStatement();
	            //5.执行sql
	            int count = stmt.executeUpdate(sql);//影响的行数
	            //6.处理结果
	            System.out.println(count);
	
	            if(count >= 0){
	                System.out.println("添加成功！");
	            }else{
	                System.out.println("添加失败！");
	            }
	
	        } catch (ClassNotFoundException e) {
	            e.printStackTrace();
	        } catch (SQLException e) {
	            e.printStackTrace();
	        }finally {
	            //stmt.close();
	            //7. 释放资源
	            //避免空指针异常
	            JDBCUtil.close(stmt,conn);
	        }
	    }
	
	    @Test
	    public void test8(){
	        String sql = "select count(id) from emp";
	        Long total = template.queryForObject(sql, Long.class);
	        System.out.println(total);
	    }
	
	}
	
	}
	
	```

# Redis

## 概念

redis是一款高性能的NOSQL系列的非关系型数据库

### NOSQL

NoSQL(NoSQL = Not Only SQL)，意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。
		随着互联网web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。

#### 和关系型数据库比较

##### 优点

​				1）成本：nosql数据库简单易部署，基本都是开源软件，不需要像使用oracle那样花费大量成本购买使用，相比关系型数据库价格便宜。
​				2）查询速度：nosql数据库将数据存储于缓存之中，关系型数据库将数据存储在硬盘中，自然查询速度远不及nosql数据库。
​				3）存储数据的格式：nosql的存储格式是key,value形式、文档形式、图片形式等等，所以可以存储基础类型以及对象或者是集合等各种格式，而数据库则只支持基础类型。
​				4）扩展性：关系型数据库有类似join这样的多表查询机制的限制导致扩展很艰难。

##### 缺点

​			1）维护的工具和资料有限，因为nosql是属于新的技术，不能和关系型数据库10几年的技术同日而语。
​			2）不提供对sql的支持，如果不支持sql这样的工业标准，将产生一定用户的学习和使用成本。
​			3）不提供关系型数据库对事务的处理。

### 非关系型数据库的优势

​			1）性能NOSQL是基于键值对的，可以想象成表中的主键和值的对应关系，而且不需要经过SQL层的解析，所以性能非常高。
​			2）可扩展性同样也是因为基于键值对，数据之间没有耦合性，所以非常容易水平扩展。



#### 关系型数据库的优势：

​			1）复杂查询可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询。
​			2）事务支持使得对于安全性能很高的数据访问要求得以实现。对于这两类数据库，对方的优势就是自己的弱势，反之亦然。

#### 总结

​			关系型数据库与NoSQL数据库并非对立而是互补的关系，即通常情况下使用关系型数据库，在适合使用NoSQL的时候使用NoSQL数据库，
​			让NoSQL数据库对关系型数据库的不足进行弥补。
​			一般会将数据存储在关系型数据库中，在nosql数据库中备份存储关系型数据库的数据



### 主流的NOSQL产品

​		•	键值(Key-Value)存储数据库
​				相关产品： Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
​				典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
​				数据模型： 一系列键值对
​				优势： 快速查询
​				劣势： 存储的数据缺少结构化
​		•	列存储数据库
​				相关产品：Cassandra, HBase, Riak
​				典型应用：分布式的文件系统
​				数据模型：以列簇式存储，将同一列数据存在一起
​				优势：查找速度快，可扩展性强，更容易进行分布式扩展
​				劣势：功能相对局限
​		•	文档型数据库
​				相关产品：CouchDB、MongoDB
​				典型应用：Web应用（与Key-Value类似，Value是结构化的）
​				数据模型： 一系列键值对
​				优势：数据结构要求不严格
​				劣势： 查询性能不高，而且缺乏统一的查询语法
​		•	图形(Graph)数据库
​				相关数据库：Neo4J、InfoGrid、Infinite Graph
​				典型应用：社交网络
​				数据模型：图结构
​				优势：利用图结构相关算法。
​				劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。

### Redis

​		Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库，官方提供测试数据，50个并发执行100000个请求,读的速度是110000次/s,写的速度是81000次/s ，且Redis通过提供多种键值数据类型来适应不同场景下的存储需求，目前为止Redis支持的键值数据类型如下：
   			1) 字符串类型 string
  			2) 哈希类型 hash
 			3) 列表类型 list
​			4) 集合类型 set
​			5) 有序集合类型 sortedset

### redis的应用场景

•	缓存（数据查询、短连接、新闻内容、商品内容等等）
•	聊天室的在线好友列表
•	任务队列。（秒杀、抢购、12306等等）
•	应用排行榜
•	网站访问统计
•	数据过期处理（可以精确到毫秒
•	分布式集群架构中的session分离



## 下载

1. 官网：https://redis.io
2. 中文网：http://www.redis.net.cn/
3. 解压直接可以使用：
  * redis.windows.conf：配置文件
  * redis-cli.exe：redis的客户端
  * redis-server.exe：redis服务器端



## 命令操作

#### redis的数据结构：

redis存储的是：key,value格式的数据，其中key都是字符串，value有5种不同的数据结构
* value的数据结构：
		1) 字符串类型 string
		2) 哈希类型 hash ： map格式  
		3) 列表类型 list ： linkedlist格式。支持重复元素
		4) 集合类型 set  ： 不允许重复元素
		5) 有序集合类型 sortedset：不允许重复元素，且元素有顺序

#### 字符串类型 string

1. 存储： set key value
	127.0.0.1:6379> set username zhangsan
	OK
2. 获取： get key
	127.0.0.1:6379> get username
	"zhangsan"
3. 删除： del key
	127.0.0.1:6379> del age
	(integer) 1

3. 哈希类型 hash
	1. 存储： hset key field value
		127.0.0.1:6379> hset myhash username lisi
		(integer) 1
		127.0.0.1:6379> hset myhash password 123
		(integer) 1
	2. 获取： 
		* hget key field: 获取指定的field对应的值
			127.0.0.1:6379> hget myhash username
			"lisi"
		* hgetall key：获取所有的field和value
			127.0.0.1:6379> hgetall myhash
			1) "username"
			2) "lisi"
			3) "password"
			4) "123"
		
	3. 删除： hdel key field
		127.0.0.1:6379> hdel myhash username
		(integer) 1

#### 列表类型 list

可以添加一个元素到列表的头部（左边）或者尾部（右边）

1. 添加：
	1. lpush key value: 将元素加入列表左表
		
	2. rpush key value：将元素加入列表右边
		
		127.0.0.1:6379> lpush myList a
		(integer) 1
		127.0.0.1:6379> lpush myList b
		(integer) 2
		127.0.0.1:6379> rpush myList c
		(integer) 3
2. 获取：
	* lrange key start end ：范围获取
		127.0.0.1:6379> lrange myList 0 -1
		1) "b"
		2) "a"
		3) "c"
3. 删除：
	* lpop key： 删除列表最左边的元素，并将元素返回
	* rpop key： 删除列表最右边的元素，并将元素返回

#### 集合类型 set 

**不允许重复元素**

	1. 存储：sadd key value
		127.0.0.1:6379> sadd myset a
		(integer) 1
		127.0.0.1:6379> sadd myset a
		(integer) 0
	2. 获取：smembers key:获取set集合中所有元素
		127.0.0.1:6379> smembers myset
		1) "a"
	3. 删除：srem key value:删除set集合中的某个元素	
		127.0.0.1:6379> srem myset a
		(integer) 1

#### 有序集合类型 sortedset

不允许重复元素，且元素有顺序.每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

1. 存储：zadd key score value
	127.0.0.1:6379> zadd mysort 60 zhangsan
	(integer) 1
	127.0.0.1:6379> zadd mysort 50 lisi
	(integer) 1
	127.0.0.1:6379> zadd mysort 80 wangwu
	(integer) 1
2. 获取：zrange key start end [withscores]
	127.0.0.1:6379> zrange mysort 0 -1
	1) "lisi"
	2) "zhangsan"
	3) "wangwu"

	127.0.0.1:6379> zrange mysort 0 -1 withscores
	1) "zhangsan"
	2) "60"
	3) "wangwu"
	4) "80"
	5) "lisi"
	6) "500"
3. 删除：zrem key value
	127.0.0.1:6379> zrem mysort lisi
	(integer) 1

#### 通用命令

  		1. keys * : 查询所有的键
  	
  	2. type key ： 获取键对应的value的类型
  	3. del key：删除指定的key value

## 持久化

redis是一个内存数据库，当redis服务器重启，获取电脑重启，数据会丢失，我们可以将redis内存中的数据持久化保存到硬盘的文件中。

### 持久化机制

#### RDB

默认方式，不需要进行配置，默认就使用这种机制

* 在一定的间隔时间中，检测key的变化情况，然后持久化数据
1. 编辑redis.windwos.conf文件
	
	after 900 sec (15 min) if at least 1 key changed
	
	save 900 1
	
	after 300 sec (5 min) if at least 10 keys changed
	
	save 300 10
	
	after 60 sec if at least 10000 keys changed
	
	save 60 10000
	
2. 重新启动redis服务器，并指定配置文件名称
    	D:\JavaWeb2018\day23_redis\资料\redis\windows-64\redis-2.8.9>redis-server.exe redis.windows.conf	
    	

#### AOF

日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据

1. 编辑redis.windwos.conf文件
	appendonly no（关闭aof） --> appendonly yes （开启aof）
	
	appendfsync always ： 每一次操作都进行持久化
	
	appendfsync everysec ： 每隔一秒进行一次持久化
	
	appendfsync no	 ： 不进行持久化



## Java客户端 Jedis

* Jedis: 一款java操作redis数据库的工具.
* 使用步骤：
	1. 下载jedis的jar包
	2. 使用

```java
	//1. 获取连接
	Jedis jedis = new Jedis("localhost",6379);
    //2. 操作
    jedis.set("username","zhangsan");
	//3. 关闭连接
	jedis.close();
```

### 操作各种redis中的数据结构

#### 字符串类型 string

​		set
​		get

    //1. 获取连接
    Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
    //2. 操作
    //存储
    jedis.set("username","zhangsan");
    //获取
    String username = jedis.get("username");
    System.out.println(username);
    //可以使用setex()方法存储可以指定过期时间的 key value
    jedis.setex("activecode",20,"hehe");//将activecode：hehe键值对存入redis，并且20秒后自动删除该键值对
    //3. 关闭连接
    jedis.close();

#### 哈希类型 hash 

map格式 
hset
hget
hgetAll

    	//1. 获取连接
        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
        //2. 操作
        // 存储hash
        jedis.hset("user","name","lisi");
        jedis.hset("user","age","23");
        jedis.hset("user","gender","female");
    
        // 获取hash
        String name = jedis.hget("user", "name");
        System.out.println(name);
         // 获取hash的所有map中的数据
        Map<String, String> user = jedis.hgetAll("user");
    	
        // keyset
        Set<String> keySet = user.keySet();
        for (String key : keySet) {
        //获取value
        String value = user.get(key);
        System.out.println(key + ":" + value);
        }
    	
        //3. 关闭连接
        jedis.close();



#### 列表类型 list 

 linkedlist格式。支持重复元素
			lpush / rpush
			lpop / rpop
			lrange start end : 范围获取

			 //1. 获取连接
	        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
	        //2. 操作
	        // list 存储
	        jedis.lpush("mylist","a","b","c");//从左边存
	        jedis.rpush("mylist","a","b","c");//从右边存
	
	        // list 范围获取
	        List<String> mylist = jedis.lrange("mylist", 0, -1);
	        System.out.println(mylist);
	        
	        // list 弹出
	        String element1 = jedis.lpop("mylist");//c
	        System.out.println(element1);
	
	        String element2 = jedis.rpop("mylist");//c
	        System.out.println(element2);
	
	        // list 范围获取
	        List<String> mylist2 = jedis.lrange("mylist", 0, -1);
	        System.out.println(mylist2);
	
	        //3. 关闭连接
	        jedis.close();

####  集合类型 set  

 不允许重复元素
			sadd
			smembers:获取所有元素

			//1. 获取连接
	        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
	        //2. 操作
	        // set 存储
	        jedis.sadd("myset","java","php","c++");
		
	   		 // set 获取
	         Set<String> myset = jedis.smembers("myset");
	         System.out.println(myset);
		
	    	//3. 关闭连接
	    	jedis.close();

有序集合类型 sortedset：不允许重复元素，且元素有顺序
			zadd
			zrange

			//1. 获取连接
	        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
	        //2. 操作
	        // sortedset 存储
	        jedis.zadd("mysortedset",3,"亚瑟");
	        jedis.zadd("mysortedset",30,"后裔");
	        jedis.zadd("mysortedset",55,"孙悟空");
	
	        // sortedset 获取
	        Set<String> mysortedset = jedis.zrange("mysortedset", 0, -1);
	
	        System.out.println(mysortedset);
	        //3. 关闭连接
		        jedis.close();

### jedis连接池：JedisPool

#### 使用

  1. 创建JedisPool连接池对象
 2. 调用方法 getResource()方法获取Jedis连接

		       //0.创建一个配置对象
	   JedisPoolConfig config = new JedisPoolConfig();
	   config.setMaxTotal(50);
	   config.setMaxIdle(10);
	           //1.创建Jedis连接池对象
		        JedisPool jedisPool = new JedisPool(config,"localhost",6379);
		
		        //2.获取连接
		        Jedis jedis = jedisPool.getResource();
		        //3. 使用
		        jedis.set("hehe","heihei");
		        //4. 关闭 归还到连接池中
		        jedis.close();

#### 连接池工具类



```java
public class JedisPoolUtils {
         private static JedisPool jedisPool;
		
		    static{
		        //读取配置文件
		        InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
		        //创建Properties对象
		        Properties pro = new Properties();
		        //关联文件
		        try {
		            pro.load(is);
		        } catch (IOException e) {
		            e.printStackTrace();
		        }
		        //获取数据，设置到JedisPoolConfig中
		        JedisPoolConfig config = new JedisPoolConfig();
		        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
		        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
		
		        //初始化JedisPool
		        jedisPool = new JedisPool(config,pro.getProperty("host"),Integer.parseInt(pro.getProperty("port")));
		        }
		          /**
			     * 获取连接方法
			     */
			    public static Jedis getJedis(){
			        return jedisPool.getResource();
			    }
			}       
```

## 案例：

### 案例需求

	1. 提供index.html页面，页面中有一个省份 下拉列表
	2. 当 页面加载完成后 发送ajax请求，加载所有省份



### 注意

使用redis缓存一些不经常发生变化的数据。
 * 数据库的数据一旦发生改变，则需要更新缓存。
* 数据库的表执行 增删改的相关操作，需要将redis缓存数据情况，再次存入
* 在service对应的增删改方法中，将redis数据删除。

### 实例

#### 页面

```xml
        <div class="form-group">
            <label for="address">籍贯：</label>
            <select name="address" class="form-control" id="jiguan">

            </select>
        </div>
```



```js
<script>
        $(function () {

            //发送ajax请求，加载所有省份数据
            $.get("provinceServlet",{},function (data) {
                //[{"id":1,"name":"北京"},{"id":2,"name":"上海"},{"id":3,"name":"广州"},{"id":4,"name":"陕西"}]

                //1.获取select
                var province = $("#jiguan");
                //2.遍历json数组
                $(data).each(function () {
                    //3.创建<option>
                    var option = "<option name='"+this.id+"'>"+this.name+"</option>";

                    //4.调用select的append追加option
                    province.append(option);
                });
            });

        });
    </script>
```

#### servlet

```java
@WebServlet("/provinceServlet")
public class ProvinceServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       /* //1.调用service查询
        ProvinceService service = new ProvinceServiceImpl();
        List<Province> list = service.findAll();
        //2.序列化list为json
        ObjectMapper mapper = new ObjectMapper();
        String json = mapper.writeValueAsString(list);*/

        //1.调用service查询
        ProvinceService service = new ProvinceServiceImpl();
        String json = service.findAllJson();


        System.out.println(json);
        //3.响应结果
        response.setContentType("application/json;charset=utf-8");
        response.getWriter().write(json);

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

#### service

关键技术

```java
 Jedis jedis = JedisPoolUtils.getJedis();
 String province_json = jedis.get("province");
 
List<Province> ps = userDaoImp.findAllProvince();
            //将数据转化成(map类型字符串)json
ObjectMapper mapper = new ObjectMapper();

province_json = mapper.writeValueAsString(ps);

//数据存入redis
jedis.set("province",province_json);
//归还连接
jedis.close();
```



```JAVA
  public String findAllProvince(){
        //先看jedis中是否有province数据
        Jedis jedis = JedisPoolUtils.getJedis();
        String province_json = jedis.get("province");

         //如果 province_json 为null 或者长度为空
        if(province_json==null||province_json.length()==0){
            System.out.println("redis中没有数据,从数据库中查询");

            List<Province> ps = userDaoImp.findAllProvince();
            //将数据转化成(map类型字符串)json
            ObjectMapper mapper = new ObjectMapper();
            try {
                province_json = mapper.writeValueAsString(ps);
            } catch (JsonProcessingException e) {
                e.printStackTrace();
            }
            //数据存入redis
            jedis.set("province",province_json);
            //归还连接
            jedis.close();
        }
        System.out.println("redis中有数据");
        return  province_json;
    }
```

