ThreadLocal 
https://blog.csdn.net/u010445301/article/details/111322569
https://blog.csdn.net/u010445301/article/details/124935802

ThreadLocalMap里弱引用
https://blog.csdn.net/vicoqi/article/details/79743112


ThreadContext
https://blog.csdn.net/weixin_42408447/article/details/123478975

https://blog.csdn.net/weixin_49561445/article/details/120597228


ThreadLocal 的实现
ThreadLocal 是 Java 中的一个用于实现线程局部变量的工具类。它允许你创建独立于线程的变量副本，每个使用该变量的线程都有自己的变量副本，从而实现了线程间的数据隔离。这种机制是通过 Java 内存模型中的一些机制来实现的，主要包括：

1. ThreadLocal 变量的存储结构
ThreadLocal 内部通过一个 ThreadLocalMap 来为每个线程存储变量的副本。ThreadLocalMap 是 ThreadLocal 的一个静态内部类，它以 Thread 对象作为 key，以 ThreadLocal 对象作为 key，以实际存储的对象作为 value。这种方式确保了每个线程都有自己的变量副本。

2. ThreadLocalMap 的工作原理
Key: 存储的是对 ThreadLocal 对象的弱引用（使用 WeakReference）。这样做的好处是当 ThreadLocal 对象不再被外部强引用时，可以被垃圾回收器回收，从而避免内存泄漏。

Value: 存储的是实际要保存的对象。

3. 线程局部变量的获取与设置
设置值: 当调用 set(T value) 方法时，首先会通过 Thread.currentThread() 获取当前线程，然后在当前线程的 ThreadLocalMap 中以当前 ThreadLocal 对象为 key 存储值。

获取值: 当调用 get() 方法时，同样通过当前线程获取其 ThreadLocalMap，然后以当前 ThreadLocal 对象为 key 来检索值。

4. 内存隔离的实现
由于每个线程都有自己的 ThreadLocalMap，因此每个线程访问的 ThreadLocal 变量实际上是独立的，互不干扰。这种机制确保了即使在多线程环境下，每个线程对变量的修改都不会影响到其他线程的变量值。

5. 内存泄漏问题
尽管 ThreadLocal 使用弱引用存储键（即 ThreadLocal 对象），但如果使用不当时（如在 ThreadLocal 的 get() 或 set() 方法中没有正确地使用），仍然可能导致内存泄漏。例如，如果一个对象被存储在 ThreadLocal 中，而这个对象又持有外部对象的引用，并且没有其他强引用指向这个对象，那么即使 ThreadLocal 被回收了，由于外部对象的引用仍然存在，这部分对象也无法被垃圾回收器回收。

解决方案
为了避免内存泄漏，可以采取以下措施：

使用完 ThreadLocal 变量后调用其 remove() 方法，这样可以手动从当前线程的 ThreadLocalMap 中移除对应的 entry。

在使用完 ThreadLocal 后，确保不再有对该对象的强引用。

通过这些方式，可以有效地利用 ThreadLocal 实现线程间的数据隔离，同时避免内存泄漏的问题。


java的引用类型有哪几种

Java中的引用类型包括强引用、软引用、弱引用和虚引用四种‌，它们通过不同的强度控制对象的生命周期和垃圾回收行为，其中强引用是默认类型，虚引用则用于对象回收跟踪。‌

引用类型概述‌

Java的引用类型由java.lang.ref包提供，用于管理对象的内存回收策略。这些引用类型根据强度从高到低排序，直接影响垃圾回收器（GC）对对象的处理时机。核心区别在于回收条件和应用场景，开发者可根据需求选择合适的类型以避免内存泄漏或优化缓存机制。‌

四种引用类型详解‌

强引用（Strong Reference）‌。

定义‌：通过new关键字创建的默认引用类型（如Object obj = new Object()），直接指向对象。‌
回收时机‌：只要强引用存在，对象永不回收；即使内存不足导致OOM，GC也不会回收该对象。‌
应用场景‌：普通对象创建，如核心业务逻辑中的持久化对象。‌
风险‌：不当使用可能导致内存泄漏（例如未释放的集合引用）。‌

软引用（Soft Reference）‌。

定义‌：通过SoftReference类实现，用于指向非关键对象（如缓存数据）。‌
回收时机‌：仅在内存不足时回收；JVM会尽量保留对象，直到即将抛出OOM错误。‌
应用场景‌：内存敏感型缓存（如图片缓存），可自动释放资源避免OOM。‌
示例‌：
java
Copy Code
SoftReference<byte[]> softRef = new SoftReference<>(new byte[1024 * 1024]);

弱引用（Weak Reference）‌。
定义‌：通过WeakReference类实现，强度低于软引用。‌
回收时机‌：下一次GC运行时必定回收，无论内存是否充足。‌
应用场景‌：临时缓存或避免内存泄漏（如WeakHashMap的键），或监听器/回调管理。‌
示例‌：
java
Copy Code
WeakReference<String> weakRef = new WeakReference<>(new String("Weak Reference"));
System.gc(); // 对象立即被回收

虚引用（Phantom Reference）‌。
定义‌：通过PhantomReference类实现，强度最弱，无法通过get()方法获取对象。‌
回收时机‌：对象随时可能被回收，需配合ReferenceQueue使用以跟踪回收事件。‌
应用场景‌：对象回收后的资源清理（如堆外内存管理），或替代finalize()方法。‌
示例‌：
java
Copy Code
ReferenceQueue<Object> queue = new ReferenceQueue<>();
PhantomReference<Object> phantomRef = new PhantomReference<>(new Object(), queue);


引用类型对比‌

引用类型	回收时机	是否影响GC	典型应用场景
强引用	永不回收（除非显式置为null）	不回收	普通对象创建
软引用	内存不足时回收	可能回收	内存敏感缓存
弱引用	下次GC时必定回收	必定回收	临时缓存或WeakHashMap
虚引用	随时可能回收	特殊用途	对象回收跟踪与资源清理‌


Java中存在四种引用类型（强引用、软引用、弱引用、虚引用），主要是为了在不同场景下精细化控制对象的生命周期和垃圾回收行为，从而优化内存管理并解决特定问题。以下是具体原因：

1. ‌满足不同内存敏感需求‌
强引用‌：默认引用类型，确保核心对象（如业务逻辑对象）始终存活，避免被意外回收。
软引用‌：适用于缓存场景，内存不足时自动释放，防止OOM（如图片缓存）。
弱引用‌：避免内存泄漏（如ThreadLocal中的键），GC时无条件回收。
虚引用‌：用于对象回收跟踪（如资源清理通知）。
2. ‌平衡性能与资源管理‌
通过软/弱引用实现内存敏感型缓存，既提升性能又避免内存耗尽。
虚引用结合ReferenceQueue可精确感知对象回收时机，用于外部资源释放（如文件句柄）。
3. ‌解决特定场景问题‌
强引用‌可能导致内存泄漏，而弱引用可自动解除无效关联（如监听器列表）。
软引用在Android等内存受限环境中尤为重要，可动态调整缓存大小。
4. ‌提供灵活的GC策略‌

不同引用类型为JVM垃圾回收器提供了明确的回收优先级指导：强引用 > 软引用 > 弱引用 > 虚引用。这种分级设计允许开发者根据对象重要性选择最佳引用方式，而非依赖单一的“全有或全无”回收策略。