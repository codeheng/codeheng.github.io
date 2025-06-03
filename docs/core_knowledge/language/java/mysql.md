---
comments: true
---

> 数据库（DB）：存储和管理数据的仓库
>
> SQL（Structured Query Language）：结构化查询语言，它是操作关系型数据库的编程语言，定义了一套操作关系型数据库的统一标准


![](./assets/DB_ranking.jpg)

## 概述

[官网](https://dev.mysql.com/)、社区版免费[下载](https://dev.mysql.com/downloads/)

- 连接：`$ mysql [-h数据库服务器的ip地址 -P端口号] -u 用户名 -p` 回车再输入密码
    * 默认ip: 127.0.0.1；默认端口3306 


### 数据模型

建立在关系模型基础上，由多张相互连接的二维表组成的数据（行 + 列）

![](./assets/DB数据模型.jpg)


创建数据库  -->  创建数据表  -->  数据存入表中


## SQL语句

- DDL：数据定义语言，定义数据库对象(数据库、表、字段)
- DML：数据操作语言，对表中是数据进行增删改
- DQL：数据查询语言，查询表中的记录
- DCL：数据控制语言，创建数据库用户、控制数据库的访问权限

### DDL

- 查询所有数据库：`show databases;`
- 查询当前数据库: `select database();`
- 创建数据库：`create database [if not exists] 数据库名  [default charset utf8mb4];`
- 使用数据库：`use 数据库名;`
- 删除数据库：`drop database [if exists] 数据库名;`

创建表：
```sql
create table  表名(
        字段1  字段1类型 [约束]  [comment 字段1注释],
        字段2  字段2类型 [约束]  [comment 字段2注释],
        ......
        字段n  字段n类型 [约束]  [comment 字段n注释] 
) [comment 表注释];
```

!!! Note "约束"
    
    - 作用在表中字段上的规则，用于限制存储在表中的数据

    ![](./assets/DB约束.jpg) 

**数据类型：** 数值类型(int/bigint/flout/double...)、字符串类型(char/varchar/text...)、日期时间类型(date/datetime/timestamp...)

- 查询数据库中的表: `show tables;`
- 查看指定的表结构: `desc 表名`
- 查询指定表的建表语句 ：`show create table 表名;`
- 添加字段：`alter table 表名 add 字段名 类型(长度);`
- 修改字段类型： `alter table 表名 modify 字段名 新数据类型(长度);`
- 修改字段名和字段类型：`alter table 表名 change  旧字段名  新字段名  类型(长度);`
- 删除字段：`alter table 表名 drop 字段名;`
- 修改表名：`rename table 表名 to 新表名`
- 删除表：`drop table [if exists] 表名;`

PS: ^^关于表结构的查看、修改、删除操作，工作中一般都是直接基于图形化界面操作^^ 

- (e.g. [Navicat](https://www.navicat.com/en/)或[SQLyog](https://webyog.com/product/sqlyog/))


### DML


### DQL