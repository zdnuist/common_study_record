#####Binder是一种进程间通信机制
http://blog.csdn.net/luoshengyang/article/details/6618363
https://blog.csdn.net/boyupeng/article/details/47011383
Binder数据拷贝只需要一次
基于C/S架构  

在Android系统的Binder机制中，由一系列组件组成，分别是Client,Server,Service Manager和Binder驱动程序，其中Client,Server和Service Manager运行在用户空间,Binder驱动程序运行在内核空间。


1. Client、Server和Service Manager实现在用户空间中，Binder驱动程序实现在内核空间中

        2. Binder驱动程序和Service Manager在Android平台中已经实现，开发者只需要在用户空间实现自己的Client和Server

        3. Binder驱动程序提供设备文件/dev/binder与用户空间交互，Client、Server和Service Manager通过open和ioctl文件操作函数与Binder驱动程序进行通信

        4. Client和Server之间的进程间通信通过Binder驱动程序间接实现

        5. Service Manager是一个守护进程，用来管理Server，并向Client提供查询Server接口的能力


#####Service Manager是成为Android进程间通信（IPC）机制Binder守护进程的过程是这样的：

        1. 打开/dev/binder文件：open("/dev/binder", O_RDWR);

        2. 建立128K内存映射：mmap(NULL, mapsize, PROT_READ, MAP_PRIVATE, bs->fd, 0);

        3. 通知Binder驱动程序它是守护进程：binder_become_context_manager(bs);

        4. 进入循环等待请求的到来：binder_loop(bs, svcmgr_handler);

        在这个过程中，在Binder驱动程序中建立了一个struct binder_proc结构、一个struct  binder_thread结构和一个struct binder_node结构，这样，Service Manager就在Android系统的进程间通信机制Binder担负起守护进程的职责了


Parcelable方式的实现原理是讲一个完整的对象进行分解，而分解后的每一部分都是Intent所支持的数据类型，这样也就实现传递对象的功能
  1. 在使用内存的时候，Parcelable比Serializable性能高
  2. Serializable在序列化的时候会产生大量的临时变量，从而引起频繁的GC
  3. Parcelable不能使用在要讲数据存储在磁盘上的情况，因为Parcelable不能很好的保证数据的持续性在外界有变化的情况下
