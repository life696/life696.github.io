## MySQL

[TOC]

### 数据库

crud(create、read、update、delete)：增、查、改、删 

### SQLite

SQLite是嵌入式的小型数据库，应用在手机端



### MySQL

#### MySQL安装与使用

MySQL是开源免费的数据库，小型的数据库

MySQL中管理员的用户名是root

MySQL默认的端口是3306

安装之后，启动、关闭MySQL服务，在cmd窗口执行以下命令

> net start mysql       启动
> net stop mysql        关闭

查看数据库是否启动服务：

- 在cmd窗口执行完启动命令后可以看到启动服务成功
- 控制面板->管理工具->服务->MySQL，可以查看状态（可以启动和关闭）

##### 命令行登陆mysql        

- 方式1：使用用户名和密码登陆，注意u和p后面没有空格，规则和示例如下

  > mysql -u用户名 -p密码
  >
  > mysql -uroot -proot   

- 方式2：先输入用户名，后输入密码，规则和示例如下

  > mysql -u用户名 -p

- 方式3：使用地址登陆，127.0.0.1是本机地址，规则和示例如下

  > mysql -hip地址 -u用户名 -p密码
  >
  > mysql -hip127.0.0.1 -uroot -proot

- 方式4：规则和示例如下

  > mysql --host=ip地址 --user=用户名 --password=密码
  >
  > mysql --host=127.0.0.1 --user=root --password=root

##### 退出mysql

> quit 或 exit



##### MySQL目录结构

bin目录：MySQL相关的可执行文件

data目录：系统必须的数据库所在位置

my.ini文件：mysql的配置文件，一般不修改

C:\ProgramData\MySQL\MySQL Server 5.5\data：自己简历的数据库的存储位置



##### 数据库的结构

一个数据库服务器包含多个库
一个数据库包含多张表
一张表包含多条记录



#### 图形化工具

SQLyog是另一个软件，可以图形化操作MySQL数据库



#### SQL

SQL是Structured Query Language结构化查询语言：

- 是一种所有关系型数据库的查询规范，不同的数据库都支持。
- 通用的数据库操作语言，可以用在不同的数据库中。
- 不同的数据库 SQL 语句有一些区别

#### MySQL语法

##### 基础语法

- 每条语句以分号结尾，如果在 SQLyog 中不是必须加的。
- SQL 中不区分大小写，关键字中认为大写和小写是一样的 

##### 注释

- --空格：单行注释
- /*  */ ：多行注释
- 井号 ：这是 mysql 特有的注释方式

##### 创建数据库

直接创建数据库

```mysql
CREATE DATABASE 数据库名;
```

判断数据库是否已经存在，不存在则创建数据库

```mysql
CREATE DATABASE IF NOT EXISTS 数据库名;
```

创建数据库并指定字符集

```mysql
CREATE DATABASE 数据库名 CHARACTER SET 字符集;
```

##### 查看数据库

查看所有数据库

```mysql
show databases;
```

 查看某个数据库的定义信息

```mysql
show create database db3;
```

##### 修改数据库

修改数据库默认的字符集

```mysql
ALTER DATABASE 数据库名 DEFAULT CHARACTER SET 字符集;

alter database db3 character set utf8;
```

##### 删除数据库

```mysql
DROP DATABASE 数据库名;

drop database db2;
```

##### 使用数据库

查看正在使用的数据库 

```mysql
SELECT DATABASE();       --使用的一个 mysql 中的全局函数
```

使用/切换数据库

```mysql
USE 数据库名;
```

#### DDL 操作表结构

前提先使用某个数据库

##### 创建表

CREATE：创建

TABLE：表

```mysql
CREATE TABLE  表名  (
    字段名 1  字段类型 1, 
    字段名 2  字段类型 2
);
```

新建表详细说明：

```
create table students(

	id int unsigned not null auto_increment primary key,
	name varchar(30),
	age tinyint unsigned default 0,
	high decimal(5,2),
	gender enum("male", "female","neutral", "unknown") default "unknown"
 );

```

> auto_increment：表示自动增长
>
> not null：表示不能为空
>
> primary key：表示主键
>
> default：表示默认值

##### MySQL 数据类型

常使用数据类型如下：

- int：整型
- double：浮点型
- varchar：字符串型
- date：日期类型，格式为yyyy-MM-dd，只有年月日没有时分秒

详细如下：

- 整型
  - tinyInt：微整型，占很小的整数，8位二进制
  - smallint：小整型，小的整数，占16位二进制
  - mediumint：中整型，中等长度的整数，占24位二进制
  - int(integer)：整型，整数类型，占32位二进制
- 小数
  - float：等你精度浮点数，4字节
  - double：双精度浮点型，8字节
- 日期
  - time：表示时间类型
  - data：表示日期类型
  - datetime：同时可以表示日期和事件类型
- 字符串
  - char(m)：**固定**长度的字符串，无论使用几个字符，都占满全部，M为0-255之间的整数
  - varchar(m)：**可变**长度字符串，使用几个字符就占用几个， M 为0~65535 之间的整数
- 大二进制
  - tinyblob Big Large Object：允许长度 0~255 字节
  - blob：允许长度 0~65535 字节
  - mediumblob：允许长度 0~167772150 字节
  - longblob：允许长度 0~4294967295 字节
- 大文本
  - tinytext：允许长度 0~255 字节
  - text：允许长度 0~65535 字节
  - mediumtext：允许长度 0~167772150 字节
  - longtext：允许长度 0~4294967295 字节

##### 创建表的示例

```mysql
create table student (
id int,        --  整数
name varchar(20),  --  字符串
birthday date --  生日，最后没有逗号
);
```

创建一个表，表中有int类型的id，varchar类型的name，date类型的birthday

##### 查看表

查看某个数据库中的所有表

```mysql
SHOW TABLES;
```

查看表结构

```mysql
DESC 表名;
```

查看创建表的 SQL 语句

```mysql
SHOW CREATE TABLE 表名;
```

##### 快速创建一个表结构相同的表

```mysql
CREATE TABLE 新表名 LIKE 旧表名;
```

##### 删除表

直接删除表 

```mysql
DROP TABLE 表名;
```

判断表是否存在，如果存在则删除表

```mysql
DROP TABLE IF EXISTS 表名;
```

##### 修改表结构

添加表列 ADD

```mysql
ALTER TABLE 表名 ADD 列名 类型;
```

修改列类型 MODIFY

```mysql
ALTER TABLE 表名 MODIFY 列名 新的类型;
```

修改列名  CHANGE

```mysql
ALTER TABLE 表名 CHANGE 旧列名 新列名 类型;
```

删除列  DROP

```mysql
ALTER TABLE 表名 DROP 列名;
```

##### 修改表名

```mysql
RENAME TABLE 表名 TO 新表名;
```

##### 修改字符集 character set

```mysql
ALTER TABLE 表名 character set 字符集;
```

#### DML 操作表中的数据

用于对表中的记录进行增删改操作

##### 插入记录

```mysql
INSERT [INTO]  表名  [字段名] VALUES (字段值)
```

> INSERT INTO  表名：表示往哪张表中添加数据
> (字段名 1,  字段名 2, …)：要给哪些字段设置值
> VALUES (值 1,  值 2, …)：设置具体的值

##### 插入全部字段

所有的字段名都写出来

```mysql
INSERT INTO 表名 (字段名 1, 字段名 2, 字段名 3…) VALUES (值 1, 值 2, 值 3);
```

不写字段名 

```mysql
INSERT INTO 表名 VALUES (值 1, 值 2, 值 3…);
```

插入部分数据

```mysql
INSERT INTO 表名 (字段名 1, 字段名 2, ...) VALUES (值 1, 值 2, ...);
```

注：没有添加数据的字段会使用 NULL

##### DOS 命令窗口操作数据乱码问题的解决

使用 DOS 命令行进行 SQL 语句操作如有有中文会出现乱码

insert 的注意事项：

- 插入的数据应与字段的数据类型相同
- 数据的大小应在列的规定范围内，例如：不能将一个长度为 80 的字符串加入到长度为 40 的列中。
- 在 values 中列出的数据位置必须与被加入的列的排列位置相对应。在 mysql 中可以使用 value， 但不建议使用， 功能与 values 相同。
- 字符和日期型数据应包含在单引号中。MySQL 中也可以使用双引号做为分隔符。
- 不指定列或使用 null，表示插入空值

乱码产生的原因：客户端编码GBK，服务器编码UTF-8

###### 查看MySQL内部设置的编码

查看包含 character 开头的全局变量

```mysql
show variables like 'character%';
```

###### 解决方案

修改 client、connection、results 的编码为 GBK，保证和 DOS 命令行编码保持一致

单独设置：

- 修改客户端的字符集为 GBK：set character_set_client=gbk; 
- 修改连接的字符集为 GBK：set character_set_connection=gbk;
- 修改查询的结果字符集为 GBK：set character_set_results=gbk;

同时设置：

- set names gbk;

注意：退出 DOS 命令行就失效了，需要每次都配置

##### 蠕虫复制

将一张已经存在的表中的数据复制到另一张表中

将表名 2 中的所有的列复制到表名 1 中

```mysql
INSERT INTO  表名 1 SELECT * FROM  表名 2;
```

只复制部分列

```mysql
INSERT INTO  表名 1(列 1,  列 2) SELECT  列 1,  列 2 FROM student;
```

##### 更新表记录

```mysql
UPDATE  表名  SET  列名=值  [WHERE  条件表达式]
```

- UPDATE:  需要更新的表名
- SET:  修改的列值
- WHERE:  符合条件的记录才更新

你可以同时更新一个或多个字段。你可以在  WHERE  子句中指定任何条件。

###### 不带条件修改数据 

```mysql
UPDATE 表名 SET 字段名=值;  -- 修改所有的行
```

###### 带条件修改数据

```mysql
UPDATE 表名 SET 字段名=值 WHERE 字段名=值;
```

##### 删除表记录

```mysql
DELETE FROM  表名  [WHERE  条件表达式]
```

如果没有指定  WHERE  子句，MySQL  表中的所有记录将被删除。可以在  WHERE  子句中指定任何条件

###### 不带条件删除数据

```mysql
DELETE FROM 表名; 
```

###### 带条件删除数据

```mysql
DELETE FROM 表名 WHERE 字段名=值;
```

###### 使用 truncate 删除表中所有记录

```mysql
TRUNCATE TABLE 表名;
```

###### truncate 和 delete 的区别

truncate 相当于删除表的结构，再创建一张表。

#### DQL 查询表中的数据

查询不会对数据库中的数据进行修改.只是一种显示数据的方式，格式如下：

```mysql
SELECT  列名  FROM  表名  [WHERE  条件表达式] 
```

- SELECT  命令可以读取一行或者多行记录。
- 你可以使用星号（*）来代替其他字段，SELECT 语句会返回表的所有字段数据
- 你可以使用  WHERE  语句来包含任何条件。

##### 简单查询

###### 查询表所有行和列的数据

使用*表示所有列

```mysql
SELECT * FROM 表名;
```

###### 查询指定列

查询指定列的数据,多个列之间以逗号分隔 

```mysql
SELECT 字段名 1, 字段名 2, 字段名 3, ... FROM 表名;
```

##### 指定列的别名进行查询

###### 使用关键字

使用别名的好处：  显示的时候使用新的名字，并不修改表的结构。

对列指定别名

```mysql
SELECT 字段名 1 AS 别名, 字段名 2 AS 别名... FROM 表名 AS 表别名;
```

##### 清除重复值

查询指定列并且结果不出现重复数据

```mysql
SELECT DISTINCT  字段名  FROM  表名;
```

##### 查询结果参与运算

某列数据和固定值运算 

```mysql
SELECT  列名 1 +  固定值  FROM  表名;
```

某列数据和其他列数据参与运算

```mysql
SELECT  列名 1 +  列名 2 FROM  表名
```

注意:  参与运算的必须是数值类型

##### 条件查询

如果没有查询条件，则每次查询所有的行。实际应用中，一般要指定查询的条件。对记录进行过滤。

```mysql
SELECT  字段名  FROM  表名  WHERE  条件;
```

流程：取出表中的每条数据，满足条件的记录就返回，不满足条件的记录不返回

###### 运算符

比较运算符

- <、>、<=、>=、=、<>：<>在SQL中表示不等于，MySQL中也可随意使用 != ，没有 ==
- BETWEEN...AND：在一个范围之内，如：between 100 and 200相当于条件在 100 到 200 之间，包头又包尾
- IN(集合) ： 集合表示多个值，使用逗号分隔
- LIKE '张%'  ：模糊查询
- IS NULL ： 查询某一列为 NULL 的值，注：不能写=NULL

逻辑运算符

- and  或  && ： 与，SQL 中建议使用前者，后者并不通用。
- or  或  ||  或
- not  或  !  非

in 关键字

```mysql
SELECT  字段名  FROM  表名  WHERE  字段  in (数据 1,  数据 2...);
```

in 里面的每个数据都会作为一次条件，只要满足条件的就会显示

范围查询

```mysql
BETWEEN  值 1 AND  值 2
```

表示从值 1 到值 2 范围，包头又包尾。比如：age BETWEEN 80 AND 100  相当于：  age>=80 && age<=100

like 关键字

LIKE 表示模糊查询

```mysql
SELECT * FROM  表名  WHERE  字段名  LIKE '通配符字符串';
```

MySQL 通配符

- % ： 匹配任意多个字符串
- _  ： 匹配一个字符



#### 其他

- 查看当前数据库版本：select verdion();
- 查看时间：select now();
- 往表中插入一个数据： insert into students javavalues(0, "zhangsan", 18, 180,"male"); 
- 