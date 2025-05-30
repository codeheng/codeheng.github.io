---
comments: true
---

# Redis

> [Redis](https://redis.io/)(REmote DIctionary Server)———an [open-source](https://github.com/redis/redis)「可看出是C语言编写的」, in-memory database that stores data in **memory** for **high-performance** data retrieval and **key-value storage**
>
> 属于非关系数据库(NoSQL = not only SQL)，即并非像Mysql(关系数据库)那样使用table存储
>
> - 不遵循SQL标准 & 不支持ACID & 性能远超SQL
>
> PS: [中文文档](https://redis.com.cn/)


**非关系数据库分类：**

- key-value: **Redis**、memcached
- 列存储型: Hbase
- Graphs based: Neo4j
- 文档型：MongoDB(基于分布式文件存储的数据库) 

## 介绍

Redis可用于 ^^数据库、缓存和消息中间件^^ ，基于内存并支持持久化

[安装](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/)，如下操作验证成功否

```shell
$ redis-cli
127.0.0.1:6379> ping
PONG
```

**三大特点：** 

- 持久化：内存数据 --> Disk --> 重启还能从Disk再次加载
- 支持多种数据类型：string、set、Sorted Set、list、hash....
- 支持数据备份：即master-slave(主从复制)

!!! Note

    默认支持16个数据库，而MySQL默认仅仅4种（`information_schema,performance_schema,sys,mysql`）
    
    默认端口号`6379`，而MySQL是`3306`

**优点：**

- 性能极高：读写速率非常之快 (读取的速度是11w次/s，写的速度是8.1w次/s)
- 丰富的数据结构: ^^string，list，hash，set，zset^^
- 原子性：所有操作均是原子性的，^^但Redis里面事务没有原子性^^
- 丰富的特性：支持publish/subscribe『[观察者模式](https://refactoringguru.cn/design-patterns/observer)』、key过期

### 常见命令

默认16个数据库，编号`0~15`, 可通过`select index` 进行切换(index  = 0...15) 默认就是0

- `keys *, keys ?`  列出所有的key （若空，则展示`(empty list or set)`）
- `set k val` : 创建k-v  -->  `get k` 查看k对于的v
- `dbsize` : 当前数据库key的数量
- `flushdb` 清空当前数据库； `flushall` 清空所有数据库
- `move key index` 把key对应的从当前数据库移动到index对应的数据库中
- `exists key` 判断key是否存在
- `ttl key` 查看key的有效时间，-1代表永不过期，-2代表已过期
- `expire key seconds` 设置key的过期时间
- `type key` 查看key的类型

### 五大数据类型

String : 可存放任意类型的数据，属于二进制安全 **(Redis中最基本的类型)**

- `mset k1 val1 [k2 val2 ...]` : 可设置多个给定的key值
- `mget k1 [k2 ...]` 获取多个给定key的value值
- `getrange key start end` 返回key对应value从start到end结束的字串 
      * e.g `GETRANGE k1 0 1` 若(k1:hello)则返回`he` 
      * 若end为-1则代表最后一个字符
- `setrange key offset val` 用val覆盖key对应原本value从offset开始字符串的值
- `incr key` 或 `incrby key increment` 对key对应的val进行增加，但val必须为数字

List：列表(双向链表)，最多可存$2^32 - 1$个元素

- `lpush/rpush key val1 val2...`: 头插和尾插直key中
- `lrange key start end` 获取列表指定范围的元素
- `lpop/rpop` 删除list中的头尾值
- `lindex key index` 通过下标来获取元素 (**支持下标操作**) 
- `lset key index val` 将list中下标为index的值更改为val
- `lrem key count val` 删除list中count个val


Set是String类型的无序集合（无重复元素） 集合通过hash来实现

- `sadd key member1 member2..` 向集合添加元素
- `smembers key` 返回集合中的所有元素
- `srem key val` 删除集合中为val的元素
- `srandmember key num` 集合中随机选出num个数 （可用于抽奖...）
- `spop key [num]` 移除并返回集合中一个或num个随机元素


47:40

Sorted set 