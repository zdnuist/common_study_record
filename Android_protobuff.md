##### Android集成protobuff
protobuff是Google开源的一个二进制协议，原理是通过序列号设置成员变量位置，然后实现序列化  
在Android集成protobuff时候，不能使用google官方的集成方法，改用"Square wire"项目  
项目主页：
https://github.com/square/wire
集成方法:
1.gradle中加入项目组件
```
compile 'com.squareup.wire:wire-runtime:2.0.3'
```
2.加入代码混淆
```
-keep class com.squareup.wire.** { *; }
-keep class com.yourcompany.yourgeneratedcode.** { *; }
```
3.下载wire-compiler-VERSION-jar-with-dependencies.ja
wire-compiler-VERSION-jar-with-dependencies.jar是一个编译好的jar包，作用是根据proto语法，生成对应的java文件。
需要注意的是，原项目中并没有给出这个jar包的下载地址，想要下载它，需要去http://search.maven.org/中搜索“com.squareup.wire”
http://search.maven.org/#search|ga|1|g%3A%22com.squareup.wire%22
注意下载的jar包版本哦。必须跟gradle中编译的版本保持一致，否则可能会出现编译错误or一些bug  
4.根据proto文件生成java文件
```
java -jar -Dfile.encoding=UTF-8 wire-compiler-2.0.3-jar-with-dependencies.jar --proto_path=. --java_out=. *.proto  
```
需要注意的点：
-Dfile.encoding=UTF-8 ： 指明生成的java文件编码是utf8，不指明的话，会使用系统编码。在win7系统默认gbk，会出现中文乱码。
--proto_path：proto文件路径，“.”是当前目录。
--java_out : java文件的生成目录。
*.proto：针对所有proto文件生成java文件   
5.使用  
以阿里妈妈广告为例
```
Request data;
byte[] buffArray = Request.ADAPTER.encode(data); //进行序列化
Okhttp3.Response response;
Response.ADAPTER.decode (response.body().byteStream()); //进行反序列化
```

##### 参考
http://blog.csdn.net/oqqtim12/article/details/50505590
