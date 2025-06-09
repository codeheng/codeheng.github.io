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
- 支持多种数据类型：`string、set、Sorted Set、list、hash....`
- 支持数据备份：即master-slave(主从复制)

!!! Note "对比"

    默认支持16个数据库，而MySQL默认仅仅4种（`information_schema,performance_schema,sys,mysql`）
    
    默认端口号`6379`，而MySQL是`3306`

    - 关系型数据库集群模式一般是主从，**主从数据一致**，起到数据备份的作用，称为 **垂直扩展**
    - 非关系型数据库可以将数据拆分，存储在不同机器上，保存海量数据，解决内存大小有限的问题，称为 **水平扩展**

**优点：**

- 性能极高：读写速率非常之快 (读取的速度是11w次/s，写的速度是8.1w次/s)
- 丰富的数据结构: ^^string，list，hash，set，zset^^
- 原子性：所有操作均是原子性的，^^但Redis里面事务没有原子性^^
- 丰富的特性：支持publish/subscribe『[观察者模式](https://refactoringguru.cn/design-patterns/observer)』、key过期


默认16个数据库，编号`0~15`, 可通过`select index` 进行切换(index  = 0...15) 默认就是0

!!! Note "常见命令"

    - `keys *, keys ?`  列出所有的key （若空，则展示`(empty list or set)`）
    - `set k val` : 创建k-v  -->  `get k` 查看k对于的v
    - `dbsize` : 当前数据库key的数量
    - `flushdb` 清空当前数据库； `flushall` 清空所有数据库
    - `move key index` 把key对应的从当前数据库移动到index对应的数据库中
    - `exists key` 判断key是否存在
    - `ttl key` 查看key的有效时间，-1代表永不过期，-2代表已过期
    - `expire key seconds` 设置key的过期时间
    - `type key` 查看key的类型

### 五大基本数据类型

!!! quote "介绍"

    ![](./assets/redis基本数据类型.jpg)

**String** : 可存放任意类型的数据，属于二进制安全 **(Redis中最基本的类型)**

- String结构是将对象序列化为JSON字符串后存储，要修改对象的某个属性值的时候不方便

!!! Note "基本命令"

    - `mset k1 val1 [k2 val2 ...]` : 可设置多个给定的key值
    - `mget k1 [k2 ...]` 获取多个给定key的value值
    - `getrange key start end` 返回key对应value从start到end结束的字串 
        * e.g `GETRANGE k1 0 1` 若(k1:hello)则返回`he` 
        * 若end为-1则代表最后一个字符
    - `setrange key offset val` 用val覆盖key对应原本value从offset开始字符串的值
    - `incr key` 或 `incrby key increment` 对key对应的val进行增加，但val必须为数字

!!! Question "如何区分不同类型的Key？"

    e.g 需存储用户、商品信息到Redis，有一个用户的id是1，有一个商品的id恰好也是1
    
    如果此时使用id作为key，那么就回冲突，该怎么办？

    - **给key添加前缀加以区分**  --> `项目名:业务名:类型:id`


**Hash**：其中value是一个无序字典，类似于Java中的HashMap

- 可以将对象中的每个字段独立存储，可以针对单个字段做CRUD

![](./assets/redis-hash结构.jpg)

!!! Note "常见命令"

    ```shell
    > hset myhash field1 "Hello" (添加或者修改hash类型key的field的值)
    (integer) 1
    > HSET myhash field2 "Hi" field3 "World"
    (integer) 2
    > HGET myhash field2 (获取一个hash类型key的field的值)
    "Hi"
    > HGET myhash field3
    "World"
    > HGETALL myhash (获取一个hash类型的key中的所有的field和value)
    1) "field1"
    2) "Hello"
    3) "field2"
    4) "Hi"
    5) "field3"
    6) "World"
    > hkeys mybash (获取一个hash类型的key中的所有的field)
    1) "field1"
    2) "field2"
    3) "field3" 
    ```


**List**：列表(双向链表)，类似于Java中的LinkedList，最多可存$2^32 - 1$个元素，常用来存储一个有序数据，如：^^朋友圈点赞列表，评论列表等^^

- 有序；元素可以重复；插入和删除快；查询速度一般

!!! Note "常见命令"

    - `lpush/rpush key val1 val2...`: 头插和尾插直key中
    - `lrange key start end` 获取列表指定范围的元素
    - `lpop/rpop` 删除list中的头尾值
    - `lindex key index` 通过下标来获取元素 (**支持下标操作**) 
    - `lset key index val` 将list中下标为index的值更改为val
    - `lrem key count val` 删除list中count个val


**Set**：通过hash来实现，与Java中的HashSet类似，可以看做是一个value为`null`的HashMap

- 无序、元素不可重复、查找快、支持交集、并集、差集等

!!! Note "常见命令"

    - `sadd key member1 member2..` 向集合添加元素
    - `smembers key` 返回集合中的所有元素
    - `srem key val` 删除集合中为val的元素
    - `scard key` 返回set中元素的个数
    - `srandmember key num` 集合中随机选出num个数 （可用于抽奖...）
    - `spop key [num]` 移除并返回集合中一个或num个随机元素
    - `sismember key member` 判断一个元素是否存在于set中
    - `sinter key1 key2` 求key1与key2的交集
    - `sunion key1 key2` 并集
    - `sdiff key1 key2` 差集


**SortedSet**: 可排序的set集合，与Java中的TreeSet有些类似，但底层数据结构却差别很大

- SortedSet中的每一个元素都带有一个score属性，可以基于score属性对元素排序
    * 底层的实现是跳表（SkipList）+ hash

!!! Note "常见命令"

    - `zadd key score member` 添加若干元素到sorted set，若存在则更新其score值
        * score 定义 member 在Sorted Set中的排序位置 
    - `zrem key member` 删除其中指定的元素
    - `zscore key member` 获取sorted set中的指定元素的score值
    - `zrank key member` 获取sorted set 中的指定元素的排名
    - `zcard key`获取sorted set中的元素个数
    - `zcount key min max` z统计score值在给定范围内的所有元素的个数
    - `zrange key min max` 按score排序后，获取指定 ^^排名范围内^^ 的元素
    - `zrangebysocre key min max` 按score排序后，获取指定 ^^score范围内^^ 的元素
        * 所有的排名默认都是升序，若降序则在命令的Z后面添加REV即可
            +  `zrevrank key memeber`

??? example "例子"

    将班级的下列学生得分存入Redis的SortedSet中

    - `Jack 85, Lucy 89, Rose 82, Tom 95, Jerry 78, Amy 92, Miles 76`

    ```shell
    127.0.0.1:6379> zadd stu 85 Jack 89 Lucy 82 Rose 95 Tom 78 Jerry 92 Amy 76 Miles
    (integer) 7
    127.0.0.1:6379> zrem stu Tom
    (integer) 1
    127.0.0.1:6379> zscore stu Amy
    "92"
    127.0.0.1:6379> zrank stu Rose
    (integer) 2
    127.0.0.1:6379> zcount stu 0 80
    (integer) 2
    127.0.0.1:6379> zincrby stu 2 Amy  (给Amy同学加2分)
    "94"
    127.0.0.1:6379> zrange stu 0 2 (查出成绩前3名的同学)
    1) "Miles"
    2) "Jerry"
    3) "Rose"
    127.0.0.1:6379> zrangebyscore stu 0 80
    4) "Miles"
    5) "Jerry"
    ```


### Redis的Java客户端

- Jedis和Lettuce : 主要提供Redis命令对应的API；SpringDataRedis对其进行了抽象和封装
- Redisson：在Redis基础上实现了分布式的可伸缩的java数据结构 e.g. `Map, Queue`； 支持跨进程同步机制：`Lock、Semaphore`