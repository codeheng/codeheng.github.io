---
comments: true
---

# Redis

> [Redis](https://redis.io/)(REmote DIctionary Server)———an [open-source](https://github.com/redis/redis)「可看出是C语言编写的」, in-memory database that stores data in **memory** for **high-performance** data retrieval and **key-value storage**
>
> 属于非关系数据库(NoSQL = not only SQL)，即并非像Mysql(关系数据库)那样使用table存储
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

- 性能极高：读写速率非常之快
- 丰富的数据结构
- 原子性：所有操作均是原子性的，^^但Redis里面事务没有原子性^^
- 丰富的特性：支持publish/subscribe『[观察者模式](https://refactoringguru.cn/design-patterns/observer)』、key过期

### 常见命令

默认16个数据库，编号`0~15`, 可通过`select index` 进行切换(index  = 0...15) 默认就是0

- `keys *, keys ?`  列出所有的key （若空，则展示`(empty list or set)`）
- `set k val` :创建k-v  -->  `get k` 查看k对于的v
- `dbsize` : 当前数据库key的数量
- `flushdb` 清空当前数据库； `flushall` 清空所有数据库
- `move key index` 把key对应的从当前数据库移动到index对应的数据库中
- `exists key` 判断key是否存在
- `ttl key` 查看key的有效时间，-1代表永不过期，-2代表已过期
- `expire key seconds` 设置key的过期时间
- `type key` 查看key类型