---
title: "Redis"
description: "Redis中数据类型、数据结构。"
published: "2023-11-26"
cover: "https://r2-img.lnbiuc.com/blog/2023/11/c3ed9c147860e89096d76bc4fcf8ad20.png"
tags: ["redis"]
views: "77"
---

# 简介

## 什么是Redis

Redis是内存高速缓存数据库。全称为：Remote Dictionary Server（远程数据服务）。

Redis由c语言编写，key-value存储系统，支持string、list、set、zset、hash物种数据类型。

Redis可用于缓存、事件发布或订阅、高速队列等场景。基于内存，可持久化。

## Redis的优势

- 读写性能优异
- 丰富的数据类型
- 原子性
- 丰富的特性（发布订阅、通知、过期）
- 支持持久化（RDB和AOF方式持久化）
- 分布式

## Redis可以做什么

- 数据缓存
- 限时业务
- 计数器（incrby）
- 分布式锁（基于Redisson实现分布式锁）
- 限流（使用Redis中的List作为消息队列）
- 消息队列

# 安装/启动

## 启动

```bash
# 在前台启动
redis-server
# 在后台启动
brew services start redis
# 关闭
brew services stop redis
```

## 连接

```bash
redis-cli
```

> 默认6379端口
>
> redis默认有16个数据库，0-15
>
> 使用`select 1`来切换

# 基础命令

- `keys *`查看当前数据库中有哪些key（有16个数据库）
- `exists key`判断这个key是否存在
- `type key`查看这个key的类型
- `del key`或`unlink key`删除某个key
  - `unlink`异步删除
- `expire key 10`设置key过期时间 秒
- `ttl key`查看key多少秒过期 -1永不过期 -2已经过期
- `select`切换数据库
- `dbsize`查看当前数据库中key数量
- `flushdb`清除当前数据库所有key

# 基础类型（5种）

## string（字符串）

> - string类型是二进制安全的，可以包含任何数据
> - string是redis的基本数据类型，一个value最大512Mb

### 命令

- `set <key> <value>`
- `get <key>`查看
- `append <key> <value>`在key后追加value
  - 如果这个key不存在，则会创建
- `strlen <key>`获取这个key对应value长度
- `setnx <key> <value>`只有当key不存在是创建
- `incr <key>`这个key的value+1
- `decr <key>`value-1
- `incrby/decrby <key> <num>`key的value加num或者减num

这些操作是原子性操作，在执行操作期间不会别别的线程打断，因为redis是单线程

- `mset <key> <value> <key> <value> <key> <value>`一次设置多个ky
- `mget <key> <key> <key>`一次获取多个key
- `msetnx <key> <value> <key> <value>`一次设置多个 当key不存在时候
  - 原子性操作 一个失败全部失败
- `getrange <key> <开始> <结束>`获取key中第<开始><结束>中间的所有value
- `setrange <key> <offset> <value>`在key中第offset之后覆盖设置value
- `setex <key> <过期时间> <value>`在设置key时设置过期时间
- `get set <key> <value>`取之前的，取完之后再设置新的

### 数据结构

Arraylist

### 使用场景

1. 缓存：使用Redis作为MySQL缓存，降低MySQL读写压力
2. 计数器：Redis是单线程的优势
3. session：spring session + redis实现session共享

## list（列表）

> 单键多值，底层双向链表

### 命令

- `lpush/rpush <key> <v1> <v2> <v3>`从双向链表左右两边push值
- `Large <key> 0 -1`编列链表，获取所有
- `rpop/lpop <key>`从左右获取并删除值
- `rpoplpush <k1> <k2>`从k1右边pop一个值，在push到k2左边
- `lindex <key> idnex`获取第index个值
- `llen <k>`获取长度
- `linsert before/after <v1> <v2>`在v1前/后插入v2
- `lrem <key> <n> <v>`从做百年删除n个value
- `lset <k> <idnex> <v>`将index设置成v

### 数据结构

元素较少的情况下是ziplist，元素多了之后会将多个ziplist组成一个quicklist

### 使用场景

1. 消息队列

## set（集合）

> set集合中的元素不能重复，底层hash表，增删改查都是O(1)复杂度

### 命令

- `sadd <k> <v1> <v2>`添加
- `smamber <k>`取出该集合中所有值
- `sismamber <k> <v>`判断是否存在这个value
- `scard <k>`返回这个集合长度
- `srem <k> <v> <v>`删除
- `spop <k>`随机取出并删除一个
- `srandmameber <k> <n>`随机取出n个，不删除
- `smove <k1> <k2> <v>`把k1中的value取出来，放到k2中
- `sinter <k1> <k2>`返回 交集
- `sunion <k1> <k2>` 返回 并集
- `sdiff <k1> <k2>`返回差集

### 使用场景

1. 点赞、转发、收藏等

## hash（散列）

> 类似于java中的map<string,object>，key是对象名，value里边是fild和value

### 命令

- `hset <k> <field> <value>`设置key 和key中的field和value
- `hget <k> <field>` 获取k中field中的value
- `hmset <k> <field1> <v1> <field2> <v2>`一次设置多个
- `hexists <k1> <field>`查看key中的filed是否存在
- `hkeys <key>`获取该key的所有field
- `havals <key>`获取该key的所有vlaue，没有field
- `hincrby <k> <f> <n>`给f的value加n
- `hsetnx <k> <f> <v>`filed不存在则设置value

### 数据结构

当field-value长度短且个数少时使用ziplist，否则使用hashtable

### 使用场景

1. 缓存

## Zset（有序集合）

> 有序集合
>
> 每个元素都有评分（score），使用这个score来给集合排序，score可以重复
>
> 加权排序set，类似于Java中优先队列，只不过redis中将队列更换为set

### 命令

- `zadd <k> <score> <k> <score> <k> <score>`添加key和score，会根据score排名
- `arange <k> <start> <stop> withscores`获取scores在开始和结束之间的元素
- `zrangebyscore <k> <mim> <max>`返回并且排名 最小scores和最大scores
- `zincrbg <k> <n> <v>`为元素v添加n的scores
- `zrem <k> <v>`删除
- `zcount <k> <min> <max>`统计min scores 和max scores之间元素个数
- `zrank <k> <v>`获取该元素的排名，从0开始

### 数据结构

hash 跳跃表

### 使用场景

1. 排行榜

# 特殊类型（3种）

> 以下内容的学习均来自于此，使用了代码片段
>
> [原文地址:link:](https://www.pdai.tech/md/db/nosql-redis/db-redis-data-type-special.html#hyperloglogs%E5%9F%BA%E6%95%B0%E7%BB%9F%E8%AE%A1)

## HyperLogLogs（基数统计）

### 什么是基数

两个集合的全集

例如：A = {1,2,3,4,5,6} ; B = {5,6,7,8,9} ; 基数 = {1,2,3,4,7,8,9} ，即不重复元素。

### 使用场景

统计和计数，用于统计注册IP数，每日访问IP书，页面实时UV，在线用户数等。

### 优势

基于基数估算的算法，能比较准确的估算出基数，可以使用少量固定的内存存储并识别集合中唯一的原色。

这个算法并不一定准确，有0.81%的错误率，基数统计适合接受一定容错的场景，比如统计访客数量等。

### 命令

```sh
127.0.0.1:6379> pfadd key1 a b c d e f g h i	# 创建第一组元素
(integer) 1
127.0.0.1:6379> pfcount key1					# 统计元素的基数数量
(integer) 9
127.0.0.1:6379> pfadd key2 c j k l m e g a		# 创建第二组元素
(integer) 1
127.0.0.1:6379> pfcount key2
(integer) 8
127.0.0.1:6379> pfmerge key3 key1 key2			# 合并两组：key1 key2 -> key3 并集
OK
127.0.0.1:6379> pfcount key3
(integer) 13
```

## Bitmap（位存储）

Bitmap是通过二进制记录信息，只有 0 和 1 两个状态

### 使用场景

1. 统计用户登陆信息，登陆和未登陆
2. 统计用户点赞信息，点了和没点
3. 统计员工打卡信息，打了和没打

### 命令

- 使用`setbit <key> <offset> <value>`写入数据，其中key可以任意，offset只能为Integer，value只能为0或1

  如果要统计员工一周的打卡情况，key可以设置为sign，offset设置为0-6，打卡为0，未打卡为1

```sh
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 1
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 0
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 1
(integer) 0
```

- 使用`getbit <key> <offset>`来查看某一个key的状态，返回0或1

```sh
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 5
(integer) 0
```

- 使用`bitcount <key>`统计某一项数据的总数

```sh
127.0.0.1:6379> bitcount sign
(integer) 3
```

## geospatial（地理位置）

可以用户推算地理位置信息，如两地之间距离等

### 命令

- `geoadd`添加位置

```sh
127.0.0.1:6379> geoadd china:city 118.76 32.04 nanjing 112.55 37.86 taiyuan 123.43 41.80 shenyang
(integer) 3
127.0.0.1:6379> geoadd china:city 144.05 22.52 shengzhen 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 3
```

- `geopop`获取指定成员的地理位置

```sh
127.0.0.1:6379> geopos china:city taiyuan nanjing
1) 1) "112.54999905824661255"
   1) "37.86000073876942196"
2) 1) "118.75999957323074341"
   1) "32.03999960287850968"
```

- `geodist`获取两地之间的距离，可以指定单位

```sh
127.0.0.1:6379> geodist china:city taiyuan shenyang m
"1026439.1070"
127.0.0.1:6379> geodist china:city taiyuan shenyang km
"1026.4391"
```

- `georadius`

[这里有详细解释:link:](https://cloud.tencent.com/developer/section/1374022)

附近的人，获得所有附近的人的地址, 定位, 通过半径来查询

- `georadiusbymember`

[link:link:](https://cloud.tencent.com/developer/section/1374023)

显示与指定成员一定半径范围内的其他成员

# Redis中的数据结构

- 5种基础数据结构：string（字符串）、list（列表）、set（集合）、hash（散列）、zset（有序集合）
- 3种特殊数据结构：HyperLogLogs（基数统计）、Bitmap（位存储）、Geospatial（地理位置）

# 对应关系

> [图片来自](https://www.pdai.tech/md/db/nosql-redis/db-redis-data-type-enc.html)

![image-20221016185227739](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20221016185227739.png)

# 动态字符串SDS

Redis并没有使用C中的字符串，而是自己定义了一种数据结构，即简单动态字符串（Simple Dynamic String SDS）。Redis中String的底层数据结构使用的就是SDS。

## SDS内存结构

- `len`保存SDS保存字符串的长度
- `buf[]`数组保存字符串的每个元素
- `alloc`分别以unint8、unint16、unint32、unint64表示整个SDS
- `flags`始终为一字符，以低三位标示着头部的类型

## SDS的优势

- **O(1)复杂度获取字符串长度**：在Redis中如果要获取字符串长度只需读取len属性即可，在C中如果要获取字符串长度需要遍历整个字符串，是O(n)复杂度。使用`strlen <K>`获取字符串长度。
- **防止缓冲区溢出**：在C中，使用`strcat`中拼接两字符串，如果没有分配足够的内存空间，会造成缓冲区溢出。SDS会先根据len先推算拼接后的长度是否满足内存空间要求。不满足会先扩展，再拼接，从而防止缓冲区溢出。
- **减少修改字符串内存重新分配次数**：SDS使用`len`和`alloc`实现**空间预分配**和**惰性空间释放**。
  - **空间预分配**：对字符串空间扩展时，扩展大于需求，从而减少字符串长度增加所需的内存重新分配次数。当字符串小于1M时，redis会分配字符串大小一倍的空间，当字符串大小大于1M是，额外多分配1M空间。
  - **惰性空间释放**：字符串长度减少时，不立即归还内存，而是使用`alloc`属性将这些空间记录下来，等待后续使用。如果觉得这样会浪费空间，可以自行设置定时释放未使用空间。
- **二进制安全**：C中使用空字符来表示一个字符串的结束。但是如果Redis中保存的二进制文件（如图片）中可能包含空字符，C无法正确读取。SDS使用`len`属性表示的长度来判断字符串是否结束。

# 压缩列表ZipList

ziplist是一种为提高存储效率设计的特殊编码的双向链表。可用于存储字符串或整数。

## ziplist内存结构

- `zlbytes`存储整个ziplist所占用内存字节数
- `zltail`存储ziplist中最后一个entry的偏移量，用于快速定位最后一个entry，方便快速pop操作
- `zllen`存储ziplist中entry的数量。
- `zlend`是终止字节

## Entry结构

<prevlen> <encoding> <entry-data>

- `prevlen`前一个entry的大小
- `encoding`当前entry的类型和长度
- `entry-data`entry数据

PS：如果entry-data中保存的是整形数据，encoding会和entrydata合为一个，此时entry结构为<prevlen> <encoding>

## ziplist优点

节省内存

- 普通List保存是，每个元素占用空间大小一致，均为当前List中最大元素所占的空间大小，造成了空间浪费。
- ziplist通过特殊的数据结构，尽量是每个元素占用与自身大小相同的空间，减少空间浪费。
- 传统List在内存中地址连续，ziplist所占大小不一致，所以地址不连续，需要通过`prelen`字段保存上一个元素的length来解决遍历问题。

## ziplist缺点

- 不预留内存空间，list中元素被移除之后list无用空间立即归还，这导致每一次修改list都需要重新分配空间。

# 快表QuickList

以ziplist作为结点的双端链表，如何理解QuickList：`List<List<ziplist>> quickList`

## quicklist在内存中结构

- `quicklistNode`链表中的节点，在quicklist中，这个节点是ziplist
- `quickListLZF`ziplist经过LZ4算法压缩后，可以包装成一个`quickListLZF`结构
- `quicklistBookmark`位于quicklist尾部
- `quicklist`
  - head、tail指向头尾指针
  - len表示链表中的节点
  - count表示quicklist中ziplist中entry数目
  - fill控制每个quicklist中ziplist最大所占空间
  - compress控制是否要对每个ziplist以LZ4算法进行压缩以节省空间
- `quicklistIter`迭代器
- `quicklistEntry`对ziplist中entry封装

# 哈希表-Dict-HashTable

哈希表于大多数语言中的HashTable无异，差别主要存在于解决哈希冲突、扩容。

## 哈希冲突

使用链地址法，[在Java数据结构中已经学习过了](https://www.beyondhorizon.top/article/xclfxsXC)

## 扩容

扩容步骤：

1. 根据原哈希表所占空间大小创建一个新哈希表，大小为原哈希表一倍，（收缩也是减小为原来一倍大小）
2. 使用哈希算法重新计算索引值
3. 新表创建并赋值完毕之后删除原表

扩容发生条件

1. redis没有执行`bgsave`或`bgrewriteaof`命令，负载因子大于等于1
2. redis没有执行`bgsave`或`bgrewriteaof`命令，负载因子大于等于5

负载因子 = 哈希表中节点数量 / 哈希表大小

# 整数集 IntSet

intset是redis中集合类型底层实现之一，当一个集合只包含整数数值元素，并且元素数量较少，redis使用intset作为集合键的底层实现

## 内存结构

- `encoding`编码方式
- `length`存储整数个数
- `contents`指向实际存储数值的连续内存区域

## 扩容

intset底层为数组，当新插入的元素大小大于原数组中最大元素的大小是，就需要对整个数组进行扩容，每个元素所占空间大小修改为最大元素的大小

会扩容但不会缩容！

# 跳表ZSkipList

跳跃表的目的类似于红黑树，为了性能。跳跃表可以在对数时间内完成插入、查找、删除。但是跳跃表所占空间较大，属于空间换时间的数据结构。

## 什么是跳跃表

传统链表如果需要查询元素，需要从头遍历整个链表知道找到查询元素，时间复杂度O(n)。跳跃表通过增加多级索引的方式减少遍历长度，从而加快查找速度。跳跃表实现了平衡树、哈希表类似的效果。

为什么不使用平衡树，而是跳跃表

> [链接:link:](https://www.jianshu.com/p/8ac45fd01548)

[图片来自](https://www.pdai.tech/md/db/nosql-redis/db-redis-x-redis-ds.html#%E6%95%B4%E6%95%B0%E9%9B%86---intset)

![image-20221009184634434](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20221009184634434.png)
