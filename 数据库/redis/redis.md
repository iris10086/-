# 下载

无windows版，windows版是民间移植

# 安装

绿色免安装

# 使用

先打开服务器.exe

再使用客户端进行操作

# redis数据类型及命令

## keys * 查看所有key



## exists key 判断是否存在



## expire key second 设置过期时间

## ttl key 查看time to live

```bash
127.0.0.1:6379> exists name
(integer) 0
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set name haha
OK
127.0.0.1:6379> exists name
(integer) 1
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> set age 18
OK
127.0.0.1:6379> expire name 10
(integer) 1
127.0.0.1:6379> ttl name
(integer) 8
127.0.0.1:6379> ttl name
(integer) 6
127.0.0.1:6379> ttl name
(integer) 3
127.0.0.1:6379> ttl name
(integer) 1
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> ttl name
(integer) -2
```



## type key 查看当前key对应的value的类型

## String

使用时无前缀

### 存储

set key value存储

### 获取

get key 获取值

### 删除

del key 删除

### strlen key 返回string的长度

### append key value 追加value

若不存在，就相当于set

```bash
127.0.0.1:6379> strlen name
(integer) 4
127.0.0.1:6379> get name
"haha"
127.0.0.1:6379> append name heihei
(integer) 10
127.0.0.1:6379> strlen name
(integer) 10
```

## 自增自减

### incr key自增

### decr key自减

### incrby key step

### decrby key stepde

```bash
127.0.0.1:6379> set views 0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views
(integer) 1
127.0.0.1:6379> git views
(error) ERR unknown command 'git', with args beginning with: 'views' 
127.0.0.1:6379> get views
"1"
127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> decr views
(integer) 1
127.0.0.1:6379> incrby views 10
(integer) 11
127.0.0.1:6379> decrby views 5
(integer) 6
```

## 截取部分

### getrange key start end

getrange name 1 5 [1,5)

getrange name 0 -1获取全部

### setrange key offset value

修改部分

## 逻辑判断

### setex 设置过期时间

set with expire

### setnx 不存在再设置

set if not exist

## 批量操作

mset 批量设置

pget 批量获取

### getset 先get再set



# list(linkedlist)

使用时前缀表示左右

### 添加

lpush key value 

rpush key value

### 获取

lrang key start end 范围获取(同Python)

### 删除

lpop key 弹出最左边的元素

rpop  key 同上

### 删除指定的值

lrem key count element 移除count个element

### 截取指定范围的值

ltrim key start stop

### rpoplpush 移除最后一个元素。



### 替换

lset  key index element

替换key中index位置的值

### 插入

linsert key <before/after> element value

将value插入到element的前面/后面



## set

使用时前缀s

### 存储

sadd key value

### 获取

smembers key 获取set中所有元素

### 删除

srem key value 删除某个元素

### 获取元素个数

scard key

### 随机取元素

srandmember key [count]

### 随机弹出元素

spop key [count]

### 将指定的值移动到另外的key中

smove source destination member



### 集合运算

#### 并集

sunion key1 key2

#### 交集

sinter key1 key2

#### 差集

sdiff key1 key2



## Hash(map集合)

key:map

使用时前缀h

### 存储

hset key field value存储

hmset key field value [...] 批量添加

### 获取

hget key field 获取指定值

hgetall key 获取所有值

### 删除

del key  field删除

### 获取长度

hlen key

获取字段数量

### 获取键

hkeys key 

### 获取值

hvals key

### 自增自减

hincrby key field step



### 逻辑

hsetnx



## Zset

使用时前缀z

按照给定的分数排序

### 存储

zadd key score value

### 获取

zrang key start end (with scores)

### 删除

zrem key value

### 实现排序

zrangebyscore key min max [withscores] 升序

zrevrangebyscore key min max [withscores] 降序

### 获取大小

zcard key

### 获取区间的值

zcount key min max 

获取指定区间的成员数量



## 通用命令

keys *查询所有键

type key 查询类型





# 特殊数据类型

## geospatial 地理位置(geo)

底层是zset集合。

可以使用zset的命令操作。

## hypeloglog

## bitemap

## 添加

见文档

# 查官方文档为第一要义

[中文网](www.redis.cn)

[英文网](www.redis.io)



# 持久化

需要更改和加载配置文件

## RDB

默认方式，不需要配置

### 原理

在一定时间内监测key变化情况然后持久化

### 步骤

1. 编辑redis.windows.conf

   save 900 1

   after 900 sec (15 min) if at least 1 key changed

   save 300 10

   after 300 sec (5 min) if at least 10 keys changed

   save 60 10000

   after 60 sec if at least 10000 keys changed

2. 使用命令行窗口打开redis

   cmd:>redis-server.exe redis.windows.conf

   

## AOF



### 原理

日志记录，记录每一条命令，然后持久化

### 步骤

配置文件中找到

appendonly no(关闭) --> appendonly yes

appendfsync always 每次都持久化

appendfsync evertsec 每一秒进行持久化

appendfsync no 不持久化

# jedis

Java操作redis数据库工具

## 步骤

下载jar包

获取链接对象

操作	

```java
// 获取链接对象
Jedis jedis = new Jedis("localhost", 6379);
// 操作
jedis.set("zhangsan","123");
//关闭
jedis.close();
```

命令名称与方法名称一致

## Jedis连接池

内置连接池

### 步骤

```java
// 创建配置对象
JedisPoolConfig config = new JedisPoolConfig();
// 配置
config.setMaxTotal(50);
config.setMaxIdle(10);
// 创建连接池对象
JedisPool jedisPool= new JedisPool(config,"127.0.0.1",6379);
//JedisPool jedisPool= new JedisPool();// 默认参数
// 获取链接对象
Jedis jedis = jedisPool.getResource();
// 操作
jedis.set("zhangsan","123");
System.out.println(jedis.get("zhangsan"));
// 归还链接
jedis.close();
```





# Springboot整合

简单。略
