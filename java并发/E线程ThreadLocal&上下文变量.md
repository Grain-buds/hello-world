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