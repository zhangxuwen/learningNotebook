# MySQL 进阶知识







## MySQL的数据目录

```shell
find / -name mysql
```

### MySQL8地主要目录结构

<img src = "img/数据库文件的存放路径.png">

### 相关命令目录

<img src = "img/相关命令目录.png">

### 配置文件目录

<img src = "img/配置文件目录.png">







## 用户与权限管理



### 用户管理

<img src = "img/用户管理.png">



### 登录MySQL服务器

<img src = "img/登录MySQL服务器.png">



### 创建用户

<img src = "img/创建用户.png">

### 修改用户

<img src = "img/修改用户.png">



### 删除用户

<img src = "img/删除用户1.png">	

<img src = "img/删除用户2.png">



### 设置当前用户密码

<img src = "img/设置当前用户密码1.png">

<img src = "img/设置当前用户密码2.png">



### 修改其它用户密码

<img src = "img/修改其它用户密码.png">

<img src = "img/修改其它用户密码2.png">







## 权限管理



### 授予权限的原则

<img src = "img/授予权限的原则.png">



### 授予权限

<img src = "img/授予权限1.png">

<img src = "img/授予权限2.png">



### 收回权限

<img src = "img/收回权限.png">







## 角色管理

`角色是权限的集合`

### 创建角色

<img src = "img/创建角色.png">

### 给角色赋予权限

<img src = "img/给角色赋予权限.png">

### 查看角色的权限

<img src = "img/查看角色的权限.png">

### 回收角色的权限

<img src = "img/回收角色的权限.png">

### 给用户赋予角色

<img src = "img/给用户赋予角色.png">



### 激活角色

<img src = "img/激活角色.png">

<img src = "img/激活角色2.png">



### 撤销用户的角色

<img src = "img/撤销用户.png">



### 设置强制角色

<img src = "img/设置强制角色.png">







## 逻辑架构



### 逻辑架构剖析

#### 服务器处理客户端请求

<img src = "img/服务器处理客户端请求.png">

#### Connectors

<img src = "img/Connectors.png">

#### 第一层：连接层

<img src = "img/连接层.png">

<img src = "img/连接管理.png">

#### 第二层：服务层

<img src = "img/服务层.png">

<img src = "img/SQL接口.png">

<img src = "img/解析器.png">

<img src = "img/查询优化器.png">

<img src = "img/查询缓存.png">

#### 第三层：引擎层

<img src = "img/引擎层.png">

#### 存储层

<img src = "img/存储层.png">







### SQL执行流程

#### MySQL中的 SQL 执行流程

<img src = "img/SQL执行流程.png">

<img src = "img/语法树.png">

<img src = "img/SQL语法分析.png">

<img src = "img/优化器.png">

<img src = "img/查询优化.png">

<img src = "img/SQL流程.png">







## 存储引擎

### 1.  查看存储引擎

```mysql
show engines;
```

### 2. 设置系统默认的存储引擎

<img src = "img/设置系统默认的存储引擎1.png">

<img src = "img/设置系统默认的存储引擎2.png">



### 3. 引擎介绍

#### 3.1 InnoDB 引擎：具备外键支持功能的事务存储引擎

<img src = "img/innodb1.png">

<img src = "img/innodb2.png">

#### 3.2 MyISAM 引擎：主要的非事务处理存储引擎

<img src = "img/MyISAM.png">







## 索引的数据结构

<img src = "img/索引概述.png">

### 优点

<img src = "img/优点.png">

### 缺点

<img src = "img/索引缺点.png">







## InnoDB中索引的推演

### 索引之前的查找

```mysql
SELECT [列名列表] FROM 表名 WHERE 列名 = xxx;
```

### 在一个页中的查找

<img src = "img/在一个页中的查找.png">

### 在很多页中查找

<img src = "img/在很多页中查找.png">

### 设计索引

<img src = "img/设计索引.png">

<img src = "img/设计索引2.png">

<img src = "img/设计索引3.png">

#### 一个简单的索引设计方案

<img src = "img/一个简单的索引设计方案.png">

<img src = "img/一个简单的索引设计方案2.png">

<img src = "img/一个简单的索引设计3.png">

<img src = "img/一个简单的索引设计4.png">

<img src = "img/一个简单的索引设计5.png">

<img src="img/一个简单的索引设计6.png">

#### InnoDB中的索引方案

<img src="img/迭代一次.png">

<img src="img/迭代一次2.png">

<img src="img/迭代一次3.png">

<img src="img/迭代两次.png">

<img src="img/迭代两次2.png">

<img src="img/迭代三次.png">

<img src="img/迭代三次2.png">

<img src="img/迭代三次3.png">

<img src="img/b+树.png">

<img src="img/b+树2.png">

### 常见索引概念

索引按照物理实现方式，索引可以分为2种：聚簇（聚集）和非聚簇（非聚集）索引。我们也把非聚集索引成为二级索引或者辅助索引

#### 聚簇索引

<img src="img/聚簇索引.png">

<img src="img/聚簇索引2.png">

<img src="img/聚簇索引2.png">

<img src="img/聚簇索引3.png">

<img src="img/聚簇索引4.png">

<img src="img/聚簇索引5.png">

#### 二级索引（辅助索引、非聚簇索引）

<img src="img/二级索引.png">

<img src="img/二级索引2.png">

<img src="img/二级索引3.png">

<img src="img/二级索引4.png">

<img src="img/二级索引5.png">

#### 联合索引

<img src="img/联合索引.png">

<img src="img/联合索引2.png">

### InnoDB的B+树索引的注意事项

#### 根页面位置万年不变

<img src="img/根页面位置万年不动.png">

#### 内节点中目录项记录的唯一性

<img src="img/内节点中目录项记录的唯一性.png">

<img src="img/内节点中目录项记录的唯一性2.png">

<img src="img/内节点中目录项记录的唯一性3.png">

#### 一个页面最少存储2条记录

<img src="img/一个页面最少存储2条记录.png">







### MyISAM中的索引方案

<img src="img/MyISAM中的索引方案.png">

#### MyISAM索引的原理

<img src="img/MyISAM索引的原理.png">

<img src="img/MyISAM索引的原理2.png">

<img src="img/MyISAM索引的原理3.png">

<img src="img/MyISAM索引的原理4.png">

#### MyISAM与InnoDB对比

<img src="img/MyISAM与InnoDB对比.png">

### 索引的代价

<img src="img/索引的代价.png">







## MySQL数据结构选择的合理性

<img src="img/MySQL数据结构选择的合理性.png">

### 全表遍历

略

### Hash结构

<img src="img/Hash结构.png">

<img src="img/Hash结构2.png">

<img src="img/Hash结构3.png">

<img src="img/Hash结构4.png">

<img src="img/Hash结构6.png">

<img src="img/Hash结构7.png">

<img src="img/Hash结构8.png">

<img src="img/Hash结构9.png">

### 二叉搜索树

<img src="img/二叉搜索树.png">

<img src="img/二叉搜索树2.png">

<img src="img/二叉搜索树3.png">



### AVL树

<img src="img/AVL树.png">

<img src="img/AVL树2.png">



### B-Tree

<img src="img/B-Tree.png">

<img src="img/B-Tree2.png">

<img src="img/B-Tree3.png">



### B+Tree

<img src="img/B+Tree.png">

。。。略



### R树

<img src="img/R树.png">

<img src="img/MySQL数据结构选择的合理性小结.png">







## InnoDB数据存储结构

### 数据库的存储结构：页

<img src="img/数据库的存储结构：页.png">

#### 磁盘与内存交互基本单位：页

<img src="img/磁盘与内存交互基本单位：页.png">

#### 页结构概述

<img src="img/页结构概述.png">

#### 页的大小

<img src="img/页的大小.png">

#### 页的上层结构

<img src="img/页的上层结构.png">

<img src="img/页的上层结构2.png">



### 页的内部结构

<img src="img/页的内部结构.png">

<img src="img/页的内部结构2.png">



#### 第1部分：File Header（文件头部）和File Trailer（文件尾部）

##### File Header

<img src="img/文件头部.png">



<img src="img/FIL_PAGE_OFFSET.png">



<img src="img/FIL_PAGE_TYPE.png">



<img src="img/FIL_PAGE_PREV和FIL_PAGE_NEXT.png">



<img src="img/FIL_PAGE_SPACE_OR_CHKSUM.png">

<img src="img/FIL_PAGE_SPACE_OR_CHKSUM2.png">



<img src="img/FIL_PAGE_LSN.png">



##### File Trailer

<img src="img/File Trailer.png">





#### 第2部分 记录部分

<img src="img/记录部分.png">



<img src="img/Free Space.png">



<img src="img/User Records.png">



##### 记录头信息

<img src="img/记录头信息.png">

<img src="img/记录头信息2.png">

<img src="img/记录头信息3.png">

<img src="img/记录头信息4.png">

<img src="img/记录头信息5.png">

<img src="img/记录头信息6.png">

<img src="img/记录头信息7.png">

<img src="img/记录头信息8.png">

<img src="img/记录头信息9.png">

<img src="img/记录头信息11.png">

<img src="img/记录头信息12.png">

<img src="img/记录头信息13.png">

<img src="img/记录头信息14.png">

<img src="img/记录头信息15.png">

<img src="img/记录头信息16.png">



##### Infimum + Supremum

<img src="img/Infimum + Supremum.png">

<img src="img/记录头信息10.png">





#### 第3部分 页结构之页目录与页头

##### Page Directory

<img src="img/页结构之页目录与页头.png">

<img src="img/页结构之页目录与页头2.png">

<img src="img/页结构之页目录与页头3.png">

<img src="img/页结构之页目录与页头4.png">

<img src="img/页结构之页目录与页头5.png">

<img src="img/页结构之页目录与页头6.png">

<img src="img/页结构之页目录与页头7.png">

<img src="img/页结构之页目录与页头8.png">

<img src="img/页结构之页目录与页头9.png">

<img src="img/页结构之页目录与页头10.png">

##### Page Header

<img src="img/页面头部.png">

<img src="img/页面头部2.png">

<img src="img/页面头部3.png">

<img src="img/页面头部4.png">







###  InnoDB行格式

<img src="img/InnoDB行格式.png">

#### 指定行格式的语法

<img src="img/指定行格式的语法.png">



#### COMPACT行格式

<img src="img/COMPACT行格式.png">

##### 变长字段长度列表

<img src="img/变长字段长度列表.png">

<img src="img/变长字段长度列表2.png">



##### NULL 值列表

<img src="img/NULL值列表.png">





##### 记录真实的数据

<img src="img/记录真实的数据.png">



#### Dynamic和Compressed行格式

##### 行溢出

<img src="img/行溢出.png">

<img src="img/行溢出2.png">

<img src="img/行溢出4.png">



#### Redundant行格式

##### 字段长度偏移格式

<img src="img/Redundant行格式.png">

<img src="img/Redundant行格式2.png">



##### 记录头信息

<img src="img/Redundant行格式3.png">







### 区、段与碎片区



#### 为什么要有区

<img src="img/为什么要有区.png">



#### 为什么要有段

<img src="img/为什么要有段.png">



#### 为什么要有碎片区

<img src="img/为什么要有碎片区.png">



#### 区的分类

<img src="img/区的分类.png">







### 表空间

<img src="img/表空间.png">



#### 独立表空间

<img src="img/独立表空间.png">

<img src="img/独立表空间2.png">



#### 系统表空间

<img src="img/系统表空间.png">

<img src="img/系统表空间2.png">

<img src="img/系统表空间3.png">

<img src="img/系统表空间4.png">

<img src="img/系统表空间5.png">

<img src="img/系统表空间6.png">

<img src="img/系统表空间7.png">







## 索引的创建与设计原则





### 索引的声明与使用



#### 索引的分类

<img src="img/索引的分类.png">

<img src="img/索引的分类3.png">

<img src="img/索引的分类4.png">



#### 创建索引

<img src="img/创建索引.png">

<img src="img/创建索引2.png">



#### 删除索引

```mysql
SHOW INDEX FROM book5;

# 方式1：ALTER TABLE ... DROP INDEX ...
ALTER TABLE book5
DROP INDEX idx_cmt;

# 方式2：DROP INDEX ... ON ...
DROP INDEX uk_idx_bname ON book5;

ALTER TABLE book5
DROP COLUMN book_name;
```







# MySQL8.0索引新特性

## 支持降序索引

<img src="img/支持降序索引.png">

<img src="img/支持降序索引2.png">

<img src="img/支持降序索引3.png">



## 隐藏索引

<img src="img/隐藏索引.png">







# 适合创建索引的情况



<img src="img/适合创建索引的情况.png">

<img src="img/适合创建索引的情况2.png">

<img src="img/适合创建索引的情况3.png">

<img src="img/适合创建索引的情况4.png">

<img src="img/适合创建索引的情况5.png">

<img src="img/适合创建索引的情况6.png">

<img src="img/适合创建索引的情况7.png">

<img src="img/适合创建索引的情况8.png">

<img src="img/适合创建索引的情况9.png">

<img src="img/适合创建索引的情况10.png">

<img src="img/适合创建索引的情况11.png">

<img src="img/适合创建索引的情况12.png">



