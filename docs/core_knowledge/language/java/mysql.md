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

表(Table) 是由一行一行的记录组成的 (逻辑上的概念)

- 物理上如何表示记录? 怎么从表中读取数据? 怎么把数据写入具体的物理存储器上?
    * **存储引擎负责的事情**  --> `MyISAM/ InnoDB/Memory...`
    * `SHOW ENGINES;` 查看当前服务器程序支持的存储引擎
            + 表的默认存储引擎为`InnoDB` 

**InnoDB逻辑存储结构**：

![](./assets/innoDB逻辑存储结构.jpg)

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

**Insert**:

- 向指定字段添加数据: `insert into 表名(字段名1, 字段名2) values(值1, 值2);`
- 全部字段添加数据： `insert into 表名 values(值1, 值2);`
    * 字段顺序要与值顺序一一对应
    * 字符串和日期型数据应 该包含在引号中  

**update:**  `update 表名 set 字段名1 = 值1, 字段名2 = 值2,...[where 条件];`

- 若没有条件，则会修改整张表的所有数据

**delete**：`delete from 表名 [where 条件];`

### DQL

Select 最重要的操作

```sql
SELECT
    字段列表
FROM
    表名列表
WHERE
    条件列表
GROUP  BY
    分组字段列表
HAVING
    分组后条件列表
ORDER BY
    排序字段列表
LIMIT
    分页参数
```

- 去除重复记录: `select distinct 字段列表 from 表名;`

**构造条件查询的运算符**：

- 比较：`<,<=,>,>=,=,<>或!=, between...and..., in(...), like 占位符, is null`
    * 占位符：`_` 匹配单个字符；`%` 匹配任意个字符
- 逻辑：`and或&&, or或||, not或!`

**聚合函数**：会将一列数据作为整体，纵向计算，返回结果 （纵向查询）

- `count, max, min, avg, sum`
- PS: 聚合函数会忽略空值，对NULL值不作为统计

**分组查询：** 按照某一列或者某几列，把相同的数据进行合并输出 （通常会使用聚合函数）

- `select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];`

??? Question "面试题——where与having区别"

    - **执行时机不同**：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤
    - **判断条件不同**：where不能对聚合函数进行判断，而having可以

**排序方式：** `desc` 降序，`asc` 升序


**分页查询**：`select 字段列表 from 表名 limit 起始索引, 查询记录数;`

- e.g. `limit 0,5` 从索引0开始，向后取5条记录

### DCL

**管理用户**: 

- 查询用户：`select * from mysql.user;`
    * MySQL中通过 ^^用户名@主机名^^，来唯一标识一个用户
- 创建用户: `CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';`

**函数**：字符串函数、数值函数、日期函数、流程函数

**约束**：`not null, unique, primary key, default, check, foreign key`

## 事务

!!! Quote 

    事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求
    
    - **即这些操作要么同时成功，要么同时失败**

开始事务：

1. `SELECT @@autocommit;`  默认是1，若set为0，则执行的DML语句都不会提交，需要手动commit
2. `start transaction` 或 `begin;`

提交事务commit；回滚事务rollback


**四大特性：ACID**

- 原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败
- 一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态
- 隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
- 持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

!!! Question "如何保证事务ACID"

    - 原子性、一致性、持久性由InnoDB中的redo log和undo log保证
    - 隔离性通过锁 + MVCC(多版本并发控制)保证

**并发事务问题**：脏读、不可重复读、幻读  --> 引入事务隔离级别

![](./assets/事务隔离级别.jpg)

隔离级别越高，数据越安全，性能越低

- 查看事务隔离级别：`SELECT @@TRANSACTION_ISOLATION;`


## JDBC

> 即使用Java语言操作关系型数据库的一套API——Java DataBase Connectivity(JDBC)最为底层、最为基础

- 企业项目开发中，一般都会使用基于JDBC的封装的高级框架，如：Mybatis、MybatisPlus

**JDBC的缺点**：

- url、username、password等相关参数全部硬编码在java代码中
- 查询结果的解析、封装比较繁琐
- 每一次操作数据库之前，先获取连接，操作完毕之后，关闭连接。频繁的获取连接、释放连接造成资源浪费

### Mybatis

> 持久层框架，用于简化JDBC的开发 [官网](https://mybatis.org/mybatis-3/)

- SpringBoot工程，导入Mybatis的起步依赖、mysql的驱动包、lombok
- 需要在`application.properties`配置mybatis：url、类名、用户名和密码

Mybatis解决JDBC的缺点：

- 数据库连接四要素(驱动、链接、用户名、密码)，都配置在springboot默认的配置文件`application.properties`或`application.yml`中
    * `application.yml`配置，更加简洁明了、以数据为中心 
- 查询结果的解析及封装，由mybatis自动完成映射封装
- 在mybatis中使用了数据库连接池技术，避免了频繁的创建连接、销毁连接带来的资源浪费
    * 数据库连接池即是个容器，负责分配、管理数据库连接 
        + 常见的数据库连接池：C3P0 、DBCP 、Druid 、Hikari (springboot默认)
        + 现在使用更多的是：Hikari、Druid （性能更优越）

**Mybatis开发的两种方式：注解和XML**

- 注解方式，主要是来完成简单的增删改查。若需要实现复杂的SQL功能，一般使用XML来配置映射语句，即将SQL语句写在XML配置文件中
    * XML映射文件的名称与Mapper接口名称一致，并且将XML映射文件和Mapper接口放置在相同包下（同包同名）
    * XML映射文件的namespace属性为Mapper接口全限定名一致
    * XML映射文件中sql语句的id与Mapper接口中的方法名一致，并保持返回类型一致


## 索引index

> 索引（index）是帮助MySQL高效获取数据的数据结构，在存储引擎层实现
>
> - 降低IO成本，提高数据检索效率 (PS: 索引也占空间)

**索引结构**：[B+Tree](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)、Hash、R-Tree(空间索引)、Full-Text(全文索引)

??? Question "为什么InnoDB存储引擎选择使用B+tree索引结构？"

    1. B+ 树是一种多路平衡搜索树，具有“矮胖”特点。相对于二叉树，层级更少，搜索效率高
    2. 对于B-tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低
    3. 相对Hash索引，B+tree支持范围匹配及排序操作

**索引分类：** ==主键索引(PRIMARY)、唯一索引(UNIQUE)、常规索引(Normal) 、全文索引(FULLTEXT)==

- 在InnoDB存储引擎中，根据索引的存储形式，分为：**^^聚集索引、二级索引^^**
    * 聚集索引：数据与索引放到一块，索引结构的叶子节点保存了行数据（row） 
        + 必须有，且只有一个 
    * 二级索引：将数据与索引分开存储，索引结构的叶子节点关联对应的主键
        + 可以存在多个

!!! Question "思考"

    两条SQL语句AB，哪个执行效率高? why?

    ```sql
    select * from user where id = 10;
    select * from user where name = 'Arm';
    ```
    PS: id为主键，name字段创建的有索引；

    答：A性能高于B，A语句走聚集索引，直接返回数据；而B先查询name字段的二级索引，再查询聚集索引，即需要回表查询

- **创建索引：** 
    * `create [unique | fulltext] index index_name on table_name(index_col_name, ...);`
- 查看索引: `show index from table_name;`
- 删除索引：`drop index index_name on table_name;`

### SQL性能

`show [session/global] status` 查看状态信息（比如insert等访问频次）

- 可得知以查询为主还是增删改为主，然后进行优化
- 若查询为主，为何定位  --> **慢查询日志**

!!! Quote "慢查询日志"

    慢查询日志记录了所有执行时间超过指定参数（`long_query_time`，默认10秒）的所有SQL语句的日志

    - 查看是否开启: `show variables like 'slow_query_time';`
        * 可在配置文件`/etc/my.cnf`中开启

`show profiles;` 能够在做SQL优化时帮助了解时间都耗费到哪里去了

- `select @@have_profiling;` 查看是否支持  --> `set profiling = 1` 开启
    * 开启后所有的SQL语句都会被记录，此时`show profiles;`可看到每一条SQL的耗时情况
    * `show profile for query query_id;` 指定query_id的SQL语句各个阶段的耗时情况


!!! Note
    
    `EXPLAIN` 或 `DESC`命令获取 MySQL 如何执行 SELECT 语句的信息

    - 直接在select语句之前加上 explain / desc

若索引了多列(联合索引)：`create index idx_user_pro_age_sta on tb_user(profession,age,status);`

- **遵循最左前缀法则**：查询从索引的最左列开始，且不跳过索引中的列。若跳跃某一列，索引后面的字段索引失效
- PS: 即联合索引的最左边的字段(即是第一个字段)必须存在，与SQL条件里的顺序无关

!!! warning "索引失效情况"

    1. 在联合索引中，若出现范围查询`<,>` 范围右侧列索引失效 （`>= 或 <=` 并不会失效）
    2. 若在索引列进行运算操作，则会失效 e.g `where substring(phone, 10, 2) = '15'`
    3. 字符串不加引号，则索引失效
    4. 若是头部模糊查询，索引失效 e.g `where profession like '%工'`
    5. or连接条件，若有列没有索引，则会失效 (左右两侧必须都有索引才生效)
    6. MySQL在查询时，会评估使用索引的效率与走全表扫描的效率，若走全表扫描更快，则放弃索引，走全表扫描

        - 因为索引是用来索引少量数据的，如果通过索引查询返回大批量的数据，则还不如走全表扫描来的快，此时索引就会失效

**SQL提示**（优化数据库的一手段）：`use index, ignore index, force index`

尽量使用覆盖索引，而不使用`select *`。 

- 覆盖索引：查询使用了索引，且需要返回的列，在该索引中已经全部能够找到
    * 即不需要回表查询，性能更高 

!!! Question 

    一张表, 有四个字段(id, username, password, status), 由于数据量大, 需要对以下SQL语句进行优化, 该如何进行才是最优方案:

    ```sql
    select id,username,password from tb_user where username = 'itcast';
    ```

    针对于username, password建立联合索引
    
    - `create index idx_user_name_pass on tb_user(username,password);`

??? Note "索引设计原则"

    1. 对于数据量较大，且查询比较频繁的表建立索引
    2. 对于查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引
    3. 尽量使用联合索引，减少单列索引，查询时避免回表，提高查询效率
    4. 若是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引

        - e.g. 为tb_user表的email字段，建立长度为5的前缀索引:
         - `create index idx_email_5 on tb_user(email(5));` 

### SQL优化

**插入：** (主键顺序插入性能 > 乱序插入)

- 若大量数据插入  --> load指令

```sql
set global local_infile = 1; --开启从本地加载文件导入数据的开关

load data local infile "E:/code/sql_data/sql_1000w/tb_sku1.sql" into table `tb_sku` fields terminated by ',' lines terminated by '\n';
```

## 视图

> 视图（View）是一种虚拟存在的表。视图中的数据并不在数据库中实际存在
>
> 视图只保存了查询的SQL逻辑，不保存查询结果

- 创建：`create view view_name as select语句;`
- 查看创建视图：`show create view view_name;`
    * 查看视图数据：`select * from view_name;` 
- 修改: `alter view view_name as select语句`
- 删除：`drop view view_name;`

!!! Note "视图作用"

    1. 简化用户对数据的理解，经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件
    2. 通过视图用户只能查询和修改他们所能见到的数据

### 存储过程

> 存储过程是事先经过编译并存储在数据库中的一段 SQL 语句的集合，可提高数据处理的效率

```sql
-- 创建：
create procedure 存储过程名称([ 参数列表 ])
begin
    -- SQL语句
end;

-- 调用：
call 名称([参数])
```

## 锁

> 锁是计算机协调多个进程或线程并发访问某一资源的机制  (数据并发访问的一致性、有效性)

### 全局锁

> 锁定数据库中的所有表，对整个数据库实例加锁 （仅可读）

应用场景：全库的逻辑备份，对所有的表进行锁定，从而获取一致性视图，保证数据的完整

- 加全局锁: `flush tables with read lock;`
- 备份（在shell中）：`mysqldump  -uroot –p123  test > test.sql`
- 释放锁：`unlock tables;`


### 表级锁

> 每次操作锁住整张表，锁定粒度大，发生锁冲突的概率最高，并发度最低
加锁：`lock tables 表名 read/write;`

- **表锁**: 
    * read lock：客户端1对指定表加了读锁，不会影响客户端2的读，但是会阻塞其写
    * write lock: 客户端1对指定表加了写锁，客户端2的读/写均阻塞
- **元数据锁(meta data lock, MDL)**：维护表元数据(表结构)的数据一致性
- **意向锁**: 
    * 意向共享锁IS：由语句`select ... lock in share mode`添加。与表锁read lock兼容，与表锁write lock互斥
    * 意向排他锁IX: 由`insert、update、delete、select...for update`添加。与表锁read lock及write lock都互斥，意向锁之间不会互斥

!!! Note "意向锁"

    客户端1，在执行DML操作时，会对涉及的行加行锁，同时也会对该表加上意向锁

    其他客户端，在对这张表加表锁的时候，会根据该表上所加的意向锁来判定是否可以成功加表锁，而不用逐行判断行锁情况


### 行级锁

> 每次操作锁住对应的行数据，锁定粒度最小，发生锁冲突的概率最低，并发度最高（在InnoDB存储引擎中）

行锁是通过对索引上的索引项加锁来实现的，而非对记录加锁

- 共享锁S：允许一个事务去读一行，阻止其他事务获得相同数据集的排他锁
- 排他锁X：允许获取排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁

## InnoDB引擎





## 面试题

