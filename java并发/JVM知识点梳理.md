## JVM总览
* J核心组件与运行机制
* 内存管理
  * JVM堆
  * JVM方法区
* JVM垃圾回收
  * 可达分析算法
  * 垃圾回收原理
  * 垃圾回收算法
  * 垃圾回收器
* 性能优化
  * 堆内存分配
  * 垃圾回收期选项


## JVM的组成 & 区域的作用


## java 对象的内存管理流程是如何的
1、对象创建 在堆内存（Heap）‌，对象主体存储在堆中（包括实例变量、对象头等）；
2、大多数新对象在Eden区分配（新生代 Young Generation）；大对象‌直接进入老年代（Old Generation）；
3、JVM通过 ‌可达性分析算法（Reachability Analysis）‌ 判断对象是否存活；GC Roots作为起点（包括栈引用、静态变量、JNI引用等）（四种引用类型）。
从GC Roots向下搜索，形成引用链。‌不在引用链上的对象视为可回收，
4、如果分配在Eden区的对象创建多导致，导致Eden区满时触发，‌新生代回收（Minor GC）；采用回收算法：
   （复制算法Copying ：存活对象从Eden和Survivor区（From区）复制到另一块Survivor区（To区）年龄计数‌：每次存活的对象年龄+1，年龄达到阈值（默认15）则晋升到老年代），若Survivor区空间不足，对象直接进入老年代
5、如果（老年代空间不足、方法区（元空间）不足、调用 System.gc()）导致处罚老年代回收（Major GC/Full GC）采用回收算法：
标记-清除（Mark-Sweep）‌：标记垃圾对象后清除（产生内存碎片）。
‌标记-整理（Mark-Compact）‌：标记后清理并整理内存（避免碎片）。
6、对象创建 → 分配至Eden区
                ↘ 若Eden满 → Minor GC
                    → 存活对象进入Survivor区（年龄+1）
                    → 年龄达到阈值 → 晋升老年代
            ↘ 老年代满 → Full GC（回收新生代+老年代）
                    → 不可达对象被回收
                    → 内存整理


## 吞吐量还是响应时间
首先引入两个概念：吞吐量和低延迟

吞吐量 = CPU在用户应用程序运行的时间 / （CPU在用户应用程序运行的时间 + CPU垃圾回收的时间）

响应时间 = 平均每次的GC的耗时

通常，吞吐优先还是响应优先这个在JVM中是一个两难之选。

堆内存增大，gc一次能处理的数量变大，吞吐量大；但是gc一次的时间会变长，导致后面排队的线程等待时间变长；相反，如果堆内存小，gc一次时间短，排队等待的线程等待时间变短，延迟减少，但一次请求的数量变小（并不绝对符合）。

无法同时兼顾，是吞吐优先还是响应优先，这是一个需要权衡的问题。

## 垃圾回收器设计上的考量
JVM在GC时不允许一边垃圾回收，一边还创建新对象（就像不能一边打扫卫生，还在一边扔垃圾）。

JVM需要一段Stop the world的暂停时间，而STW会造成系统短暂停顿不能处理任何请求；

新生代收集频率高，性能优先，常用复制算法；老年代频次低，空间敏感，避免复制方式。

所有垃圾回收器的涉及目标都是要让GC频率更少，时间更短，减少GC对系统影响！


目前主流的垃圾回收器配置是新生代采用ParNew，老年代采用CMS组合的方式，或者是完全采用G1回收器


## ‌关键优化机制‌
‌分代收集（Generational Collection）‌

新生代（Eden + Survivor ×2）使用复制算法（高频GC，对象朝生夕死）。
老年代使用标记-清除/整理算法（低频GC，对象存活时间长）。
Stop-The-World（STW）‌
垃圾回收时需暂停用户线程（耗时与堆大小成正比），影响响应时间。优化目标：减少STW时长。

‌垃圾收集器选择‌

Serial（单线程）、Parallel（吞吐量优先）、CMS（低延迟）、G1（平衡）、ZGC（超低延迟）等。
不同收集器适用于不同场景（如CMS已废弃，推荐G1/ZGC）。


## 常用JVM 参数的概念
https://mp.weixin.qq.com/s/VVXuTvOuuqeF5dYZ_Hy5NA

基于4C8G系统的ParNew+CMS回收器模板（响应优先），新生代大小根据业务灵活调整！

```json
-Xms4g  
-Xmx4g
-Xss1m
-Xmn2g
-XX:SurvivorRatio=8  
-XX:MaxTenuringThreshold=10
‐XX:PretenureSizeThreshold=1M
‐XX:+UseParNewGC
-XX:+UseConcMarkSweepGC 
-XX:CMSInitiatingOccupancyFraction=70  
-XX:+UseCMSInitiatingOccupancyOnly  
-XX:+AlwaysPreTouch  
-XX:+HeapDumpOnOutOfMemoryError  
-verbose:gc  
-XX:+PrintGCDetails  
-XX:+PrintGCDateStamps  
-XX:+PrintGCTimeStamps  
-Xloggc:gc.log  
```
参数解读：
1、‐Xms4g ‐Xmx4g 最小最大堆设置为4g，最大最小设置为一致防止内存抖动
2、‐Xss1M 线程栈1m
3、‐Xmn2g ‐XX:SurvivorRatio=8 年轻代大小2g，eden与survivor的比例为8:1:1，也就是1.6g:0.2g:0.2g
4、-XX:MaxTenuringThreshold=10 年龄为10进入老年代 
5、‐XX:PretenureSizeThreshold=1M 大于1m的大对象直接在老年代生成
6、‐XX:+UseParNewGC ‐XX:+UseConcMarkSweepGC 使用ParNew+cms垃圾回收器组合
7、‐XX:CMSInitiatingOccupancyFraction=70 老年代中对象达到这个比例后触发fullgc
8、‐XX:+UseCMSInitiatinpOccupancyOnly  老年代中对象达到这个比例后触发fullgc，每次
9、‐XX:+AlwaysPreTouch 强制操作系统把内存真正分配给IVM，而不是用时才分配。
10、HeapDumpOnOutOfMemoryError：配置OOM时候的内存dump文件和GC日志，注意输出4G的HeapDump，会导致IO性能问题，在普通硬盘上，会造成20秒以上的硬盘IO跑满，
11、-Xloggc:gc.log ，GC的日志的输出


如果是GC的吞吐优先，推荐使用G1，基于8C16G系统的G1回收器模板
```json
-Xms8g  
-Xmx8g  
-Xss1m  
-XX:+UseG1GC  
-XX:MaxGCPauseMillis=150  
-XX:InitiatingHeapOccupancyPercent=40  
-XX:+HeapDumpOnOutOfMemoryError  
-verbose:gc  
-XX:+PrintGCDetails  
-XX:+PrintGCDateStamps  
-XX:+PrintGCTimeStamps  
-Xloggc:gc.log
```
解释：
针对-XX:MaxGCPauseMillis来说，参数的设置带有明显的倾向性：调低↓：延迟更低，但MinorGC频繁，MixGC回收老年代区减少，增大Full GC的风险。调高↑：单次回收更多的对象，但系统整体响应时间也会被拉长。
针对InitiatingHeapOccupancyPercent来说，调参大小的效果也不一样：调低↓：更早触发MixGC，浪费cpu。调高↑：堆积过多代回收region，增大FullGC的风险。



