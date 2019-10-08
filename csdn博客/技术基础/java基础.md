java基础
======

1、Java里面有哪几种基础数据类型
------------------

  ![](https://images2017.cnblogs.com/blog/1313428/201801/1313428-20180120182441428-1016525855.png)
&emsp;&emsp;（数据类型--字节--默认值）(byte--1--0)、(short--2--0)、(int--4--0)、(long--8--0)、(float--4--0.0f)、(double--8--0.0d)、(char--2--'\u0000')、(boolean--4--false)

2、Char为何是两个字节
-------------

   在中文条件下：在uft8编码下占三个字节； 在GBK编码下占2个字节;   
   在英文条件下： 在uft8编码下占一个字节； 在GBK编码下还是占2个字节；  
   所以char类型的值不管是英文还是中文都是统一两个字节！ 

3、Object有哪些方法
-------------

   **(1).** registerNatives()   //私有方法   
   **(2).** getClass()//返回此 Object 的运行类。   
   **(3).** hashCode()//用于获取对象的哈希值。   
   **(4).** equals(Object obj) //用于确认两个对象是否“相同”。   
   **(5).** clone()//创建并返回此对象的一个副本。   
   **(6).** toString()   //返回该对象的字符串表示。   
   **(7).** notify()//唤醒在此对象监视器上等待的单个线程。   
   **(8).** notifyAll() //唤醒在此对象监视器上等待的所有线程。   
   **(9).** wait(long timeout)//在其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量前，导致当前线程等待。  
   **(10).** wait(long timeout, int nanos)//在其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量前，导致当前线程等待。   
   **(11).** wait()//用于让当前线程失去操作权限，当前线程进入等待序列   
   **(12).** finalize()//当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。   

**4、final修饰变量，函数，类的作用**
-------------

   **(1).** 不可被继承、不可被重写、不可被修改。   
   **(2).** 当方法被该关键字标记时会将该方法锁定，防止修改该方法的定义逻辑及实现，同时会将方法转入内嵌机制，提高效率；   
   **(3).** final修饰的变量有三种：静态变量、实例变量和局部变量，分别表示三种类型的常量；   
   **(4).** final修饰的变量可以先不给值，但是要在使用之前初始化；   

5、ArrayList的数据结构（参考：[https://www.cnblogs.com/lighten/p/7291339.html](https://www.cnblogs.com/lighten/p/7291339.html "https://www.cnblogs.com/lighten/p/7291339.html")）
-------------

   **(1).** 其数据结构非常简单，就是一个数组，再加上一个计算元素个数的size。默认初始化数组大小是10。扩容方式如下：默认扩容原数组长度的两倍，也就是新的数组大小是原来的三倍，但是指定扩容的值比这个值还大的情况，使用制定的值。   
   **(2).** size()和isEmpty方法都是依靠size这个计数字段完成。contains(Object)方法借助于indexOf(Object)方法，做法就是遍历了。   
   **(3).** ArrayList实现了Cloneable接口，其拷贝只是拷贝了这个List对象，而没有拷贝list中的元素，这意味着两个list持有的是同一组对象。   
   **(4).** 插入以及移除操作时都会移动数组元素，例如插入到指定位置，要讲指定位置后面的所有元素移动，移除也是同样。   
   **(5).** ArrayList在多线程的环境下是线程不安全的   

**6、HashMap的数据结构**
-------------

   **(1).** 它是基于哈希表的 Map 接口的实现，以key-value的形式存在;   
   **(2).** 在HashMap中，key-value总是会当做一个整体来处理，系统会根据hash算法来来计算key-value的存储位置，我们总是可以通过key快速地存、取value。   
   **(3).** 默认容量为16，默认加载因子为0.75；（容量表示哈希表中桶的数量；加载因子是哈希表在其容量自动增加之前可以达到多满的一种尺度，它衡量的是一个散列表的空间的使用程度，值越大，空间利用更充分，但是效率越低，值越小，空间越容易浪费）   
   **(4).** HashMap底层实现还是数组，只是数组的每一项都是一条链。其中参数initialCapacity就代表了该数组的长度。   

**7、ArrayList的父类有哪些**
-------------

   继承了AbstractList（-->AbstractCollection-->Collection-->）类，实现了List<E>, RandomAccess, Cloneable, java.io.Serializable接口   

**8、为什么覆盖equal的时候必须覆盖hashcode**
---------------------------

   **(1).** 如果没有遵守覆盖equals时同时覆盖hashCode，就会违反Object.hashCode的通用约定（具体的约定大家可以至Obejct类hashCode方法注释处查看），从而导致该类无法结合所有基于散列的集合一起正常工作，这样的集合包括HashMap，HashSet和HashTables   
   **(2).** 因为HashMap有一项机制：如果两个实例散列码不匹配， 直接放弃比较其等同性   

**9、反射应用场景，优缺点**
------------

   反射机制是一种程序自我分析的能力。用于获取一个类的类变量，构造函数，方法，修饰符。   
   **优点：** 运行期类型的判断，动态类加载，动态代理使用反射。   
   **缺点：** 性能是一个问题，反射相当于一系列解释操作，通知jvm要做的事情，性能比直接的java代码要慢很多。   

10、自定义线程池的参数以及意义(参考：[https://blog.csdn.net/zhangfg_666/article/details/90672721](https://blog.csdn.net/zhangfg_666/article/details/90672721 "https://blog.csdn.net/zhangfg_666/article/details/90672721"))
-------------

   线程池的ThreadPoolExecutor实现了Executor接口：   
   **tasks：** 每秒需要处理的最大任务数量，假设用100-1000   
   **tasktime：** 处理第个任务所需要的时间，假设为0.1s   
   **responsetime：** 系统允许任务最大的响应时间，比如每个任务的响应时间不得超过1秒。   
   **corePoolSize:** 核心线程池数，系统每秒有多少个线程在运行   
   **workQueue：** 任务队列长度   
   **maxPoolSize:** 最大增加线程的大小   
   **keepAliveTime：** 线程允许空闲的时间，达到空闲时间就退出（默认情况下线程池最少会保持corePoolSize个线程。如果将allowCoreThreadTimeout参数设置为true，则核心线程也可以退出）   

11、当线程池不断接受新任务，活跃线程数怎么变化
-------------

   **(1).** 新增新任务时如果队列没有满的话则不会增加新线程，如果队列满了则增加新线程。如果使用execute方法提交新任务的时候，即使队列有空闲也会开启新线程。   
   **(2).** 线程池内活跃的线程数大于corePoolSize时, 新来的任务会先进队列, 如果队列满时会继续新建线程直到池内线程数达到maximumPoolSize为止。这时如果再请求新的任务, 会执行设定的策略, 默认是直接拒绝创建。   

12、线程池的四种拒绝策略
-------------

   **(1).** ThreadPoolExecutor.AbortPolicy()抛出java.util.concurrent.RejectedExecutionException异常   
   **(2).** ThreadPoolExecutor.CallerRunsPolicy 用于被拒绝任务的处理程序，它直接在 execute 方法的调用线程中运行被拒绝的任务；如果执行程序已关闭，则会丢弃该任务   
   **(3).** RejectedExecutionHandler handler = new ThreadPoolExecutor.DiscardOldestPolicy();这样运行结果就不会有100个线程全部被执行   
   **(4).** ThreadPoolExecutor.DiscardPolicy 用于被拒绝任务的处理程序，默认情况下它将丢弃被拒绝的任务。运行结果也不会全部执行100个线程。   

13、同步与异步，阻塞与非阻塞(参考：[https://baijiahao.baidu.com/s?id=1612594086537323804&wfr=spider&for=pc](https://baijiahao.baidu.com/s?id=1612594086537323804&wfr=spider&for=pc "https://baijiahao.baidu.com/s?id=1612594086537323804&wfr=spider&for=pc"))
-------------

   同步就是发起一个请求，直到请求返回结果之后，才进行下一步操作   
   当一个异步操作发出后，调用者在没有得到结果之前，可以继续执行后续操作   
   二者的区别还是很明显的：请求发出后，是否需要等待请求结果，才能继续执行其他操作。   

   阻塞一般是指：在调用结果返回之前，当前线程会被挂起   
   非阻塞式的调用指：在结果没有返回之前，该调用不会阻塞住当前线程。   

   阻塞/非阻塞：进程/线程需要操作的数据如果尚未就绪，是否妨碍了当前进程/线程的后续操作。   
   同步/异步：数据如果尚未就绪，是否需要等待数据结果。   

14、BIO，NIO，AIO的区别(参考：[https://qindongliang.iteye.com/blog/2018539](https://qindongliang.iteye.com/blog/2018539 "https://qindongliang.iteye.com/blog/2018539"))
-------------

   **同步阻塞IO（JAVA BIO）：**  
   &emsp;&emsp;同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。  

   **同步非阻塞IO(Java NIO) ：**   
   &emsp;&emsp;同步非阻塞，服务器实现模式为一个请求一个线程，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。用户进程也需要时不时的询问IO操作是否就绪，这就要求用户进程不停的去询问。   

   **异步阻塞IO（Java NIO）：**   
   &emsp;&emsp;此种方式下是指应用发起一个IO操作以后，不等待内核IO操作的完成，等内核完成IO操作以后会通知应用程序，这其实就是同步和异步最关键的区别，同步必须等待或者主动的去询问IO是否完成，那么为什么说是阻塞的呢？因为此时是通过select系统调用来完成的，而select函数本身的实现方式是阻塞的，而采用select函数有个好处就是它可以同时监听多个文件句柄（如果从UNP的角度看，select属于同步操作。因为select之后，进程还需要读写数据），从而提高系统的并发性！   

   **（Java AIO(NIO.2)）异步非阻塞IO:**   
   &emsp;&emsp;在此种模式下，用户进程只需要发起一个IO操作然后立即返回，等IO操作真正的完成以后，应用程序会得到IO操作完成的通知，此时用户进程只需要对数据进行处理就好了，不需要进行实际的IO读写操作，因为真正的IO读取或者写入操作已经由内核完成了。   

   同步IO和异步IO的区别就在于第二个步骤是否阻塞，如果实际的IO读写阻塞请求进程，那么就是同步IO。 
   阻塞IO和非阻塞IO的区别在于第一步，发起IO请求是否会被阻塞，如果阻塞直到完成那么就是传统的阻塞IO，如果不阻塞，那么就是非阻塞IO。 

15、加入要处理100个连接，用BIO和NIO分别需要多少个线程
-------------

  &emsp;&emsp;假如有10000个连接，4核CPU ，那么bio 就需要一万个线程，而nio大概就需要5个线程(一个接收请求，四个处理请求)。如果这10000个连接同时请求，那么bio就有10000个线程抢四个CPU ，几乎每个CPU 平均执行2500次上下文切换，而nio 四个处理线程，几乎每个线程都对应一个CPU ，也就是几乎没有上下文切换。效率就体现出来了。

16、synchronized的原理，偏向锁，轻量级锁，重量级锁，sleep和wait的区别，线程状态有哪些，线程之间通信
-------------

  参考：[https://blog.csdn.net/kirito_j/article/details/79201213](https://blog.csdn.net/kirito_j/article/details/79201213 "https://blog.csdn.net/kirito_j/article/details/79201213")

  &emsp;&emsp;对于sleep()方法，我们首先要知道该方法是属于Thread类中的。而wait()方法，则是属于Object类中的。  
  &emsp;&emsp;sleep()方法导致了程序暂停执行指定的时间，让出cpu该其他线程，但是他的监控状态依然保持者，当指定的时间到了又会自动恢复运行状态。  
  &emsp;&emsp;在调用sleep()方法的过程中，线程不会释放对象锁。  
  &emsp;&emsp;而当调用wait()方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用notify()方法后本线程才进入对象锁定池准备  

  &emsp;&emsp;线程从创建、运行到结束总是处于下面五个状态之一：新建状态、就绪状态、运行状态、阻塞状态及死亡状态。（参考：https://www.cnblogs.com/shenjiangwei/p/7501110.html）

  线程控制和线程之间的通信：主要是三个方法wait(); notify(); notifyAll();  

17、ReentrantLock的原理，和synchronized的区别
-------------

  &emsp;&emsp;ReenTrantLock的实现是一种自旋锁，通过循环调用CAS操作来实现加锁。它的性能比较好也是因为避免了使线程进入内核态的阻塞状态。  
  &emsp;&emsp;想尽办法避免线程进入内核的阻塞状态是我们去分析和理解锁设计的关键钥匙。  

 （参考：https://www.cnblogs.com/baizhanshi/p/7211802.html）   

  **区别:** 可重入性、锁的实现（Synchronized是依赖于JVM实现的，而ReenTrantLock是JDK实现的）   
  **性能的区别** （Synchronized优化以前，synchronized的性能是比ReenTrantLock差很多的，但是自从Synchronized引入了偏向锁，轻量级锁（自旋锁）后， 两者的性能就差不多了）  
  **功能区别** （很明显Synchronized的使用比较方便简洁，并且由编译器去保证锁的加锁和释放，而ReenTrantLock需要手工声明来加锁和释放锁）、锁的细粒度和灵活度：很明显ReenTrantLock优于Synchronized  

18、CAS操作
-------------

  CAS（Compare-and-Swap），即比较并替换，是一种实现并发算法时常用到的技术.  
  CAS是英文单词CompareAndSwap的缩写，中文意思是：比较并替换。CAS需要有3个操作数：内存地址V，旧的预期值A，即将要更新的目标值B。  
  CAS指令执行时，当且仅当内存地址V的值与预期值A相等时，将内存地址V的值修改为B，否则就什么都不做。整个比较并替换的操作是一个原子操作。  

  CAS的缺点：CAS虽然很高效的解决了原子操作问题，但是CAS仍然存在三大问题。  
  **(1).** 循环时间长开销很大。（如果CAS失败，会一直进行尝试。）  
  **(2).** 只能保证一个共享变量的原子操作。（对多个共享变量操作时，循环CAS就无法保证操作的原子性，这个时候就可以用锁来保证原子性。）  
  **(3).** ABA问题。（如果内存地址V初次读取的值是A，并且在准备赋值的时候检查到它的值仍然为A，那我们就能说它的值没有被其他线程改变过了吗？如果在这段期间它的值曾经被改成了B，后来又被改回为A，那CAS操作就会误认为它从来没有被改变过。这个漏洞称为CAS操作的“ABA”问题）  

19、AtomicInteger的原理
-------------

  &emsp;&emsp;AtomicInteger的本质：自旋锁+CAS原子操作  
  &emsp;&emsp;AtomicInteger的本质 int value，私有的  
  &emsp;&emsp;JDK1.7及以前（获取volatitle修饰的变量，最新的主存值）、JDK1.8及以后（封装了自旋锁 直接封装unsafe方法中了，保证原子性，unsafe是JDK私有的我们不能调用）  

20、volatile能不能保证线程安全
-------------

  &emsp;&emsp;volatile是不能保证线程安全的，它只是保证了数据的可见性，不会再缓存，每个线程都是从主存中读到的数据，而不是从缓存中读取的数据，

21、线程安全的单例模式（参考：https://www.cnblogs.com/xudong-bupt/p/3433643.html）
-------------

  **(1).** 多线程安全单例模式实例一(不使用同步锁)  
  **(2).** 多线程安全单例模式实例二(使用同步方法)  
  **(3).** 多线程安全单例模式实例三(使用双重同步锁,对new方法进行同步)  

22、HashMap，HashTable，ConcurrentHashMap的区别（参考：https://www.cnblogs.com/heyonggang/p/9112731.html）
-------------

  **HashTable:** 底层数组+链表实现，无论key还是value都不能为null，线程安全，实现线程安全的方式是在修改数据时锁住整个HashTable，效率低，ConcurrentHashMap做了相关优化，初始size为11，扩容：newsize = olesize\*2+1*  
  **HashMap**  
  **(1).** 底层数组+链表实现，可以存储null键和null值，线程不安全   
  **(2).** 初始size为16，扩容：newsize = oldsize*2，size一定为2的n次幂   
  **(3).** 扩容针对整个Map，每次扩容时，原来数组中的元素依次重新计算存放位置，并重新插入   
  **(4).** 插入元素后才判断该不该扩容，有可能无效扩容（插入后如果扩容，如果没有再次插入，就会产生无效扩容）  
  **(5).** 当Map中元素总数超过Entry数组的75%，触发扩容操作，为了减少链表长度，元素分配更均匀  
  **ConcurrentHashMap**  
  **(1).** 底层采用分段的数组+链表实现，线程安全  
  **(2).** 通过把整个Map分为N个Segment，可以提供相同的线程安全，但是效率提升N倍，默认提升16倍。(读操作不加锁，由于HashEntry的value变量是 volatile的，也能保证读取到最新的值。)  
  **(3).** Hashtable的synchronized是针对整张Hash表的，即每次锁住整张表让线程独占，ConcurrentHashMap允许多个修改操作并发进行，其关键在于使用了锁分离技术  
  **(4).** 有些方法需要跨段，比如size()和containsValue()，它们可能需要锁定整个表而而不仅仅是某个段，这需要按顺序锁定所有段，操作完毕后，又按顺序释放所有段的锁  
  **(5).** 扩容：段内扩容（段内元素超过该段对应Entry数组长度的75%触发扩容，不会对整个Map进行扩容），插入前检测需不需要扩容，有效避免无效扩容  

  **区别:**  
  **(1).** Hashtable是线程安全的，它的方法是同步的，可以直接用在多线程环境中。而HashMap则不是线程安全的，在多线程环境中，需要手动实现同步机制。  
  **(2).** Hashtable与HashMap另一个区别是HashMap的迭代器（Iterator）是fail-fast迭代器，而Hashtable的enumerator迭代器不是fail-fast的。所以当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException，但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。  
  **(3).** 在HashMap中，null可以作为键，这样的键只有一个，但可以有一个或多个键所对应的值为null。而在Hashtable中，无论是key还是value都不能为null。  
  **(4).** Hashtable和HashMap都实现了Map接口，但是Hashtable的实现是基于Dictionary抽象类的。Java5提供了ConcurrentHashMap，它是HashTable的替代，比HashTable的扩展性更好。  

23、jdk1.8对HashMap做了哪些改动
-------------

  ++1.7++HashMap底层是使用数组+链表实现  
  ++1.8++HashMap底层是使用数组+链表+红黑树实现  

  如果数组位置有链表了，是将新key加在链表尾部  
  在添加时，如果链表大小大于预设阈值，则将链表转换为红黑树（查找更高效）  

  1.8抛弃了原有的Segment分段锁设计，而采用了CAS+synchronized 来保证并发安全性  

24、JVM内存模型，GC算法，CMS有几次stop the world(参考：[https://www.cnblogs.com/kingszelda/p/7226080.html](https://www.cnblogs.com/kingszelda/p/7226080.html "https://www.cnblogs.com/kingszelda/p/7226080.html"))
-------------

![](https://images2015.cnblogs.com/blog/1196330/201707/1196330-20170723142133809-928230884.png)

  **GC算法**  
  **(1).标记-清除算法**
  &emsp;&emsp;最基础的垃圾收集算法是“标记-清除”(Mark Sweep)算法，正如名字一样，算法分为2个阶段：1.标记处需要回收的对象，2.回收被标记的对象。  
  &emsp;&emsp;标记算法分为两种:1.引用计数算法(Reference Counting) 2.可达性分析算法(Reachability Analysis)。由于引用技术算法无法解决循环引用的问题，所以这里使用的标记算法均为可达性分析算法。  
  **(2).复制算法**  
  &emsp;&emsp;为了解决效率与内存碎片问题，复制(Copying)算法出现了，它将内存划分为两块相等的大小，每次使用一块，当这一块用完了，就讲还存活的对象复制到另外一块内存区域中，然后将当前内存空间一次性清理掉。这样的对整个半区进行回收，分配时按照顺序从内存顶端依次分配，这种实现简单，运行高效。不过这种算法将原有的内存空间减少为实际的一半，代价比较高。  
  **(3).标记-整理算法**  
  &emsp;&emsp;复制算法在极端情况下(存活对象较多)效率变得很低，并且需要有额外的空间进行分配担保。所以在老年代中这种情况一般是不适合的。

  &emsp;&emsp;CMS，全称Concurrent Mark and Sweep，用于对年老代进行回收，目标是尽量减少应用的暂停时间，减少full gc发生的机率，利用和应用程序线程并发的垃圾回收线程来标记清除年老代。CMS并非没有暂停，而是用两次短暂停来替代串行标记整理算法的长暂停。  
  &emsp;&emsp;CMS有几次stop the world:两次标记时；（安全点）循环的末尾、（安全点）方法临返回前 / 调用方法的call指令后、（安全点）可能抛异常的位置  

25、新生代gc几次存活之后才能进去老年代
-------------

   **(1).** Eden区满时，进行Minor GC，当Eden和一个Survivor区中依然存活的对象无法放入到Survivor中，则通过分配担保机制提前转移到老年代中。   
   **(2).** 若对象体积太大, 新生代无法容纳这个对象，-XX:PretenureSizeThreshold即对象的大小大于此值, 就会绕过新生代, 直接在老年代分配,此参数只对Serial及ParNew两款收集器有效。  
   **(3).** 长期存活的对象将进入老年代。  
   &emsp;&emsp;虚拟机对每个对象定义了一个对象年龄（Age）计数器。当年龄增加到一定的临界值时，就会晋升到老年代中，该临界值由参数：-XX:MaxTenuringThreshold来设置。  
   &emsp;&emsp;如果对象在Eden出生并在第一次发生MinorGC时仍然存活，并且能够被Survivor中所容纳的话，则该对象会被移动到Survivor中，并且设Age=1；以后每经历一次Minor GC，该对象还存活的话Age=Age+1。  
   **(4).** 动态对象年龄判定。  
   &emsp;&emsp;虚拟机并不总是要求对象的年龄必须达到MaxTenuringThreshold才能晋升到老年代，如果在Survivor区中相同年龄（设年龄为age）的对象的所有大小之和超过Survivor空间的一半，年龄大于或等于该年龄（age）的对象就可以直接进入老年代，无需等到MaxTenuringThreshold中要求的年龄。  

26、频繁GC的可能原因
-------------

  **(1).年老代空间比较小**  
  解决方法：第一，增大年老代空间。第二：使用CMS GC，对年老代进行回收，减少full gc发生的几率。  
  **(2).调用了System.gc()**  
  解决方法：-XX:+DisableExplicitGC   忽略手动调用GC, System.gc()的调用就会变成一个空调用，完全不触发GC

27、线上OOM，日志十几个G，怎么快速定位
-------------

  **(1).** 确认是不是内存本身就分配过小(方法：jmap -heap 10765)  
  **(2).** 找到最耗内存的对象(jmap -histo:live 10765 | more)  
  **(3).** 确认是否是资源耗尽  

28、导致ANR的问题有哪些，都是怎样出现的(参考：[https://www.jianshu.com/p/410b35dc16e0](https://www.jianshu.com/p/410b35dc16e0 "https://www.jianshu.com/p/410b35dc16e0"))
-------------

  **官方给出的产生ANR一般有**   
  **(1).** KeyDispatchTimeout（5秒） - 主要类型   按键或触摸事件在特定时间内无响应  
  **(2).** BroadcastTimeout（10秒）BroadcastReceiver在特定时间内无法处理完成  
  **(3).** ServiceTimeout （20秒） - 小概率类型   服务在特定的时间内无法处理完成  

  **anr出问题原因：**
  **(1).** dispatchTimeout输入事件分发超时，一般是由于主线程在5秒之内没有响应输入事件。  
  **(2).** BroadcastReceiver没有在系统设定的时间内完成并返回。  
  **(3).** (细分到主线程)在主线程做耗时网络访问  
  **(4).** (细分到主线程)当有大量数据读写操作时，在主线程再请求数据读写  
  **(5).** (细分到主线程)数据库操作（比如其他大数据量应用访问数据库导致数据库负载过重时）  
  **(6).** (细分到主线程)硬件操作（比如Camera）  
  **(7).** (细分到主线程)Service binder数量达到上限  
  **(8).** (细分到主线程)在system_server中发生WatchDog（ANRhttps：//zhuanlan.zhihu.com/p/20488872 )   
  **(9).** (细分到主线程)Service耗时的操作大于15s ，导致超时无响应，  
  **(10).** (细分到主线程)初始化的数据和控件太多（不正确和低效率的初始化）;   
  **(11).** (细分到主线程)主线程中加载过大数据和图片;   
  **(12).** (细分到主线程)远程传递数据 多或过大。  
  **(13).** (细分到主线程)内存不足导致块在创建位图上  
  **(14).** (细分到主线程)调用线程的join（）/ Sleep（）/ Wait（）或者等待locker的时候，造成线程块   
  **(15).** (细分到非主线程)非主线程持有锁，导致主线程等待锁超时  
  **(16).** (细分到非主线程)主非线程终止或者崩溃导致主线程一直等待  

29、事务的原理，事务的特性，事务的传播行为，事务的隔离级别
30、分布式事务，二阶段提交，三阶段提交，tcc能不能保证100％一致性
31、CAP，BASE理论，最终一致性的概念
32、A和B用户在不同的节点，用最终一致性设计转账功能
33、判断集群保证了CAP里面的哪些要素，MySql主备集群，MySql范围分区集群，HBase，Redis-Cluster，Redis哨兵集群，Zookeeper集群，Kafka集群
34、一致性哈希节点分布不均匀怎么办
35、MySql分库分表策论:范围分库，取模，一致性哈希的优缺点
36、MyCat和Sharding-JDBC的区别，优缺点
37、索引原理，索引失效的原因，ABC联合索引实际建了几个索引，MYASIAM和INNODB的区别
38、什么情况下锁行，什么情况下锁表，MySql乐观锁，排它锁，间隙锁
39、Select for update分别在主键，唯一索引，分索引列，锁了哪些东西(一次写不下)

40、java文件new一个新对象加载流程
-------------

  **(1).** 首先是jvm工作，找到A.class，classloader开始工作，包括各种检查、校验  
  **(2).** 在类装载时，类中的static部分开始初始化（第一次装载的时候初始化，代码块、变量按顺序初始化）  
  **(3).** new出a，堆上开辟空间  
  **(4).** 所有成员变量初始化，基本类型赋值默认值，引用类型赋值null  
  **(5).** 执行构造函数  

  **类首次加载及new对象： **  
  **(1).** 先执行父类的静态代码块和静态变量初始化，并且静态代码块和静态变量的执行顺序只跟代码中出现的顺序有关。 （静态函数只初始化，函数被动调用，代码块是主动调用）  
  **(2).** 执行子类的静态代码块和静态变量初始化。   
  **开始new对象**
  **(3).** 执行父类的实例变量初始化   
  **(4).** 执行父类的构造函数   
  **(5).** 执行子类的实例变量初始化   
  **(6).** 执行子类的构造函数   
  如果类已经被加载及new对象：则静态代码块和静态变量就不用重复执行，再创建类对象时，只执行与实例相关的变量初始化和构造方法。  

41、Handler四大组件
-------------

  **(1).Message**  
  &emsp;&emsp;Message是在线程之间传递的消息，它可以在内部携带少量的信息，用于在不同线程之间交换数据。  
  &emsp;&emsp;例：Message的what字段、arg1字段、arg2字段来携带整型数据，obj字段携带一个Object对象。  
  **(2).Handler**  
  &emsp;&emsp;处理者，它主要用来发送和处理消息。发送消息一般是使用Handler的sendMessage()方法，消息经过处理后，最终传递到Handler的handlerMessage()方法中。  
  **(3).MessageQueue**  
  &emsp;&emsp;消息队列，它主要用来存放所有通过Handler发送的消息，这部分消息会一直存在于消息队列中，等待被处理。  
  &emsp;&emsp;注意：每个线程中只会有一个MessageQueue对象。  
  **(4).Looper**  
  &emsp;&emsp;是每个线程中MessageQueue的管家，调用Looper的loop()方法后，就会进入到一个无限循环当中，每当发现MessageQueue中存在一条消息，就会将其取出传递到Handler的handleMessage()方法当中。  

     **Looper的quit方法或quitSafely方法**  
          **相同点:** 将不在接受新的事件加入消息队列。
    
          **不同点:**(1)当我们调用Looper的quit方法时，实际上执行了MessageQueue中的removeAllMessagesLocked方法，该方法的作用是把MessageQueue消息池中所有的消息全部清空，无论是延迟消息（延迟消息是指通过sendMessageDelayed或通过postDelayed等方法发送的需要延迟执行的消息）还是非延迟消息。(2)当我们调用Looper的quitSafely方法时，实际上执行了MessageQueue中的removeAllFutureMessagesLocked方法，通过名字就可以看出，该方法只会清空MessageQueue消息池中所有的延迟消息，并将消息池中所有的非延迟消息派发出去让Handler去处理，quitSafely相比于quit方法安全之处在于清空消息之前会派发所有的非延迟消息。

  &emsp;&emsp;注意：每个线程中只会有一个Looper对象。

42、Handler消息机制：
-------------

  **作用：** 跨线程通信。当子线程中进行耗时操作后需要更新UI时，通过Handler将有关UI的操作切换到主线程中执行。  
  **四要素：**  
  **(1).** Message（消息）：需要被传递的消息，其中包含了消息ID，消息处理对象以及处理的数据等，由MessageQueue统一列队，最终由Handler处理。  
  **(2).** MessageQueue（消息队列）：用来存放Handler发送过来的消息，内部通过单链表的数据结构来维护消息列表，等待Looper的抽取。  
  **(3).** Handler（处理者）：负责Message的发送及处理。通过 Handler.sendMessage() 向消息池发送各种消息事件；通过 Handler.handleMessage()处理相应的消息事件。
  **(4).** Looper（消息泵）：通过Looper.loop()不断地从MessageQueue中抽取Message，按分发机制将消息分发给目标处理者。  
  **具体流程:**  
  &emsp;&emsp;Handler.sendMessage()发送消息时，会通过MessageQueue.enqueueMessage()向MessageQueue中添加一条消息；  
  &emsp;&emsp;通过Looper.loop()开启循环后，不断轮询调用MessageQueue.next()；  
  &emsp;&emsp;调用目标Handler.dispatchMessage()去传递消息，目标Handler收到消息后调用Handler.handlerMessage()处理消息。  

43、关键字final和static是怎么使用的。
-------------

  **final:**  
  **(1).** final变量即为常量，只能赋值一次。  
  **(2).** final方法不能被子类重写。  
  **(3).** final类不能被继承。  

  **static：**  
  **(1).** static变量：对于静态变量在内存中只有一个拷贝（节省内存），JVM只为静态分配一次内存，在加载类的过程中完成静态变量的内存分配，可用类名直接访问（方便），当然也可以通过对象来访问（但是这是不推荐的）。  
  **(2).** static代码块 static代码块是类加载时，初始化自动执行的。  
  **(3).** static方法static方法可以直接通过类名调用，任何的实例也都可以调用，因此static方法中不能用this和super关键字，不能直接访问所属类的实例变量和实例方法(就是不带static的成员变量和成员成员方法)，只能访问所属类的静态成员变量和成员方法。  

44、String,StringBuffer,StringBuilder区别
-------------

  **(1).** 三者在执行速度上：StringBuilder > StringBuffer > String (由于String是常量，不可改变，拼接时会重新创建新的对象)。  
  **(2).** StringBuffer是线程安全的，StringBuilder是线程不安全的。（由于StringBuffer有缓冲区）  

45、Java中重载和重写的区别：
-------------

  **重载：** 一个类中可以有多个相同方法名的，但是参数类型和个数都不一样。这是重载。  
  **重写：** 子类继承父类，则子类可以通过实现父类中的方法，从而新的方法把父类旧的方法覆盖。  
