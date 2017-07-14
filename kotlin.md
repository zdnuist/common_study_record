##### @JvmOverloads
通常，如果你写一个有默认参数值的 Kotlin 方法，在 Java 中只会有一个所有参数都存在的完整参数签名的方法可见，如果希望向 Java 调用者暴露多个重载，可以使用 @JvmOverloads 注解  
```
@JvmOverloads fun f(a: String, b: Int = 0, c: String = "abc") {
    ……
}
```
对于每一个有默认值的参数，都会生成一个额外的重载，这个重载会把这个参数和它右边的所有参数都移除掉。在上例中，会生成以下方法  
```
// Java
void f(String a, int b, String c) { }
void f(String a, int b) { }
void f(String a) { }
```
该注解也适用于构造函数、静态方法等。它不能用于抽象方法，包括在接口中定义的方法。

请注意，如次构造函数中所述，如果一个类的所有构造函数参数都有默认值，那么会为其生成一个公有的无参构造函数。这就算没有 @JvmOverloads 注解也有效  

##### let
if not null 执行代码
```
val data = ....
data?.let {
   .... // 代码会执行到此处, 假如data不为null
}
```

##### companion
伴生对象
```
class MyClass{
  companion object Factory{
    fun create(): MyClass = MyClass()
  }
}
```
该伴生对象的成员可通过只使用类名作为限定符来调用：
```
val instance = MyClass.create()
```
可以省略伴生对象的名称，在这种情况下将使用名称 Companion：
```
class MyClass {
    companion object {
    }
}

val x = MyClass.Companion
```
请注意，即使伴生对象的成员看起来像其他语言的静态成员，在运行时他们仍然是真实对象的实例成员，而且，例如还可以实现接口：
```
interface Factory<T> {
    fun create(): T
}


class MyClass {
    companion object : Factory<MyClass> {
        override fun create(): MyClass = MyClass()
    }
}
```  
伴生对象的初始化是在相应的类被加载（解析）时，与 Java 静态初始化器的语义相匹配

##### 模块
可见性修饰符 internal 意味着该成员只在相同模块内可见。更具体地说， 一个模块是编译在一起的一套 Kotlin 文件：

一个 IntelliJ IDEA 模块；
一个 Maven 项目；
一个 Gradle 源集；
一次 ＜kotlinc＞ Ant 任务执行所编译的一套文件
