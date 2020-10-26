# Java面试题汇总

## Java基础

#### 1.多态的实现

多态，是指父类引用能指向子类对象。调用方法时会使用子类重写后的方法。多态的实现的关键在于“动态绑定”。多态的实现方式：接口，重写，抽象方法等。可以使代码更加灵活，消除耦合

#### 2.==和equals的区别，为什么重写equals要一同重写hashcode

对于基本数据类型，\==比较的是他们的值，对于引用类型（类、接口、数组），\==比较的是引用的地址

equals是Object中的方法，初试是比较两个对象的地址值，但是可以被覆盖，定义不同的比较内容

一同重写hashcode的原因是，在调用hashmap这些Java底层工具时，会先进行hash值的判断，只有当hash值相同时才会调用equals或是==去判断是否真的相同。如果在逻辑上（用equals函数）是相等的情况下，那么该对象的hashcode也应当是相同的，所以equals的结果应当与hash的结果保持一致。不然即使equals返回相等，hashcode不一致的两个对象，hashmap仍然认为这是两个键而不是同一个。

#### 3.equals和hashcode 的区别

hashCode() 返回哈希值，而 equals() 是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价，这是因为计算哈希值具有随机性，两个值不同的对象可能计算出相同的哈希值。

在重写 equals() 方法时应当总是重写 hashCode() 方法，保证等价的两个对象哈希值也相等。

#### 4.BIO，NIO，AIO有什么区别

BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。

NIO：JDK1.4引入，是一种同步非阻塞IO,弥补了原来的 I/O 的不足，提供了高速的、面向块的 I/O。I/O 以流的方式处理数据，而 NIO 以块的方式处理数据。主要有三大核心部分：Channel(通道)，Buffer(缓冲区), Selector（多路复用器）。

- 通道 Channel 是对原 I/O 包中的流的模拟，可以通过它读取和写入数据。通道与流的不同之处在于，流只能在一个方向上移动(一个流必须是 InputStream 或者 OutputStream 的子类)，而通道是双向的，可以用于读、写或者同时用于读写。
- 发送给一个通道的所有数据都必须首先放到缓冲区中，同样地，从通道中读取的任何数据都要先读到缓冲区中。也就是说，不会直接对通道进行读写数据，而是要先经过缓冲区。缓冲区实质上是一个数组，但它不仅仅是一个数组。缓冲区提供了对数据的结构化访问，而且还可以跟踪系统的读/写进程。
- Selector与Channel是相互配合使用的，将Channel注册在Selector上之后，才可以正确的使用Selector，但此时Channel必须为非阻塞模式。Selector可以监听Channel的四种状态（Connect、Accept、Read、Write），当监听到某一Channel的某个状态时，才允许对Channel进行相应的操作。

AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。

#### 5.Java有多继承吗？如何实现类似多继承的功能

不支持。

多继承是指：一个子类同时继承多个父类，从而具备多个父类的特征。

Java实现多继承的三种方式：多层继承，内部类，接口

#### 6.final，finally，finalize

final：是一个关键字也是一个修饰符

- 被final修饰的类无法被继承
- 对于一个final变量，如果是基本数据类型的变量，其数值在初始化以后就不能被更改，如果是引用类型的变量，对其初始化以后，便不能再指向另一个对象，但它指向对象的内容是可变的
- 被final修饰的方法无法被重写，但允许重载

finally：是一个关键字

- finally在异常处理时提供finally块来提供清除操作，无论是否有异常被抛出或被捕获，finally块都会执行，通常用于释放资源

- finally块正常情况下一定会被执行，但又两种特殊情况：

  对应的try块没有执行；在try块中JVM关机

- finally块中如果有return语句，则会覆盖try或catch中的return语句

finalize():是Object类的protected方法，子类可以覆盖该方法以实现资源清理工作。GC在回收对象之前会调用该方法。

#### 7.重载（overlord）和重写（overwrite）

方法的重载与重写都是实现多态的方式，区别在于前者实现的是编译时的多态，后者实现的是运行时的多态。重载发生在一个类中，同名的方法如果有不同的参数列表（参数类型不同，参数个数不同或二者都不同），则视为重载。重写发生在子类与父类之间，重写要求子类的重写方法与父类的被重写方法有相同的参数列表，有兼容的返回类型，比父类被重写方法更容易访问，不能比父类被重写方法声明更多的异常。

重载对返回类型没有特殊的要求，不能根据返回值进行区分。

#### 8.comparator和comparable

Comparable 接口用于定义对象的自然顺序，是排序接口，而 comparator 通常用于定义用户定制的顺序，是比较接口。我们如果需要控制某个类的次序，而该类本身不支持排序(即没有实现Comparable接口)，那么我们就可以建立一个“该类的比较器”来进行排序。Comparable 总是只有一个，但是可以有多个 comparator 来定义对象的顺序。

#### 9.String,StringBuilder,StringBuffer

String 声明的是不可变的对象，每次操作都会生成新的 String 对象，然后将指针指向新的 String 对象，而 StringBuffer、StringBuilder 可以在原有对象的基础上进行操作，所以在经常改变字符串内容的情况下最好不要使用 String。

StringBuffer 和 StringBuilder 最大的区别在于，StringBuffer 是线程安全的，而 StringBuilder 是非线程安全的，但 StringBuilder 的性能却高于 StringBuffer，所以在单线程环境下推荐使用 StringBuilder，多线程环境下推荐使用 StringBuffer。

#### 10.Java与C++

- Java 是纯粹的面向对象语言，所有的对象都继承自 java.lang.Object，C++ 为了兼容 C 即支持面向对象也支持面向过程。
- Java 通过虚拟机从而实现跨平台特性，但是 C++ 依赖于特定的平台。
- Java 没有指针，它的引用可以理解为安全指针，而 C++ 具有和 C 一样的指针。
- Java 支持自动垃圾回收，而 C++ 需要手动回收。
- Java 不支持多重继承，只能通过实现多个接口来达到相同目的，而 C++ 支持多重继承。
- Java 不支持操作符重载，虽然可以对两个 String 对象执行加法运算，但是这是语言内置支持的操作，不属于操作符重载，而 C++ 可以。
- Java 的 goto 是保留字，但是不可用，C++ 可以使用 goto。

#### 11.JRE与JDK

- JRE：Java Runtime Environment，Java 运行环境的简称，为 Java 的运行提供了所需的环境。它是一个 JVM 程序，主要包括了 JVM 的标准实现和一些 Java 基本类库。
- JDK：Java Development Kit，Java 开发工具包，提供了 Java 的开发及运行环境。JDK 是 Java 开发的核心，集成了 JRE 以及一些其它的工具，比如编译 Java 源码的编译器 javac 等。

## Java容器

#### 1.Java集合的类层次关系，各种集合容器

集合类存放于 Java.util 包中，主要有 3 种：set(集）、list(列表包含 Queue）和 map(映射）。

list，set继承自Collection接口，Map不是。

Collection：Collection 是集合 List、Set、Queue 的最基本的接口。

list是一个有序集合，可以包含重复的元素

- ArrayList:基于动态数组实现，支持随机访问

- Vector：与ArrayList类似，但是线程安全的。

- LinkedList：是用链表结构存储数据的，很适合数据的动态插入和删除，随机访问和遍历速度比较

  慢。另外，他还提供了 List 接口中没有定义的方法，专门用于操作表头和表尾元素，可以当作堆

  栈、队列和双向队列使用。

Set：不能包含重复元素的集合

- TreeSet：基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet，HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。
- HashSet：基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。
- LinkedHashSet：具有 HashSet 的查找效率，并且内部使用双向链表维护元素的插入顺序。

Map：是一个将key映射到value的对象

- TreeMap：基于红黑树实现。
- HashMap：基于哈希表实现。
- HashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程同时写入 HashTable 不会导致数据不一致。它是遗留类，不应该去使用它，而是使用 ConcurrentHashMap 来支持线程安全，ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。
- LinkedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。

#### 2.**Collection** 和 **Collections** 有什么区别

Collection是一个集合接口，提供了对集合对象进行基本操作的通用接口方法；

Collections是一个包装类，包含了很多静态方法，但是不能被实例化

#### ==3.Hashmap==（高频）

##### 1.数据结构

**哈希表结构（链表散列：数组+链表）**实现，结合数组和链表的优点。当链表长度超过 **8** 时，链表转换为**红黑树**。

Node<K,V>[] table

<img src="https://upload-images.jianshu.io/upload_images/7779232-1a3b5f1c556ce0c9.png?imageMogr2/auto-orient/strip|imageView2/2/w/552/format/webp" alt="img" style="zoom:67%;" />

##### 2.hash的实现

JDK1.8中通过hashcode（）的高16位异或低16位来实现的(h = k.hashCode()) ^ (h >>> 16)，主要是从速度，功效和质量来考虑的，**减少系统的开销**，也不会造成**因为高位没有参与**下标的计算，从而引起的**碰撞**。

##### 3.工作原理

HashMap 底层是 **hash 数组**和**单向链表**实现，数组中的每个元素都是**链表**，由 **Node 内部类（实现 Map.Entry<K,V>接口）**实现，HashMap 通过 put & get 方法存储和获取。

**存储对象**时，将 K/V 键值传给 put() 方法：①、调用 hash(K) 方法**计算 K 的 hash 值**，然后结合数组长度，计算得数组下标；②、**调整数组大小**（当容器中的元素个数大于 capacity * loadfactor 时，容器会进行扩容resize 为 2n）；
 ③、i.如果 **K 的 hash 值**在 HashMap 中**不存在**，则执行**插入**，若存在，则发生**碰撞**；
 ii.如果 K 的 hash 值在 HashMap 中**存在**，且它们两者 **equals 返回 true**，则**更新键值对**；
 iii. 如果 K 的 hash 值在 HashMap 中**存在**，且它们两者 **equals 返回 false**，则**插入链表的尾部（尾插法）或者红黑树中（树的添加方式）**。

（**JDK 1.7** 之前使用**头插法**、**JDK 1.8** 使用**尾插法**）
 （注意：当碰撞导致链表大于 TREEIFY_THRESHOLD =  8 时，就把链表转换成红黑树）

**获取对象**时，将 K 传给 get() 方法：①、调用 hash(K) 方法（**计算 K 的 hash 值**）从而**获取该键值所在链表的数组下标**；②、顺序遍历链表，equals()方法查找**相同 Node 链表中 K 值**对应的 V 值。

hashCode 是定位的，**存储位置**；equals是定性的，**比较两者是否相等**

##### 4.扩容机制

①、**table 数组大小**是由 **capacity** 这个参数确定的，默认是**16**，也可以构造时传入，最大限制是1<<30；
 ②、**loadFactor 是装载因子**，主要目的是用来**确认table 数组是否需要动态扩展**，默认值是**0.75**，比如table 数组大小为 16，装载因子为 0.75 时，threshold 就是12，当 table 的实际大小超过 12 时，table就需要动态扩容；
 ③、扩容时，调用 resize() 方法，将 **table 长度变为原来的两倍**（注意是 **table 长度**，而不是 threshold）
 ④、如果数据很大的情况下，扩展时将会带来性能的损失，在性能要求很高的地方，这种损失很可能很致命。

#### 4.hashmap为什么线程不安全

在并发场景下使用hashmap可能会造成死循环，更普遍的是在多线程场景下，由于堆内存对于各个线程是共享的，而hashmap的put方法不是原子操作，假设Thread1先 put 值，然后 sleep 2秒(也可以是系统时间片切换失去执行权)，在这2秒内值被Thread2改了，Thread1“醒来”再 get 的时候发现已经不是原来的值了，这就容易出问题。

**ConcurrentHashMap** 类（是 Java并发包 java.util.concurrent 中提供的一个**线程安全且高效**的 HashMap 实现）。
 **HashTable** 是使用 **synchronize** 关键字加锁的原理（就是对**对象**加锁）；
 而针对 **ConcurrentHashMap**，在 **JDK 1.7** 中采用 **分段锁**的方式；**JDK 1.8** 中直接采用了**CAS（无锁算法）+ synchronized**。

二者除了加锁，原理上无太大区别

#### 5.hashmap 1.7,1.8的区别

1. `jdk7 数组+单链表 jdk8 数组+(单链表+红黑树) `
2. `jdk7 链表头插 jdk8 链表尾插 `
3. `    头插: resize后transfer数据时不需要遍历链表到尾部再插入`
4. `    头插: 最近put的可能等下就被get，头插遍历到链表头就匹配到了`
5. `    头插: resize后链表可能倒序; 并发resize可能产生循环链`
6. `jdk7 先扩容再put jdk8 先put再扩容  (why?有什么区别吗?)`
7. `jdk7 计算hash运算多 jdk8 计算hash运算少(http://www.jasongj.com/java/concurrenthashmap/#寻址方式-1)`
8. `jdk7 受rehash影响 jdk8 调整后是(原位置)or(原位置+旧容量)`

#### 7.hashset 与 hashmap的区别，用hashmap简单实现hashset

hashset是基于hashmap实现的，底层使用hashmap来保存所有元素。

区别是hashset不允许重复的值

#### 8.Treemap和hashmap如何选择

**TreeMap** 实现 **SortMap** 接口，能够把它保存的记录**根据键排序**（默认按键值升序排序，也可以指定排序的比较器）

一般情况下，使用最多的是 **HashMap**。
**HashMap**：在 Map 中**插入、删除和定位元素**时；
**TreeMap**：在需要**按自然顺序或自定义顺序遍历键**的情况下；

#### 9.HashMap 和 HashTable 有什么区别？

- **HashMap** 是线程不安全的，**HashTable** 是**线程安全**的
- 由于线程安全，所以 HashTable 的效率比不上 HashMap
- **HashMap**最多只允许**一条记录的键为null**，允许**多条记录的值为null**，而 **HashTable** 不允许；
- **HashMap** 默认初始化数组的大小为**16**，**HashTable** 为 **11**，前者扩容时，扩大**两倍**，后者**扩大两倍+1**；
- HashMap 需要重新计算 hash 值，而 HashTable 直接使用对象的 hashCode

## Java并发

Java锁机制：

对象头、MarkWord、synchrobized-->monitor-->metux lock、进一步优化至四种锁状态

#### 1.Java.util.concurrent包下的类

#### 2.进程与线程

进程：系统中正在运行的一个程序，可以看做是程序运行的一个实例，进程是系统资源独立分配的一个实体，每个进程都有独立的地址空间，进程之间相互访问需要使用进程间通信

线程：操作系统能够进行运算调度的最小单位，包含在进程中，是进程的实际运作单位，一个进程可以并发多个线程，每条线程并行执行不同的任务

#### 3..创建线程的方法，Java线程的实现方式

有三种使用线程的方法：

- 实现Runnable接口
- 实现Callable接口
- 继承Thread类

##### 1.实现Runnable接口

```java
实现Runnable接口
* 1.创建一个实现了Runnable接口的类
* 2.实现类去实现Runnable中的抽象方法：run()
* 3.创建实现类的对象
* 4.将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
* 5.通过Thread类的对象调用start()
public class MyRunnable implements Runnable{
    @Override
    public void run(){
        //...
    }
}
public static void main(String[] args){
    MyRunnable instance = new MyRunnable();
    Thread thread1 = new Thread();
    thread1.start();
}
```

##### 2.实现Callable接口

与Runnable接口相比，Callable接口可以有返回值，返回值通过FutureTask进行封装

```java
public class MyCallable implements Callable{//1.创建一个Callable的实现类
    //2.实现call方法，线程的操作声明在call（）中
    @Override
    public Object call() throws Exception {
        int sum = 0;
        for (int i = 1; i < 100 ; i++) {
            if (i %2 == 0) {
                sum += i;
            }
        }
        return sum;
    }
}
public static void main(String[] args) {
        //3.创建callable接口实现类的对象
        NumThread numThread = new NumThread();
        //4.将此callable接口实现类的对象传递到FutureTask构造器中，创建FutureTask的对象
        FutureTask futureTask = new FutureTask(numThread);
        //5.将FutureTask类的对象作为参数传递到Thread类的构造器中，创建Thread类的对象，并调用start()
        new Thread(futureTask).start();
        try {
            //获取callable中call方法的返回值
            Object sum =  futureTask.get();
            System.out.println(sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
```

##### 3.继承Thread接口

同样也是需要实现 run() 方法，因为 Thread 类也实现了 Runable 接口。

当调用 start() 方法启动一个线程时，虚拟机会将该线程放入就绪队列中等待被调度，当一个线程被调度时会执行该线程的 run() 方法。

```java
* 1.创建一个继承于Thread类的子类
 * 2.重写Thread类的run()-->此线程执行的操作声明在run()中
 * 3.创建Thread类的子类的对象
 * 4.通过此对象调用start()
public class MyThread extends Thread {
    public void run() {
        // ...
    }
}
public static void main(String[] args) {
    MyThread mt = new MyThread();
    mt.start();
}
```

##### 实现接口 VS 继承 Thread

实现接口会更好一些，因为：

- Java 不支持多重继承，因此继承了 Thread 类就无法继承其它类，但是可以实现多个接口；
- 类可能只要求可执行就行，继承整个 Thread 类开销过大。

#### 4.线程状态

一个线程只能处于一种状态，并且这里的线程状态特指 Java 虚拟机的线程状态，不能反映线程在特定操作系统下的状态

- 新建（NEW）

  线程创建之后，未执行start（）方法

- 可运行（RUNABLE）/就绪

  正在 Java 虚拟机中运行，线程执行start方法进入RUNABLE状态。但是在操作系统层面，它可能处于运行状态，也可能等待资源调度（例如处理器资源），资源调度完成就进入运行状态。所以该状态的可运行是指可以被运行，具体有没有运行要看底层操作系统的资源调度。

- 运行（RUNNING）

  处于就绪状态的线程获得了CPU，开始执行run()方法的线程执行体

- 阻塞（BLOCKED）

  线程等待获取锁

- 无限期等待（WAITING）

  等待其他线程显示的唤醒。阻塞和等待的区别在于，阻塞是被动的，它是在等待获取 monitor lock。而等待是主动的，通过调用 Object.wait() 等方法进入。

- 限期等待（TIMED_WAITING）

  无需等待其它线程显式地唤醒，在一定时间之后会被系统自动唤醒。调用 Thread.sleep() 方法使线程进入限期等待状态时，常常用“使一个线程睡眠”进行描述。调用 Object.wait() 方法使线程进入限期等待或者无限期等待时，常常用“挂起一个线程”进行描述。睡眠和挂起是用来描述行为，而阻塞和等待用来描述状态。

- 死亡（TERMINATED）

  可以是线程结束任务之后自己结束，或者产生了异常而结束。

  ![image-20201023172514227](C:\Users\hply\AppData\Roaming\Typora\typora-user-images\image-20201023172514227.png)

#### 5.ReetrantLock和synchronized的区别

Java 提供了两种锁机制来控制多个线程对共享资源的互斥访问，第一个是 JVM 实现的 synchronized，而另一个是 JDK 实现的 ReentrantLock。

##### 1.synchronized

由JVM实现，主要作用有三个：1.确保线程互斥的访问同步代码。2.确保共享变量的修改能够及时可见。3.有效的解决重排序问题。

从语法上讲，共有三种用法：修饰普通方法、修饰静态方法、修饰代码块

synchronized锁存在Java对象头。Java对象的内存主要包含以下三个部分：

- 对象头区域：包括对象自身的运行时数据 (MarkWord) 存储 hashCode、GC 分代年龄、锁类型标记、偏向锁线程 ID 、 CAS 锁指向线程 LockRecord 的指针等， synchronized 锁的机制与这个部分 (markwork) 密切相关，用 markword 中最低的三位代表锁的状态，其中一位是偏向锁位，另外两位是普通锁位；还包括对象类型指针 (Class Pointer)，即对象指向它的类元数据的指针，JVM 就是通过它来确定是哪个 Class 的实例。
- 实例数据区域：对象中字段内容等。
- 对象填充区域：如果一个对象实际占的内存大小不是 8byte 的整数倍时，就"补位"到 8byte 的整数倍。

底层实现使用到了monitor，monitor依赖于操作系统底层的mutex lock，线程的挂起和唤醒涉及到了内核态与用户态的切换，一些情况下，切换时间甚至要高于执行时间，严重影响性能。

<img src="C:\Users\hply\AppData\Roaming\Typora\typora-user-images\image-20201025113056081.png" alt="image-20201025113056081" style="zoom:67%;" />

Java1.6对synchronized进行了优化，引入了偏向锁，轻量级锁，至此，锁共有四种状态。从低到高依次是无锁，偏向锁，轻量级锁，重量级锁，只能升级不能降级

![image-20201025114359255](C:\Users\hply\AppData\Roaming\Typora\typora-user-images\image-20201025114359255.png)

##### 2.ReetrantLock

由JDK实现，是JUC包下的锁，顾名思义，即支持重进入的锁，表示该锁支持一个线程对资源的重复加锁。

ReentrantLock中有两种锁：公平锁和非公平锁。

- 公平锁： 按照时间顺序，先来先获取锁，也就是FIFO，维护一个有序队列
- 非公平锁： 请求获取锁的顺序是随机的，不是公平的，可能一个请求多次获得锁，一个请求一次锁也获得不了

默认情况下，ReentrantLock获得的锁是非公平的

除非需要使用 ReentrantLock 的高级功能，否则优先使用 synchronized。这是因为 synchronized 是 JVM 实现的一种锁机制，JVM 原生地支持它，而 ReentrantLock 不是所有的 JDK 版本都支持。并且使用 synchronized 不用担心没有释放锁而导致死锁问题，因为 JVM 会确保锁的释放。

#### 6.JAVA中乐观锁，悲观锁，自旋锁。

乐观锁：是一种乐观思想，认为读多写少，遇到并发写的可能性较低，每次拿数据时都认为别人不会修改，所以不会上锁，需要借助冲突检查机制来判断在更新过程中是否有来自其他线程的干扰，如果存在，更新操作将失败，并且可以重试 。

Java中的乐观锁基本是通过CAS（比较并交换）来实现的，CAS是一种更新的原子操作，比较当前值与传入值是否一样，一样就更新，否则就失败

悲观锁：是一种悲观思想，认为写多，遇到并发写的可能性高，每次拿数据时都认为会被修改，所以每次在读写数据时都会上锁。Java中典型的悲观锁是Sychronized。

自旋锁：如果持有锁的线程能在很短的时间内释放掉锁资源，那么那些等待竞争锁资源的线程就不需要做内核态和用户态的切换进入阻塞挂起状态，它们只需要等一等（自旋），等待持有锁的线程释放锁后，即可立即持有锁资源，避免了切换的资源消耗。但是线程自旋需要消耗CPU，所以需要设定一个自旋等待的最大时间。jdk1.6引入了自适应自旋锁，意味着自旋时间不再固定，而是根据前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。

#### 7.AQS(抽象队列同步器)

核心思想：如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是用 CLH 队列锁实现的，即将暂时获取不到锁的线程加入到队列中。

CLH(Craig,Landin,and Hagersten) 队列是一个虚拟的双向队列（虚拟的双向队列即不存在队列实例，仅存在结点之间的关联关系）。AQS 是将每条请求共享资源的线程封装成一个 CLH 锁队列的一个结点 Node 来实现锁的分配。

![img](https://user-gold-cdn.xitu.io/2020/6/7/1728e03f48a29021?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

队列同步器 `AbstractQueuedSynchronizer`（以下简称同步器），是用来构建锁或者其他同步组件的基础框架，它使用了一个 `int` 类型的成员变量 `state` 表示同步状态，通过内置的 FIFO 队列来完成资源获取线程的排队工作。

同步器的主要使用方式是继承，子类通过继承同步器并实现它的抽象方法来管理同步状态，在抽象方法的实现过程中免不了要对同步状态进行更改，这时就需要使用同步器提供的 3 个方法 `getState()`、 `setState(int newState)` 和 `compareAndSetState(int expect, int update)` 来进行操作，因为它们能够保证状态的改变是安全的。

子类推荐被定义为自定义同步组件的**静态内部类**，同步器自身没有实现任何同步接口，它仅仅是定义了若干同步状态获取和释放的方法来供自定义同步组件使用，同步器既可以支持**独占式地获取同步状态**，也可以支持**共享式地获取同步状态**，这样就可以方便实现不同类型的同步组件（`ReentrantLock`、 `ReentrantReadWriteLock`和`CountDownLatch`等）。

同步器是实现锁的关键，在锁的实现中聚合同步器，利用同步器实现锁的语义。可以这样理解二者之间的关系：**锁是面向使用者的，它定义了使用者与锁交互的接口，隐藏了实现细节**；**同步器面向的是锁的实现者，它简化了锁的实现方式，屏蔽了同步状态管理、线程的排队、等待与唤醒等底层操作**。锁和同步器很好地隔离了使用者和实现者所需关注的领域。

#### 8.HashMap，HashTable，ConcurrentHashmap区别，底层源码 

在并发场景下使用hashmap可能会造成死循环，更普遍的是在多线程场景下，由于堆内存对于各个线程是共享的，而hashmap的put方法不是原子操作，假设Thread1先 put 值，然后 sleep 2秒(也可以是系统时间片切换失去执行权)，在这2秒内值被Thread2改了，Thread1“醒来”再 get 的时候发现已经不是原来的值了，这就容易出问题。

因为hashmap是不安全的，为了避免hashmap在多线程的情况下的问题，常规思路是给hashmap的put方法加锁（synchronized）,这是hashtable的做法。

##### 1.7和1.8中的concurrenthashmap有了许多变化

**jdk1.7**<img src="https://user-gold-cdn.xitu.io/2019/12/11/16ef52f1ce824293?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" alt="jdk1.7 ConcurrentHashMap结构" style="zoom: 80%;" />

ConcurrentHashMap 是由 Segment 数据结构和 HashEntry 数组结构构成。采取分段锁来保证安全性。

Segment 是 ReentrantLock 重入锁，在 ConcurrentHashMap 中扮演锁的角色；HashEntry 则用于存储键值对数据。

一个 ConcurrentHashMap 里包含一个 Segment 数组，一个 Segment 里包含一个 HashEntry 数组，Segment 的结构和 HashMap 类似，是一个数组和链表结构。

- 初始化：通过位运算来初始化segment的大小，默认大小为16

- get：先经过一次再散列，然后使用这个散列值通过散列运算定位到 Segment，再通过散列算法定位到元素。

  ```java
  public V get(Object key){
      int hash = hash(key.hashCode());
      return segmentFor(hash).get(key,hash);
  }
  ```

  get 操作的高效之处在于整个 get 过程都不需要加锁，除非读到空的值才会加锁重读。原因就是将使用的共享变量定义成 `volatile` 类型。

- put：进行两次 Hash 去定位数据的存储位置

  当执行put操作时，会经历两个步骤：

  1. 判断是否需要扩容
  2. 定位到添加元素的位置，将其放入 HashEntry 数组中

  插入过程会进行第一次 key 的 hash 来定位 Segment 的位置，如果该 Segment 还没有初始化，即通过 CAS 操作进行赋值，然后进行第二次 hash 操作，找到相应的 HashEntry 的位置，这里会利用继承过来的锁的特性，在将数据插入指定的 HashEntry 位置时，会通过继承 ReentrantLock 的 `tryLock()` 方法尝试去获取锁，如果获取成功就直接插入相应的位置，如果已经有线程获取该Segment的锁，那当前线程会以自旋的方式去继续的调用 `tryLock()` 方法去获取锁，超过指定次数就挂起，等待唤醒。

- size

  计算 ConcurrentHashMap 的元素大小是并发操作的，就是在你计算 size 的时候，他还在并发的插入数据，这就可能会导致你计算出来的 size 和你实际的 size 有相差。

  ConcurrentHashMap 采取的解决方法是先尝试 2 次通过不锁住 Segment 的方式来统计各个 Segment 大小，统计过程中如果 count 发生变化，则再采用加锁的方式来统计所有 Segment 的大小。

**jdk1.8**<img src="https://user-gold-cdn.xitu.io/2019/12/11/16ef52f1cf5e2104?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" alt="jdk1.8 ConcurrentHashMap结构" style="zoom:80%;" />

JDK1.8 的实现已经摒弃了 Segment 的概念，而是直接用 Node 数组+链表+红黑树的数据结构来实现，并发控制使用 Synchronized 和 CAS 来操作，整个看起来就像是优化过且线程安全的 HashMap，虽然在 JDK1.8 中还能看到 Segment 的数据结构，但是已经简化了属性，只是为了兼容旧版本。

##### concurrentHashmap如何实现的并发扩容

ComcurrentHashMap在扩容中主要使用sizeCtl和transferIndex这两个属性来协调多线程之间的并发操作，并且在扩容过程中依旧可以保证访问不阻塞

什么时候会触发扩容：

(1) 在调用 addCount 方法增加集合元素计数后发现当前集合元素个数到达扩容阈值时就会触发扩容 。

(2) 扩容状态下其他线程对集合进行插入、修改、删除、合并、compute 等操作时遇到 ForwardingNode 节点会触发扩容 。

(3) putAll 批量插入或者插入节点后发现存在链表长度达到 8 个或以上，但数组长度为 64 以下时会触发扩容 。

注意：桶上链表长度达到 8 个或者以上，并且数组长度为 64 以下时只会触发扩容而不会将链表转为红黑树 。

#### 9.blockingqueue实现原理

阻塞队列（BlockingQueue）被广泛使用在“生产者-消费者”问题中，其原因是 BlockingQueue 提供了可阻塞的插入和移除的方法。**当队列容器已满，生产者线程会被阻塞，直到队列未满；当队列容器为空时，消费者线程会被阻塞，直至队列非空时为止。**

java.util.concurrent.BlockingQueue 接口有以下阻塞队列的实现：

- **FIFO 队列** ：LinkedBlockingQueue、ArrayBlockingQueue（固定长度）
- **优先级队列** ：PriorityBlockingQueue

提供了阻塞的 take() 和 put() 方法：如果队列为空 take() 将阻塞，直到队列中有内容；如果队列为满 put() 将阻塞，直到队列有空闲位置。

#### 10.lock ，synchronized，volatile的区别

lock:

synchronized:

volatile:与内存模型JMM有关，线程同步的轻量级实现

synchronized与volatile的区别：

- volatile关键字解决的是变量在多个线程之间的可见性；而sychronized关键字解决的是多个线程之间访问共享资源的同步性
- volatile只能修饰变量，synchronized可以修饰方法和代码块
- 多线程访问volatile不会发生阻塞，synchronized会阻塞
- volatile能保证变量在多个线程之间的可见性，但不能保证原子性；而sychronized可以保证原子性，也可以间接保证可见性，因为它会将私有内存和公有内存中的数据做同步

线程安全包含原子性和可见性两个方面。
对于用volatile修饰的变量，JVM虚拟机只是保证从主内存加载到线程工作内存的值是最新的。
一句话说明volatile的作用：实现变量在多个线程之间的可见性。

synchronized和lock区别：

- Lock是一个接口，而synchronized是Java中的关键字，synchronized是内置的语言实现
- synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁
- Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断
- 通过Lock可以知道有没有成功获取锁，而synchronized却无法办到

#### 11.++i这个操作在多线程并发的情况下是否线程安全？

虽然自增（++i）看起来像是一个原子操作，但事实上它包含了三个独立的操作--获取变量的当前值，将这个值+1，再写入新的值。在并发条件下，此操作并非线程安全。

#### 12.ABA问题

CAS在进行更新操作时，会检查当前值与传入值是否一致，如果一致，就更新，但是如果一个值原来是A，变成了B，然后又变回A，就会出现ABA问题。

解决方案：jdk1.5新增AtomicStampedReference，将对象更新为一个“对象--引用”二元组，通过在引用上增加“版本号”来解决ABA问题

#### 13.线程间协作

- join（）

在线程中调用另一个线程的 join() 方法，会将当前线程挂起，而不是忙等待，直到目标线程结束。

对于以下代码，虽然 b 线程先启动，但是因为在 b 线程中调用了 a 线程的 join() 方法，b 线程会等待 a 线程结束才继续执行，因此最后能够保证 a 线程的输出先于 b 线程的输出。

```java
public class JoinExample {

    private class A extends Thread {
        @Override
        public void run() {
            System.out.println("A");
        }
    }

    private class B extends Thread {

        private A a;

        B(A a) {
            this.a = a;
        }

        @Override
        public void run() {
            try {
                a.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("B");
        }
    }

    public void test() {
        A a = new A();
        B b = new B(a);
        b.start();
        a.start();
    }
}
public static void main(String[] args) {
    JoinExample example = new JoinExample();
    example.test();
}
```

- wait（），notify（），notifyAll（）

调用 wait() 使得线程等待某个条件满足，线程在等待时会被挂起，当其他线程的运行使得这个条件满足时，其它线程会调用 notify() 或者 notifyAll() 来唤醒挂起的线程。

它们都属于 Object 的一部分，而不属于 Thread。

只能用在同步方法或者同步控制块中使用，否则会在运行时抛出 IllegalMonitorStateException。

使用 wait() 挂起期间，线程会释放锁。这是因为，如果没有释放锁，那么其它线程就无法进入对象的同步方法或者同步控制块中，那么就无法执行 notify() 或者 notifyAll() 来唤醒挂起的线程，造成死锁。

wait（）与sleep（）的区别

1. wait() 是 Object 的方法，而 sleep() 是 Thread 的静态方法；
2. wait() 会释放锁，sleep() 不会。

- await（），signal（），signalAll（）

ava.util.concurrent 类库中提供了 Condition 类来实现线程之间的协调，可以在 Condition 上调用 await() 方法使线程等待，其它线程调用 signal() 或 signalAll() 方法唤醒等待的线程。

相比于 wait() 这种等待方式，await() 可以指定等待的条件，因此更加灵活。

使用 Lock 来获取一个 Condition 对象。

























