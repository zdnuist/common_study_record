#### JVM区域划分
![image](./img/jvm_data_areas.png)

通常可以把JVM区域分为下面几个方面，其中有的区域是以线程为单位，有的则是整个JVM进程唯一的。
1. 程序计数器（PC,Program Counter Register）. 在JVM规范中，每个线程都有它自己的程序计数器，并且任何时间一个线程都只有一个方法在执行，也就是所谓的当前方法。程序计数器会存储当前线程正在执行的java方法的JVM指令地址；或者，如果是在执行本地方法，则是未指定（undefined）。
2. Java虚拟机栈（Java Virtual Machine Stack），早期也叫Java栈。每个线程在创建时都会创建一个虚拟机栈，其内部保存一个个的栈帧（Stack Frame），对应着一次次的Java方法调用。在一个时间点，对应的只有一个活动的栈帧，通常叫作当前帧，方法所在的类叫作当前类，一直到它返回结果或者执行结束。JVM直接对Java栈的操作只有两个，就是对栈帧的压栈和出栈。
3. 堆（Heap），它是Java内存管理的核心区域，用来存放Java对象实例，几乎所有创建的Java对象实例都是被直接分配在堆上。堆被所有的线程共享，在虚拟机启动时，我们指定的“Xmx”之类的参数就是用来指定最大堆空间等指标。  
堆是垃圾收集器重点照顾的区域，所有堆内空间还会被不同的垃圾收集器进行进一步的细分，比如新生代，老年代的划分。
4. 方法区（Method Area）。这也是所有线程共享的一块内存区域，用来存储所谓的元数据（Meta），例如类结构信息，以及对应的运行时常量池、字段、方法代码等。
由于早期的Hotspot JVM实现，很多人习惯将方法区称为永久代（Permanent Generation）。Oracle JDK 8 中将永久代移除，同时增加了元数据区（Metaspace）。
5. 运行时常量池（Run-Time Constant Pool），这是方法区的一部分。Java的常量池可以存放各种常量信息，不管是编译期生成的各种字面量，还是需要在运行时决定的符号引用，所以它比一般语言的符号表存储的信息更加宽泛。
6. 本地方法栈（Native Method Stack）。它和Java虚拟机栈是非常相似的，支持对本地方法的调用，也是每个线程都会创建一个。在Oracle Hotspot JVM中，本地方法栈和Java虚拟机栈是在用一块区域，这完全取决于技术实现的决定，并未在规范中强制。

##### 扩展
1. 直接内存（Direct Memory）区域，它是Direct Buffer所直接分配的内存，也是个容易出现问题的地方。尽管在 JVM 工程师的眼中，并不认为它是 JVM 内部内模型中。
2. JVM 本身是个本地程序，还需要其他的内存去完成各种基本任务,比如JIT Compile 在运行时对热点方法进行编译，就会将编译后的方法储存在Code Cache里面；GC等功能需要运行在本地线程之中，类似部分都需要占用内存空间。

##### 问题
Java对象是不是都创建在堆上呢？  
目前有一些观点认为通过[逃逸分析](https://blog.csdn.net/blueheart20/article/details/76167489),JVM会在栈上分配那些不会逃逸的对象，这在理论上是可行的，但是取决于JVM设计者的选择。Oracle Hotspot JVM中并未这么做，这一点在逃逸分析相关的[文档](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/performance-enhancements-7.html#escapeAnalysis)里已经说明，所以可以明确所有的对象实例都是创建在堆上。

##### 参考
[1]《Java核心技术36讲--杨晓峰》https://time.geekbang.org/column/article/10192  
[2]    https://docs.oracle.com/javase/specs/jvms/se9/html/jvms-2.html#jvms-2.5


### 下面的是我的公众号二维码图片，欢迎关注

![图注:Android上下而求索](./img/qrcode_zdnuist.png)
