# Redis

## 为什么用 Cache
1. 性能：查询耗时又不频繁改动，适合用Cache
2. 并发：先查Cache，若无再查DB or 均从Cache返回，起异步线程查DB写Cache

> 参考：[为什么要用 Redis](https://juejin.im/post/5b516dc75188251af363492d)

## Cache Q&A
1. 缓存与数据库的双写一致性/同步
    
   先写数据库，再删缓存

> 参考：[分布式之数据库与缓存一致性](http://www.cnblogs.com/rjzheng/p/9041659.html)

2. 缓存击透

   1. key合法性校验 
   2. 有无均从Cache返回，异步查DB写Cache

3. 缓存雪崩
 
   1. 过期时间设置随机数
   2. 双缓存：缓存A有过期时间，缓存B无过期。先从A查，若无查B，再无查DB or 直接返回，异步查DB再写A和B 

4. Redis为什么快

   1. 纯内存操作
   2. Redis是单线程，避免了多线程的上下文切换
   3. 网络层使用epoll解决高并发问题，采用了非阻塞I/O多路复用机制(Pipeline)

5. 多线程竞争key

   分布式锁：乐观锁（version 或 timestamp）

6. 过期key删除机制 and 内存淘汰机制

> 参考：[Redis 3种过期键删除机制](https://segmentfault.com/a/1190000004866645)

## Redis 持久化
### RDB (redis database)：保存数据快照到磁盘
### AOF (append only file)：保存数据生成命令文件到磁盘

> 参考：[Redis 持久化原理](https://juejin.im/post/5b70dfcf518825610f1f5c16)


## Redis 数据类型与底层实现
### Redis 支持的数据类型
   
   1. String
   2. Hash
   3. List
   4. Set
   5. ZSet (Sorted Set)

### Redis 底层数据结构

   1. SDS：Simple Dynamic String 简单动态字符串
   2. LinkedList：双向链表
   3. ZipList：压缩链表
   4. SkipList：跳跃表
   5. HashTable：hash表
   6. IntSet：整数集合
   7. Dict：字典

> 参考：[Redis 底层实现](http://princessdudu.com/2018/10/15/redis%E5%9F%BA%E7%A1%80/)  

> 参考：[Redis 底层数据结构](https://www.jianshu.com/p/f8ccf8806095)

### 数据类型内外对应关系

   1. String: int, raw, embstr, sds
   2. Hash: hashtable, ziplist 
   3. List: linkedlist, ziplist
   4. Set: hashtable, intset
   5. ZSet: skiplist, ziplist

> 参考：[跳表SkipList 一](https://www.jianshu.com/p/fcd18946994e)  

> 参考：[跳表SkipList 二](https://my.oschina.net/swearyd7/blog/420939)  

> 参考：[跳表SkipList 三](http://daoluan.net/%E6%9C%AA%E5%88%86%E7%B1%BB/2014/06/26/decode-redis-data-struct-skiplist.html)  
