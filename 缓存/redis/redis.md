# 概栏
* Redis原理
* Redis的数据结构
* Redis的高可用部署
* Redis的用法&方案
* Redis的优化

# Redis的数据结构
## Redis 中常见的数据类型有哪些？
- Redis有以下这五种基本类型：  
  String（字符串）  
  Hash（哈希）  
  List（列表）  
  Set（集合）  
  zset（有序集合）
- 三种特殊的数据结构类型:  
  Geospatial(地理位置,主要用途：朋友的定位，附近的人，打车距离计算)  
  Hyperloglog  
  Bitmap



### String（字符串）
- 简介:String是Redis最基础的数据结构类型，它是二进制安全的，可以存储图片或者序列化的对象，值最大存储为512M
- 简单使用举例:
  - 增:set key value、
  - 查询：get key
  - 删除：del key
- 应用场景：共享session、分布式锁，计数器、限流。
- 内部编码有3种，int（8字节长整型）/embstr（小于等于39字节字符串）/raw（大于39个字节字符串）


内部编码机制自动识别存储的数据是整数还是字符串，具体规则如下：

1. int 编码（整数类型）‌
   触发条件‌：当存储的值是整数且可以用 long 类型表示时（如 SET key 123），Redis 会直接以整型存储，编码标记为 int。
   优化机制‌：Redis 会预创建 0-9999 的共享整数对象池，若值在此范围内则直接复用，减少内存分配。
2. embstr 编码（短字符串）‌
   触发条件‌：存储的字符串长度 ≤44 字节（部分版本阈值可能不同），且内容非整数时（如 SET key "hello"），Redis 使用连续内存块存储字符串和元数据，编码标记为 embstr。
   特点‌：内存分配一次完成，减少碎片，适合短字符串高频访问场景。
3. raw 编码（长字符串或动态数据）‌
   触发条件‌：字符串长度 >44 字节或动态修改后（如追加操作），编码转为 raw，此时 SDS（简单动态字符串）与元数据分离存储。
   特点‌：支持动态扩容，适合大文本或二进制数据。
   动态识别逻辑
   类型检查‌：写入时 Redis 会尝试将值解析为整数，成功则用 int 编码，失败则按字符串处理。
   编码转换‌：若对 int 编码的值执行字符串操作（如 APPEND），会自动转为 raw 编码。

C语言的字符串是char[]实现的，而Redis使用SDS（simple dynamic string） 封装，sds源码如下：
```
struct sdshdr { 
unsigned int len; // 标记buf的长度 
unsigned int free; //标记buf中未使用的元素个数 
char buf[]; // 存放元素的
}
```

Redis为什么选择SDS结构，而C语言原生的char[]不香吗？
```
1. ‌性能优化‌
O(1) 时间复杂度获取长度‌
SDS 通过 len 字段直接记录字符串长度，而 C 字符串需遍历直到 \0 结束符，时间复杂度为 O(n)。
减少内存重分配‌
SDS 采用空间预分配（扩展时多分配空闲空间）和惰性空间释放（缩短时不立即回收），避免频繁内存操作。例如，扩展时若长度 <1MB，会额外分配与 len 等长的空闲空间。
2. ‌安全性增强‌
杜绝缓冲区溢出‌
C 字符串拼接时需手动检查空间，而 SDS 通过 free 字段自动判断剩余空间并扩容。
二进制安全‌
C 字符串以 \0 结尾，无法存储含 \0 的二进制数据（如图片），而 SDS 通过 len 判断结束，支持任意二进制数据。
3. ‌功能扩展‌
兼容 C 字符串函数‌
SDS 在 buf 末尾保留 \0，可复用部分 C 库函数，同时通过 len 避免依赖 \0。
动态扩容机制‌
SDS 自动处理字符串增长，而 C 字符串需手动重新分配内存。
4.C 字符串的缺陷‌
长度计算低效‌：遍历字符数组耗时。
内存管理风险‌：拼接易溢出，缩短需立即释放内存。
功能局限‌：仅支持文本数据，无法处理二进制。

总结
SDS 通过空间换时间策略（预分配、惰性释放）和结构设计（len、free 字段）解决了 C 字符串的性能瓶颈与安全隐患，同时满足 Redis 高频修改和二进制存储的需求。

```





### List（列表）

- 简介：列表（list）类型是用来存储多个有序的字符串，一个列表最多可以存储2^32-1个元素。
- 简单实用举例：lpush key value [value ...] 、lrange key start end
- 内部编码：ziplist（压缩列表）、linkedlist（链表）
- 应用场景：消息队列，文章列表,
  一图看懂list类型的插入与弹出：


编码转换机制
Redis 会在满足以下条件时自动触发编码转换：

当 ziplist 中的元素数量超过 list-max-ziplist-entries（默认 512）
当 ziplist 中任一元素大小超过 list-max-ziplist-value（默认 64 字节）

转换是单向的：一旦从 ziplist 转为 linkedlist，就不会再转回 ziplist。


ziplist:特殊编码的双向链表，它存储在连续内存上，由 zlbytes（记录 ziplist 整个结构体的占用空间大小）、zltail（记录最后一个 entry 的偏移量）、zllen（记录 entry 的数量）、entry（真正存数据的结构）和 zlend（结束标识）组成。
这种编码方式能提高内存利用率，但查找和插入的时间复杂度为 O (n)，适用于元素较少的情况



### Hash（哈希）

- 简介：在Redis中，哈希类型是指v（值）本身又是一个键值对（k-v）结构
- 简单使用举例：hset key field value 、hget key field
- 内部编码：ziplist（压缩列表） 、hashtable（哈希表）
- 应用场景：缓存用户信息等。
- 注意点：如果开发使用hgetall，哈希元素比较多的话，可能导致Redis阻塞，可以使用hscan。而如果只是获取部分field，建议使用hmget。

ziplist（压缩列表）实现hash:
压缩列表中，哈希的键值对按「键 1 → 值 1 → 键 2 → 值 2 → ...」的顺序依次存储。例如，哈希 user:1 包含 name: "张三" 和 age: "20" 两个键值对，在压缩列表中会存储为：
["name", "张三", "age", "20"]

哈希的键值对数量超过 hash-max-ziplist-entries（默认 512），或任一键 / 值的长度超过 hash-max-ziplist-value（默认 64 字节）时，Redis 会自动将压缩列表编码转换为哈希表（hashtable）

### Set（集合）

- 简介：集合（set）类型也是用来保存多个的字符串元素，但是不允许重复元素
- 简单使用举例：sadd key element [element ...]、smembers key
- 内部编码：intset（整数集合）、hashtable（哈希表）
- 注意点：smembers和lrange、hgetall都属于比较重的命令，如果元素过多存在阻塞Redis的可能性，可以使用sscan来完成。
- 应用场景：用户标签,生成随机数抽奖、社交需求。


intset（整数集合）：存储的所有元素都是整数且元素数量较少（元素数量不超过 set-max-intset-entries 配置（默认 512）   ）时，会使用 intset（整数集合）；intset 中的元素按升序排列，查询时通过二分查找实现 O (log n) 的时间复杂度，比无序数组的遍历（O (n)）更高效。


### 有序集合（zset）

- 简介：已排序的字符串集合，同时元素不能重复
- 简单格式举例：zadd key score member [score member ...]，zrank key member
- 底层内部编码：ziplist（压缩列表）、skiplist（跳跃表）
- 应用场景：排行榜，社交需求(如用户点赞)
- 有序集合的有序如何实现的:  
  1、当有序集合的元素个数小于zset-max-ziplist-entries（默认为128个），并且每个元素成员的长度小于zset-max-ziplist-value（默认为64字节）,使用压缩列表;  每个集合元素由两个紧挨在一起的两个压缩列表结点组成，其中第一个结点保存元素的成员，第二个结点保存元素的分支。压缩列表中的元素按照分数从小到大依次紧挨着排列，有效减少了内存空间的使用
  2、当有序集合的元素个数大于等于zset-max-ziplist-entries（默认为128个），或者每个元素成员的长度大于等于zset-max-ziplist-value（默认为64字节）的时候，使用跳跃表+哈希表作为有序集合的内部实现;当条件不满足时，压缩列表可以转换为跳跃表，但跳跃表不能转换为压缩列表。  
  https://www.cnblogs.com/WJ5888/p/4516782.html
  https://blog.csdn.net/weixin_42002747/article/details/115598516?spm=1001.2101.3001.6650.11&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-11-115598516-blog-123549890.pc_relevant_recovery_v2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-11-115598516-blog-123549890.pc_relevant_recovery_v2&utm_relevant_index=12

  



## Redis 中跳表的实现原理是什么？



# Redis的高可用部署

## Redis 主从复制的实现原理是什么？

## Redis 集群的实现原理是什么？

  

# Redis的用法&方案
## Redis 中如何实现分布式锁？

## 分布式锁在未完成逻辑前过期怎么办？

## Redis 的 Red Lock 是什么？你了解吗？


## Redis 实现分布式锁时可能遇到的问题有哪些？


## Redis 中如何保证缓存与数据库的数据一致性？

    - 

# Redis的优化
## Redis 中的 Big Key 问题是什么？如何解决？


## 如何解决 Redis 中的热点 key 问题？
