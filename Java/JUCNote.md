## ConcurrentHashMap
- ConcurrentHashMap 1.7，Segment 的个数一旦初始化就不能改变，默认 Segment 的个数是 16 个，你也可以认为 ConcurrentHashMap 默认支持最多 16 个线程并发
![20](./image/20.png)
- ConcurrentHashMap 1.8
![21](./image/21.png)
## volatile 关键字解决了什么问题，它的实现原理是什么？
- 它最原始的意义就是禁用 CPU 缓存，实现可见性
- 禁止指令重排序
- （1）修改volatile变量时会强制将修改后的值刷新的主内存中。（2）修改volatile变量后会导致其他线程工作内存中对应的变量值失效。因此，再读取该变量值的时候就需要重新从读取主内存中的值。
- 一个是对象头的markword，一个是aqs类的volatile int status

## synchronized 关键字底层是如何实现的？它与 Lock 相比优缺点分别是什么？
![22](./image/22.jpg)

## ThreadLocal 实现原理是什么？
- 有一个 Map，叫做 ThreadLocalMap，持有 ThreadLocalMap 的是 Thread。Thread 这个类内部有一个私有属性 threadLocals，其类型就是 ThreadLocalMap，ThreadLocalMap 的 Key 是 ThreadLocal
![23](./image/23.png)
- 线程池中线程的存活时间太长，往往都是和程序同生共死的，这就意味着 Thread 持有的 ThreadLocalMap 一直都不会被回收，再加上 ThreadLocalMap 中的 Entry 对 ThreadLocal 是弱引用（WeakReference），所以只要 ThreadLocal 结束了自己的生命周期是可以被回收掉的。但是 Entry 中的 Value 却是被 Entry 强引用的，所以即便 Value 的生命周期结束了，Value 也是无法被回收的，从而导致内存泄露。

## Java 线程和操作系统的线程是怎么对应的？Java线程是怎样进行调度的?
![24](./image/24.png)

## Java 常见锁有哪些？ReetrantLock 是怎么实现的？
- AQS


## Java内存模型
- 导致可见性的原因是缓存，导致有序性的原因是编译优化，那解决可见性、有序性最直接的办法就是禁用缓存和编译优化
- 合理的方案应该是按需禁用缓存以及编译优化
- Java 内存模型规范了 JVM 如何提供按需禁用缓存和编译优化的方法，具体来说，这些方法包括 volatile、synchronized 和 final 三个关键字，以及六项 Happens-Before 规则

## 简述 Java 的 happen before 原则
- 前面一个操作的结果对后续操作是可见的
- 程序的顺序性规则
- volatile 变量规则
- 传递性
- 管程中锁的规则
- 线程 start() 规则
- 线程 join() 规则
