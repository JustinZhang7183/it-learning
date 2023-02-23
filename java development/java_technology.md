Java 技术攻关  
====  

这是我的笔记, 排版比较乱。  
2023-01-31已整理, 现在可用markdown查看。  

# 八股文必考  
1. Java core 集合 锁  
2. 多线程  
3. 缓存 Redis  
4. 事务， 分布式事务。  
5. db优化  
6. JVM  
7. MQ  
8. Spring AOP/ bean  

其它:  
 - UT AT, devops经验  
 - scrum 敏捷经验  

# Java  Glossary 术语  
 
- J2SE - Java Platform, Standard Edition   ( - 2005)
- J2EE - Java 2 Platform, Enterprise Edition     ( - 2005)
什么是Java 2EE？  
Java 2 Platform, Enterprise Edition  
- JavaEE    (2005 - 2018)
JavaEE是一组建立在JavaSE之上的标准，解决企业级开发中的一系列问题。请特别留意，它仅仅是个标准，是对一系列接口的约定，众多厂商围绕这个标准做实现。  
例如 Servlet，JTA JPA 等等都属于J2EE标准   (总的来说还是些老古董了?)

- Jakarta EE - Jakarta Enterprise Edition (EE)   (2019 - )
  Eclipse Foundation. 提供和Java EE相同的API。 
  2017.08, Oracle将Java EE（Java SE还自己保留）交给开源组织，Eclipse基金会接手。但Oracle不允许开源组织使用Java名号，所以Jakarta EE名称于2018.02.26应运而生 

  因此， `javax` `Jakarta` 是同一个东西, 只是 EE 版本不同包名不同。
- SSH - Struts + Spring + Hibernate  
- SSM - Spring MVC + Spring + Mybatis 


ACID 内部一致性。 CAP 外部一致性。  
- BASE：全称 Basically Available（基本可用），Soft state（软状态），和 Eventually consistent（最终一致性）三个短语的缩写，来自 ebay 的架构师提出。 base理论是针对一致性。  
既然无法做到强一致性（Strong consistency），但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性（Eventual consistency）。  


- SOA（面向服务的软件架构、Service Oriented Architecture）  
  例如WEB Service, Spring MVC 都是SOA?  
RMI和RPC的区别。 什么是RMI. RMI（Remote Method Invocation，远程方法调用）需要知道对象和方法, 一般是相同的Java/c#语言之间的调用。  
- RPC （Remote Procedure Call Protocol）远程过程调用，能够跨方法。  


- OLAP(OnLine Analysis Processing ，联机分析处理) 是数据仓库系统的主要应用，支持复杂的分析操作，侧重决策支持，并且提供直观易懂的查询结果  

- JIT 即时编译（默认）  
- AOT Ahead-Of-Time, 提前编译  
Java 9 jaotc  应该还有很多坑。  

# linux 基础  
Java linux 堆栈分析  
进程 堆栈分析  
堆内存主要用于存储实例对象和JRE classes，  
栈内存用于存储基本变量和对象的引用。 （Stack pop先进后出） （栈空间比较小，但是读取速度快。）  

top  

linux 抓包  
 对网卡进行tcpdump, 再将文件放入windows wireshark进行分析。  


# network   

Wireshark 抓包是网卡，功能更强大，窃听网络通信，需要网卡支持混杂模式才能工作。 fiddler是通过网络代理方式。  
TCP/IP 报文格式（报文头+数据）  Wireshark 抓包是比较容易理解tcp/ip协议的。 源地址，目标地址，协议，长度一级封包信息。  
https://www.cnblogs.com/163yun/p/9552368.html  


* 单工：A 可以发给 B ，B 不能发给 A ，叫做单工。   
* 半双工：A 可以发给 B , B 也可以发给 A ，但是两者的步骤不能同时进行，即 A 给 B 发信息的时候，B 不能给 A 发。  
* 全双工：即客户端 A 在给服务器端 B 发信息的同时，服务器端 B 也可以给客户端 A 发送信息。 http 2.0 支持全双工  

TCP三次握手属全双工  
<pre>
1. 我要给你发信息 A->B  
2. 响应ack B->A  
3. 发信息 A->B （如果没有这一步， B不确定A是否收到了消息）  
必须3次，才能确保两两确认成功。  
</pre>

TCP 断开需要四次握手   
<pre>
[Shake 1] 套接字A：“任务处理完毕，我希望断开连接。”  
[Shake 2] 套接字B：“哦，是吗？请稍等，我准备一下。”  
处理其它未发送完的请求， 等待片刻后……  
[Shake 3] 套接字B：“我准备好了，可以断开连接了。”  
[Shake 4] 套接字A：“好的，谢谢合作。”  
</pre>

双向通信会选择TCP/IP 或者WebSocket 协议 。 长链接。  
http是单向协议。  

http协议 method 状态码 header  url地址。  


# MySQL
这部分见  
> [mysql技术攻关](../mysql_technology.md)  

# Java 基础  

var 关键字  

Q: 泛型 super/extends 区别。 List<? super Number>  与List<? extends Number>  2个关键字的区别  
  泛型 super 逆变 下界, 不是很有用, 非常少见。

.Java文件会被编译为.class 字节码文件,非机器可直接识别的二进制文件. 如果含有内部类, 匿名类， 还会生成ClassA@ClassB.class 文件。   

Java 是跨平台的, 所以jar不是可执行文件。  
Java程序启动 类加载机制  
  1. 启动（根加载）类加载器 BootstrapClassLoader jre/lib  
  2. 扩展类加载器ExtClassLoader jre/lib/ext  
  3. 应用类加载器ApplicationClassLoader  classPath 参数对应的类  

类的生命周期：   
  * 加载 loading  
  * 链接 验证Verification-准备Preparation 初始化   
  * 初始化  
  * 使用  
  * 卸载  

## 异常 处理  
Q：异常处理流程  
异常处理程序. 通过调用堆栈,从错误发生的方法开始,按照方法调用相反的顺序寻找(栈有先进后出的特点). 当找到合适的异常处理程序时（catch Exception, 运行时系统就会把异常传递给处理程序.   
所以Exception里面也有StackTrace 的相关方法。

Q：checked Exception 和RuntimeException区别。  
RuntimeException extends Exception  
优先使用标准的异常  
对可以恢复的情况使用受检异常，对编程错误使用运行时异常。  
Java最佳实践 -- 和C#一样，最好是抛弃checked Exception.

## 各集合内部实现  
HashMap  
ConcurrentHashmap   
红黑树 >8 转红黑树, <=8 还是单链表  
HashSet 内部实际上就是HashMap实现的， Value 为固定的值。  

确保一个集合不被修改,   
可使用 `Collections.unmodifiableCollection(Collection c)  `

栈和向量的关系  
Stack<E> extends Vector extends AbstractList<E> 实际上里面也是一个data数组, 通过index取值。 当元素超过数组大小的适合也会进行Arrays.copy扩容。 线程安全的。  
删除栈顶的原始直接值为null即可，Vector非top删除的话会重构数组（当然Stack不支持底部删除）。  


Q: hashMap 扩容机制。  
hashKey 计算hash槽， 如果hash碰撞， key 存在就覆盖， 不存在插入。  
遍历插入链表， 如果链表长度>8 则转红黑树  
默认负载因子/加载因子 loadFactory = 0.75f; 超过后resize()扩容。  

Q: 讲一下 HashMap 中 put 方法过程？  
<pre>
对 Key 求 Hash 值，然后再计算 下标。  
如果没有碰撞，直接放入桶中，  
如果碰撞了，以链表的方式链接到后面，  
如果链表长度超过阀值（TREEIFY_THRESHOLD == 8），就把链表转成红黑树。  
如果节点已经存在就替换旧值  
如果桶满了（容量 * 加载因子），就需要 resize。  
</pre>

## 其它
Q: 面向对象和面向过程的区别  
面向对象易维护, 适合常见的业务系统开发。  
面向过程代码简单，没有很多类需要实例化,性能更高。 适合嵌入式开发。  


Q: Integer 注意的地方  
Java在编译Integer i = 100 ;时，会翻译成为Integer i = Integer.valueOf(100)；，而Java API中对Integer类型的valueOf的定义如下：  
```java  
public static Integer valueOf(int i){  
    assert IntegerCache.high >= 127;  
    if (i >= IntegerCache.low && i <= IntegerCache.high){  
        return IntegerCache.cache[i + (-IntegerCache.low)];  
    }  
    return new Integer(i);  
}  
```  

另外关于equals Integer 的比较实际上就是小写 int 的比较。 （== 基本类型比较值， 其它类型比较地址）  
```java  
public boolean equals(Object obj) {  
	if (obj instanceof Integer) {  
		return value == ((Integer)obj).intValue();  
	}  
	return false;  
}  
```  

<mark>重要</mark>: 所以c#的同学们, 由于Java不支持操作符重载, 在判断值相等的时候, 要用`.equals()`而不是 `==`。 即时127以内的数可以用 ==, 也要按规范都统一采用equals()


Q: Java `Object.java`  
```java
  public native int hashCode(); // 这是一个本地方法， 由c++返回内存地址。 

  public boolean equals(Object obj) {
      return (this == obj);  // 判断地址相等
  }

  protected native Object clone(); // 浅拷贝, 必须要实现 Cloneable 空方法接口, 否则会报错。为什么不把方法定义到Cloneable里面呢?

  public String toString() {
      return getClass().getName() + "@" + Integer.toHexString(hashCode()); // e.g.  com.example.MyExample@691a7f8f
  }

  public final native void notify();

  public final void wait()  { // 当前线程阻塞挂起, 直到其它线程调用notify(). // wait() 会释放synchronized锁,待证实?  
      wait(0L);
  }

``` 

Q: LongAdder 与 AtomicLong  
 LongAdder从性能上来说要远远好于AtomicLong， 但get结果可能不准确。一般情况下是可以直接替代AtomicLong使用的。 但是AtomicLong 可以立即获取值。   LongAdder 是空间换时间。  
 在idea智能提示下 AtomicLong 和AtomicInteger 可以自动替换为 LongAdder。  
   
 LongAdder用到了Cell[] cells, 类似分段锁, 空间换时间。  
 AtomicLong CAS  
   

Q：什么情况会导致 XXX 程序 运行变慢？  
cpu  
磁盘 IO 、 网络IO  
内存  
性能？  
依赖的服务，例如数据库?  

Q: 如何调用操作系统本地函数  
1. System.loadLibrary("");   
2. 定义相同的 native 方法  


Q: String为什么被涉及为不可变的。  
String 内部实际上是维护了一个char[]数组字段。 // Java 17 change to byte[] 数组  
String 不可变, hashCode 就不会变。可以更好的缓存。  
多线程安全。 因为不可更改, 总是返回新的值。  
方便放入字符串池。  
同样的 public final class Integer  包装类，  public final class LocalDateTime 日期类型也是被设计为了不可变类型。  

String Java 是 `final` 类, C# 里面是sealed 密封类。  

Java 17也推出了sealed密封类, 与`final`不同的是, 可以配合 `permits` 允许特定的继承。（感觉好复杂, 定义的时候父类还要依赖子类?）  

* `final` 变量 在对其初始化之后便不能再让其指向另一个对象。  


Q：关于嵌套类, Static Nested Class 和 Inner Class。  
 内部类可以访问外部类变量。  因为new 内部类的时候, 需要先实例化外部类。
 内部静态类只能访问外部静态变量

# Java Core
Java Core 是指下面这些 还是集合，多线程？  
目前Java Core被定义为以下集合：  
基本技术  
CORBA (Common Object Request Broker Architecture) 公共对象请求代理体系结构 (这个是啥东西)    
HotSpot虚拟机 （ 也就是默认的JDK JVM实现）  
Java命名和目录接口（JNDI）  
应用程序监视和管理  
工具API  
XML  
但是，正如你可能理解的那样，即使术语'基础技术'有点不清楚;-)所以这不是那么严格的定义。这是这个词的官方网页：  

## Modules



## 什么是Java 代理/Java 探针  
 Java -Javaagent:xxx.jar   

实际上这是一个含 有premain(String agentArgs, Instrumentation inst) 方法的jar， 并指定 Premain-Class: xxx.ClassExample  

通过agentArgs 可以获取启动参数, 修改程序字节码实现一些拦截操作。  

# Java reflection 反射
```java  
Class cls = Class.forName("constructor1");
Class cls = new MyClass().getClass();
Class cls = MyClass.class
```

## Type vs Class 区别
Class是类, Type是接口。Class实现了Type接口 `class Class<T> implements Type`  
Type 是操作泛型的时候比较有用。包括原始类型、参数化类型、数组类型、类型变量和基本类型。

# Thread

Q：线程有哪几个状态?  
五种状态 State (通过jstack pid可以查看想各线程状态)  见 Thread.Java里面的枚举类 public enum State   
  1. New(new Thread())  新建, 还没调用start()  
  2. Runnable 运行 (thread.start(), ready 等待就绪, 拿到CPU使用权限后 runing  )  可运行 和运行中是同一种状态  
  3. Blocked 阻塞  
	3.1 等待阻塞。 thread.wait()   
	3.2 同步阻塞  synchronized 代码， 还没有获取到锁。 等待monitor lock 释放  
	3.3 其它阻塞。 thread.sleep join() 等  
  4. Waiting 等待 thread.wait(1000) // 注意, thread.wait实际上是Object里方法， 等待其它线程调用Object.notify()  
  5. 超时等待(TIMED_WAITING)：  thread.wait(1000) 时间到。 会抛异常IllegalMonitorStateException，主线程可捕获。  
  6. Terminated 终止， 执行完成。  


Runnable与Callable不同点：  
<pre>
  Runnable不返回任务执行结果，Callable可返回任务执行结果Future  
  Callable在任务无法计算结果时抛出异常，而Runnable不能  
  Runnable任务可直接由Thread的start方法或ExecutorService的submit方法去执行  
</pre>

使用Future的好处：  
<pre>
  获取任务的结果，判断任务是否完成，中断任务  
  Future的get方法很好的替代的了Thread.join或Thread,join(long millis)  
  Future的get方法可以判断程序代码(任务)的执行是否超时  
</pre>
CompletableFuture :    since Java 8  


# 多线程 Multithreading  
线程五种状态 State (通过jstack pid可以查看想各线程状态)  
  1. New(new Thread())  新建, 还没调用start()  
  2. Runnable 运行 (thread.start(), ready 等待就绪, 拿到CPU使用权限后 runing  )  可运行 和运行中是同一种状态  
  3. Blocked 阻塞 synchronized 代码， 还没有获取到锁。  
  4. Waiting 等待 thread.wait(1000)  
  5. 超时等待(TIMED_WAITING)：  thread.wait(1000) 时间到。 会抛异常IllegalMonitorStateException，主线程可捕获。  
  6. Terminated 终止， 执行完成。  

synchronized 只适合锁少量代码, 可以加在方法上或者代码块上. Lock是接口（有多个实现类）, 可以锁大量代码  


常用并发同步器工具类: CountDownLatch /  CyclicBarrier /  Semaphore /  Exchanger  
CompletableFuture 可完成Future, 等价于js promise  
CAS是compare and swap的缩写，即我们所说的比较交换。例如 AtomicInteger， 底层依赖CPU的支持. 乐观锁即为CAS.  


Thread.start()和Thread.run()的区别. Thread.run()是主线程运行,阻塞的。  
ThreadLocal实际上是Thead.threadLocalMap的代理。 key为threadLocal.hashmap 弱引用,value 为ThreadLocal  
Java 引用类型:Strong Reference(内存不足会OOM) > Soft Reference 软(内存不足) > Weak Reference（正常gc回收） > Phantom Reference 虚引用.  

InheritableThreadLocal （ extends ThreadLocal）  
  可以选择是否继承父线程的变量。缺点是创建后，如果父线程变量再次修改，它拿不到新变量。 可用于全链路追踪传TraceID值?  

Q: 线程调度机制  
有两种调度模型：分时调度模型（轮询）和抢占式调度模型（优先级）。 JVM采用抢占式  

当Java线程数大于cpu线程数，操作系统使用时间片机制，采用线程调度算法，频繁的进行线程切换。  

Q: Java程序至少有哪些线程?   
main / gc  

Q：Thread.yield(); 和 Thread.sleep() 区别？  
Thread.sleep 后其它cpu获取不分优先级。  
Thread.yield() 只有相同优先级和更高优先会获取cpu机会  


Java 19 支持 Java virtual threads  虚拟线程

## 线程池 Thread Pool   

ThreadPoolExecutor Parameters  
  - corePoolSize 线程池里面保持的线程数量, 即使是空闲的。 除非调用方法设置allowCoreThreadTimeOut(value)  
  - maximumPoolSize （>= corePoolSize）最多线程池数量。   
  - keepAliveTime 默认60s。 当线程数量大于corePoolSize时的存活时间。     
  - allowCoreThreadTimeOut 默认false， 如果设置为true 核心线程也会变为0.  
  - ThreadFactory threadFactory 可以用来指定线程名字。  
  - BlockingQueue<Runnable> workQueue,   
    一般设置为LinkedBlockingQueue 是常用的阻塞队列（可以为有界 int capacity 和无界（intMax））     
  - RejectedExecutionHandler handler 4个拒绝策略：  
    1. ThreadPoolExecutor.AbortPolicy 直接抛出异常RejectedExecutionException  
    2. ThreadPoolExecutor.CallerRunsPolicy 直接调用run方法并且阻塞执行。 主线程运行则会使主线程变慢，不会导致线程越来越多。  
    3. ThreadPoolExecutor.DiscardPolicy 直接丢弃后来的任务  
    4. ThreadPoolExecutor.DiscardOldestPolicy 丢弃在队列中队首的任务  

线程创建的规则。  
  - 如果当前线程 < `corePoolSize`.   -- 创建core thread.
  - 如果当前线程 >=`corePoolSize` and < `maximumPoolSize`, 并且队列已满。 -- 创建additional threads    
      因此,   
      - task是先尝试添加如queue再创建additional threads.  
      - additional threads will only be created if the queue is full.（经验证确实是这样）   
        这可能也是为什么我们把 `corePoolSize` 和 `maximumPoolSize`这2个值设置为一样?  

ThreadPoolExecutor  如果异步线程过多会产生OOM, 如果限制队列大小则会抛出 RejectedExecutionException , 可以考虑用Java 信号波让其阻塞尔非异常。  

线程池大小参考值，CPU密集型 N+1 IO密集型 2N   
   
CountDownLatch 倒数器。  
new Semaphore(num) 信号量  含方法 acquire() vs release()  

常见线程池, 以及工厂方法：
- Executors.newSingleThreadExecutor()
- Executors.newFixedThreadPool()  
- Executors.newCachedThreadPool() 无界线程池, 适合执行很短的异步任务。  
- Executors.newScheduledThreadPool() delayQueue   
- ForkJoinPool.commonPool()   
  parallelism = Math.max(1, Runtime.getRuntime().availableProcessors() - 1); 以我intel 8线程CPU为例, 此值=7。   
  可通过`java.util.concurrent.ForkJoinPool.common.maximumSpares` 配置修改。  

`ForkJoinPool` 常见于 `CompletableFuture`, `@async`. 这种默认公共实现。

提交任务 exxcuteService.execute() / .submit()  

**volatile** 
1. 申明读写变量会同步到主内存, 多线程之间才能共享内存。 (static 是实例间可见.) 
如果不使用volatile则直接从寄存器上获取, 比内存更快?
2. 可保证代码执行顺序。 （volatile 不是线程安全的）  
  单例double check双检可能在极端情况下第1个线程赋值未完成，第2个线程就读取return了。 
  加volatile 可避免指令重排序。  
  PS: 这个貌似是编译阶段的compiler reordering, 无法写代码模拟这种场景，一般很少会发生这种情况吧？  

volatile 可以保证原子性, 但是不具备排它性

Atomic 原子类型类， CPU级别方法。 不会加锁。 多CPU之间会同步内存变量。 Atomic类 和 `volatile` 关键字 都不是线程安全的。  

Q： 并发和并行的区别。  
并发，同一个CPU线程分成多个时间片。 - 间隔发生。  
并行，2个CPU同时运行各自线程。 - 同时发生。  

**CPU超线程**  
intel CPU超线程技术, 增加了5%的晶体管面积，可以提高15%-30%的性能。  

# Java Lock 锁  

锁的概念。   
* 可重入锁 不会自己对自己加锁。  
* 读写锁 读是共享锁, 适用于读多写少的场景。  
* 数据库 间隙锁-对不存在的记录进行了加锁。  
* 互斥锁 - 获取不到锁就阻塞等待。  
* 自旋锁 - tryLock没有获取到锁就一直循环等待判断（节约线程开销但是占CPU做无用功, 有点浪费CPU）。 自选锁需要设置超时时间避免一直等待。  

ReentrantLock 可重入锁如何实现可重入性的。 它维护的双向链表线程id和当前请求的线程id相同就可重入。  
 内部通过AQS 维护了线程id来实现可重入。  

乐观锁缺点：无法解决多共享变量的问题。 长时间自旋占用cpu。需要用版本号解决ABA问题  


1. 能结合ConcurrentHashMap的源代码，说出 (编码规范static在final前面) final,volatile(同步到主内存，多线程共享内存),transient（不需要序列号）的用法，以及在其中如何用Lock对象防止写并发。  
```java
   private static final long serialVersionUID = 7249069246763182397L;  
   transient volatile Node<K,V>[] table;  
   
   class Segment extends ReentrantLock  
```
## synchronized 和 ReentrantLock  

AQS（抽象队列同步器） ReentrantLock （可重入锁） 里面的lock就是基于 AbstractQueuedSynchronizer 实现的。是并发控制核心组件。  
AQS 内部有一个private volatile int state 变量表示锁状态以及Node排队队列(FIFO双向链表)  

synchronized 代码块  monitor 锁 enter monitorexit 指令 jvm +1 -1. 方法上加的是ACC_SYNCHRONIZED  标志。（都是monitor 锁、监视器模式, 排队的线程进入entrylist 持有锁的线程有 own标志）  

发生异常处理方式。  lock接口 finally手动释放lock.unlock(). synchronized jvm会自动释放锁。  

ReentrantLock 内部有两个内部类，分别是 FairSync :Sync:AQS （公平锁） 和 NoFairSync :Sync:AQS （非公平锁）   
ReentrantLock() // 默认构造函数参数fair=false  
ReentrantLock(boolean fair) // sync = fair ? new FairSync() : new NonfairSync();  

公平锁只多了一个 tryAcquire..hasQueuedPredecessors() //在竞争锁之前判断一下等待队列中有没有线程在等待。等价于比自己排队时间更长。 （这个和Thread.sleep(0)没有关系呀）  
tryAcquire 失败了则加入等待队列。  


Q: synchronized 和ReentrantLock的区别  
一个依赖JVM监视器一个依赖AQS，lock更灵活可以超时，尝试。 有公平锁选项。 可以关联多个条件。  

Q: 当一个线程进入一个对象的synchronized方法A之后，其它线程是否可进入此对象的synchronized方法B？  
不能。其它线程只能访问该对象的非同步方法， 因为锁定的是同一个对象。  
synchronized锁住的是括号里的对象，而不是代码。  没有锁代码块，方法块的概念。

## 如何避免死锁dead lock  
- 分析死锁的原因, 按照一定的顺序来申请和释放资源, 避免出现循环依赖的情况。
- 使用超时机制。
- 资源预分配, 这样可以避免资源竞争。
- 使用同步机制如信号量或者互斥锁（获取不到锁就阻塞等待）。 协调多线程对共同资源的访问，避免冲突，减少概率。
- 使用重入锁来代替嵌套锁
- 使用死锁检测和恢复机制。（这个应该是发生后的补偿措施而不是事先避免）  
  检测到死锁就杀线程?  

# 设计模式 Design Patterns  

## DesignPatternsBook
Gang of Four / GOF - DesignPatternsBook 4个作者写的, 代称, 

GoF Design Patterns 分为3大类, 共23种。

### 1. 创建型 Creational（5种）
这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象。这使得程序在判断针对某个给定实例需要创建哪些对象时更加灵活。

* 单例模式（Singleton Pattern）  
  The singleton pattern restricts the initialization of a class to ensure that only one instance of the class can be created.  
  确保只能创建一个实例。

* 工厂模式（Factory Pattern）  
  The factory pattern takes out the responsibility of instantiating a object from the class to a Factory class.  
  通过工厂来生产创建实例。  

  其中又细分:    
  1. 简单工厂 Simple Factory 
  只有一个factory, 根据参数不同返回不同的实例。  
  e.g. `public final static DateFormat getDateInstance(int style);`  

  2. 工厂方法 Factory Method  
  e.g. `Executors.newSingleThreadExecutor(); Executors.newCachedThreadPool()`  

* 抽象工厂模式（Abstract Factory Pattern）  
  Allows us to create a Factory for factory classes.
  工厂也是抽象的。  
  普通的工厂模式是根据输入的参数来创建对象, 里面需要写if else; 但是抽象工厂是传入具体的工厂实现类。
  ```java
  public class XXXFactory {
      public static XXX getXXX(XXXAbstractFactory factory){
        return factory.createXXX();
      }
  }
  ```
  e.g. BeanFactory 的子类  ClassPathXmlApplicationContext、XmlWebApplicationContext  

* 建造者模式（Builder Pattern）  
  Creating an object step by step and a method to finally get the object instance.  
  e.g. `HttpRequest.newBuilder().url("xxx").build()`, lombok @Builder  
* 原型模式（Prototype Pattern）  
  Creating a new object instance from another similar instance and then modify according to our requirements.  
  用一个现成的对象创建方法, 或者从一个现成的对象clone()复制, 然后再进行修改满足我们的需求。这样减少对象的创建。   

### 2. 结构型 Structural（7种）  
这些设计模式关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式。

* 适配器模式（Adapter Pattern）
  Provides an interface between two unrelated entities so that they can work together.  
  适配不同版本的接口。统一为一个新的接口。

* 组合模式（Composite Pattern）
  Used when we have to implement a part-whole hierarchy. For example, a diagram made of other pieces such as circle, square, triangle, etc.  
  多个对象组合为一个对象, 多个方法组合为一个方法。

* 代理模式（Proxy Pattern）
  Provide a surrogate or placeholder for another object to control access to it.  
  访问一个代理类, 实际上功能是由另外一个实现类控制。

* 享元模式（Flyweight Pattern）
  Caching and reusing object instances, used with immutable objects. For example, string pool.  
  重用公共对象减少内存。 e.g. `StringUtils.EMPTY`, `Color.RED`
* 外观模式（Facade Pattern） 
  Creating a wrapper interfaces on top of existing interfaces to help client applications.   
  为了方便客户端统一接口调用, 采用在最外层包装接口方法的方案。 把根据不同的场景隐藏到包装类里面。
  ```java
    HelperFacade.generateReport(HelperFacade.DBTypes.MYSQL, HelperFacade.ReportTypes.HTML, tableName);
    HelperFacade.generateReport(HelperFacade.DBTypes.ORACLE, HelperFacade.ReportTypes.PDF, tableName);
  ```
* 桥接模式（Bridge Pattern）
  The bridge design pattern is used to decouple the interfaces from implementation and hiding the implementation details from the client program.  
  常用于接口和实现的解耦，和隐藏实现的细节。  
  这个比较接近尽量依赖接口, 而不是抽象类?
  ```cs
  interface MessageSender
  class TextMessageSender : MessageSender {}
  class EmailMessageSender : MessageSender {}

  abstract class Message {
    Message(MessageSender sender){}  // bridge 桥接
  }
  class TextMessage : Message {}
  class EmailMessage : Message {}

  ```
* 装饰器模式（Decorator Pattern）  
  The decorator design pattern is used to modify the functionality of an object at runtime.  
  运行时的更改, 例如下面的例子有很多种汽车 10多种, 这个时候不可能定义(编译时)把所有的功能都组装一遍。我们可以创建Decorator类, 让所有子类都继承Decorator class
```cs
class CarDecorator : Car {
	protected Car car;	
	CarDecorator(Car c){
		this.car=c;
	}
}
class SportsCar : CarDecorator {}
class LuxuryCar : CarDecorator {}

Car sportsLuxuryCar = new SportsCar(new LuxuryCar(new BasicCar()));
```

常见的如 IO 相关  InputStream class 的构造函数。     

### 3. 行为型 Behavioral（11种）
这些设计模式特别关注对象之间的通信。

* 模版方法模式（Template Method Pattern）  
  used to create a template method stub and defer some of the steps of implementation to the subclasses. 

  模板方法定义了执行算法的步骤，它可以提供对所有或部分子类通用的默认实现。  
  例如 `RestTemplate.java` 不管底层是基于HttpClentClient还是okHttp. 但是参数header组装，httpstatus，response解析等都是通用的代码。这部分代码完全可以公用起来。 另外常见的还有JdbcTemplate

* 中介者模式（Mediator Pattern）  
  调停者模式  
  used to provide a centralized communication medium between different objects in a system.  
  例如 空中交通管制员是中介模式的一个很好的例子，机场控制室是不同航班之间通信的中介。

```java
    ChatMediator mediator = new ChatMediatorImpl();
    User user1 = new UserImpl(mediator, "Pankaj");
    User user2 = new UserImpl(mediator, "Lisa");
    User user3 = new UserImpl(mediator, "Saurabh");
    User user4 = new UserImpl(mediator, "David");
    mediator.addUser(user1);
    mediator.addUser(user2);
    mediator.addUser(user3);
    mediator.addUser(user4);

    user1.send("Hi All");

    //Pankaj: Sending Message=Hi All
    //Lisa: Received Message:Hi All
    //Saurabh: Received Message:Hi All
    //David: Received Message:Hi All
```

* 职责链模式（Chain of Responsibility Pattern）  
  used to achieve loose coupling in software design where a request from the client is passed to a chain of objects to process them.   
  在软件设计中，责任链模式用于实现松耦合，客户端的请求被传递到一个对象链来处理它们。然后链中的对象将自己决定谁将处理请求，以及是否需要将请求发送给链中的下一个对象。

  例如: 
  - try-catch多个catch代码块。  
  - javax.servlet.Filter#doFilter()  
  - 取钱的时候输入71元, 可以分为50/10/1元的流程取钱。  

* 观察者模式（Observer Pattern）  
  useful when you are interested in the state of an object and want to get notified whenever there is any change. 

  例如: Spring event 事件  

* 策略模式（Strategy Pattern）  
  Strategy pattern is used when we have multiple algorithm for a specific task and client decides the actual implementation to be used at runtime.

  e.g. Collections.sort() 会根据不同的Comparator parameter 参数执行对应的排序算法。

* 命令模式（Command Pattern）  
  Command Pattern is used to implement lose coupling in a request-response model.

  e.g. `java.lang.Runnable` interface, Action

* 状态模式（State Pattern）  
  State design pattern is used when an Object change it’s behavior based on it’s internal state.  
  可避免 if-else or switch-case
  State Pattern is very similar to Strategy Pattern   

* 访问者模式（Visitor Pattern）  
  Visitor pattern is used when we have to perform an operation on a group of similar kind of Objects.

  通过this参数，将当前对象传给其它类，将操作逻辑放到另外一个类。
  ps: 看了java例子感觉不是特别有用你。

* 解释器模式（Interpreter Pattern） 
  defines a grammatical representation for a language and provides an interpreter to deal with this grammar.

  e.g. 自定义Expression 解析?
* 迭代器模式（Iterator Pattern）  
  used to provide a standard way to traverse through a group of Objects.  
  e.g. List 
* 备忘录模式（Memento Pattern）  
  The memento design pattern is used when we want to save the state of an object so that we can restore later on.  
  例如定义一个临时变量保存之前的值, 将原对象修改后，最终又还原这个值。  

## Miscellaneous Design Patterns 其它设计模式
Miscellaneous = 杂项, 无法归类

设计模式不仅仅是23种, 23种仅是GOF书中提到的。
此外, 常见的还有:

* DAO - Data Access Object, 目的是将持久层独立。
* IOC
* DI
* MVC  
  - Model - XXXDTO.java  
  - View - html / json  
  - Controller -  XXXController.java  
* MVVM  
ViewModel - in WPF

## 什么是 反面(教程)模式（Anti-pattern）  
design pattern是设计模式，通常是前人在软件开发过程中积累出来的解决一些问题的现成套路，按它们来做可获益无穷。  
anti-pattern也是一些现成的套路，但它们是现成的错误套路，避免它们则亦可获益无穷。 可理解为“反面教材”模式。  

## IOC
IOC (Inversion of Control 控制反转)实现方式，例如spring bean 的获取   
DI 依赖注入（例如构造函数注入）. 例如spring service构造函数参数依赖    
依赖查找（container）    

advantages:  
- decoupling 
- 扩展性

example:
```java
MyService myService = applicationContext.getBean(MyService.class);
MyService myService = applicationContext.getBean("myService");
```

## AOP - Aspect Oriented Programming 面向切面的编程  

aop interface jdk, 其它 cglib  
Spring AOP 四个概念（除了切面）  
1.@Aspect 切面 （依赖 spring-boot-starter-aop/aspectjweaver.jar）  
2.@Pointcut 切点, 要拦截的法 。表达式匹配  
3.@Advice 通知, @Before 之前 @After之后 环绕 返回  @AfterThrowing异常  
4. JoinPoint  连接点， 2切好的方法执行点。  
AOP 采用了两种实现方式：cglib静态织入(AspectJ 实现)和动态代理(Spring AOP实现)  

当调用的类虽然实现了接口，但是方法却不是接口方法时，无法使用JDK动态代理，也只能走cglib. 因为JDK动态代理的原理是实现和目标对象同样的接口，因此只能调用接口的方法。  

基于CGlib 动态代理模式 基于继承被代理类生成代理子类，不用实现接口。只需要被代理类是非final 类即可。(cglib动态代理底层是借助asm字节码技术， 属于spring-core-5.3.9.jar下的包）  
JDK动态代理是Proxy.newProxyInstance(class, InvocationHandler)。  JDK需要Method反射调用，性能稍低。  

JDK 和CGLib 都会生成新类。  

```java  
 public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
        System.out.println("---------before-------");  
        Object invoke = method.invoke(vehical, args); // 这里是反射。。  
        System.out.println("---------after-------");  
   
        return invoke;  
    }  
```  

基于Aspectj 实现静态代理（修改目标类的字节，织入代理的字节，在程序编译的时候 插入动态代理的字节码，不会生成全新的Class ） AspectJ 适用于非public方法  
Aspectj  是静态代理, 编译的适合就改了代码。 是独立的，不属于Spring.  
Spring AOP虽然使用了Aspect的Annotation（@Aspect，@Pointcut，@Before等），但是并没有使用它的编译器和织入器。其实现原理是JDK动态代理和CGLIB，在运行时生成代理类。  (待研究 - 需要通过.aj文件来创建切面，并且需要使用ajc（Aspect编译器）来编译代码)  

```java  
@RestController  
@Aspect // 切面 = （spring的切面实现=通知+切点 组合到一起, 其它实现方法应该没有这些概念，不要去分别找 A、O、P的概念了。 AOP= Aspect Oriented Programming 和下面的无关）  
public class HelloController {  

    @RequestMapping("/hello")  
    public String index() {  
        return "Hello World";  
    }  

    // 以下是切面的实现  
    // 切点-何处  

    @Pointcut("execution(* com.neo.controller.HelloController.index())")  
    public void invokeAspectJTestInAspectJ2() {  

    }  

    // 通知-何时  
    @Before("invokeAspectJTestInAspectJ2()")  
    public void beforeInvokeaspectJTestInAspectJ2(  
	JoinPoint joinPoint // 能够插入切面的点. around的适合 需要调用procced()方法  
	  
	) throws Throwable {  

    }  
}  
```  

## Spring 用到哪些设计模式  
1. 工厂模式 Factory  - BeanFactory.getBean(x)  
2. 工厂方法  - Executors.newSingleThreadExecutor()  
3. 单例模式 (Singleton pattern) - Spring Bean 单例  
4. 适配器模式 Adapter patter - HandlerAdapter 
5. 装饰器模式 Decorator -  XXXInputStream
6. 代理模式 Proxy pattern -  AOP  
7. 观察者模式(Observer) - event 事件  
8. 策略模式(Strategy Pattern) - Collections.sort() 
9. 模版方法模式 Template Method	pattern - JdbcTemplate RestTemplate  
10. Dependency Injection or Inversion of Control - Bean init
11. Front Controller pattern  // used to centralize request handling in a single place.
12. DAO
13. Service Locator Pattern 
14. Intercepting Filter Pattern

Q: 用过哪些设计模式？  
参考Spring用到的...


# 面向对象设计模式五大原则 SOLID  
 - Single-responsibility principle: 单一功能 - 认为“对象应该仅具有一种单一功能”的概念。    
 - Open–closed principle: 开闭原则 - 认为“软件应该是对于扩展开放的，但是对于修改封闭的”的概念。  　 
 - Liskov substitution principle: 里氏替换 - 认为“程序中的对象应该是可以在不改变程序正确性的前提下被它的子类所替换的”的概念。  // 子类替换父类不会出现编译或逻辑错误, 如果出现说明代码设计问题。  
 - Interface segregation principle: 接口隔离 - 认为“多个特定客户端接口要好于一个宽泛用途的接口”[5] 的概念。 客户端不应该被强迫依赖不用的接口。  // 尽量细化接口，接口中的方法尽量少  
 - Dependency inversion principle:  依赖反转/依赖倒置 - 认为一个方法应该遵从“依赖于抽象而不是一个实例”[5] 的概念。  // 若依赖实现, 以后更换实现类，此类也需要同步更改, 这会违法开闭原则， // 通俗：类似IOC 

# JVM  
jvm面试题介绍, **建议点开看, 这里有配图说明** 
> https://github.com/Homiss/Java-interview-questions/blob/master/JVM/JVM%E9%9D%A2%E8%AF%95%E9%A2%98.md  

JVM 字节码。  

jmap dump   

## 组成
- 类加载器（Class Loader）
- 运行时数据区（Runtime Data Area）  
运行时数据区是JVM用来存储数据的区域
  - Heap 堆  
    存放Java对象的地方
    not thread safe  
    All instance fields, static fields, and array elements are stored in heap memory.    
  - Stack 栈 
    存放基本类型和对象引用的地方      
    thread safe   
  - Frames 帧      
    A new frame is created each time a method is invoked.   
    debug的时候可以看到 frame 栈帧 Method Stacks   
  - 方法区  
    存放类信息、常量池和静态变量的地方
  - 程序计数器 
    记录线程执行的位置的地方

- 执行引擎（Execution Engine）
- 本地接口（Native Interface）
- Java Native Interface（JNI）

PS: 官方文档
> https://docs.oracle.com/javase/specs/jvms/se17/html/jvms-2.html

## Java Memory Model (JMM) Java 内存模型

1. 主内存（Main Memory）
2. 本地内存/工作内存（Working Memory）
3. 内存屏障 
Java Memory Model通过内存屏障确保共享变量的可见性和有序性，保证多线程程序的正确性。它是一种同步机制。
 
<pre>
 Release Barrier，释放屏障  
 Store Barrier，存储屏障  
 Load Barrier，加载屏障  
 Acquire Barrier，获取屏障  
</pre>

CPU L1 L2 L3 缓存。
- L1 32KB - 64KB  速度最快。
- L2 256KB - 512KB
- L3 4MB - 16MB

CPU读取数据时，会首先在L1（最快的缓存）中查找，如果未命中则会在 L1 > L2 > L3 > 主内存 中读取数据。  
CPU写入数据时, 先写入最近的缓存，最后写入主内存 (待确定)  

In order to ensure thread safety, Java provides various synchronization mechanisms, such as locks 锁, synchronized blocks同步块, and volatile variables 变量 

其它
-  Atomicity: 原子性
  读写是原子操作的。如 `volatile`
- Visibility: 可见性
  其它线程能看到当前线程修改的结果。
- Happens-before relationship: 通过内存屏障 确保之前的所有操作在其它线程里面是可见的。


PS: 如有需要, 这部分看官方文档有详细、准确的介绍。(但是官方文档并没有这些术语)  
> https://docs.oracle.com/javase/specs/jls/se17/html/jls-17.html#jls-17.4


Q: 基本类型变量一定是保存在栈中吗? 不是，  
基本数据类型是放在栈中还是放在堆中，这取决于基本类型在何处声明。new出来的类对象是存放在堆中的，而对象包含的成员不管是对象还是基本数据（field）都一起放在堆中的。  

教程上说in Java, all local variable created in stack. 这里指的是method 里面的栈帧, 所以和上面不冲突。  


## GC (Garbage Collection)  垃圾回收  
Java 8 以后的版本, Java 11 是LTS 长周期版本  
jdk8 默认垃圾收集器ParallelGC, 新生代，老年代。 也支持G1（最好最新JDK 8的版本比较成熟）  
jdk9 默认垃圾收集器G1GC， JDK9 以后默认都是G1（Garbage First）, 最小化fullGC暂停的时间。  
jdk11 G1GC  
jdk17 G1GC, 经测试 jdk17 后可以较容易释放物理内容给host主机, 在jdk 11比较难, 因为分配后就一直占用。  

GC回收条件. 1.引用计数器为0, 无法解决循环引用。 2. 引用链 root 可达性分析（有没有和GC root相连）。 没有gc root相连标记一次, finalize调用后再二次标记才是等待回收。  

GC算法：引用计数法，标记清除，标记压缩，复制算法。  

### G1（Garbage First）   
目的是减少停顿时间而设计  

JDK 8 开启G1  
```sh
java -Xmx1g -Xms1g -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -jar demo.jar  
```

MaxGCPauseMillis最大GC停顿时间毫秒。  
以前的ParallelGC 是连续的。 G1 的堆是基于Region设计的。是可以不连续的。 YGC是多线程并行的  

.NET GC回收的内存会直接返还操作系统， 而JVM只有在特定的情况下才会（基本上不会, JDK 17开始会）。  

**2种GC模式**
- Young GC
- Mixed GC

G1 一般不会进行full gc， 老年的收集全靠 mixed GC完成。 实在搞不定了才会转到其它GC收集器进行full gc.  
G1模式下不要设置年轻代大小 -Xmn2g 以及 XX:NewRatio， 原因是它会动态调整?  

rocketmq 默认用的g1, 8G内存  在这个脚本中可以看到。 -XX:+UseG1GC https://github.com/apache/rocketmq/blob/master/distribution/bin/runbroker.sh  

建议堆大小设置为6G以上时使用  

【结论】 G1适合rocket mq这种停顿时间要求很短的场景。 ParallelGC合适更大的吞吐量要求。  


G1 Java Memory Model 内存模型
G1 按Region划分:  
<pre>
0. 未使用的空间
Young Region
  1. Eden regions 伊甸园 (red)  - 用来存放新创建的对象
  2. Survivor regions 存活区 (red with "S") , 经过至少一次minor GC 
Old Region
  3. Old Region 小对象 （age >= n, 大概是16）
  4. Humongous Regions   大对象(超过50%分区容量大小)，连续区域
Metaspace Region  
  这些数据通常存在于JVM的整个生命周期. e.g. Class metadata 
</pre>


回收过程 - 标记，混合回收...   

### Partial GC 
JVM 1.8 结构  
> https://www.cnblogs.com/yichunguo/p/12007038.html  

新生代 eden:s0(from survivor)0:s1(to survivor1)  = 8:1:1 (参数–XX:SurvivorRatio)  ； 新生代：老年代=1:2 (–XX:NewRatio=1:2)   
 Minor GC (Eden 区)， Full GC =Major GC。 可以通过配置让full gc前先执行一次minor gc  
设置两个Survivor区最大的好处就是解决了碎片化。  复制到s1然后清空eden+s0.  
GC 顺序 eden > S0 > S1 > Old > Permanent 。  edeb+s0=>s1 然后交换s0 s1  （总有一个survicor是空闲的）  
年龄达到15岁后被转义到老年代（可通过jvm参数修改）。 
GC是针对堆内存的  
 方法区（元空间） 类信息，类常量。  
 JVM-元空间， 可使用物理内存。   

大对象由于需要比较大的空间，会直接放入老年代。(G1也相同)  
元空间MaxMetaspaceSize 程序会运行时动态分配，但也需要合理的限制。     
程序计数器， 记录字节码代码执行号。 方便线程切换。  

### CMS GC  
  最新JDK已经被废除。  
### ZGC  
  一般来说，ZGC 是为堆较大的应用程序设计的，而 G1 是为堆大小适中且需要可预测暂停时间的应用程序设计的。  

## JVM参数 -Xmx 与物理内存  
JVM初始分配的内存由-Xms指定，默认是物理内存的1/64；JVM最大分配的内存由-Xmx指定，默认是物理内存的1/4。  
默认空余堆内存小于40%时，JVM就会增大堆直到-Xmx的最大限制；  
空余堆内存大于70%时，JVM会减少堆直到-Xms的最小限制。因此服务器一般设置-Xms、 -Xmx相等以避免在每次GC后调整堆的大小。  

**in container**  
Java10 之前的版本JVM没有容器识别功能, 不知道自己是否在容器里面运行, 内存默认最大值为物理机的1/4而不是container的1/4。  

-XX:MaxRAMPercentage=75.0 按host物理内存比例或者container内存比例。(Java 8+)    
Q: 为什么我在container中给JVM分配MaxRAMPercentage = 100, 百分之百也不会报错呢?    
  JVM会尝试使用操作系统剩余的所有可用内存.  所以不会100%都是此JVM占用。

maxMemory // 最大可向物理机申请的内存  
freeMemory // 空闲内存  
totalMemory // 最大内存.  total = free + used,  <= maxMemory,  

## OOM  
内存溢出（Out Of Memory---OOM)  

在JMM里面，除了程序计数器，所有地方都可能发生OOM。 

如果异步方法@Async 返回类型不是void, OOM  可能不会打印OOM heaps 信息。 这种情况调试起来会比较困难。  

## JVM逃逸分析（Escape Analysis）, 方便内存回收。  
```java  
public void fun(){  
    Person p = new Person();  
    System.out.println("person's age:"+p.age);  
}  
```  
经过优化之后会变成类似下方的代码：  
```java  
public void fun(){  
    int age=20;  
    System.out.println("person's age:"+age);  
}  
```  
前提条件是p没有被其它地方引用。  

## JVM Tuning 调优  

JVM 调优目的是阻止 STW(stop the world). minor gc / full GC都会导致STW gc的时候会挂起非GC线程，导致程序卡顿阻止影响用户的使用体验。那就是说调优的目的是减少 gc的次数？  
Full GC 一天超过一次肯定就不正常了。 1-2天一次比较合适？ 或者夜间固定重启程序强制清空内存(手机很卡运行不动了靠重启手机, 同样我们也可以靠定时重启程序。能避免fullGC也永远释放不了的对象--不过是下策)。  
新生代内存不能超过存活区1半?  
minor gc 很快的 0.05s处理完(几十毫秒级别)  
minor gc 不同的垃圾收集器可只回收一部分内存。  

选择合适的垃圾收集器：Partial GC, G1（不用满了才收集）, ZGC.   

调优工具.   
jdk只带的工具jvisualvm.exe(JDK 12后需要单独下载) 可以看本地或者远程。 可以看内存和线程。  

PS: .NET runtime 根本不需要像JVM一样进行调优。

### Partial GC 调优
JVM调优可以考虑, 减少老年代内存，放到新生代。 或者 8:1:1 改为5:1:1。 JVM调优原因很多，是非常复杂的。  

### G1调优  
 * -XX:MaxGCPauseMillis=200  
  如果MaxGCPauseMillis设置的过小，那么GC就会频繁，吞吐量就会下降。 需要根据情况调整为合适时间   
 * 避免使用 -Xmn 选项或 -XX:NewRatio 等其他相关选项显式设置年轻代大小。固定年轻代的大小会覆盖暂停时间目标。    

## 其它
JVM 安全性, 按需加载class， 例如第三方包定义Integer, 然后再里面做一些破坏系统的操作。 因为jdk类加载使用双亲委派机制（实际上应该叫做【父委派机制】）, 会让上级类加载先执行，没有加载自己才会参试加载，所以永远会优先加载jdk类。  
避免重复加载, 保护程序安全，防止核心api被随意篡改。  

什么时候用finalize()方法, 析构函数一般不需要自己写， 只有调用JNI（Java natice interface） 的时候才需要指定释放这部分非Java内存。  
如果内存充足, 永远也不会回收这部分内存。  

# Springboot  
- Spring Boot 1.1（2014年6月）  
- Spring Boot 2.0.0，于2018年3月1日  
- Spring Boot 3.0，2022-11-28 Java 17  

Springboot 执行流程  
spring cloud实现原理（了解即可 ， 这里的cloud不是云计算,只是分布式管理）  
spring cloud demo 搭建。 done  注册中心, 配置中心，客户端负载均衡  


# Spring  
## Spring 事务隔离级别  
共5个属性, 实际上只有4个:   
- DEFAULT：数据库默认 (常用)；  
  MySQL - RR  
  Oracle和MSSQL - RC  
- 1. read uncommited:脏读； 
- 2. read commited 提交读取，避免脏读; 
- 3. reapeatable read (RR 可重复读, 常用)； 事务开启后读取结果就不受其它事务影响了, 但是当前事务中的修改可以查询到。直到commit.  
- 4. serializable 事务串行执行 -避免以上所有问题; ）  

## Spring 事务传播性. 
7种, 我们常用的只有2种 REQUIRED，REQUIRES_NEW  

## Spring Q&A  
Q: Spring MVC工作原理。  
首先tomcat 监听端口8080。  
客户端发送的请求到 DispatcherServlet.Java（处理所有的Http请求和响应）， doDispatch() 方法可以拿到 HttpServletRequest request, HttpServletResponse response 对象。  
再查询 HandlerExecutionChain /handlerMapping  处理器映射器 -》HandlerAdapter(通过 url method找到Controller) -> Controller 返回ModelAndView 最终返回客户端。 视图解析器 ViewReslover.render(xxx)  


Q： Spring MVC 开启gzip 减少客户端 net IO  
`server.compression.enabled=true`  

Q: ApplicationContext beanfactory 区别  
beanFactory 定义了如何初始化和销毁一个bean.   
ApplicationContext 继承 beanFactory(都是接口), 在Spring启动的适合会初始化所有的bean. 另外还支持国际化。  
```
public interface ApplicationContext extends XXXBeanFactory
```

Sping 配置方式, 基于 xml /注解+Java  

Q： ApplicationContext通常的实现是什么？  

FileSystemXmlApplicationContext   
ClassPathXmlApplicationContext  
WebXmlApplicationContext  

Q：什么是BeanDefinition  
Spring 是在BeanFactory中根据BeanDefinition(bean的定义：单例、原型/多例，@Lazy是否延迟加载, beanName，scope（作用域、单例、多例、requesst、session）等)的定义去初始化一个bean  

Q: Spring 生命周期  
1.实例化  
2.属性注入   构造函数注入的方式=1+2同时进行  
3.初始化  
<pre>  
Aware： 
	BeanNameAware：注入当前 bean 对应 beanName；  
	BeanClassLoaderAware：注入加载当前 bean 的 ClassLoader；  
	BeanFactoryAware：  


BeanPostProcessor.before()/BeanPostProcessor.after()  

init-method/  
InitializingBean.afterPropertiesSet(), 或者  @PostConstruct  注解（这个是Javax EE 注解，不是Spring注解）。  
</pre>

4.销毁  
https://blog.csdn.net/zl1zl2zl3/article/details/105044954  

DisposableBean.destroy() / @PreDestroy  

Q: 什么时候调用DiposibleBean.destory()  
 beanFactory.getBean();  

 bean的生命周期由Spring 容器管理。  
 容器关闭时会调用bean的销毁方法。  
 从容器中移除bean时会调用destroy()  
   
 单例模式是在Spring启动的时候创建的，关闭的时候销毁。注意，原型模式下 Spring不能管理destroy() Spring bean的（不能调用@PreDestroy 方法）。 如果要销毁东西只能写在~finilay析构函数里面?  
   

Q: Spring 如何解决循环依赖。  
Spring 创建bean流程是  实例化，填充属性，初始化。 依赖问题发生在实例化和填充属性上。 通过递归调用赋值。  

多例和构造函数不支持循环依赖, 当不支持时会报Exception错。  
多例应该没有缓存，所以无法完成循环依赖赋值。 多例循环依赖的时候不知道具体依赖谁。 如果都是多例A-B-A-B-A-B... 不就无限循环了吗.  
关于这个问题https://developer.51cto.com/art/202005/615924.htm 这篇文章写得好。  

Spring为了解决循环依赖，内部维护了3个map, （三级缓存的叫法官方貌似没有, 注解里面倒是叫cache, 叫三级缓存也形象。 因为查询顺序是1,2,3）。 见 `DefaultSingletonBeanRegistry.Java`  
```java
/** 第一级缓存：单例bean的缓存 */  
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256); // 成品  
/** 第二级缓存：早期暴露的bean的缓存 */  
private final Map<String, Object> earlySingletonObjects = new HashMap<>(16); // 半成品， 从3级缓存get一次后放入2级缓存。  
/** 第三级缓存：单例bean工厂的缓存 */  
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<>(16); //   前面的2个缓存是bean原始引用，但很多类都是动态代理(有注解@ AOP)。  
```

第三级别存的实际上是一个匿名ObjectFactory类  
```java
this.addSingletonFactory(beanName, () -> {  
		return this.getEarlyBeanReference(beanName, mbd, bean);  
    });  
```
   
优先从第1级里取，取不到再从第2级里取, 最后从第三级里去。  同一个bean只会存在与其中一个缓存，是互斥的。  


Q: Spring Boot 热部署方案  
1. 引入devtools jar包， 但这种方案会重启，重启时间太长比较麻烦。我一般用第2种方案  
2. idea手动编译自己改动的当前类.  


Q: OkHttp 是什么。 Android下首选的OkHttp网络请求。 来自squareup公司, 因为google sdk里面没有httpClient库。  
特点：  
<pre>
http/2 同一主机共享Socket  
gzip  
响应缓存避免重复请求  
</pre>

OkHttp的拦截器 有哪些  

RestTemplate 底层还是基于OkHttp, HttpClient实现  

Q: Spring 如何做权限控制。  
Spring Security  
基于用户和角色。  


Q: caffeine 如何实现缓存功能的。 高性能的 Java 缓存库。  
缓存和 Map 之间的一个根本区别在于缓存可以回收存储的 item。  
```java
Cache<String, Object> cache = Caffeine.newBuilder()
```
```xml  
<dependency>  
    <groupId>com.github.ben-manes.caffeine</groupId>  
    <artifactId>caffeine</artifactId>  
    <version>2.6.2</version>  
</dependency>  
```  
多个缓存要指定 cacheManager `@Cacheable`注解中没指定 cacheManager 则为`@primary`  


# 缓存 Cache  

- 缓存穿透 Cache Penetration：  
缓存层始终查询不到，总是到DB里查询。 失去缓存意义。  
避免方法:
  * 如果查询的数据为空, 设置一个默认值到缓存。// 这个方法最简单暴力。  
  * Bloomfilter 维护一个 hashset (一个很大的01位图,因为都是01所以不占空间，通过多个hash函数计算槽位, 需要所有槽位都命中)根据槽位确定里面是否有这个数据。 // 存在的一定会match, 不存在 3%容错(可配置，函数越多，容错率越低)  
    `Bloomfilter` 有现成第三方的实现,例如google.  

- 缓存击穿 Cache Breakdown:   
某一个key是热点数据，过期点刚好高并发。走DB导致崩溃。 
避免方法：  在查询数据库的时候加锁。 例如 redis.setnx也可以。

- 缓存雪崩 Cache Avalanche：  
多个key缓存同时过期集体失效，导致缓存在同一时刻同时请求DB。DB瞬间压力过大雪崩。  
避免方法: 设置不同的TTL。

- 削峰：利用MQ 队列。  

## Q&A
网络占满： 解决方案-本地缓存。  

Q:如何保证缓存一致性。  
推荐做法 【延时双删】   
更新缓存时采用 延时双删。 先删除一次，然后更新数据库， 再延迟1s（避免被其它线程缓存到旧数据）第2次删除缓存。  
如果对一致性有硬性要求, 只能串行化加多写锁？。  
设置缓存过期时间。  
MQ队列最终一致性。  
用删除而不是更新目的是更简单。  

Q：读写分离的数据库， 缓存不一致解决方案。  
从库发生变化后，再更新一次缓存。  

## 算法
FIFO, LRU 等，可参考
> [Redis内存淘汰策略](#redis内存淘汰策略)

# MQ  
用途 - 解耦、异步、削峰。  
缺点 - 系统复杂性提高。  

Q:1.如何保证消息不丢失。  
ACK机制- 生产阶段（错误重试），存储阶段（2个以上节点确认成功），消费阶段（处理完业务再ack）。  

Q:消息堆积如何处理  
先解决消费者问题，再考虑扩容。  

Q:消息过期   
重发mq, 批量重新导入mq  

Q:2、如何保证RabbitMQ消息的顺序性？ 消息如何分发？  
顺序性： 将需要顺序消费的消息 拆分到同一个topic里面的同一个队列/partition， 并且只能一个consumer 同步 消费 + 单线程.  
 方案2：在发送数据时给消息打上顺序标记，消费端数据库落库校验。  
路由：交换器 topic 模式  
分发: 循环的方式发送给消费者。  

Q:3.如何保证消息不被重复消费. （MQ保证至少一次消息发送）  
MQ服务端无法保证消息的重复消费, 只能由客户端实现幂等。 采用redis 或者DB 唯一索引, CAS之类。  

Q :如何保证高可用  
建高可用集群模式  

 - kafka 通过zk选举。数据有多个副本。异步刷盘。  （broker通过选举成为 master/slave）  
 - rocektmq 主从同步, 采用2个多个Master(broker-a, broker-b)/2 slave. （每个实例要么是master,要么是slave不能变，更不能选举）  
   rocketmq master down了就down了，没有选举的功能。 可以继续从slave读取未消费的消息。如果没有slave broker宕机后未消费的消息会丢失。  

## RockeMQ - alibaba apache  
RocketMQ借鉴了Kafka的设计  
服务端有3个进程。  
1. nameserver 保存topicName,队列等元信息 (kafka直接使用ZooKeeper)  
2. broker 存储消息体。 用netty与nameserver之间保持心跳信息同步。 broker分担topic分组。 (多broker这里是负载均衡轮询发送?)  
3. console 后台管理  

客户端  
4. Producer rocketmqTemplate. 同步、异步发送。  
5. Consumer pull 方式。 
push 模式实际上也是pull的实现, 只是服务没有数据的时候需要hold住，待有数据了再相应。客户端收到消息后需要发起下一轮pull. 实时性更高。  
可以从master(brokerid=0)也可以从slave（only brokerid=1）拉取消息。  

消费者组（Consumer Group）   
 - Clustering 集群消费模式下,相同Consumer Group的每个Consumer实例平均分摊消息。   
 - Broadcasting 广播消费模式下，相同Consumer Group的每个Consumer实例都接收全量的消息。  

Order 消息顺序。  
 - 分区顺序消息/普通顺序消息（Normal Ordered Message） 同一个消息队列（ Topic 分区，称作 Message Queue） 收到的消息是有顺序的  
 - 全局顺序消息/严格顺序消息（Strictly Ordered Message） 消费者收到的所有消息均是有顺序的。 企业版才支持。  

Tag 消息过滤  
 RocketMQ的消费者可以根据Tag进行消息过滤，由broker实现。  

topic 主题 Message(Key Body Tag)  
Queue 队列，对应kafka中的partition. 默认为轮询发送。 可以在rocket-console指定队列数量或者发送的时候指定producer.setDefaultTopicQueueNums(8);  
&emsp;&emsp;队列的目的是用来做负载均衡的。 一个队列只能分配给一个consumer, 和kafka一样。consumer数量不能大于队列数量。  
offset 消息数组的下标， 用于记录消费位置。  
group 一个分组只会被一个消费者消费。  

延迟消息. rocketmq broker内部有一个独立的中转主题中。 通过delay service检查到期后才发送到目标topic  
 - 定时消息topic为 SCHEDULE_TOPIC_XXXX的topic  

回溯消费， 消费了的消息暂时不会删除，可以通过topic/key查询重新发起。（kafka不支持消息查询）  

消息重试 -放入 %RETRY%+consumerGroup 队列  

分布式事务消息, 实现的是最终一致性。如果没有收到事务消息, 会回查commit情况, 如本地事务没有commit/roback则会再执行一次。  - 开源版本暂时不支持。  

## kafka - Linkedin apache  
1. zookeeper  
2. broker 服务器节点  

进程如下：  
```shell  
$ wget https://www.apache.org/dyn/closer.cgi?path=/kafka/2.8.0/kafka_2.13-2.8.0.tgz  
$ tar -xzf kafka_2.13-2.8.0.tgz  
$ cd kafka_2.13-2.8.0  
$ bin/zookeeper-server-start.sh config/zookeeper.properties  
$ bin/kafka-server-start.sh config/server.properties  
```  

推送次数:  
<pre>
  最多一次  
  最少一次  （rocketmq仅支持最少一次）  
  精确一次  
</pre>
poll vs push. 生产消息是push, 消费是pull.  有参数可以阻塞避免循环。 这里的push 不是指广播.  
kafka 线程数量 和 partition关系。  

kafka每次消费数据都会记录物理偏移量offset的位置进行增量消费。  
通过key 计算出集群的partition.  

吞吐量 -kafka producer合并多个小消息批量发送到broker, 吞吐量更高。  

topic / partition - 一个partition对应一个物理文件，一一对应。 创建topic的时候可以指定partition数量。  

partition 在集群中存储 min(partion num, broker num)个。 brocker存储一个或多个Partion.  

顺序消费 - kafka 不支持全局顺序消息。单个 partition 是顺序的。  

Leader 仅有一个partition副本作为leader负责读写。  
Follower 跟随leader.  


# Redis  
redis和 集群模式，哨兵模式原理。 重点！！！  
redis部署分为  
 * 单点部署  
 * 集群模式，Master挂了slave选举， 投票一半赞成。  


Q: MySQL里有2000w数据，Redis中只存20w的数据，如何保证Redis中的数据都是热点数据?  
热数据， 设置TTL 过期，留下来的大部分就是热数据。  
设置redis过期策略为 allkeys-lru(Least Recently Used)  

Q: **事务**  
redis 命令合并一条可以实现事务性,例如10s过期  
```
set key1 val1 ex 10 nx  
```

Q: redis如何保证线程安全  
redis工作线程是单线程的,封闭的。事件驱动。  
6.0以上支持多线程（仅IO） 默认是关闭的，需要配置文件打开。。 例如网络IO， 同时执行读写。  

Q: Redis 主从配置  
 1主2从3哨兵（高可用的）  
   
哨兵模式, 由哨兵来管理集群。  
  集群监控，消息通知，故障转义，配置中心（通知client客户端用新的Master地址）。  
  哨兵会给集群打分，优先级 、 offset id同步进度来选举Master. 哨兵会通过pub sub 通知client 新的Master/slave地址  
  哨兵自己也要选举leader. 按投票先后顺序选举leader.  
 3哨兵挂掉1个后可以获得2票完成选举。 客户端配置哨兵实际上连接的是redis集群（待确认！）  

Q Redis 如何实现主从复制。  
主节点将自己的内存数据写入快照， 发给从节点。 从节点恢复到内存。 类似于mysql binlog.  

Q Redis 持久化。  
https://www.cnblogs.com/traditional/p/13296648.html  
RDB （Redis Database） 全量备份 10分钟？备份一次，耗时文件小，恢复快。  
AOF (Append Only File) 增量备份 默认1s级别备份一次，最终文件大。 最多丢失1s的数据。  
建议采用混合模式(5.0 版本 默认开启 aof-use-rdb-preamble=yes，最新的docker:latest=v=6.2.5)。 redis重启时优先使用AOF 恢复，没有再使用RDB恢复.  
混合模式下 AOF和RDB文件都会保存（有歧义，1个文件还是2个文件）。 文件前面的为RDB格式, 后面为AOF格式。  

主动持久化  
save /bgsave(建议使用background save)  
redis.conf 配置文件  

confg get dic / save 可以查看rdb持久化策略  
时间 修改次数触发save  
<pre>
3600 1   
300 100   
60 10000  
</pre>

## Redis 延时队列。  
平时我们简单的做法是把消息放在task表里面, 设置NextRunTime 定时扫描表然后丢到队列里面。 同样的，redis实现就是把时间作为score, id作为值放在有序集合sorted set里面, 通过now参数定时扫描时间range然后丢到list集合里面取按顺序取。  

## rocketmq 延迟队列
内部专门建立了一个延时队列topic。通过delay service检查 到达时间后再重新丢到正式队列。  

## Redis内存淘汰策略   
redis受系统内存和配置内存影响有最大值。当内存用完时：  
 - 不淘汰-noeviction(默认策略)：对于写请求不再提供服务，直接返回错误（DEL请求和部分特殊请求除外）  
 - 随机淘汰，   
 - 最早ttl过期淘汰  
 - 最近最少使用 LRU(Least Recently Used)  

配置  
```sh  
 > config get maxmemory-policy  
 > config set maxmemory-policy allkeys-lru  
```  

### LRU 算法实现
 可以利用linkedList链表的快速删除和hashMap的快速查找能力相结合。  
 即get时移除（如过去存在）key, 再放入最后。 如果队列已满则remove first. （LRU可以保留首部也可以保留尾部，插入数据的时候也可以选择中间任意例如50%的位置）   

 LinkedHashMap 已实现LRU， 但需要重写removeEldestEntry。  
   

## Redisson  
Q: Redisson 可重入锁的实现原理？  
hash 数据结构，字段为进程号，值为Number数字记录重入次数。 释放的时候-1. 当=0时publish消息资源可用。  

RedissonLock 利用了redis的发布订阅pub/sub功能, 但还是需要自旋抢锁(分订阅消息抢锁和过期抢锁?). 但<mark>这个自旋并不频繁</mark>。
见`RedissonLock.lock()` 里面有while循环. // 这部分只是看了代码, 没有去调试验证。

也支持公平锁见 `RedissonFairLock.java`

Q: Redisson 读写锁的实现原理  
Redisson 源码 见 RedissonLock.forceUnlockAsync() , 就是一批lua脚本redis命令操作 hash的key:  

Redis 真实hash 值  
1. read锁
<pre>
mode	read  
634f9999-b774-4067-93a6-68aae50b5797:89	1  
</pre>  
2. write锁，和read的key相同,只是name不同。  
<pre>
mode	write  
634f9999-b774-4067-93a6-68aae50b5797:85:write	1  
634f9999-b774-4067-93a6-68aae50b5797:85	1  
</pre>  

Q: Watchdog后台线程  
锁占用默认30s过期, 由独立的线程定时每10s给未释放的锁续期  

# 分布式锁  
redis锁实际上就是利用setNx  
zk 就是创建一个有序的节点临时文件。 但节点号最小时获取到锁。能够等待，通过watch机制获得通知。  
zk性能差一些，开销大。 但是在客户端挂掉时能自动释放锁。  

# 分布式事务  
分布式锁解决的是分布式资源抢占的问题；分布式事务和本地事务是解决流程化提交问题。  
## XA协议/方案：  
XA接口是双向的系统接口。 txManager  JTA 是xa协议在Java的实现。  XA 协议的基础是 2PC 一致性算法  
  - 全局事务管理器与资源管理器的接口。单体应用跨多数据库。  Spring + JTA 可以实现例如spring-boot-starter-jta-atomikos  
  - 依赖协调者  
   2阶段2PC协议（询问，执行（全部yes则commit，否则rollback））。 缺点：1. 同步执行性能较慢。2.协调者单点故障。 3.commit可能不完整。  
   3阶段3PC提交 增加了Prepare步骤,超时自动提交的功能。  
   
   2PC 和 3PC  都无法解决分布式彻底的一致性。  

在微服务构架中，分布式事务不再是推荐做法。而应该采用saga模式， 用异步或者状态机的方式实现最终一致性。  
   
## TCC 方案：  
Try（冻结部分数据，尝试执行操作）、Confirm（执行）、Cancel（1阶段有try失败的节点则需要回滚。）。和2PC（Prepare,Commit）类似 跟钱打交道的场景用? 缺点，每个操作都要3个接口并且要实现幂等性。  

## 本地消息表
（以前公司就是这种模式，这种比较好理解很简单, 缺点是有延迟, 可能需要人工干预。） 利用数据库的事务特性， 操作数据库后写入一条消息到table里面，再由MQ异步同步到其它的数据库。  

## Rocket MQ  事务
实际上就是本地消息表的封装。只是把本地消息表放到了MQ里面 保证最终一致性。  

参考文章 https://juejin.cn/post/6844903647197806605  
【结论】 分布式事务过于复杂，不要轻易使用。 建议把需要事务的聚合为一个单机服务，使用本地数据库事务。  


# 微服务，分布式。  
# Spring Cloud
Spring Cloud 不是云计算, 是分布式微服务架构。   
- Eureka - Netflix  
Eureka的使用，作为服务注册中心，Eureka 分为 Server 端和 Client 端  
- Ribbon - Netflix  
Ribbon是客户端负载均衡器。 默认LoadBalance策略为轮询。  
- Hystrix 断路器、熔断。 可以在gateway里面配置或者feign里面设置callback。  
- Feign - Netflix (读fein)  面向接口编程，基于注解的动态代理实现。  
- nacos 配置中心 服务注册和发现.  // 单机部署-加standalone参数， 集群部署-自身VIP用nginx+至少3个节点即可。把各节点ip都放入cluster.conf启动即可。  启动leader follower是CP 一致性，多个几点同时服务属于高可用AP  
- gateway 网关 - netty, 比zuul多依赖了spring-webflux 支持异步, 是spring cloud的套件。实现了限流，负载均衡,扩展性更好，，  
- Zuul - netflix:　服务网关  
- Dubbo - Ali. 服务注册和服务发现，还有远程调用。  
- Prometheus 指标采集  
- filebeat 日志采集  
- apollo 携程 配置中心  
- Spring Cloud Config 配置中心， 没有原装的管理界面。  
- alibaba/Sentinel  分布式系统的流量防卫兵。前身 Hystrix  

## 服务治理 Service Governance

- **降级 Downgrade** - 关掉非核心功能。  
e.g.  
    1.仅保留主站可用，电商系统仅仅能下单,不能退换货。关闭文件系统，监控系统算不算?.   
    2.定时任务增加间隔时间，放入MQ延迟队列延迟执行。   
可以代码降级或者由运维手动降级。  

- **熔断 fuse / Circuit Break** - A-B 2个内部系统，B系统出现了问题，A不再访问B。  
当下游系统出现异常达到一定比例时，停止请求下游系统。  

客户端使用方法 
```
feign.hystrix.enabled=true 
@FeignClient(name= "spring-cloud-producer",fallback = HelloRemoteHystrix.class)  
```

  1.信号量  
- **Rate Limit 限流** - 限制访问。1秒只能接收N个请求，超过直接拒绝访问。  
   1. 计数器 / 窗口计数  
   2. 令牌  
   3. 漏桶  

## Resource governance
CPU, storage, RAM  
multicloud management  

## Cloud governance 
Resource governance  
Service Governance  
Security and Governance   
Compliance and Governance
Governance Tools  
Governance Deployment  

## 微服务缺点  
增加故障排除难度  
远程调用导致延迟增加。  

## Load Balance 负载均衡-算法  
https://www.jianshu.com/p/40e196414cfa  
- 轮询法（Round Robin） 有锁，性能低。平均分配， 服务器性能可能不一样。  
- 加权轮询法（Weight Robin） 给服务器加权重， 性能好的权重多设置一点， 增加命中率。  
- 随机法（Random） ...比轮询法稍微好点,量大接近平均分配，  
- 加权随机法（Weight Random） 和加权轮询一个道理。（权重和下限在spring - cloud下效果不同）  
- 最小连接法（Least Connections） ...  
- 源地址哈希法（Hash） 对ip/url取模。相同的参数/ip 计算目标server, 保证每次访问的服务器都是相同的。可以保持session会话。  
-	一致性hash算法  
Q：什么是一致性哈希算法?   
首先构建一个hash环，映射一个范围，节点分布在这个环上。  计算hash值后顺时针找最近的节点。 这样可以很方便的进行伸缩。 只会影响相邻的一个节点。  


# Zookeeper  
Zookeeper - hadoop子项目，第一个分布式协调服务（CAP 中满足CP一致性）。包括：  
* 命名服务，集群管理  
* 配置管理  
* 分布式锁  
* 队列管理 FIFO  

zookeeper 有自己的一致性文件系统。只适合存储少量的数据。 可基于znode 创建临时文件加锁。  

HBase zookeeper存储hbase schema.  
yarn 也是zookeeper 实现对resource manager的高可用。  

## CAP 定理  
* Consistency 一致性  
* Availability 可用性  
* Partition 分区容错性， 出现问题仍然能提供服务。  

zookeeper是CP  

# NIO  
Non-blocking IO. IO多路服用的基础。  
IO面向stream流,NIO面向buffer缓冲区。 采用selector 事件驱动模型。  
NIO 缺点是解析数据相对要更复杂一些。  
IO多路复用 - 一个线程管理多个IO流（网络或者文件）的操作。  

channel - 表示io源与目标打开的连接。 客户端只能与buffer交互。  
buffer - 与channel交互， 数据是从channel读入buffer, 从buffer写入channel  


NIO客户端请求会注册到多路复用器。  

## AIO  
Asynchronous IO、NIO2 是NIO的升级版本。  

Q：字节流和字符流的区别？  
字符流和字符集有关， 如果是处理纯文本应该优先使用。 其它类型用字节流。  

## NIO or AIO 选择  
大量的数据少量的链接，用IO  
少量的数据大量的链接，用NIO更合适。  

# Netty 
// 没什么使用经验, 就是没经验...
基于NIO  
支持多种协议如 ftp smtp http  
0 buffer拷贝。  
避免 空轮询导致cpu 100%  
比直接使用 Java 核心 API 有更高的吞吐量、更低的延迟、更低的资源消耗和更少的内存复制  

解决了TCP 粘包/拆包。  

应用：MQ, 实时通讯系统， 提供http服务器类似tomcat   

## 线程模型  
 单线程  
 多线程  

# Nginx  
master 进程：读取配置。  
work进程：处理请求  

# ES/Elasticsearch   
分布式搜索引擎  
分布式实时文件存储系统，每个字段都被索引并可搜索  
和solr一样是基于 Lucene 的搜索服务器。 ES第一受欢迎，solr第2.  

ELK Elasticsearch(搜索分析引擎)\LogStash(日志收集、过滤处理、格式转换的数据处理管道，转换数据发送到ES中存储)\ Kibana（日志展示，数据可视化）  

ES和MySQL 对应关系  
ES             | MySQL
---            |---
index         | database  
type          | table  
document | row  
field           | column  
mapping    | schema  
DSL           | sql  
get   http://  | select * from table  
put  http:// | update table set xxx   

es集群架构  
分片 - 索引被分割成多个节点上。 通过doc id 进行hash计算出shard 随机轮询负载均衡。  
  　shard = hash(document_id) % (num_of_primary_shards)  
master的选举 - 必须master:true 配置（具有master选举资格）。 通过id小的来选举。 投票占一半以上就可以完成全局。 n/2+1  

分配数量创建索引后通过rest api指定，指定后不能修改。  


ES term 也是有B-tree 索引的. (MySQL 只有字典)  
* segment 元信息文件  
* doc 一条记录  
* doc/field 字段  
* doc/field/term 最小单位。分词后的多个内容，不分词就是field 内容。  
* docvalues 列存文件  

## 容错性  
通过索引副本 replica shard 实现。  
可以通过rest api修改副本 数量。  

## ES 写数据  
客户端向任意一个node发送请求。 写入primary shard, 再同步给replica shard  

es 更新和删除数据都是对旧记录进行标记删除， 更新实际上是新增版本号。 ES支持部分字段更新。   

## ES读数据  
通过doc id  hash 查询到shard, 可从primary replica shard 查询。  

## 调优  
 - 设计调优。mapping 是否需要检索, 是否需要存储。  
 - 写入调优, bulk批量写入  
 - 查询调优。   

Q：什么是脑裂。   
避免产生2个Master。 过半机制可以避免脑裂。  
旧master即使上线后已只能作为slave.  

Q: 倒排索引  
分词器 然后对应docid。 由协调节点进行合并结果集返回。  

Q: 正排索引  
原始数据doc list 的查询。  

## UI工具   
貌似Dejavu 应该是比Kibana好用, 没有用过Dejavu, 待证实。  

# oauth2  
oauth2不兼容1.0, 废弃了1.0  

>clientId  
clientSecret  
token  
responseType code 授权码(常用), token 令牌  
grantType authrizationCode（授权码模式 常用） password密码模式 client_credentials客户端模式 refresh_token  
redirect_uri 重定向url  

令牌的使用，在header Authorization 中放入token。   

认证流程  
1. 获得授权码code  
2. 通过code获取access token  
3. 通过access token 获取openId  
4. 通过access token + openid 获取用户授权信息。  

### JWT - JSON Web Token  
 是一种内部系统登陆信息规范。（之前公司只用到了userName放入header, 但并没有用到JWT）  
JWT分3段
<pre>
	header  
	payload(可选择不加密)  
	signature(加密)  
</pre>

payload里面可以存储用户基本信息, 服务端只需要验证payload没有被更改, 可以减少查询次数。
  UID, 权限等其它信息可以考虑服务器端再次查询. （待确认最佳实践）

针对Spring  
1. 登录授权后返回 JWT json 给客户端
2. 客户端拿到后设置Authorization Header 请求后面的api  
3. 服务端接收到请求, OncePerRequestFilter.doFilterInternal() 里面, 解析Bearer Token 设置 `SecurityContextHolder.getContext().setAuthentication(authentication)` 完成授权认证  
4. JWT secret 可保存到环境变量设置或者统一配置中心。 Spring sessionCreationPolicy设置STATELESS, 不再返回session  

以上已有实现 `BearerTokenAuthenticationFilter`,`JwtAuthenticationProvider` , 见package org.springframework.security.oauth2.server.resource.web.authentication 我在项目`sample-in-spring`中已经作了demo.

# tomcat   
这个应该是运维的工作.  
```
/bin/启动关闭  
/conf/server.xml 这里可以配置虚拟目录  
/conf/Catalina/localhost/ 自定义web部署文件  
/webapps  
/work webapps 编译后的目录  
```

# Docker
from Docker Inc.   
常用命令
```
docker images  
docker ps  
docker start {contrainerid}  
docker stop {contrainerid}  
docker rmi {imageid}  
docker rm {contrainerid}  
```

# kubernetes - 领航员 - google 产品  
k8s 8代表中间有8个字母  
k8s是一个开源的容器集群管理系统，可以实现容器集群的自动化部署、自动扩缩容、维护等~  
kubernetes可以管理Docker集群。维护起来更方便。 小型应用没有k8s也是可以的。  
k8s，负责在大规模的服务器环境中，部署和管理容器组，用于解决掉容器的复制，扩展，健康，启动，负载均衡。  

## k8s VS Docker Swarm 
Q: 为什么有docker swarm了还需要k8s  
Docker Swarm 是 Docker 自家针对集群化部署管理的解决方案，优点很明显，可以更紧密集成到 Docker 生态系统中。  
虽说 Swarm 是 Docker 亲儿子，但依旧没有 k8s 流行，不流行很大程度是因为商业、生态的原因，不多解释。  
k8s 以后会变成默认的任务编排工具。 Docker Swarm将会淘汰  

## service mesh 服务网格  
k8s 能管理docker, 但是不能管理流量, 所以出现了service mesh 例如其中一个代表产品为 Istio  


# UML （70%）  
 draw.io(优先) / processOn（processOn是国产的） 画（UML 不仅仅包含类图, 平时工作中画的很多图都是）  
  - 泳道图也是UML， 是一种UML活动图。  
  - 时序图  
  - 类图 （重要）  


# 算法  

常见的时间复杂度量级有：  
- 常数阶O(1)  
- 对数阶O(logN)  
- 线性阶O(n)  
- 线性对数阶O(nlogN)  
- 平方阶O(n²)  
- 立方阶O(n³)  
- K次方阶O(n^k)  
- 指数阶(2^n)  

空间复杂度， 空间换时间。  
https://juejin.cn/post/6844904167824162823  


## 链表复制
先复制原始节点，再根据map映射复制random字段。  
```java  
public class RandomListNode {  
    int label;  
    RandomListNode next = null;  
    RandomListNode random = null;  

    RandomListNode(int label) {  
        this.label = label;  
    }  
}  
```  

## 包含最小值的栈
即维护一个小栈列表。 同原始栈数据量进出。  
```java  
public class Solution {  
    Stack<Integer> stackTotal = new Stack<Integer>();  
    Stack<Integer> stackLittle = new Stack<Integer>();  
```  
## 大文件找重复单词  
导入数据库?   
先排序再找重?  
按hashcode分片为小文件, 然后再小文件里面查找重复。（对数据分区,分而治之）。 但可能存在数据倾斜的问题。  


# scrum 敏捷开发  
	- Backlog 列表, 用户讲述自己的story。 Sprint计划会议  
	- 转移到Sprint 看板， 和上级进行工作需求量确认, 当不能按时完成的时候排除某些优先级较低的story确认。  
	- 每日站会 - 昨天做了什么， 有没有遇到问题。 今天做什么。  
	- story 拆分, 点数评估。 分配或者自领。  
	- 角色 1.Scrum Master，2.产品负责人, 3.开发团队  
	- 燃尽图  
	- Sprint评审回顾会议。1.交付验收 2.总结和完善后续Sprint.  


需求分析、需求评审、设计评审、回顾会议。  


# TDD 和BDD  
TDD： Test-driven development （测试驱动开发）  
先编写测试用例再编写真正实现功能的代码。  

BDD:Behavior-Driven Development (行为驱动开发)  
BDD是在应用程序存在之前，写出用例与期望，从而描述应用程序的行为，并且促使在项目中的人们彼此互相沟通。  
BDD关注的是业务领域，而不是技术。  

TDD、BDD不是一个层面的东西。TDD是开发人员和测试人员；BDD是客户和开发者。  

BDD 常见框架如 cucumber  

## 什么是测试 pyramid 金字塔结构
<pre>
   人工  
  IT IT  
UT UT UT  
</pre>
金字塔最上层小, 减少人工成本  

# 大数据（了解）  
flink 是德国一家初创公司Data Artisans 在2015年开发，然后捐给Apache的, 后来母公司被阿里收购所以在国内被强推。  


# Spring-AMQP  
AMQP – Advanced Message Queuing Protocol  
Spring-AMQP是Spring的一个子项目，是对amqp协议的抽象实现。  

与JMS的差异  

AMQP是平台独立二进制协议标准，库可以使用不同语言实现并运行与不同环境中。不受限与特定厂商，因此可以实现JMS代理之间迁移，主流的AMQP消息代理是 RabbitMQ, OpenAMQ, StormMQ。  

# Hateoas  

media type: application/hal+json  
```json  
{  
  "_links" : {  
    "self" : {  
      "href" : "https://myhost/people/42"  
    }  
  },  
  "firstname" : "Dave",  
  "lastname" : "Matthews"  
}  
```  

# 其它
### Liquibase 
Liquibase is a database schema change management solution that enables you to revise and release database changes faster and safer from development to production.  

DATABASE CHANGE LOG, 适用于团队大量开发人员使用local DB, 同步DB表结构和数据的常见。

### Tuning vs Optimization 调优和优化的区别

Tuning 是指调节参数和设置提高性能 improve performance  
Optimization 包含调节算法，更深度的修改试系统更高效。 more efficient

### Authorization 授权
授权几种方式：
- Form 表单认证   
   跳转到/login HTML页面上输入账号密码那种表单方式
- Basic 基本认证   
	浏览器弹出一个对话框可以输入账号密码的传统方式。(不需要HTML页面)  
	原理是采用Header `Authorization: Basic BASE64STRING1`  BASE64STRING1 实际上就是`username:pwd`的字符串base64编码。
- Bearer Token
	Header `Authorization: Bearer BASE64STRING1`  , BASE64STRING1  解码后可能是json, 也可以是其它自定义格式。

- OAuth 2.0  
	Access Token
- Digest Auth 摘要认证  
	类似 Basic 认证, 密码不再是明文传输。由于有HTTPS，又复杂而等，不常用。

在postman里面的Authorization 页面,  对不同的Type有输入支持， 可以更好的理解各种授权类型的差别。

不使用session则不支持Remember-me  


### package-info.java
package-info.java 文件估计大家见过但是自己却很少去创建和使用它、因为对于一般应用来说可能真的太少见了。
它的作用主要是三个  
• 描述包  
  是一个用来定义包级别注释的特殊 Java 文件。它可以帮助提供该包内所有类和接口的信息，并且这些信息可以在 JavaDoc 中使用。

• 使用注解修饰包、达到修饰该包下的类
  文件中的内容可以被看作是包级别的 Javadoc 注释，提供了关于该包的信息，包括该包中包含哪些类，接口或枚举，这些类、接口或枚举的作用是什么以及如何使用它们等等。  
  package-info.java 文件还可以被用来指定一些注解或者进行特殊处理。  

• 声明包中使用的类和常量(这个比较少用)   
  文件通常会包含一些常量或者是该包的配置信息，或者是提供该包的一些公共接口。  

# mybatis  
mybatis缓存。   
  一级缓存 默认开启  SqlSession 级别, executor里面有localcache  
  二级缓存 mapper级别。需要配置 `<setting name="cacheEnabled" value="true"/>`, spring boot里面是单例。  

## MyBatis vs JPA

- MyBatis - 所有SQL都需要全部手写。 所以算半成品, 但灵活度也很高。  
  lightweight and easier to configure and use  
  - MyBatis-Plus - 对MyBatis的二次封装, 不用手写SQL。中国人开发，在外企可能接受度低。 因为属于纯粹的扩展, 这个应该是比较灵活的。 看上去网上对这个库的评价也不是想象中那么好。   
  - tk MyBatis  通用 MyBatis, 另外一个第三方库。  
    tk mybatis 实体类使用的注解是jpa注解   
  - MyBatis Generator 自动生成所需要的类，文件，SQL,字段映射。  

- JPA (Jakarta Persistence API) 不用手写SQL. 是更高级别的抽象  
   JPA is a more full-featured and complex framework that is part of the Java EE (now Jakarta EE) standard.  

# java code sample
> https://github.com/dakin001/java_code_sample.git


