#### 7.0新特性
[7.0特性介绍](http://www.jianshu.com/p/5a1440d3ce14)

[官网说明](https://developer.android.com/about/versions/nougat/android-7.0-changes.html)  

##### Doze休眠机制
[Doze休眠机制](http://www.jianshu.com/p/6221863215b9)

##### 后台优化
Android 7.0 移除了三项隐式广播，以帮助优化内存使用和电量消耗。此项变更很有必要，因为隐式广播会在后台频繁启动已注册侦听这些广播的应用。删除这些广播可以显著提升设备性能和用户体验。

移动设备会经历频繁的连接变更，例如在 WLAN 和移动数据之间切换时。目前，可以通过在应用清单中注册一个接收器来侦听隐式 CONNECTIVITY_ACTION 广播，让应用能够监控这些变更。由于很多应用会注册接收此广播，因此单次网络切换即会导致所有应用被唤醒并同时处理此广播。

同理，在之前版本的 Android 中，应用可以注册接收来自其他应用（例如相机）的隐式 ACTION_NEW_PICTURE 和 ACTION_NEW_VIDEO 广播。当用户使用相机应用拍摄照片时，这些应用即会被唤醒以处理广播。

为缓解这些问题，Android 7.0 应用了以下优化措施：

面向 Android 7.0 开发的应用不会收到 CONNECTIVITY_ACTION 广播，即使它们已有清单条目来请求接受这些事件的通知。在前台运行的应用如果使用 BroadcastReceiver 请求接收通知，则仍可以在主线程中侦听 CONNECTIVITY_CHANGE。
应用无法发送或接收 ACTION_NEW_PICTURE 或 ACTION_NEW_VIDEO 广播。此项优化会影响所有应用，而不仅仅是面向 Android 7.0 的应用。
如果您的应用使用任何 intent，您仍需要尽快移除它们的依赖关系，以正确适配 Android 7.0 设备。Android 框架提供多个解决方案来缓解对这些隐式广播的需求。例如，JobScheduler API 提供了一个稳健可靠的机制来安排满足指定条件（例如连入无限流量网络）时所执行的网络操作。您甚至可以使用 JobScheduler 来适应内容提供程序变化  
##### 应用间共享文件
对于面向 Android 7.0 的应用，Android 框架执行的 StrictMode API 政策禁止在您的应用外部公开 file:// URI。如果一项包含文件 URI 的 intent 离开您的应用，则应用出现故障，并出现 FileUriExposedException 异常。

要在应用间共享文件，您应发送一项 content:// URI，并授予 URI 临时访问权限。进行此授权的最简单方式是使用 FileProvider 类。
##### 配置文件指导的 JIT/AOT 编译  
在 Android 7.0 中，我们添加了即时 (JIT) 编译器，对 ART 进行代码分析，让它可以在应用运行时持续提升 Android 应用的性能。JIT 编译器对 Android 运行组件当前的 Ahead of Time (AOT) 编译器进行了补充，有助于提升运行时性能，节省存储空间，加快应用更新和系统更新速度。

配置文件指导的编译让 Android 运行组件能够根据应用的实际使用以及设备上的情况管理每个应用的 AOT/JIT 编译。例如，Android 运行组件维护每个应用热方法的配置文件，并且可以预编译和缓存这些方法以实现最佳性能。对于应用的其他部分，在实际使用之前不会进行编译。

除提升应用的关键部分的性能外，配置文件指导的编译还有助于减少整个 RAM 占用，包括关联的二进制文件。此功能对于低内存设备非常尤其重要。

Android 运行组件在管理配置文件指导的编译时，可最大程度降低对设备电池的影响。仅当设备处于空闲状态和充电时才进行编译，从而可以通过提前执行该工作节约时间和省电。  

Android 运行组件的 JIT 编译器最实际的好处之一是应用安装和系统更新的速度。即使在 Android 6.0 中需要几分钟进行优化和安装的大型应用，现在只需几秒钟就可以完成安装。系统更新也变得更快，因为省去了优化步骤  

##### SurfaceView
Android 7.0 可同步移动到 SurfaceView 类，此类在某些情况下提供的电池性能优于 TextureView：在渲染视频或 3D 内容时，包含滚动和动画视频位置的应用在使用 SurfaceView 时比 TextureView 耗电更少。

SurfaceView 类可减少屏幕合成对电池的消耗，因为它是在专用硬件中合成，与应用窗口内容分隔开。因此，它产生的中间副本少于 TextureView。
现在，SurfaceView 对象的内容位置和包含的应用内容同步更新。这一变化导致的一个结果是，在画面移动时，SurfaceView 中播放的视频的简单的平移或缩放不再在画面侧面产生黑条。

从 Android 7.0 开始，我们强烈建议您使用 SurfaceView 代替 TextureView，以实现省电。  


##### 示例
[多窗口 Playground](https://github.com/googlesamples/android-MultiWindowPlayground)

[活动通知](https://github.com/googlesamples/android-ActiveNotifications)  

[消息传递服务](https://github.com/googlesamples/android-MessagingService)

[直接启动](https://github.com/googlesamples/android-DirectBoot  )

[作用域目录访问](https://github.com/googlesamples/android-ScopedDirectoryAccess)
