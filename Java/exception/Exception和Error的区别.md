## Exception 和 Error 有什么区别？



![](https://tva1.sinaimg.cn/large/00831rSTly1gctepegnjqj30mb0d6aaf.jpg)

Exception 和 Error 都是继承了 Throwable 类，在 Java 中只有 Throwable 类型的实例才可以被抛出（throw）或者捕获（catch），它是异常处理机制的基本组成类型。

Exception 和 Error 体现了 Java 平台设计者对不同异常情况的分类。**Exception** 是程序正常运行中，可以预料的意外情况，可能并且应该被捕获，进行相应处理。**Error** 是指在正常情况下，不大可能出现的情况，绝大部分的 Error 都会导致程序（比如 JVM 自身）处于非正常的、不可恢复状态。既然是非正常情况，所以不便于也不需要捕获，常见的比如 OutOfMemoryError 之类，都是 Error 的子类。

Exception 又分为受检（checked）异常和免检（unchecked）异常，受检异常在源代码里必须显式地进行捕获处理，这是编译期检查的一部分。免检异常就是所谓的运行时异常，类似 NullPointerException、 ArrayIndexOutOfBoundsException 之类，通常是可以编码避免的逻辑错误，具体根据需要来判断是否需要捕获，并不会在编译期强制要求。

第一，尽量不要捕获类似 Exception 这样的通用异常，而是应该捕获特定异常。

第二，不要生吞（swallow）异常。

第三，尽量不要使用 `e.printStackTrace();`。 printStackTrace() 的文档，开头就是「Prints this throwable and its backtrace to the standard error stream」。问题就在这里，在稍微复杂一点的生产系 统中，标准出错（STERR）不是个合适的输出选项，因为你很难判断出到底输出到哪里去 了。

> ### NoClassDefFoundError 和 ClassNotFoundException 有什么区别？
>
> **NoClassDefFoundError**：当 JVM 在加载一个类的时候，如果这个类在编译时是可用的，但是在运行时找不到这个类的定义的时候，JVM 就会抛出一个 NoClassDefFoundError 错误。比如 new 一个类的实例，编译后将该类的 .class 文件删除掉，就会由 JVM 抛出该异常。
>
> **ClassNotFoundException**：发生在编译的时候，在 classpath 中找不到对应的类而发生的错误。一般情况下，当我们使用 Class.forName() 或者 ClassLoader.loadClass 以及使用 ClassLoader.findSystemClass() 在运行时加载类的时候，如果类没有被找到，那么就会导致 JVM 抛出 ClassNotFoundException。