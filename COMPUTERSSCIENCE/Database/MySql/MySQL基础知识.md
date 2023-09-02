# MySQL基础知识



## 数据库概述

### 为什么要使用数据库

* 持久化：数据持久化意味着将内存中的数据保存到硬盘上加以“固化”。
* 持久化的主要作用是```将内存中的数据存储在关系型数据库中```，当然也可以存储在磁盘文件、`XML`数据文件中。

### 数据库相关概念

| ```OB：数据库(Database)```                                   |
| ------------------------------------------------------------ |
| 即存储数据的“仓库”，其本质是一个文件系统。它保存了一系列有组织的数据。 |

| ```DBMS：数据库管理系统(Database Management System)```       |
| ------------------------------------------------------------ |
| 是一种操纵和管理数据库的大型软件，用于建立、使用和维护数据库，对数据库进行统一管理和控制。用户通过数据库管理系统访问数据库中表内的数据。 |

| ```SQL：结构化查询语言(Structured Query Language)``` |
| ---------------------------------------------------- |
| 专门用来与数据库通信的语言                           |

### 数据库与数据库管理系统的关系

数据库管理系统（DBMS）可以管理多个数据库。







## RDBMS与非RDBMS

### RDBMS

#### 实质

* 这种类型的数据库是**最古老**的数据库类型，关系型数据库模型是把复杂的数据结构归结为简单的**二元关系**（即二维表格形式）。
* 关系型数据库以**行（row）**和**列（column）**的形式存储数据，以便于用户理解。这一系列的行和列被称为**表（table）**，一组表组成了一个**库（database）**。
* 表与表之间的数据记录有关系（relationship）。现实世界中的各种实体以及实体之间的谷中联系军用**关系模型**来表示。关系型数据库，就是建立在**关系模型**基础上的数据库。
* `SQL`就是关系型数据库的查询语言。

#### 优势

* 复杂查询

可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询。

* 事务支持

使得对于安全性能很高的数据访问要求得以实现。

### 非RDBMS

**非关系型数据库**，可看成传统关系型数据库的功能`阉割版本`，基于键值对存储数据，不需要经过SQL层的解析，**性能非常高**。同时，通过减少不常用的功能，进一步提高性能。

#### 有哪些非关系型数据库

* **键值型数据库**

键值型数据库通过**key-Value**键值的方式来存储数据，其中key和Value可以是简单的对象，也可以是复杂的对象。**key**作为唯一的标识符，优点是查找速度快，在这方面明显优于关系型数据库，缺点是无法像关系型数据库一样使用条件。键值型数据库典型的使用场景是缓存，例如，**Redis**。

* **文档型数据库**

此类数据库可存放并获取文档，可以是XML、JSON等格式。在数据库中文档作为处理信息的基本单位，一个文档就相当于一条记录。`MongoDB`是最流行的文档型数据库之一。

* **搜索引擎数据库**

搜索引擎数据库是应用在搜索引擎领域的数据存储形式，由于搜索引擎会爬取大量的数据，并以特定的格式进行存储，核心原理是**倒排索引**。（关系型数据库针对全文索引效率较低）如`Elasticsearch`。

* **列式数据库**

将数据按照列存储到数据库中，这样做的好处是可以大量降低系统的`I/O`，适合于分布式文件系统，不足在于功能相对有限。例如，`HBase`。

* **图形数据库**

图形数据库利用了图这种数据结构存储了实体（对象）之间的关系。







## 关系型数据库设计原则

* 关系型数据库的典型数据结构就是**数据表**，这些数据表的组成都是结构化（`Structed`）。
* 将数据放到表中，表再放到库中。
* 一个数据库中可以有多个表，每个表都有一个名字，用来标识自己。表名具有唯一性。
* 表具有一些特性，这些特性定义了数据在表中如何存储，类似`Java`和`Python`等中的类的设计。

### 表、记录、字段

* E-R（entity-relationship，实体-联系）模型中有三个主要概念是：`实体集`，`属性`，`联系集`。
* 一个实体集（`class`）对应于数据库中的一个表（`table`），一个实体（`instance`）则对应于数据库表中的一行（`row`），也称为一条记录（`record`）。一个属性（`attribute`）对应于数据库表中的一列（`column`），也称为一个字段（`field`）。

### 表的关联关系

* 表与表之间的数据记录有关系（`relationship`）。现实世界中的各种实体以及实体之间的各种联系均用关系模型来表示。
* 四种：一对一关联、一对多关联、多对多关联、自我引用。

#### 一对一关联（`one-to-one`）

* 在实际的开发中应用不多，因为一对一可以创建一张表。

* 举例：设计学生表：学号、姓名、手机号、班级、...
  * 拆为两个表：两个表的记录是一一对应关系。
  * 基础信息表（常用信息）：学号、姓名、手机号码、...
  * 档案信息表（不常用信息）：学号、身份证号码、家庭住址、...
* 两种建表原则：
  * 外键唯一：主表的主键和从表的外键（唯一），形成主外键关系，外键唯一。
  * 外键是主键：主表的主键和从表的主键，形成主外键关系。

#### 一对多关系（one-to-many）

* 常见实例场景：`客户表和订单表`，`分类表和商品表`，`部门表和员工表`。
* 举例：
  * 员工表：编号、姓名、...、所属部门。
  * 部门表：编号、名称、简介。
* 一对多建表原则：在从表（多方）创建一个字段，字段作为外键指向主表（一方）的主键。

#### 多对多关系（many-many）

要表示多对多关系，必须创建第三个表，该表通常称为`联接表`，它将多对多关系划分为两个一对多关系。将这两个表的主键都插入到第三个表中。

* 举例：学生-课程
  * 学生信息表：一行代表一个学生的信息（学号、姓名、手机号码、班级、系别、...）。
  * 课程信息表：一行代表一个课程的信息（课程编号、授课老师、简介、...）。
  * 选课信息表：一个学生可以选多门课，一门课可以被多个学生选择。

#### 自我引用

举例：

| 员工编号 | 姓名 | 部门编号 | 主管编号 |
| -------- | ---- | -------- | -------- |
| 101      | hh   | 1        | NULL     |
| 103      | ww   | 1        | 101      |
| 104      | kk   | 1        | 103      |
| 201      | dkjf | 2        | 101      |







## MySQL环境搭建（Windows）



### 1. MySQL的卸载

卸载之前，先停止`MySQL8.0`的服务。在任务管理器中可以停止其运行。

#### 软件的卸载

1. 方式一：通过控制面板卸载软件
2. 方式二：通过电脑管家等软件卸载
3. 方式三：通过安装包提供的卸载功能卸载
   * 选择下载的`mysql-installer-community-8.0.26.0.msi`文件
   * 选择要卸载的`MySQL`服务器程序，单击`Remove`即可卸载

#### 残余文件的清理

如果再次安装不成功，可以卸载后对残余文件进行清理后再安装

1. 服务目录：`MySQL`服务的安装目录
2. 数据目录：默认在`C:\ProgramData\MySQL`

如果自己单独指定过数据目录，就找到自己的数据目录进行删除即可

**请在写在前做好数据备份**

#### 清理注册表（选做）

如果前几步做了，再次安装还是失败，那么可以清理注册表。

<img src="img/注册表删除.png">



### 2. MySQL的下载、安装、配置

#### 2.1 软件的下载

1. <a href="https://www.mysql.com">官网</a>
2. 在`DOWNLOAD`可以选择`community`下载（免费）
3. 在安装离线版后，选择下列选项
   1. **Custom**
   2. <img src = "img/mysql安装.png">
   3. 一路**Next**以及**Execute**
   4. 配置mysql端口用户密码等

#### 2.2 环境变量配置

将软件安装位置的bin目录添加到环境变量-系统变量-Path里。

#### 2.3 启动服务

* 可以在任务管理器进行设置

* 也可以在终端用脚本命令

  ```bash
  # 启动MySQL服务
  net start MySQL服务名
  
  # 停止MySQL服务
  net stop MySQL服务名
  ```

#### 2.4 登录

```shell
mysql -u root -p # (-P 可以指定端口号 -h 可以指定主机) 这样能访问一台主机下不同的mysql服务
```

然后输入密码（这样比较好）

### 3. MySQL简单入门以及字符集设置

```mysql
# 展示数据库
show databases;

# 创建数据库
create database databasename;

# 使用某个数据库
use databasename;

# 展示表
show tables;

# 创建表
create table tablename(...); # example(...) => (id int, log string)

# 查看表信息
show create table tablename;

# 查看编码以及比较规则命令
show variables like 'character_%';
show variables like 'collation_%';
```

修改**MySQL**的数据目录下的**my.ini**配置文件（一般是MySQL 5.x才需要去改，MySQL 8.x 默认是utf8mb4 ）

```ini
[mysql] # 大概在63行左右，在其下添加
...
default-character-set=utf8	# 默认字符集

[mysqld] # 大概在76行左右，在其下添加
...
charater-set-server=utf8
collation-server=utf8_general_ci
```







## MySQL目录结构以及源码



### 1. 目录结构

| MySQL的目录结构                             | 说明                                 |
| ------------------------------------------- | ------------------------------------ |
| bin目录                                     | 所有MySQL的可执行文件，如：myal.exe  |
| MySQLInstanceConfig.exe                     | 数据库的配置向导，在安装时出现的内容 |
| data目录                                    | 系统数据库所在的目录                 |
| my.ini文件                                  | MySQL的主要配置文件                  |
| C:\ProgramData\MySQL\MySQL Server 8.0\data\ | 用户创建的数据库所在的目录           |



### 2. 源码

在进入MySQL下载界面。不选择默认的"Micosoft Windows"，而是通过下拉栏，找到"Source Code"，在下面的操作系统版本里面，选择"Windows (Architecture Independent)"，然后点击下载。接下来，把下载下来的压缩文件解压，就能得到MySQL的源代码。

* sql子目录是MySQL的核心代码
* libmysql子目录是客户端程序api
* mysql-test子目录是测试工具
* mysys子目录是操作系统相关函数和辅助函数







## SQL

SQL（Structured Query Language，结构化查询语言）是使用关系模型的数据库应用语言。



### 1. SQL分类

* **DDL（Data Definition Languages、数据定义语言）**，这些语句定义了不同地数据库、表、视图、索引等数据库对象，还可以用来创建、删除、修改数据库和数据表结构
  * 主要语句关键字包括`CREATE`、`DROP`、`ALTER`等
* **DML（Data Manipulation Language、数据操作语言）**，用于添加、删除、更新和查询数据库记录，并检查数据完整性
  * 主要的语句关键字包括`INSERT`、`DELETE`、`SELECT`等
  * `SELECT`是`SQL`语言的基础，最为重要
* **DCL（Data Control Language、数据控制语言）**，用于定义数据库、表、字段、用户的访问权限和安全级别。
  * 主要的语句关键字包括`GRANT`、`REVOKE`、`COMMIT`、`ROLLBACK`、`SAVEPOINT`等



### 2. SQL语言的规则与规范

#### 2.1 基本规则

* SQL可以写在一行或者多行。必要时缩进
* 每条命令以`;`或`\g`或`\G`结束
* 关键字不能被缩写也不能分行
* 关于标点符号
  * 保证所有的`()`、双引号、单引号都是成对结束的
  * 必须使用英文状态下的半角输入方式
  * 字符串和日期时间类型的数据可以使用单引号表示
  * 列的别名，尽量使用双引号，而且不建议省略as

#### 2.2 SQL大小写规范

* **MySQL**在**Windows**环境下是**大小写不敏感**的
* **MySQL**在**Linux**环境下是**大小写敏感**的
  * 数据库名、表名、表的别名、变量名是严格区分大小写的
  * 关键字、函数名、列名（或字段名）、列的别名（字段的别名）是忽略大小写的
* 推荐采用统一的书写规范：
  * 数据库名、表名、表别名、字段名、字段别名等都小写
  * **SQL**关键字、函数名、绑定函数名都大写

#### 2.3 注释

```mysql
单行注释：# 注释文字（MySQL特有的方式）
单行注释：-- 注释文字（--后面必须包含一个空格。）
多行注释：/* 注释文字 */
```

#### 2.4 命名规则

* 数据库、表名不得超过30个字符，变量名限制为29个
* 必须只能包含 A-Z,a-z-9，共63个字符
* 数据库名、表名、字段名等对象名中间不要包含空格
* 同一个MySQL软件中，数据库不能同名，同一个库中，表不能重名同一个表中，字段不能重名
* 必须保证你的字段没有和保留字、数据库系统或常用方法冲突。如果坚持使用，请在SOL语句中使用’(着重号)引起来
* 保持字段名和类型的一致性，在命名字段并为其指定数据类型的时候一定保证一致性。假如数据类型在一个表里是整数，那在另一个表里可就别变成字符型了

#### 2.5 数据导入指令

1. 终端登录**MySQL**命令导入（使用**source**命令）

   ```mysql
   mysql> source 文件的全路径名;
   ```

2. 基于具体的图形化界面的工具可以导入数据







## 基本的SELECT语句



### 1. 最基本的SELECT语句：**SELECT ...**  以及 **SELECT 字段1, 字段2, ... FROM 表名**

```mysql
# example
SELECT 1 + 1, 3 * 2;

SELECT 1 + 1, 3 * 2
FROM DUAL; # dual：伪表

# *：表中的所有字段（或列）
SELECT * FROM 表名
```



### 2. 列的别名

```mysql
# example
# as：全称：alias（别名），可以省略
# 列的别名可以使用一对""引起来（有的情况得加""就像"annual sal"这里）。不要使用''（MySQL虽然可以，但不符合标准）。

SELECT employee_id emp_id, last_name AS lname, department_id "department_id", salary * 12 "annual sal"
FROM employees;
```



### 3. 去除重复行

**DISTINCT**

```mysql
# example

# 没有去重的情况
SELECT department_id
FROM employees;

# 去重的情况
SELECT DISTINCT department_id
FROM employees;


# 错误的：只有一个去重无法做成一张表输出
SELECT salary, DISTINCT department_id
FROM employees;

# 仅仅能够运行（看成是整体完全相同去重），没有什么实际意义
SELECT DISTINCT department_id, salary
FROM employees;
```



### 4. 空值参与运算

* 空值：**null**
* **null**不等同于0，''，'null'

```mysql
# example

# 空值参与运算：结果一定也为空
# 结果发现凡是commisson_pct是null的年工资都是null
SELECT employee_id, salary "月工资", salary * (1 + commisson_pct) * 12 "年工资", commisson_pct
FROM employees;

# 实际问题的解决方案：引入IFNULLL
# 这样如果是null就拿0来计算
SELECT employee_id, salary "月工资", salary * (1 + IFNULL(commisson_pct, 0)) * 12 "年工资", commisson_pct
FROM employees;
```



### 5. 着重号 	``

```mysql
# example

# 修饰跟关键字重复冲突的字段（表名）
SELECT * FROM `ORDER`;
```



### 6. 查询常数

```mysql
# example

# '常数'、123都会匹配每一行
SELECT '常数', 123, employee_id, last_name
FROM employees
```



### 7. 显示表结构

使用**DESCRIBE**或**DESC**命令，表示表结构

```mysql
# example

DESCRIBE employees
或
DESC employees
```

```mysql
# example

mysql> desc employees # 显示了表中字段的详细信息
```



### 8. 过滤数据

**WHERE**

```mysql
# example

# 查询90号部门的员工信息
SELECT *
FROM employees
# 过滤条件，声明在FROM结构的后面
WHERE dapartment_id = 90;
```







## 运算符



### 1. 算术运算符

算术运算符主要用于数学运算，其可以连接运算符前后的两个数值或表达式，对数值或表达式进行加（`+`）、减（`-`）、乘（`*`）、除（`/`）和取模（`%`）运算

```mysql
# example

SELECT 100, 100 + 0, 100 -50, 100 - 35.5
FROM DUAL;

# 结果
/*
|----------|
|100 + '1' |
|----------|
|       101|
|----------|
*/
# 在SQL中， +没有连接的作用，就表示加法运算。此时，会将字符串转换为数值（隐式转换）
SELECT 100 + '1' # 在Java语言中，结果是：1001。
FROM DUAL;

SELECT 100 + 'a' # 此时将'a'看做0处理
FROM DUAL;

SELECT 100 + NULL # NULL值参与运算结果为空
FROM DUAL;



# 取模运算：	%(mod)

SELECT 12 % 3, 12 % 5, 12 MOD -5, -12 % -5
FROM DUAL;

# 练习：查询员工id为偶数的员工信息
SELECT employee_id, last_name, salary
FROM employees
WHERE employee_id % 2 = 0;
```



### 2.比较运算符

比较运算符用来对表达式左边的操作数和右边的操作数进行比较，比较的结果为真则返回1，比较的结果为假则返回0，其他情况则返回NULL。

比较运算符经常用来作为SELECT查询语句的条件来使用，返回符合条件的结果记录

| 运算符     | 名称           | 作用                                                         | 示例                              |
| ---------- | -------------- | ------------------------------------------------------------ | --------------------------------- |
| **=**      | 等于运算符     | 判断两个值、字符串或表达式是否相等                           | SELECT C FROM TABLE WHERE A = B   |
| **<=>**    | 安全等于运算符 | 安全地判断两个值、字符串或是否相等                           | SELECT C FROM TABLE WHERE A <=> B |
| **<>(!=)** | 不等于运算符   | 判断两个值、字符串或表达式是否不相等                         | SELECT C FROM TABLE WHERER A <> B |
| **<**      | 小于等于运算符 | 判断前面的值、字符串或表达式是否小于小于后面的值、字符串或表达式 | SELECT C FROM TABLE WHERE A < B   |
| **<=**     | 小于等于运算符 | 判断前面的值、字符串或表达式是否小于等于后面的值、字符串或表达式 | SELECT C FROM TABLE WHERE A <= B  |
| **>**      | 大于运算符     | 判断前面的值、字符串或表达式是否大于后面的值、字符串或表达式 | SELECT C FROM TABLE WHERE A > B   |
| >=         | 大于等于       | 判断前面的值、字符串或表达式是否大于等于后面的值、字符串或表达式 | SELECT C FROM TABLE WHERE A >= B  |

此外，还有非符号类型的运算符

| 运算符                | 名称             | 作用                                 | 示例                                        |
| --------------------- | ---------------- | ------------------------------------ | ------------------------------------------- |
| **IS NULL**           | 为空运算符       | 判断值、字符串或表达式是否为空       | SELECT B FROM TABLE WHERE A IS NULL         |
| **IS NOTNULL**        | 不为空预算符     | 判断值、字符串或表达式是否不为空     | SELECT B FROM TABLE WHERE A IS NOT NULL     |
| **LEAST**             | 最小值运算符     | 在多个值中返回最小值                 | SELECT D FROM TABLE WHERE C LEAST(A, B)     |
| **GREATEST**          | 最大值运算符     | 在多个值中返回最大值                 | SELECT D FROM TABLE WHERE C GREATEST(A, B)  |
| **BETWEEN**...**AND** | 两者之间的运算符 | 判断一个值是否在两个值之间           | SELECT D FROM TABLE WHERE C BETWEEN A AND B |
| **ISNULL**            | 为空运算符       | 判断一个值、字符串或表达式是否为空   | SELECT B FROM TABLE WHERE A ISNULL          |
| **IN**                | 属于运算符       | 判断一个值是否为列表中的任意一个值   | SELECT D FROM TABLE WHERE C IN(A, B)        |
| **NOT IN**            | 不属于运算符     | 判断一个值是否不是列表中的任意一个值 | SELECT D FROM TABLE WHERE C NOT IN(A, B)    |
| **LIKE**              | 模糊匹配运算符   | 判断一个值是否符合模糊匹配规则       | SELECT C FROM TABLE WHERE A LIKE B          |
| **REGEXP**            | 正则表达式运算符 | 判断一个值是否符合正则表达式的规则   | SELECT C FROM TABLE WHERE A REGEXP B        |
| **RLIEK**             | 正则表达式运算符 | 判断一个值是否符合正则表达式的规则   | SELECT C FROM TABLE WHERE A  RLIKE B        |

```mysql
# example

# 两边都是字符串的话，则按照ANSI的比较规则进行比较
SELECT 	'a' = 'a', 'ab' = 'ab'
FROM DUAL;

# 只有有null参与判断，结果就为null
SELECT 1 = NULL
FROM DUAL;

# <=> 安全等于，跟=唯一区别是可以对null进行判断
SELECT 1 <=> NULL, NULL <=> NULL # 0, 1 在两个操作数均为NULL时，则返回值为1，而不为NULL；当一个操作数为NULL时，其返回值为0
FROM DUAL;



# IS NULL / IS NOT NULL / ISNULL
# 练习：查询表中commission_pct为null的数据有哪些
SELECT  last_name, salary, commisson_pct
FROM employees
WHERE commission_pct IS NULL;
# 或
SELECT  last_name, salary, commisson_pct
FROM employees
WHERE ISNULL(commission_pct);

# 练习：查询表中commission_pct不为null的数据有哪些
SELECT  last_name, salary, commisson_pct
FROM employees
WHERE commission_pct IS NOT NULL;
# 或
SELECT  last_name, salary, commisson_pct
FROM employees
WHERE NOT commission_pct <=> NULL;



# LEAST() / GREATEST()

SELECT LEAST('g'. 'b', 't', 'm'), GREATEST('g'. 'b', 't', 'm')
FROM DUAL; # 'b', 't'


# BETWEEN 条件下界 AND 条件上界

# 查询工资在6000到8000的员工信息
SELECT employee_id, last_name, salary
FROM employees
WHERE salary BETWEEN 6000 AND 8000;

# 交换6000和8000之后，查询不到数据
SELECT employee_id, last_name, salary
FROM employees
WHERE salary BETWEEN 8000 AND 6000;

# 查询工资不在6000到8000的员工信息
SELECT employee_id, last_name, salary
FROM employees
WHERE salary NOT BETWEEN 6000 AND 8000;



# IN (set) / NOT IN (set)

# 练习：查询部门为10，20，30部门的员工信息
SELECT last_name, salary, department_id
FROM employees
# 这样写麻烦
# WHERE department_id = 10 or WHERE department_id = 20 or WHERE department_id = 30
WHERE department_id IN (10, 20, 30)

# 查询工资不是6000，7000，8000的员工信息
SELECT employee_id, last_name, salary
FROM employees
WHERE salary NOT IN (6000, 7000, 8000);



# LIKE：模糊查询

# % ：代表不确定个数的字符（0个，1个，或多个）
# 练习：查询last_name中包含字符'a'的员工信息
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%';

# 练习：查询last_name以字符'a'开头的员工信息
SELECT last_name
FROM employees
WHERE last_name LIKE 'a%';

# 练习：查询last_name中包含字符'a'并且包含字符'e'的员工信息
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%e%' OR last_name LIKE '%e%a%';


# _ ：代表一个不确定的字符
# 练习：查询第2个字符是'a'的员工信息
SELECT last_name
FROM employees
WHERE last_name LIKE '_a%';

# 练习：查询第2个字符是'_'并且第3个字符是'a'的员工信息
# 需要使用转义字符：\
SELECT last_name
FROM employees
WHERE last_name LIKE '_\_a%';
# WHERE last_name LIKE '_$_a%' ESCAPE '$'; 这样能用$表示转义字符（\）


# REGEXP \ RLIKE：正则表达式

# 练习
SELECT 'dkjfalkdjfk' REGEXP '^dkj', 'kdjfkkdjfal' REGEXP 'l$', 'skdjfla' REGEXP 'dj'
FROM DUAL; # 1 1 1

SELECT 'kdjk' REGEXP 'k.j', 'dfkdjka' REGEXP '[da]'
FROM DUAL; # 1 1
```



### 3.逻辑运算符

逻辑运算符主要用来判断表达式的真假，在**MySQL**中，逻辑运算符的返回结果为1，0或NULL。

**MySQL**支持四种逻辑运算符如下

| 运算符     | 作用     | 示例                           |
| ---------- | -------- | ------------------------------ |
| NOT 或 ！  | 逻辑非   | SELECT NOT A                   |
| AND 或 &&  | 逻辑与   | SELECT A AND B(SELECT A && B)  |
| OR 或 \|\| | 逻辑或   | SELECT A OR B(SELECT A \|\| B) |
| XOR        | 逻辑异或 | SELECT A XOR B                 |



### 4. 位预算符

<img src = "img/位预算符.png">



### 5. 运算符的优先级

<img src = "img/运算符优先级.png">







## 排序与分页

```mysql
# example

# 排序

# 如果没有使用排序操作，默认情况下查询返回的数据是按照添加数据的顺序显示的
SELECT *
FROM employees;


# 使用 ORDER BY 对查询到的数据进行排序操作（默认是升序）
# 升序：ASC (ascend)
# 降序：DESC	(descend)

# 练习：按照salary从高到低的顺序显示员工信息
SELECT employee_id, last_name, salary
FROM employees
ORDER BY salary DESC;

# 可以使用列的别名，进行排序
SELECT employee_id, last_name, salary * 12 annual_sal
FROM employees
ORDER BY annual_sal DESC;
# 别名只能在 ORDER BY 中使用，不能在 WHERE 中使用
# 错误的，会报错
SELECT employee_id, last_name, salary * 12 annual_sal
FROM employees
WHERE annual_sal > 816000;

# 二级排序

# 练习：显示员工信息，按照department_id的降序排列，salary的升序排列
SELECT employee_id, last_name, salary
FROM employees
ORDER BY department_id DESC, salary ASC;
```

```mysql
# example

# 分页

# mysql使用LIMIT实现数据的分页显示

# 需求：每页显示20条记录，此时显示第1页
SELECT employee_id, last_name
FROM employees
LIMIT 0, 20;

# 需求：每页显示20条记录，此时显示第2页
SELECT employee_id, last_name
FROM employees
LIMIT 20, 20;



# WHERE ... ORDER BY ... LIMIT 声明顺序

# LIMIT 的格式：严格来说 LIMIT 位置偏移量，条目数
# 结构"LIMIT 0, 条目数"等价于"LIMIT 条目数"
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > 6000
ORDER BY salary DESC
# LIMIT 0, 10;
LIMIT 10;


# MySQL8.0新特性：LIMIT ... OFFSET ...
SELECT employee_id, last_name
FROM employees
# LIMIT 31, 2;
LIMIT 2 OFFSET 31;
```







## 多表查询



### 1. 笛卡尔积（或交叉连接）的理解

笛卡尔积是一个数学运算。假设有两个集合`X`和`Y`，那么`X`和`Y`的笛卡尔积就是`X`和`Y`的所有可能组合。

```mysql
# example

# 笛卡尔积错误
SELECT employee, department_name
FROM employees, departments;

# 多表查询的正确方式：需要有连接条件
SELECT employee, department_name
FROM employees, departments
# 两个表的连接条件
WHERE employees.`department_id` = departmets.department_id;

# 如果查询语句中出现了多个表中都存在的字段，则必须指明此字段所在的表
SELECT employee, department_name，employees.department_id
FROM employees, departments
WHERE employees.`department_id` = departmets.department_id;

# 建议：从sql优化的角度，建议多表查询时，每个字段前都指明其所在的表

# 可以给表起别名，在SELECT和WHRER中使用表的别名
SELECT emp.employee, dept.department_name，emp.department_id
FROM employees emp, departments dept
WHERE emp.`department_id` = dept.department_id;
# 如果给表请了别名，一旦在SELECT或WHERE中使用表名的话，则必须使用表的别名，而不能使用表的别名
# 如下操作是错误的，会报错
SELECT emp.employee, dept.department_name，emp.department_id
FROM employees emp, departments dept
WHERE emp.`department_id` = departments.department_id;


# 练习：查询员工的employee_id, last_name, department_name, city
SELECT employee_id, last_name, department_name, city
FROM employees e, departments d, locations l
WHERE e.`department_id` = d.`department_id`
AND d.`location_id` = l.`location_id`;
```

​	

### 2. 多表查询的分类

角度一：等值连接	vs	非等值连接

角度二：自连接		vs	非自连接

角度三·：内连接		vs	外连接

```mysql
# example

# 等值连接	vs	非等值连接

# 例子非等值连接
SELECT e.last_name, e.salary, j.grade_level
FROM employees e, job_grades j
WHERE e.`salary` BETWEEN j.`lowest_sal` AND j.`highest_sal`;


# 自连接	vs	非自连接

# 自连接例子
# 练习：查询员工id，员工名字及其管理者的id和姓名
SELECT emp.employee_id, emp.last_name, emp.employee_id, mgr.last_name
FROM employees emp, employees mgr
WHERE emp.`manager_id` = mgr.`employee_id`


# 内连接	vs	外连接

# 内连接：合并具有同一列的两个以上的表的行，结果集中不包含一个表与另一个表不匹配的行


# 外连接：合并具有同一列的两个以上的表的行，结果集中除了包含一个表与另一个表匹配的行之外，还查询到左表或右表中不匹配的行
# 外连接的分类：左外连接、右外连接、满外连接
# 左外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回左表不满足条件的行
#右外连接：两个表在连接过程中除了返回满足连接条件的行以外还返回右表不满足条件的行
# -----------------
# 练习：查询所有的员工的last_name, department_name信息

# SQL92语法实现外连接：使用 + --- MySQL不支持SQL92语法实现的外连接
/*
失败的，会报错的左外连接
SELECT employee_id, department_name
FROM employees e, department d
WHERE e.`department_id` = d.department_id (+);
*/

# SQL99语法中使用 JOIN ... ON 的方式实现多表的查询，这种方式也能外连接的问题。MySQL支持这种方式
# 内连接
SELECT employee_id, department_name
FROM employees e JOIN department d
ON e.`department_id` = d.`department_id`;
# 左外连接(OUTER可以省略掉)
SELECT employee_id, department_name
FROM employees e LEFT OUTER JOIN department d
ON e.`department_id` = d.`department_id`;

# 满外连接（全外连接）：MySQL 不支持 FULL OUTER JOIN
SELECT employee_id, department_name
FROM employees e FULL OUTER JOIN department d
ON e.`department_id` = d.`department_id`;
# -----------------
```

SQL99语法JOIN

<img src = "img/sqlJOIN.png">

### 3. UNION的使用

<img src = "img/union1.png">

<img src = "img/union2.png">

### 4. SQL99语法的新特性1：自然连接

SQL99在SQL92的基础上提供了一些特殊语法，比如`NATURAL JOIN`用来表示自然连接。它会自动查询两张表中**所有相同的字段**，然后进行**等值连接**。

```mysql
# example

SELECT employee_id, department_name
FROM employees e JOIN department d
ON e.`department_id` = d.`department_id`
AND e.`manger_id` = d.`manger_id`;
#等价于自然连接
SELECT employee_id, department_name
FROM employees e NATURAL JOIN department d;
```

### 5. SQL99语法的新特性2：USING

```mysql
# example

SELECT employee_id, department_name
FROM employees e JOIN department d
ON e.`department_id` = d.`department_id`;
#等价于USING 
SELECT employee_id, department_name
FROM employees e JOIN department d
USING (department_id);
```







## 函数



### MySQL的内置函数以及分类

<img src="img/mysql函数介绍.png">



### 1. 数值函数



#### 1.1 基本函数

<img src = "img/基本函数.png">



#### 1.2 角度与弧度互换函数

<img src = "img/角度与弧度互换函数.png">



#### 1.3 三角函数

<img src = "img/三角函数.png">



#### 1.4 指数与对数

<img src = "img/指数与对数.png">



#### 1.5 进制间的转换

<img src = "img/进制间转换.png">

