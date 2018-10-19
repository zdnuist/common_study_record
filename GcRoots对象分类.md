GC Roots的大致分类：
1. Class 由System Class Loader/Boot Class Loader加载的类对象，这些对象不会被回收。其它的Class Loader实例加载的类对象不一定是GC Root，除非这个类对象恰好是其它形式的GC root
2. Thread 线程，激活状态的线程
3. Stack Local栈中的对象，每个线程都会分配一个栈，栈中的局部变量或者参数都是GC root，因为它们的引用随时可能被用到
4. JNI Local JNI中的局部变量和参数引用的对象；可能在JNI中定义的，也可能在虚拟机中定义
5. JNI Global JNI中的全局变量引用的对象
6. Monitor Used 用于保证同步的对象，例如wait(),notify()中使用的对象，锁等
7. Held by JVM JVM持有的对象。JVM为了特殊用途保留的对象，它与JVM的具体实现有关。比如有System Class Loader，一些Exceptions对象，和一些其他的Class Loader。

##### 参考
1. https://www.jianshu.com/p/f5582d9a0f73
2. 
