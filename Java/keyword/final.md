# final 关键字

> final 表示最终，可以修饰类、属性、方法

## 1、final 修饰类

**final修饰的类，表示该类是最终的类，也就是该类不能被继承**

```java
public final class类名{}
```

如：String、System、Scanner……都是使用final修饰的，也就是最终的类。

## 2、final 修饰属性

**final 修饰的属性表示该属性只能显式初始化，且只能被赋值一次，赋值后其值不再改变。**

初始化有两种方式：

（1）在声明的同时赋值。

（2）在构造方法中完成赋值。

> 一般 final 修饰的属性都与 static 关键字配合使用，作为类常量使用，节省空间(缓存在常量池中)，使用比较方便(使用类名.静态常量名)。
>
> 常量名建议全部大写，如果有多个单词之间使用下划线隔开。

## 3、final 修饰方法

**final修饰符方法，表示该方法是最终的，也就是不能被重写**

```java
public final void print(){ 方法体 }
```

> 使用final方法的原因主要有两个：
>
> 　　(1) 把方法锁定，以防止继承类对其进行更改。
>
> 　　(2) 效率，在早期的java版本中，会将final方法转为内嵌调用。但若方法过于庞大，可能在性能上不会有多大提升。因此在最近版本中，不需要final方法进行这些优化了。

### final 关键字的知识点

1. final成员变量必须在声明的时候初始化或者在构造器中初始化，否则就会报编译错误。final变量一旦被初始化后不能再次赋值。
2. 本地变量必须在声明时赋值。 因为没有初始化的过程
3. 在匿名类中所有变量都必须是final变量。
4. final方法不能被重写, final类不能被继承
5. 接口中声明的所有变量本身是final的。类似于匿名类
6. final和abstract这两个关键字是反相关的，final类就不可能是abstract的。
7. final方法在编译阶段绑定，称为静态绑定(static binding)。
8. 将类、方法、变量声明为final能够提高性能，这样JVM就有机会进行估计，然后优化。

### final方法的好处:

1. 提高了性能，JVM在常量池中会缓存final变量
2. final变量在多线程中并发安全，无需额外的同步开销
3. final方法是静态编译的，提高了调用速度
4. **final类创建的对象是只可读的，在多线程可以安全共享**