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



