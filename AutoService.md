#### AutoService
[官方文档](https://github.com/google/auto/tree/master/service)

#### 引入
```
compile 'com.google.auto.service:auto-service:1.0-rc4'
```

#### 官方示例说明
```
package foo.bar;

import javax.annotation.processing.Processor;

@AutoService(Processor.class)
final class MyProcessor implements Processor {
  // …
}
```
AutoService会自动在build/classes输入目录下生成文件META-INF/services/javax.annotation.processing.Processor，文件的内容如下
```
foo.bar.MyProcessor
```

在javax.annotation.processing.Processor的情况下，如果一个jar中包含metadata文件，并且在javac的classpath下，javac会自动加载这个jar，同时包含它的普通注解编译环境。

[API 详解](https://blog.csdn.net/dd864140130/article/details/53875814)

[示例](https://github.com/zdnuist/AnnotationProcessorExample)
