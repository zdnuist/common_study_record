#### Android O
[官网地址](https://developer.android.com/preview/behavior-changes.html?hl=zh-cn#o-apps)

##### 提醒窗口
使用 SYSTEM_ALERT_WINDOW 权限的应用无法再使用以下窗口类型来在其他应用和系统窗口上方显示提醒窗口：

TYPE_PHONE
TYPE_PRIORITY_PHONE
TYPE_SYSTEM_ALERT
TYPE_SYSTEM_OVERLAY
TYPE_SYSTEM_ERROR
相反，应用必须使用名为 TYPE_APPLICATION_OVERLAY 的新窗口类型。

使用 TYPE_APPLICATION_OVERLAY 窗口类型显示应用的提醒窗口时，请记住新窗口类型的以下特性：

应用的提醒窗口始终显示在状态栏和输入法等关键系统窗口的下面。
系统可以移动使用 TYPE_APPLICATION_OVERLAY 窗口类型的窗口或调整其大小，以改善屏幕显示效果。
通过打开通知栏，用户可以访问设置来阻止应用显示使用 TYPE_APPLICATION_OVERLAY 窗口类型显示的提醒窗口。  

##### 视图焦点
可点击的 View 对象现在默认也可以成为焦点。如果您希望 View 对象可点击但不可成为焦点，请在包含 View 的布局 XML 文件中将 android:focusable 属性设置为 false，或者将 false 传递至应用界面逻辑中的 setFocusable()。  

##### 安全性
如果您的应用的网络安全性配置选择退出对明文流量的支持，那么您的应用的 WebView 对象无法通过 HTTP 访问网站。每个 WebView 对象必须转而使用 HTTPS。

##### 权限
在 Android O 之前，如果应用在运行时请求权限并且被授予该权限，系统会错误地将属于同一权限组并且在清单中注册的其他权限也一起授予应用。

对于针对 Android O 的应用，此行为已被纠正。系统只会授予应用明确请求的权限。然而，一旦用户为应用授予某个权限，则所有后续对该权限组中权限的请求都将被自动批准。

例如，假设某个应用在其清单中列出 READ_EXTERNAL_STORAGE 和 WRITE_EXTERNAL_STORAGE。应用请求 READ_EXTERNAL_STORAGE，并且用户授予了该权限。如果该应用针对的是 API 级别 24 或更低级别，系统还会同时授予 WRITE_EXTERNAL_STORAGE，因为该权限也属于同一 STORAGE 权限组并且也在清单中注册过。如果该应用针对的是 Android O，则系统此时仅会授予 READ_EXTERNAL_STORAGE；不过，如果该应用后来又请求 WRITE_EXTERNAL_STORAGE，则系统会立即授予该权限，而不会提示用户。  

##### 类加载行为
Android O 检查确保类加载器在加载新类时不会违反运行时假设条件。不论类引用自 Java（来自 forName()）、Dalvik 字节码还是 JNI，都会执行这些检查。平台不会拦截 Java 对 loadClass() 函数的直接调用，也不会检查此类调用的结果。此行为不应影响运行良好的类加载器的正常运行。

平台将检查类加载器返回的类描述符是否与预期的描述符一致。如果返回的描述符与预期不符，平台会引发 NoClassDefFoundError 错误，并在异常日志中存储一条注明不一致之处的详细错误消息。

平台还检查请求的类描述符是否有效。此检查捕获间接加载诸如 GetFieldID() 等类的 JNI 调用，向这些类传递无效的描述符。例如，找不到包含 java/lang/String 签名的字段，是因为此签名无效；它应为 Ljava/lang/String;。

这与 JNI 对 FindClass() 的调用不同，其中 java/lang/String 是一个有效的完全限定名称。

Android O 不支持多个类加载器同时尝试使用相同的 DexFile 对象来定义类。尝试进行此操作，会导致 Android 运行时引发 InternalError 错误，同时显示消息“Attempt to register dex file <filename> with multiple class loaders”。

DexFile API 现已弃用，强烈建议您改为使用此平台的类加载器之一，包括 PathClassLoader 或 BaseDexClassLoader。

注： 您可以创建多个引用文件系统中同一个 APK 或 JAR 文件容器的类加载器。这样做通常不会占用大量内存：如果存储而不压缩容器中的 DEX 文件，平台可以对此类文件执行 mmap 操作，而不直接提取它们。但是，如果平台必须从容器中提取 DEX 文件，以这种方式引用 DEX 文件可能占用大量内存。

在 Android 中，所有类加载器都被视为支持并行运行。当多个线程争用同一个类加载器加载相同的类时，第一个完成此操作的线程胜出，而操作结果将用于其他线程。无论类加载器是返回同一个类、返回不同的类还是引发异常，都将发生此行为。该平台静默忽略此类异常。

注意： 在低于 Android O 的平台版本中，违反这些假设条件可能导致多次定义同一个类、由于类混淆造成堆损坏和其他不良影响。
