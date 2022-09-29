https://blog.csdn.net/weixin_44795847/article/details/123702038
https://dragon.blog.csdn.net/article/details/81907022?spm=1001.2101.3001.6650.4&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4-81907022-blog-114673408.t0_edu_mlt&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4-81907022-blog-114673408.t0_edu_mlt&utm_relevant_index=9

一、Guava Cache适用场景
1、你愿意消耗一部分内存来提升速度；
2、你已经预料某些值会被多次调用；
3、缓存数据不会超过内存总量；
4、数据允许不时时一致
Guava Cache是一个全内存的本地缓存实现，它提供了线程安全的实现机制。整体上来说Guava cache 是本地缓存的不二之选，简单易用，性能好。


一、Guava Cache 的优势
缓存过期和淘汰机制
在GuavaCache中可以设置Key的过期时间，包括访问过期和创建过期
GuavaCache在缓存容量达到指定大小时，采用LRU的方式，将不常使用的键值从Cache中删除
并发处理能力
GuavaCache类似CurrentHashMap，是线程安全的。
提供了设置并发级别的api，使得缓存支持并发的写入和读取
采用分离锁机制，分离锁能够减小锁力度，提升并发能力
分离锁是分拆锁定，把一个集合看分成若干partition, 每个partiton一把锁。ConcurrentHashMap就是分了16个区域，这16个区域之间是可以并发的。GuavaCache采用Segment做分区。
更新锁定
一般情况下，在缓存中查询某个key，如果不存在，则查源数据，并回填缓存。（Cache Aside Pattern）在高并发下会出现，多次查源并重复回填缓存，可能会造成源的宕机（DB），性能下降
GuavaCache可以在CacheLoader的load方法中加以控制，对同一个key，只让一个请求去读源并回填缓存，其他请求阻塞等待。
集成数据源
一般我们在业务中操作缓存，都会操作缓存和数据源两部分GuavaCache的get可以集成数据源，在从缓存中读取不到时可以从数据源中读取数据并回填缓存
监控缓存加载/命中情况
统计


二、Guava提供两种不同的方法来加载数据：
CacheLoader：在build cache的时候定义一个CacheLoader来获取数据，适用的情况：有固定的方式可以根据key来加载或计算value的值，比如从数据库中获取数据
 ```java
//生成本地缓存，初始化为1000，最大为10000，当超过10000时就会使用LRU算法（最小使用算法）进行清除，有效期是12小时
private static LoadingCache<String,String> localCach = CacheBuilder
.newBuilder()
.initialCapacity(1000)
//缓存有效期12小时
.maximumSize(10000).expireAfterAccess(12, TimeUnit.HOURS)  
 .build(new CacheLoader<String, String>() {
                //默认的数据加载实现,当调用get取值的时候,如果key没有对应的值,就调用这个方法进行加载.
                @Override
                public String load(String s) throws Exception {
                    return "null";
                }
            });
```

CacheLoader ----自动加载的原理


Callable：在get的时候传入一个Callable对象，适用的情况：如果从缓存中获取不到数据，则另外计算一个出来，并把计算结果加入到缓存中



----使用方式
插入
缓存元素也可以通过Cache.put方法直接插入，但自动加载是首选的，因为它可以更容易地推断所有缓存内容的一致性。

获取
从LoadingCache查询的正规方式是使用get(K)方法。这个方法要么返回已经缓存的值，要么使用CacheLoader向缓存原子地加载新值。由于CacheLoader可能抛出异常，LoadingCache.get(K)也声明为抛出ExecutionException异常。如果你定义的CacheLoader没有声明任何检查型异常，则可以通过getUnchecked(K)查找缓存；但必须注意，一旦CacheLoader声明了检查型异常，就不可以调用getUnchecked(K)。


删除-回收


三、缓存回收方式
---被动删除机制
1、基于容量的回收（size-based eviction），有两种方式，接近最大的size或weight时回收：
基于maximumSize(long)：一个数据项占用一个size单位，适用于value是固定大小的情况
基于maximumWeight(long)：对不同的数据项计算weight，适用于value不定大小的情况，比如value为Map类型时，可以把map.size()作为weight
回收算法采用的是LRU算法。
2、定时回收（Timed Eviction）：
expireAfterAccess(long, TimeUnit)：缓存项在给定时间内没有被读/写，则回收。----LRU
expireAfterWrite(long, TimeUnit)：缓存项在给定时间内没有被写访问（创建或覆盖），则回收。
3、基于引用的回收（Reference-based Eviction），通过使用弱引用的键或值、或软引用的值，把缓存设置为允许垃圾回收器回收：
CacheBuilder.weakKeys()：使用弱引用存储键。当键没有其它（强或软）引用时，缓存项可以被GC回收
CacheBuilder.weakValues()：使用弱引用存储值。当值没有其它（强或软）引用时，缓存项可以被GC回收
CacheBuilder.softValues()：使用软引用存储值。软引用只有在响应内存需要时，才按照全局最近最少使用的顺序回收。影响性能，不推荐使用。

----主动删除机制
4、显式清除（invalidate）
个别清除：Cache.invalidate(key)
批量清除：Cache.invalidateAll(keys)
清除所有缓存项：Cache.invalidateAll()




五、缓存失效策略
当缓存需要被清理时（比如空间占用已经接近临界值了），需要使用某种淘汰算法来决定清理掉哪些数据。常用的淘汰算法有下面几种：
FIFO：First In First Out，先进先出。判断被存储的时间，离目前最远的数据优先被淘汰。
LRU：Least Recently Used，最近最少使用。判断最近被使用的时间，目前最远的数据优先被淘汰。
1、新数据插入到链表头部；
2、每当缓存命中（即缓存数据被访问），则将数据移到链表头部；
3、当链表满的时候，将链表尾部的数据丢弃
LFU：Least Frequently Used，最不经常使用。在一段时间内，数据被使用次数最少的，优先被淘汰。
算法根据数据的历史访问频率来淘汰数据，其核心思想是“如果数据过去被访问多次，那么将来被访问的频率也更高”。
LFU的每个数据块都有一个引用计数，所有数据块按照引用计数排序，具有相同引用计数的数据块则按照时间排序。
具体实现如下：
1、新加入数据插入到队列尾部（因为引用计数为1）；
2、队列中的数据被访问后，引用计数增加，队列重新排序；
3、当需要淘汰数据时，将已经排序的列表最后的数据块删除。

六、LRU和LFU实现 
1、LRU实现
利用LinkedHashMap。 用这个类有两大好处：一是它本身已经实现了按照访问顺序的存储，也就是说，最近读取的会放在最前面，最最不常读取的会放在最后（当然，它也可以实现按照插入顺序存储）。
第二，LinkedHashMap本身有一个方法用于判断是否需要移除最不常读取的数，但是，原始方法默认不需要移除（这是，LinkedHashMap相当于一个linkedlist），所以，我们需要override这样一个方法，使得当缓存里存放的数据个数超过规定个数后，就把最不常用的移除掉。


移除监听器
同步监听器：通过CacheBuilder.removalListener(RemovalListener)，你可以声明一个监听器，以便缓存项被移除时做一些额外操作。缓存项被移除时，RemovalListener会获取移除通知[RemovalNotification]，其中包含移除原因[RemovalCause]、键和值

异步监听器：默认情况下，监听器方法是在移除缓存时同步调用的。因为缓存的维护和请求响应通常是同时进行的，代价高昂的监听器方法在同步模式下会拖慢正常的缓存请求。在这种情况下，你可以使用RemovalListeners.asynchronous(RemovalListener, Executor)把监听器装饰为异步操作


刷新
刷新和回收不太一样。正如LoadingCache.refresh(K)所声明，刷新表示为键加载新值，这个过程可以是异步的。在刷新操作进行时，缓存仍然可以向其他线程返回旧值，而不像回收操作，读缓存的线程必须等待新值加载完成
重载CacheLoader.reload(K, V)可以扩展刷新时的行为，这个方法允许开发者在计算新值时使用旧的值

项目总结用法：


guava 如何解决缓存的三大问题
1、缓存击穿
2、缓存穿透 --一个数据过期场景
已有数据场景：采用数据预热+过期时间小于刷新时间
不存在的数据场景：选择使用布隆过滤器或者前置拦截或者限流
缓存空值并且设置快速过期的方式来作为一个兜底的方案----不知道是不是兜底的方案

https://baijiahao.baidu.com/s?id=1688720168018641092&wfr=spider&for=pc
3、缓存雪崩

https://cloud.tencent.com/developer/article/2102426





